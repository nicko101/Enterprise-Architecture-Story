# Azure Hub-and-Spoke Network Infrastructure Briefing

## Executive Summary
This document outlines the architecture of a sophisticated Hub-and-Spoke network topology deployed in the Azure **westeurope** region. The central element of this infrastructure is a **Palo Alto VM-Series firewall**, configured as a Network Virtual Appliance (NVA), which serves as the primary point for traffic inspection and security policy enforcement.

All network traffic—including traffic between spokes (east-west), traffic to and from the internet (north-south), and traffic to a defined on-premises location via a Site-to-Site VPN (hybrid)—is systematically forced through this central firewall. This is achieved using a combination of VNet peering and Azure User-Defined Routes (UDRs) that direct all traffic to an internal load balancer fronting the Palo Alto appliance.

---

## Core Network Architecture
The foundation of the deployment is a classic Hub-and-Spoke model, providing network segmentation, centralized control, and scalability.

### Hub Virtual Network (`fwvnet`)
The hub VNet acts as the central point of connectivity and hosts shared network services.
* **Address Space:** `172.18.0.0/16`
* **Subnet Configuration:**

| Subnet Name | Address Prefix | Purpose |
| :--- | :--- | :--- |
| **Mgmt** | `172.18.0.0/24` | Firewall management traffic. |
| **Public** | `172.18.1.0/24` | Untrusted, internet-facing traffic (Untrust). |
| **Private** | `172.18.2.0/24` | Trusted, internal network traffic (Trust). |
| **VPN** | `172.18.3.0/24` | Traffic related to the VPN interface of the NVA. |
| **GatewaySubnet** | `172.18.4.0/27` | Dedicated subnet for the VPN Gateway. |

### Spoke Virtual Network (`nf_vm_vnet`)
Designed to host isolated workloads, with all traffic routed through the central hub.
* **Address Space:** `172.20.0.0/16`
* **Subnet (`VMs`):** `172.20.0.0/24`

---

## Central Firewall (Palo Alto VM-Series)
The core of the network's security is the `nfpalovm` virtual machine (Version 11.2.5). 

### Interface Configuration
The firewall utilizes four NICs with **IP Forwarding enabled**.

| NIC Name | Subnet | Private IP | Public IP | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **eth0** | Mgmt | `172.18.0.4` | `4.180.160.82` | Management |
| **eth1** | Public | `172.18.1.4` | `20.61.64.206` | Untrust/Internet |
| **eth2** | Private | `172.18.2.4` | None | Trust (connects to ILB) |
| **eth3** | VPN | `172.18.3.4` | None | VPN-related traffic |

---

## Traffic Management & Routing Logic

### GatewaySubnet "Split-Routing"
A platform restriction prevents a standard `0.0.0.0/0` (Default Route) from being pointed to a Virtual Appliance on the **GatewaySubnet**. To bypass this and ensure 100% "Inbound Forced Tunneling," we implement **Longest Prefix Match (LPM)** logic.

![Routing Logic](Solutions_Architecture/PoC%20and%20Validation/Azure%20Network%20Infrastructure%20Briefing:%20Palo%20Alto%20Firewall%20Deployment/routing-logic.png)

* **The Logic:** We define two routes (`0.0.0.0/1` and `128.0.0.0/1`). Because a `/1` prefix is more specific than the system's `/0` default, the Gateway is forced to hand packets to our **Internal Load Balancer (`172.18.2.5`)**.



### Spoke VNet Routing (`nf_rt_vnets`)
* **Associated Subnet:** `VMs` in `nf_vm_vnet`.
* **Route:** `0.0.0.0/0` → Next Hop: **Virtual Appliance** (`172.18.2.5`).
* **Result:** All internet-bound and cross-VNet traffic is inspected by the NVA.

---

## Hybrid Connectivity (Site-to-Site VPN)
A VPN connection bridges the Azure network with the on-premises location.
* **VPN Gateway:** `nf_vpngw` (SKU: `VpnGw1AZ`, Public IP: `132.220.97.108`)
* **Local Network Gateway:** `37.228.234.241`
* **On-Prem Address Space:** `192.168.0.0/24`, `10.0.0.0/8`
* **Protocol:** IPsec IKEv2



---

## Security Posture
The security implementation centralizes policy enforcement on the Palo Alto NVA. 

* **Network Security Groups (NSGs):** Native Azure NSGs are configured with permissive "Allow-All" rules. This ensures the NVA software—not the Azure fabric—is the primary point of control. 
* **Trust NSG (`nf_nsg_priv`):** Includes a specific outbound rule for `172.20.0.0/24` to `10.0.11.0/24`.
* **Filtering:** Comprehensive L7 inspection, URL filtering, and SNAT are performed within the Palo Alto VM-Series.

---

### Deployment Metadata
* **Region:** West Europe
* **VM Size:** `Standard_D8_v4`
* **Availability Set:** `nf_av_set`

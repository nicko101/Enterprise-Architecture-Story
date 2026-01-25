# Azure Network Infrastructure: Palo Alto NVA & S2S VPN Deployment

![Architecture Diagram](Solutions_Architecture/PoC%20and%20Validation/Azure%20Network%20Infrastructure%20Briefing:%20Palo%20Alto%20Firewall%20Deployment/vpngw-nva-vpn.png)

## 1. Executive Summary
This repository contains the technical documentation for a secure **Hub-and-Spoke** network in **Azure West Europe**. The architecture utilizes a **Palo Alto VM-Series NVA** to provide centralized deep packet inspection (DPI) for all traffic traversing the environment.

---

## 2. Architectural Overview

### 2.1. Hub-and-Spoke Topology
* **Hub VNet (`fwvnet`)**: `172.18.0.0/16`. Hosts security appliances, the VPN Gateway, and the Internal Load Balancer.
* **Spoke VNet (`nf-vm-vnet`)**: `172.20.0.0/16`. Hosts workload VMs; all traffic is forced to the Hub for inspection.

### 2.2. Subnet & Interface Mapping
| Subnet | Range | Interface IP | Purpose |
| :--- | :--- | :--- | :--- |
| **Mgmt** | `172.18.0.0/24` | `172.18.0.4` | Out-of-band management and UI/SSH access. |
| **Public** | `172.18.1.0/24` | `172.18.1.4` | External Untrust zone for internet egress. |
| **Private** | `172.18.2.0/24` | `172.18.2.4` | **Trust Zone**: Primary Backend for the ILB. |
| **VPN** | `172.18.3.0/24` | `172.18.3.4` | Transit for traffic from the S2S VPN Gateway. |
| **GatewaySubnet**| `172.18.4.0/27` | N/A | Reserved for the Virtual Network Gateway. |

---

## 3. High Availability & Traffic Engineering

### 3.1. Internal Load Balancer (ILB)
The ILB provides High Availability for the NVA cluster by acting as a single Virtual IP (VIP) for all Route Table next hops.
* **Frontend VIP:** `172.18.2.10` (Static Private IP in the Private/Trust Subnet).
* **Backend Pool:** `172.18.2.4` (Palo Alto Trust Interface).
* **HA Ports Rule:** Configured for **All Ports/All Protocols** to support multi-protocol firewall inspection.

### 3.2. User-Defined Routing (UDR)
All traffic is steered to the ILB Frontend IP to ensure zero bypass of the NVA:

| Route Table | Target Subnet | Destination | Next Hop (ILB VIP) |
| :--- | :--- | :--- | :--- |
| **`nf-rt-vnets`** | Spoke VMs | `0.0.0.0/0` | `172.18.2.10` |
| **`nf-rt-vnets`** | Spoke VMs | `10.0.0.0/8` | `172.18.2.10` |
| **`nf-vpn-gw-rt`** | GatewaySubnet | `172.20.0.0/24` | `172.18.2.10` |

---

## 4. Hybrid Connectivity (S2S VPN)
* **Local Network Gateway:** `37.228.234.241`.
* **On-Prem Prefixes:** `10.0.0.0/8`, `192.168.0.0/24`.
* **Gateway SKU:** VpnGw1.

---

## 5. Post-Deployment Checklist
1.  **NVA Management:** Connect via `http://nfpalovm-mgmt.westeurope.cloudapp.azure.com`.
2.  **Palo Alto Zones:** Assign interfaces to zones (Untrust=eth1, Trust=eth2, VPN=eth3).
3.  **Health Probes:** Enable an Interface Management Profile on `eth2` (Trust) to allow TCP 443/22 probes.
4.  **IP Forwarding:** Ensure **IP Forwarding** is enabled on all Azure data NICs.

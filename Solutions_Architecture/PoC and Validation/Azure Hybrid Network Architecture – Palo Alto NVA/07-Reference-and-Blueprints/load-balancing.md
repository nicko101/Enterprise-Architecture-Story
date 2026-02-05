# Azure High-Availability Infrastructure: Palo Alto VM-Series NGFW

![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Palo Alto Networks](https://img.shields.io/badge/palo_alto_networks-%23009AD9.svg?style=for-the-badge&logo=paloaltonetworks&logoColor=white)

## 📌 Executive Summary
This project documents the deployment of a High-Availability (HA) Palo Alto VM-Series firewall cluster in Azure. 

**The Pivot:** Originally intended as a native Azure VPN Gateway termination, the architecture was shifted to a Palo Alto NVA (Network Virtual Appliance). This move was necessitated by the need for advanced Layer 7 inspection and more granular control over the `GatewaySubnet` routing logic, which proved too restrictive for complex transit scenarios.

---

## 🏗️ Architectural Overview
The infrastructure is deployed in the `westeurope` region within a central `fwVNET` (172.18.0.0/16). 



### Core Components
* **Firewall Pair:** `nfpaloaltovm` (Standard_B4ls_v2) and `nfpaloaltovm2` (Standard_D8_v4) running PAN-OS 11.2.5.
* **External Load Balancer (`nf-elb`):** Acts as the primary ingress/egress point, terminating Site-to-Site (S2S) VPN connections.
* **Internal Load Balancer (`nf-intera`):** Configured with **HA Ports** to inspect all East-West (internal) traffic seamlessly.
* **Network Segmentation:** Dedicated subnets for Management, Public (Untrust), and Private (Trust) zones.
* **Hybrid Connectivity:** An optional, zone-redundant VPN Gateway (`nf-vpngw`) for regional redundancy.

---

## 🚦 Traffic Flow & Routing Logic

### Subnet Design
| Subnet | Address Prefix | Associated NSG | Purpose |
| :--- | :--- | :--- | :--- |
| **Mgmt** | 172.18.0.0/24 | DefaultNSG | Management access (eth0). |
| **Public** | 172.18.1.0/24 | Allow-All | Untrusted traffic / VPN Termination (eth1). |
| **Private** | 172.18.2.0/24 | Allow-All | Trusted traffic / Internal VNet access (eth2). |
| **Gateway**| 172.18.3.0/27 | None | Dedicated for the Virtual Network Gateway. |

### The "Symmetric Return" Solution (Critical)
In a dual-NVA setup, traffic often enters through one firewall while the reply is hashed to the other by the Azure Load Balancer (Asymmetric Routing), causing session drops.

**The Fix:**
We implemented a **Source NAT (SNAT)** policy on the Trust interface. This translates the source IP of inbound VPN traffic to the NVA's local Trust IP.
* **Result:** The destination VM replies directly to the NVA that initiated the session, bypassing the Load Balancer's hashing and ensuring symmetric traffic flow.

---

## 🔧 Configuration Highlights

### External Load Balancer (`nf-elb`)
| Rule | Port/Protocol | Persistence | Floating IP |
| :--- | :--- | :--- | :--- |
| **VPN IKE** | 500/UDP | **Client IP** | Disabled |
| **VPN NAT-T** | 4500/UDP | **Client IP** | Disabled |
| **Web/SSL** | 443/TCP | Default | Disabled |

### Security Policy Philosophy
Subnet-level **Network Security Groups (NSGs)** are configured as "Allow-All" for the Data Plane. This offloads all security enforcement, threat prevention, and logging to the Palo Alto appliances.

---

## 🛠️ Troubleshooting Guide

| Symptom | Probable Cause | Investigation Command |
| :--- | :--- | :--- |
| **VPN Tunnel Up, No Ping** | Zone Mismatch | `show session all filter destination <IP>` |
| **Aged-out Sessions** | Asymmetric Routing | Check if SNAT policy is active on Trust zone. |
| **Packet Loss over VPN** | Proxy ID Mismatch | `show vpn flow name <tunnel-name>` |
| **Internet Fails** | Incorrect Next-VR | `test routing fib-lookup virtual-router trust-vr ip 8.8.8.8` |

---

## 🚀 Deployment Metadata
* **Region:** West Europe
* **Software:** Palo Alto VM-Series Flex (BYOL)
* **Image Version:** 11.2.5
* **Admin:** azureuser1

---

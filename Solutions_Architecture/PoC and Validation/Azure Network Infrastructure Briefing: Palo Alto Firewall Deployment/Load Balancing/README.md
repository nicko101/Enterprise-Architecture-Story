# Azure High-Availability Infrastructure: Palo Alto VM-Series NGFW

![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Palo Alto Networks](https://img.shields.io/badge/palo_alto_networks-%23009AD9.svg?style=for-the-badge&logo=paloaltonetworks&logoColor=white)

## 📌 Executive Summary
This project documents the deployment of a High-Availability (HA) Palo Alto VM-Series firewall cluster in Azure. 

**The Pivot:** Originally intended as a native Azure VPN Gateway termination, the architecture was shifted to a Palo Alto NVA (Network Virtual Appliance). This move was necessitated by the need for advanced Layer 7 inspection and more granular control over the `GatewaySubnet` routing logic, which proved too restrictive for complex transit scenarios.

---

## 🏗️ Architectural Overview
The infrastructure is deployed in the `westeurope` region within a central `fwVNET` (172.18.0.0/16). 

[Image of Azure Load Balancer sandwich with Palo Alto NVAs and S2S VPN]

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
| **Public** | 172.18.1.0/24 | Allow-

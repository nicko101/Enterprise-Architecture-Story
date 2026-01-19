# Cloud Networking: Azure Fabric and Hybrid Connectivity

## Overview
This directory documents the cloud-native networking architecture within the Microsoft Azure environment. It serves as the cloud-side termination point for the hybrid fabric, providing the infrastructure for secure service delivery, name resolution, and cross-premises connectivity.

---

## Integrated Architecture
The Azure fabric is integrated directly with the on-premises security stack to enable seamless identity-aware policies and secure resource access.

[![Azure Cloud Networking and Hybrid Fabric](../../../resources/slides/azure.png)](../../../resources/slides/azure.png)
*Figure 1: High-level overview showing the connection between Azure resources and the on-premises infrastructure.*

---

## Technical Components

### Azure DNS
* **Private Resolver**: Implementation of Azure Private DNS Resolvers to facilitate bi-directional name resolution between the local Proxmox lab and the Azure tenant.
* **Private Zones**: Management of internal DNS zones for cloud-native services to ensure consistency across the hybrid environment.

### Virtual Network (VNet) Fabric
* **Segmentation**: Logical isolation of cloud resources through specific subnets for gateways, servers, and endpoint management.
* **Peering**: Configurations for secure communication between distinct Virtual Networks where applicable to maintain a hub-and-spoke topology.

### Hybrid Connectivity (VPN Gateway)
* **IPsec Tunneling**: Configuration artifacts for the Azure Virtual Network Gateway to terminate the Site-to-Site VPN from the Palo Alto NGFW.
* **BGP and Routing**: Implementation of dynamic or static routing tailored for the tenant environment to ensure predictable traffic flow.

---

## Directory Contents
* **configs/**: Technical export files and portal configuration artifacts for VNet and VPN settings.
* **diagrams/**: Visual representations of the Azure-side topology and traffic flow patterns.

---

## Performance and Standards
* **Name Resolution**: Cross-premises DNS ensures that on-premises domain controllers and Azure services can resolve each other with minimal latency.
* **Traffic Security**: All traffic entering the Azure fabric from the local lab is encrypted via IKEv2 and AES-256 standards.
* **Availability**: Cloud resources are architected to maintain persistent connectivity for critical identity synchronization (Entra Connect) and certificate services (NDES).

---
[Return to Networking](../) | [Return to Lab Root](../../README.md)

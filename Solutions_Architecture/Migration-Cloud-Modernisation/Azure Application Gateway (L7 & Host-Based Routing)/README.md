# Azure Hub-and-Spoke Infrastructure: Application Gateway Integration

## Project Artifacts
The following technical resources document the design and automated deployment of this infrastructure:
* **Architecture Briefing**: [Azure Hub-Spoke Infrastructure Blueprint (PDF)](../../../resources/slidedecks/Azure_Hub_Spoke_Infrastructure_Blueprint.pdf)
* **Infrastructure as Code**: [Application Gateway ARM Template (JSON)](../../../resources/templates/app_gw.json)

---

## Executive Summary
This project demonstrates the deployment of a comprehensive hub-and-spoke network topology within Microsoft Azure. The architecture establishes a central Hub Virtual Network (VNet) for shared connectivity and a peered Spoke VNet for application ingress. This design effectively segregates the application ingress point from core network services and workloads, providing a scalable foundation for modern cloud application delivery.



---

## I. Core Architectural Design: Hub-and-Spoke Topology
The deployment utilizes a modular hub-and-spoke model, interconnected via bidirectional VNet peering to ensure seamless cross-network communication.

* **Hub Virtual Network (nf-hubvnet)**: Address Space `172.16.0.0/16`. Hosts backend virtual machine workloads and the central VPN Gateway.
* **Spoke Virtual Network (nf-vnet-appgw)**: Address Space `10.14.0.0/16`. Dedicated to the Standard_v2 Application Gateway.

**VNet Peering Configuration**:
* **Hub to Spoke**: Configured with gateway transit allowed, enabling the spoke to utilize the hub's VPN for external connectivity.
* **Spoke to Hub**: Configured to use remote gateways, directing traffic destined for outside the Azure network through the hub.

---

## II. Centralized Connectivity and Workloads (Hub VNet)
The hub serves as the primary integration point for hybrid cloud connectivity and core compute resources.

### Hybrid Connectivity
A site-to-site IPsec connection integrates the Azure environment with an on-premises network endpoint.
* **VPN Gateway**: VpnGw1 SKU terminating at Public IP `20.160.99.165`.
* **Local Network Gateway**: Represents the on-premises network with gateway IP `37.228.231.33`.
* **Connection**: Utilizes IPsec/IKEv2 for resilient, encrypted transit.

### Compute Workloads
Two Linux virtual machines (Ubuntu 24.04 LTS) are provisioned directly within the hub.
* **Instances**: nf-vm1 (172.16.0.4) and nf-vm2 (172.16.0.5).
* **Security**: Configured with Trusted Launch, Secure Boot, and vTPM for hardware-level integrity.

---

## III. Application Delivery and Ingress (Spoke VNet)
The spoke VNet manages public-facing traffic ingress via a sophisticated Layer 7 Application Gateway.



* **Application Gateway**: Standard_v2 SKU with autoscaling and multi-zone availability.
* **Host-Based Routing**: The gateway utilizes unique listeners to route traffic based on requested hostnames:
    * `vm1.nginx.local` → Routed to the backend pool.
    * `vm2.lab01.local` → Routed to the backend pool.
* **Backend Address Pool**: Targets the private IPs of the virtual machines located within the peered Hub VNet.

---

## IV. Security and Network Configuration
Security is enforced through multi-layered Network Security Groups (NSGs) and advanced VM security features.

* **Network Security Groups**: Attached to each VM interface to control inbound traffic flow, including specific rules for management access via SSH.
* **Trusted Launch**: Provides UEFI security, ensuring that only verified operating systems and drivers can start.

---
[Return to Solutions Architecture](../../README.md) | [Return to Root](../../../README.md)

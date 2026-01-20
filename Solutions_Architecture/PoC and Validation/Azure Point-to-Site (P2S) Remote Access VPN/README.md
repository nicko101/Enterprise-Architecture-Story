# Azure Hybrid Connectivity: S2S VPN and P2S Client Access

## Executive Summary
This project documents the deployment of a hybrid cloud network infrastructure in the **westeurope** region using Azure Resource Manager (ARM). The primary objective is to establish a secure, dynamic **Site-to-Site (S2S) VPN** connection leveraging **Border Gateway Protocol (BGP)** for automated route exchange.

Additionally, the architecture supports **Point-to-Site (P2S)** connectivity, enabling remote clients to connect securely via OpenVPN using certificate-based authentication.

* **Project Presentation**: [Download/View Technical Slide Deck](../../../resources/slidedecks/Azure_Hybrid_Connectivity_and_Trusted_Compute_Deployment.pdf)

---

## Core Network Architecture
The foundation of the deployment is a centralized Hub Virtual Network (VNet) designed to isolate gateway traffic from general workloads.

### Virtual Network (nf-hub)
* **Address Space**: `172.16.0.0/16`
* **Location**: westeurope
* **Subnets**:
    * **default**: `172.16.0.0/24` (Hosts VM workloads)
    * **GatewaySubnet**: `172.16.1.0/27` (Dedicated VPN Gateway capacity)

### Public Endpoint
* **Public IP (nf-gwip)**: `52.166.77.218`
* **SKU**: Standard (Static Allocation)

[Image of an Azure Virtual Network architecture with a GatewaySubnet and a default subnet]

---

## Site-to-Site (S2S) VPN Connectivity
The S2S tunnel provides a robust IPsec bridge between Azure and the on-premises network.

### Gateway Specifications
* **Name**: `ng-vpngw`
* **SKU**: `VpnGw1` (Generation 1, Route-Based)
* **BGP**: Enabled

### Local Network Gateway (nf-lng)
* **On-Premises IP**: `37.228.231.33`
* **Address Space**: `10.0.0.0/8` and `192.168.0.0/24`

### BGP Configuration Summary
BGP facilitates dynamic routing, ensuring that cloud and on-premises environments are aware of each other's network changes without manual route updates.

| Parameter | Azure Gateway (ng-vpngw) | On-Premises Gateway (nf-lng) |
| :--- | :--- | :--- |
| **ASN** | 65515 | 65010 |
| **Peering IP** | 172.16.1.30 | 10.0.0.1 |

[Image of BGP peering between Azure VPN Gateway and on-premises router]

---

## Point-to-Site (P2S) VPN Configuration
The VPN Gateway is configured to provide secure remote access for individual administrative clients.

* **Client Address Pool**: `10.1.1.0/24`
* **Protocol**: OpenVPN
* **Authentication**: Certificate-based (Root Certificate embedded in ARM template)
* **Encryption**: TLS-based security for remote engineering tasks.

---

## Virtual Machine Deployment & Security
A Linux workload was provisioned to validate cross-premises connectivity.

### VM Specifications (nf-vm1)
* **OS**: Ubuntu 24.04 LTS (Trusted Launch / Secure Boot enabled)

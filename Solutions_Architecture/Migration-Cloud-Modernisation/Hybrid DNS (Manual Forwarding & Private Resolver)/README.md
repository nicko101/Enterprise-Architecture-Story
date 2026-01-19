# Azure Hybrid DNS Resolution Architecture  
**DNS Private Resolver & Private Endpoint Name Resolution**

---

## Executive Summary
This document provides a comprehensive architectural analysis of an Azure cloud deployment implementing **centralized hybrid DNS resolution** using **Azure DNS Private Resolver**.

The architecture combines a **traditional hub-and-spoke Virtual Network (VNet) design** with **Azure Virtual WAN (vWAN)** to support both **hybrid Site-to-Site (S2S)** connectivity and **scalable Point-to-Site (P2S)** remote access. The primary objective of this deployment is to ensure **consistent, secure name resolution** for private endpoints across Azure VNets, Virtual WAN–connected networks, and on-premises environments.

Key capabilities demonstrated include:
- Hybrid DNS resolution using inbound and outbound DNS Private Resolver endpoints
- Private Endpoint name resolution via Azure Private DNS Zones
- Coexistence of VNet peering and Virtual WAN connectivity models
- Secure remote administration using Azure Bastion
- Explicit identification and analysis of **security misconfigurations**

This environment is documented as an **architectural evaluation and engineering learning platform**, not as a hardened production design.

---

## Quick Navigation

- 🔹 **[DNS Resolution Architecture](#dns-resolution-architecture)**  
  How name resolution works across Azure VNets, Private Endpoints, Virtual WAN, and on-premises networks.

- 🔹 **[Security Configuration Analysis](#security-configuration-analysis)**  
  Review of Network Security Group posture, identified risks, and architectural implications.

---

## Core Network Topology

### Hub-and-Spoke VNet Architecture
Three primary VNets are deployed in the **West Europe** region.

| VNet | Address Space | Role |
|----|--------------|------|
| `nf-hub-vnet` | 172.16.0.0/16 | Central hub hosting VPN Gateway and DNS Resolver |
| `nf-vnet-spoke1` | 172.17.0.0/16 | Workload spoke hosting compute and Bastion |
| `nf-endpoint-vnet` | 172.19.0.0/16 | Services spoke hosting private endpoints |

#### Hub VNet Subnets
- `GatewaySubnet` – 172.16.1.0/27 (VPN Gateway)
- `dns-resolve-out` – 172.16.2.0/24 (DNS Resolver outbound)
- `dns-resolve` – 172.16.3.0/24 (DNS Resolver inbound)

---

## VNet Peering Configuration

All VNets are interconnected using **bidirectional VNet peering**.

### Gateway Transit Configuration
- **Hub → Spoke:** `allowGatewayTransit = true`
- **Spoke → Hub:** `useRemoteGateways = true`

This allows spoke VNets to consume the hub’s VPN gateway while preserving centralized routing control.

---

## Virtual WAN Integration

A **Standard SKU Azure Virtual WAN (`nf-vwan`)** is deployed alongside the traditional VNet hub.

### Virtual Hub
- **Name:** `nf-vhub1`
- **Address Prefix:** 172.17.0.0/24
- **Purpose:** Central P2S VPN termination and scalable VNet integration

### VNet Connections
- VNets are connected into the vWAN fabric
- A static route for `0.0.0.0/0` is defined to control internet-bound traffic via a specified next hop

This demonstrates a **dual-hub architecture**, combining VNet-based and vWAN-based connectivity patterns.

---

## Hybrid and Remote Connectivity

### Site-to-Site (S2S) VPN
- **Azure VPN Gateway:** `nf-vpngw`
  - SKU: VpnGw2AZ
  - Public IP: 51.138.69.160
- **Local Network Gateway:** `nf-LGW`
  - Gateway IP: 37.228.233.24
  - Address Spaces: 192.168.0.0/24, 10.0.0.0/8
- **Protocol:** IPsec / IKEv2

---

### Point-to-Site (P2S) VPN
Remote access is provided via the Virtual Hub.

- **Protocol:** OpenVPN (active)
- **Client Address Pool:** 10.110.0.0/24
- **Authentication:** Certificate-based
- **Custom DNS Pushed to Clients:** 192.168.0.240

This ensures remote users can resolve **private Azure and on-premises DNS zones** consistently.

---

## DNS Resolution Architecture

### DNS Private Resolver
A **DNS Private Resolver (`nf-dns`)** is deployed in the hub VNet to centralize name resolution.

#### Inbound Endpoint
- **Subnet:** `dns-resolve`
- **IP Address:** 172.16.3.4
- **Purpose:** Receives DNS queries from:
  - On-premises networks
  - Peered VNets
  - P2S VPN clients

#### Outbound Endpoint
- **Subnet:** `dns-resolve-out`
- **Purpose:** Forwards DNS queries to external resolvers (e.g., on-prem DNS servers)

---

### Private DNS Zones
- **Zone:** `privatelink.file.core.windows.net`
- **Linked VNets:**
  - `nf-hub-vnet`
  - `nf-vnet-spoke1`
- **A Record:** `nfstore1` → 172.19.0.4

This enables seamless name resolution for the storage account’s private endpoint.

---

### DNS Forwarding Rules
Two forwarding rulesets are defined:
- `dns-rules`
- `on-prem-azure`

Both forward queries for:


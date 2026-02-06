# Azure Hybrid DNS Resolution Architecture
DNS Private Resolver and Private Endpoint Name Resolution

---

## Executive Summary
This document provides a comprehensive architectural analysis of an Azure cloud deployment implementing centralized hybrid DNS resolution using Azure DNS Private Resolver.

The architecture combines a traditional hub-and-spoke Virtual Network (VNet) design with Azure Virtual WAN (vWAN) to support both hybrid Site-to-Site (S2S) connectivity and scalable Point-to-Site (P2S) remote access. The primary objective of this deployment is to ensure consistent and secure name resolution for private endpoints across Azure VNets, Virtual WAN connected networks, and on-premises environments.

Key capabilities demonstrated include:
- Hybrid DNS resolution using inbound and outbound DNS Private Resolver endpoints
- Private Endpoint name resolution via Azure Private DNS Zones
- Coexistence of VNet peering and Virtual WAN connectivity models
- Secure remote administration using Azure Bastion
- Explicit identification and analysis of security misconfigurations

This environment is documented as an architectural evaluation and engineering learning platform, not as a hardened production design.

---

## Quick Navigation

- [DNS Resolution Architecture](#dns-resolution-architecture)
- [Security Configuration Analysis](#security-configuration-analysis)

---

## Core Network Topology

### Hub-and-Spoke VNet Architecture
Three primary VNets are deployed in the West Europe region.

| VNet | Address Space | Role |
|-----|---------------|------|
| nf-hub-vnet | 172.16.0.0/16 | Central hub hosting VPN Gateway and DNS Resolver |
| nf-vnet-spoke1 | 172.17.0.0/16 | Workload spoke hosting compute and Bastion |
| nf-endpoint-vnet | 172.19.0.0/16 | Services spoke hosting private endpoints |

### Hub VNet Subnets
- GatewaySubnet – 172.16.1.0/27 (VPN Gateway)
- dns-resolve-out – 172.16.2.0/24 (DNS Resolver outbound)
- dns-resolve – 172.16.3.0/24 (DNS Resolver inbound)

---

## VNet Peering Configuration
All VNets are interconnected using bidirectional VNet peering.

### Gateway Transit
- Hub to Spoke: allowGatewayTransit = true
- Spoke to Hub: useRemoteGateways = true

This enables spoke VNets to consume the hub VPN gateway while preserving centralized routing control.

---

## Virtual WAN Integration
A Standard SKU Azure Virtual WAN is deployed alongside the traditional VNet hub.

### Virtual Hub
- Name: nf-vhub1
- Address Prefix: 172.17.0.0/24
- Purpose: Central Point-to-Site VPN termination and scalable VNet integration

### VNet Connections
- VNets are connected into the vWAN fabric
- A static route for 0.0.0.0/0 is defined for controlled internet egress

This demonstrates a dual-hub architecture combining VNet-based and vWAN-based connectivity models.

---

## Hybrid and Remote Connectivity

### Site-to-Site VPN
- Azure VPN Gateway: nf-vpngw
  - SKU: VpnGw2AZ
  - Public IP: 51.138.69.160
- Local Network Gateway: nf-LGW
  - Gateway IP: 37.228.233.24
  - Address Spaces: 192.168.0.0/24, 10.0.0.0/8
- Protocol: IPsec / IKEv2

### Point-to-Site VPN
Remote access is provided via the Virtual Hub.

- Protocol: OpenVPN
- Client Address Pool: 10.110.0.0/24
- Authentication: Certificate based
- Custom DNS pushed to clients: 192.168.0.240

---

## DNS Resolution Architecture

### DNS Private Resolver
A DNS Private Resolver is deployed in the hub VNet to centralize name resolution.

#### Inbound Endpoint
- Subnet: dns-resolve
- IP Address: 172.16.3.4
- Receives DNS queries from:
  - On-premises networks
  - Peered VNets
  - Point-to-Site VPN clients

#### Outbound Endpoint
- Subnet: dns-resolve-out
- Forwards DNS queries to external resolvers such as on-premises DNS servers

### Private DNS Zones
- Zone: privatelink.file.core.windows.net
- Linked VNets:
  - nf-hub-vnet
  - nf-vnet-spoke1
- A Record:
  - nfstore1 to 172.19.0.4

This enables private endpoint name resolution across Azure and hybrid networks.

### DNS Forwarding Rules
Two forwarding rulesets are defined:
- dns-rules
- on-prem-azure

Both forward queries for:

privatelink.file.core.windows.net

to the DNS Private Resolver inbound endpoint at 172.16.3.4.

---

## Deployed Workloads and Services

### Compute
- VM: testvm
- Size: Standard_D2s_v3
- OS: Ubuntu 24.04 LTS
- Subnet: 172.17.0.0/24

### Storage
- Storage Account: nfstore1
- SKU: Standard_LRS (GPv2)
- Private Endpoint IP: 172.19.0.4
- DNS managed via Private DNS Zone

### Secure Management
- Azure Bastion: nf-vnet-spoke1-bastion
- SKU: Basic
- Public IP: 65.52.139.98

Enables secure RDP and SSH access without exposing VM ports.

---

## Security Configuration Analysis

### Network Security Group
- Name: nf-nsg
- Attached to: testvm network interface

### NSG Rules

| Rule | Direction | Priority | Source | Destination | Port | Action |
|-----|----------|----------|--------|-------------|------|--------|
| AllowAnyCustomAnyInbound | Inbound | 100 | Any | Any | Any | Allow |
| AllowAnyCustomAnyOutbound | Outbound | 100 | Any | Any | Any | Allow |

### Security Assessment
This configuration is critically insecure.

The NSG permits unrestricted inbound and outbound traffic, effectively disabling network-layer protection and violating the principle of least privilege. This posture is acceptable only for lab validation and must be remediated before any production deployment.

---

## Architectural Intent
This deployment validates:
- Hybrid DNS resolution using Azure DNS Private Resolver
- Private Endpoint name resolution across Azure and on-premises environments
- Coexistence of VNet hub and Virtual WAN architectures
- Secure administration without public VM exposure
- Identification and documentation of security risks

---

[Return to Migration and Cloud Modernisation](../README.md)  
[Return to Solutions Architecture](../../README.md)  
[Return to Root README](../../../README.md)

# Azure Deployment: Aruba ClearPass Policy Manager (CPPM)

## Executive Summary
This document describes an Azure-based deployment of **Aruba ClearPass Policy Manager (CPPM)** provisioned using an **Azure Resource Manager (ARM) template**. The environment is deployed in the **West Europe** region and forms part of a broader **Hybrid Zero Trust Architecture**.

The design establishes a **hub-style virtual network** supporting:
- Hybrid connectivity to on-premises networks via **Site-to-Site (S2S) VPN**
- Remote administrative access via **Point-to-Site (P2S) OpenVPN**
- Dynamic routing capability using **BGP**
- A centrally hosted **Aruba ClearPass** instance reachable via a public IP

This deployment is intentionally documented as an **engineering and architectural evaluation**, including identification of a **critical security misconfiguration** for analysis and remediation planning.

---

## Architecture Slide Deck

This deployment is supported by a detailed architecture slide deck providing visual context for the hybrid Azure and Aruba ClearPass integration.

📄 **Hybrid Azure & Aruba Architecture**  
➡️ **[Open Slide Deck (PDF)](../../../resources/slidedecks/Hybrid_Azure_Aruba_Architecture.pdf)**

### Slide Deck Coverage
- Hybrid hub-and-spoke topology
- VPN and BGP routing model
- Aruba ClearPass placement and traffic flows
- Identity, access, and enforcement alignment
- Architectural rationale and design intent

The slide deck complements this README and is suitable for architectural walkthroughs and stakeholder review.

---

## Infrastructure as Code (IaC)

This deployment is fully defined using an **Azure Resource Manager (ARM) template**, ensuring repeatability and transparency.

### ARM Template
- **Path:** `resources/templates/cppm_vm.json`
- **Purpose:** Deploys the complete Aruba ClearPass infrastructure stack, including:
  - Hub virtual network and subnets
  - Virtual Network Gateway (S2S + P2S)
  - Local Network Gateway
  - VPN connections and BGP configuration
  - Network Security Group (NSG)
  - Public IP resources
  - Aruba ClearPass Policy Manager virtual machine

> **Note:**  
> The template intentionally includes permissive NSG rules. This configuration is documented for architectural risk analysis and **must not be used in production**.

---

## 1. Deployment Overview

### Metadata
- **Region:** West Europe
- **Creator:** Nick Fennell
- **Deployment Phases:**
  - **Phase 1 (08/07/2025):** Core networking, VPN gateways, and S2S connectivity
  - **Phase 2 (08/12/2025):** Aruba CPPM virtual machine and dependencies

All resources are deployed from a single ARM template and logically grouped.

---

## 2. Aruba ClearPass Policy Manager (CPPM)

### Virtual Machine Specification

| Property | Value |
|------|------|
| Name | `nf-cppm` |
| VM Size | Standard_DS1_v2 |
| OS Type | Linux |
| Image Publisher | arubanetworks-4922182 |
| Image SKU | aruba_cppm_6_11_1 |
| Image Version | Latest |
| OS Disk | 40 GB – Standard_LRS |
| Admin User | `azureuser` |
| Authentication | Password-based login enabled |

The CPPM appliance is deployed from the official **Aruba marketplace image** and functions as the central Network Access Control (NAC) platform.

---

## 3. Network Architecture

### 3.1 Virtual Network (Hub)

- **Name:** `nf-hub`
- **Address Space:** `172.16.0.0/16`
- **Encryption:** Disabled
- **DDoS Protection:** Disabled

#### Subnets
| Subnet | Address Prefix | Purpose |
|------|---------------|--------|
| default | 172.16.0.0/24 | Hosts CPPM |
| GatewaySubnet | 172.16.1.0/27 | VPN Gateway |

---

### 3.2 CPPM Network Interface

- **NIC Name:** `nf-cppm789`
- **Private IP:** 172.16.0.4 (configured as Dynamic)
- **Public IP:** 20.16.112.218 (Static)
- **Network Security Group:** `nf-cppm`

The CPPM appliance is **internet-reachable**, which has direct security implications addressed later in this document.

---

## 4. Hybrid and Remote Connectivity

### 4.1 Site-to-Site (S2S) VPN

#### Azure Virtual Network Gateway
- **Name:** `ng-vpngw`
- **Type:** Route-based
- **SKU:** VpnGw1
- **Public IP:** 108.143.37.217

#### Local Network Gateway
- **Name:** `nf-lng`
- **Gateway IP:** 37.228.231.33
- **Advertised Address Spaces:**
  - 10.0.0.0/8
  - 192.168.0.0/24

#### VPN Connection
- **Name:** `s2s`
- **Protocol:** IPsec / IKEv2

---

### 4.2 BGP Configuration

| Component | ASN | Peering Address |
|--------|----|----------------|
| Azure VPN Gateway | 65515 | 172.16.1.30 |
| On-Prem Gateway | 65010 | 10.0.0.1 |

> **Observation:**  
> Although BGP is configured on both gateways, the VPN connection resource has `enableBgp = false`.  
> As a result, **BGP is defined but not active** across the tunnel.

---

### 4.3 Point-to-Site (P2S) VPN

- **Protocol:** OpenVPN
- **Client Address Pool:** 10.110.10.0/24
- **Authentication:** Certificate-based
- **Root Certificate:** Embedded directly within the ARM template

This enables secure remote administrative access without reliance on on-prem connectivity.

---

## 5. Security Posture Analysis

### 5.1 Network Security Group (NSG)

- **Name:** `nf-cppm`
- **Attached To:** CPPM NIC (`nf-cppm789`)

#### NSG Rules

| Rule | Direction | Priority | Source | Destination | Access |
|----|---------|----------|--------|-------------|--------|
| AllowAnyCustomAnyInbound | Inbound | 100 | Any | Any | Allow |
| AllowAnyCustomAnyOutbound | Outbound | 100 | Any | Any | Allow |

---

### 5.2 Security Risk Assessment

This NSG configuration is **critically insecure**.

By allowing unrestricted inbound and outbound traffic, the NSG provides **no effective network-layer protection**, exposing all services on the Aruba CPPM appliance directly to the internet.

#### Impact
- Eliminates Azure’s primary network firewall control
- Significantly increases attack surface
- Violates Zero Trust and least-privilege principles
- Acceptable **only for lab validation**, never production

This configuration is intentionally documented to demonstrate **architectural risk awareness** and the importance of layered security controls.

---

## Architectural Intent
This deployment demonstrates:
- Integration of Aruba ClearPass within Azure
- Hybrid and remote connectivity patterns
- BGP-capable VPN architecture
- Certificate-based P2S access
- Identification and analysis of insecure configurations

The environment serves as a **technical learning and architectural evaluation platform**, not a hardened production design.

---

[Return to Migration & Cloud Modernisation](../README.md)  
[Return to Solutions Architecture](../../README.md)  
[Return to Root README](../../../README.md)

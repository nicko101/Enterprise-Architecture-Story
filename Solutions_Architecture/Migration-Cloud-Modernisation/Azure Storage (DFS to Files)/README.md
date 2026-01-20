# Azure Point-to-Site (P2S) Remote Access VPN  
## Internal PKI–Backed Certificate Authentication

---

## Executive Summary

This project documents the deployment and analysis of an **Azure hybrid VPN architecture** that combines **Site-to-Site (S2S)** and **Point-to-Site (P2S)** connectivity within a single Azure Virtual Network.

The primary focus of this implementation is **secure remote access via Point-to-Site (P2S) VPN**, using **OpenVPN and internal PKI–issued certificates** for strong, identity-based authentication.

The environment was deployed using an **Azure Resource Manager (ARM) template** in the **West Europe** region and reflects a realistic enterprise hybrid networking scenario, including BGP-based routing, certificate trust chains, and controlled VM access.

---

## Project Objectives

- Provide **secure remote user access** to Azure resources via P2S VPN
- Use **certificate-based authentication** backed by an internal PKI
- Integrate P2S access alongside an existing **BGP-enabled S2S VPN**
- Validate end-to-end connectivity to Azure workloads
- Maintain a simple but production-aligned reference architecture

---

## Core Network Architecture

The deployment is centered around a **hub Virtual Network** that aggregates both gateway services and workloads.

### Virtual Network (VNet)

| Property | Value |
|-------|------|
| Name | `nf-hub` |
| Location | West Europe |
| Address Space | `172.16.0.0/16` |
| DDoS Protection | Disabled |
| Encryption Enforcement | Disabled (`AllowUnencrypted`) |

### Subnet Design

| Subnet | Address Prefix | Purpose |
|------|---------------|--------|
| `default` | `172.16.0.0/24` | Hosts VM workloads |
| `GatewaySubnet` | `172.16.1.0/27` | Dedicated VPN Gateway subnet |

---

## VPN Gateway Configuration

A **single Azure VPN Gateway** is used to terminate both **S2S and P2S connections**, reflecting a common enterprise deployment pattern.

### Azure Virtual Network Gateway

| Property | Value |
|------|------|
| Name | `ng-vpngw` |
| Gateway Type | VPN |
| VPN Type | Route-based |
| SKU | VpnGw1 |
| Generation | Generation1 |
| BGP | Enabled |

### Public IP

| Property | Value |
|------|------|
| Name | `nf-gwip` |
| SKU | Standard |
| Allocation | Static |
| IP Address | `52.166.77.218` |

---

## Point-to-Site (P2S) Remote Access VPN

The **P2S VPN** provides secure remote access for individual users and administrators.

### P2S Configuration

| Setting | Value |
|------|------|
| Client Address Pool | `10.1.1.0/24` |
| VPN Protocol | OpenVPN |
| Authentication Method | Certificate-based |
| Root Certificate | Internal PKI (embedded in ARM template) |

### Authentication Model (Internal PKI)

- A **trusted internal Root CA** is embedded in the VPN Gateway configuration
- Client certificates are issued from this PKI
- Mutual TLS authentication is enforced:
  - Azure validates the client certificate chain
  - No username/password authentication is used
- This approach aligns with **enterprise Zero Trust access models**

---

## Site-to-Site (S2S) Context (Supporting Role)

Although not the focus of this README, the environment also includes a **BGP-enabled Site-to-Site VPN**, enabling:

- Dynamic route exchange
- Hybrid on-prem ↔ Azure reachability
- Coexistence of S2S and P2S on a single gateway

### BGP Summary

| Parameter | Azure | On-Prem |
|------|------|------|
| ASN | 65515 | 65010 |
| Peering IP | `172.16.1.30` | `10.0.0.1` |

This ensures that **P2S-connected clients** can reach both Azure and on-prem networks if required.

---

## Virtual Machine Workload

A single Linux VM is deployed to validate connectivity and access.

### VM Specifications

| Property | Value |
|------|------|
| Name | `nf-vm1` |
| Size | Standard_DS1_v2 |
| OS | Ubuntu 24.04 LTS |
| Security Type | Trusted Launch (Secure Boot + vTPM) |
| Admin User | `azureuser` |

### Network Interface

- Private IP: Dynamic
- Accelerated Networking: Enabled
- Subnet: `default`

### Security Controls

- Network Security Group applied at NIC level
- Inbound SSH allowed (TCP/22)
- Password authentication enabled (explicitly noted as a **non-recommended** but intentional configuration for testing)

---

## Security Observations

- Certificate-based P2S authentication significantly reduces credential attack surface
- Internal PKI enables:
  - Certificate lifecycle control
  - Revocation capability
  - Alignment with enterprise trust models
- Password-based SSH access is **not recommended for production** and is documented intentionally as a deviation

---

## Governance & Metadata

All resources are consistently tagged for ownership and traceability.

| Tag | Value |
|----|------|
| Creator | Nick Fennell |
| DateCreated | July 2025 |
| Location | West Europe |

---

## Key Takeaways

- P2S VPN with internal PKI provides **strong, scalable remote access**
- OpenVPN simplifies cross-platform client support
- Coexistence of S2S + P2S on a single gateway is fully supported
- Certificate trust, not routing, is the most common P2S failure point
- This architecture scales naturally into Zero Trust access models

---

## Use Cases

- Secure administrator access to Azure workloads
- Hybrid lab and production validation
- PKI-backed remote access design reference
- Foundation for identity-aware VPN access


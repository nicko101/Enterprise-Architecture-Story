# Azure Network Infrastructure: Palo Alto NVA & S2S VPN (Reference Architecture)

![Architecture Diagram](Solutions_Architecture/PoC%20and%20Validation/Azure%20Network%20Infrastructure%20Briefing:%20Palo%20Alto%20Firewall%20Deployment/vpngw-nva-vpn.png)

## Executive Summary
This repository documents a **reference implementation** of a secure **Hub-and-Spoke** network architecture in **Microsoft Azure**, designed to centralize traffic inspection using a **Palo Alto Networks VM-Series NVA**.

The focus of this design is to demonstrate:
- Centralized security enforcement
- Controlled hybrid connectivity via Site-to-Site VPN
- Deterministic traffic steering using Azure routing constructs
- High-availability patterns for NVAs

> **Note:** Exact IP addressing, resource names, and environment-specific values are intentionally abstracted.

---

## 1. Architectural Overview

### 1.1 Hub-and-Spoke Topology
The deployment follows a classic hub-and-spoke model:

- **Hub VNet**
  - Hosts shared infrastructure and security services
  - Contains the Palo Alto NVA and Azure VPN Gateway
- **Spoke VNet(s)**
  - Host application workloads
  - All ingress, egress, and inter-VNet traffic is forced through the Hub

This model enables centralized policy enforcement and simplified security operations.

---

### 1.2 Network Segmentation (Conceptual)
The Hub network is logically segmented to separate traffic planes.

| Subnet (Conceptual) | Purpose |
|---------------------|---------|
| Management | Out-of-band firewall management |
| Trust | Internal traffic inspection and east-west flows |
| Untrust | Internet egress and ingress |
| VPN | Transit for Site-to-Site VPN traffic |
| GatewaySubnet | Azure VPN Gateway (reserved) |

---

## 2. Traffic Engineering & Security Enforcement

### 2.1 Centralized Inspection Pattern
All traffic flows—whether:
- Internet-bound
- On-premises-bound
- Inter-VNet

—are explicitly routed through the Palo Alto NVA.

Azure **User-Defined Routes (UDRs)** are used to prevent bypass and ensure deterministic forwarding.

---

### 2.2 Routing Strategy (Abstracted)

| Traffic Source | Destination | Routing Intent |
|----------------|------------|----------------|
| Spoke workloads | Internet | Forced via NVA |
| Spoke workloads | On-premises | Forced via NVA |
| VPN Gateway | Spoke prefixes | Forwarded to NVA |
| NVA | Internet | Egress via untrust |

Default routes (`0.0.0.0/0`) are applied only where supported by Azure platform constraints.

---

## 3. Hybrid Connectivity (Site-to-Site VPN)

Hybrid connectivity is provided using an **Azure VPN Gateway** and a route-based IPsec tunnel.

Key design principles:
- The VPN Gateway terminates IPsec
- On-premises traffic is forwarded to the NVA for inspection
- Return traffic is symmetrically routed back through the VPN Gateway

> Full on-premises forced tunneling (default route over S2S) requires BGP-based steering (e.g., Azure Route Server) and is outside the scope of this reference design.

---

## 4. Firewall Configuration (Conceptual)

The Palo Alto VM-Series firewall is configured with:

- **Zones**
  - Untrust (internet-facing)
  - Trust (internal workloads)
  - VPN (S2S tunnel termination)
- **Security Policies**
  - Explicit allow rules for inspected flows
- **NAT**
  - Source NAT for internet-bound traffic
- **Routing**
  - Static or dynamic routes toward Azure gateways and spokes

All policies follow **least-privilege** principles.

---

## 5. Validation & Observability

Traffic validation is performed using:
- Palo Alto traffic logs
- Session browser inspection
- Azure routing diagnostics

Successful validation confirms:
- On-prem traffic arrives via the VPN tunnel
- Traffic is inspected and NATed by the NVA
- Return traffic is symmetric and stateful

---

## 6. Scope & Intent

This repository is intended as:
- A **design reference**
- A **learning and validation artefact**
- A **portfolio demonstration** of Azure networking and security architecture

It is **not** intended to be a production deployment guide.

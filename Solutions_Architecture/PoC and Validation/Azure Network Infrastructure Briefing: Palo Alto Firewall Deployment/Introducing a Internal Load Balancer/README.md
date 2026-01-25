# Azure Network Architecture: Secure Hub-and-Spoke with Palo Alto NVA

## Executive Summary
This architecture defines a high-availability **Hub-and-Spoke** topology in **West Europe**. It centralizes security via a **Palo Alto Networks NVA**, providing a single point of inspection for all ingress, egress, and east-west traffic.

---

## I. Network Infrastructure
The deployment utilizes a Hub VNet for shared services and a Spoke VNet for application workloads, integrated via VNet Peering with Gateway Transit enabled.

### 1. Hub VNet (`fwvnet`) | 172.18.0.0/16
Dedicated subnets for Management, Public (Untrust), Private (Trust), and VPN traffic planes ensure strict isolation of administrative and data traffic.

### 2. Spoke VNet (`nf_vm_vnet`) | 172.20.0.0/16
Designed for workload scalability, currently hosting the `nf-vm1` Ubuntu instance.

---

## II. Security & Traffic Engineering
### Centralized Inspection (NVA)
A Palo Alto VM-Series firewall provides Deep Packet Inspection (DPI). The NVA is integrated with a **Standard Internal Load Balancer (ILB)** at `172.18.2.5` to provide a resilient next-hop for the network fabric.

### High-Level Design (HLD)
The architectural layout and component interaction are documented in:
`Solutions_Architecture/PoC and Validation/Azure Network Infrastructure Briefing: Palo Alto Firewall Deployment/Introducing a Internal Load Balancer/HLD.png`

[Image of an Azure hub-and-spoke virtual network architecture featuring a centralized firewall and VPN gateway]

### Routing Logic
Custom Route Tables (UDRs) are applied to the Spoke and Gateway subnets to enforce "Force Tunneling" through the security stack.

| Route Table | Target Subnet | Traffic Scope | Next Hop |
| :--- | :--- | :--- | :--- |
| **nf-rt-vnets** | Spoke VMs | Internet & On-Prem | 172.18.2.5 (ILB) |
| **nf-vpn-gw-rt** | GatewaySubnet | Inter-VNet & External | 172.18.2.5 (ILB) |

---

## III. Hybrid Connectivity
A Site-to-Site (S2S) VPN tunnel connects the environment to the on-premises footprint (`10.0.0.0/8`).
* **Gateway SKU:** `VpnGw1AZ` (Zone-Redundant).
* **Connection:** IKEv2 IPsec tunnel.

---

## IV. Availability & Metadata
* **Resilience:** Availability Sets for the NVA and Zone-Redundant SKUs for the VPN Gateway and Public IPs.
* **Creator:** Nick Fennell
* **Creation Date:** 01/25/2026

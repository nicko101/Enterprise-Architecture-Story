# Azure Deployment: Palo Alto Networks NGFW (VM-Series)
## Forced-Tunnel Security Architecture with Centralized Traffic Inspection

## Executive Summary
This document provides a detailed architectural and engineering analysis of an Azure Resource Manager (ARM) template used to deploy a **Palo Alto Networks VM-Series Next-Generation Firewall (NGFW)** in Microsoft Azure.

The deployment implements a **forced-tunnel security architecture**, where all outbound traffic originating from protected workloads is explicitly routed through the Palo Alto NGFW for inspection and policy enforcement. The firewall operates as the **centralized security control point**, inspecting both inbound and outbound traffic.

A segmented Virtual Network (VNet) is deployed in the **West Europe** region with dedicated subnets for management, untrusted (public) traffic, and trusted (private) workloads. A custom route table applied to the Private subnet enforces forced tunneling by directing all internet-bound traffic (0.0.0.0/0) to the firewallÔÇÖs trust interface.

Inbound internet traffic is handled via a **Standard Azure Load Balancer**, which forwards traffic to the firewallÔÇÖs untrusted interface using inbound NAT rules. Azure Network Security Groups (NSGs) are intentionally permissive, delegating all traffic filtering responsibility to the Palo Alto NGFW to avoid policy conflicts and ensure consistent Layer 7 enforcement.

This deployment is documented as an **engineering and architectural evaluation**, highlighting valid enterprise design patternsÔÇösuch as forced tunneling and centralized inspectionÔÇöalongside explicit risks, including the lack of high availability and permissive management access.
---

## Quick Navigation
- [Network Architecture](#network-architecture)
- [Traffic Routing and Inspection](#traffic-routing-and-inspection)
- [Security Configuration Analysis](#security-configuration-analysis)
- [Architectural Observations](#architectural-observations)

---

## Core Infrastructure Overview

All resources are deployed to the **westeurope** Azure region and are tagged for ownership and traceability.

- **Creator:** Nick Fennell  
- **Creation Date:** 07/15/2025 (template metadata)  
- **Deployment Tag:** panNGFWijqemnqqaaona  

The ARM template provisions:
- A Palo Alto VM-Series NGFW
- A Linux workload virtual machine
- A segmented Virtual Network
- Route tables enforcing forced tunneling
- Network Security Groups
- Standard Azure Load Balancer
- Standard SKU Public IP Prefix and addresses

---

## Compute Resources

### Palo Alto NGFW Virtual Machine (nfazpalo)

This virtual machine is the **primary security appliance**, deployed from the official Palo Alto Networks marketplace image and configured with three network interfaces.

| Property | Value |
|------|------|
| Image | paloaltonetworks / vmseries-flex |
| Plan / SKU | bundle1 |
| Version | 11.2.5 |
| VM Size | Standard_D8_v4 |
| OS Type | Linux |
| Availability | Zone 1 |
| OS Disk | 60 GB, Standard_LRS |
| Admin User | azureuser |

#### Network Interfaces
- **eth0 (Management):** Mgmt subnet
- **eth1 (Untrust):** Public subnet
- **eth2 (Trust):** Private subnet  
IP forwarding is enabled on eth1 and eth2 to allow inter-subnet routing.

---

### Workload Virtual Machine (nf-vm1)

A Linux virtual machine deployed in the Private subnet to represent a protected workload.

| Property | Value |
|------|------|
| Image | Canonical Ubuntu 24.04 LTS |
| VM Size | Standard_DS1_v2 |
| OS Disk | ReadWrite, delete on VM delete |
| Admin User | azureuser |

---

## Network Architecture

### Virtual Network (fwVNET)

A single Virtual Network is deployed with an address space of **172.16.0.0/16**, segmented into three subnets.

| Subnet | Address Prefix | Role |
|------|---------------|-----|
| Mgmt | 172.16.0.0/24 | Firewall management |
| Public | 172.16.1.0/24 | Untrusted / internet-facing traffic |
| Private | 172.16.2.0/24 | Protected workloads |

---

## Traffic Routing and Inspection

### Forced Tunneling (Private Subnet)

A route table (`nf-rt`) is associated **only** with the Private subnet.

This route table implementation represents a classic Azure forced-tunneling design, ensuring that no workload in the Private subnet can bypass firewall inspection for outbound connectivity.

| Destination | Next Hop Type | Next Hop |
|------------|--------------|----------|
| 0.0.0.0/0 | VirtualAppliance | 172.16.2.4 |

This enforces **forced tunneling**, ensuring:
- All outbound traffic from workloads is sent to the Palo Alto NGFW
- No direct internet access is possible from the Private subnet
- The firewall becomes the mandatory inspection point

---

### Azure Load Balancer (nf-elb)

A **Standard SKU regional Load Balancer** manages inbound internet traffic.

#### Frontend Configuration
- Uses public IPs allocated from a /30 Public IP Prefix
- Multiple frontend IPs support scalability and future expansion

#### Backend Pool
- Contains the NGFW public interface (eth1)

#### Inbound NAT Rules

| Rule Name | Protocol | Frontend Port | Backend Port |
|---------|----------|---------------|--------------|
| nginx-8080 | TCP | 80 | 8080 |
| vm1-8080-in | TCP | 8080 | 80 |
| vm1-ssh | TCP | 22 | 1022 |

#### Outbound Rule
- Explicit outbound rule allowing all protocols
- SNAT performed using a frontend public IP

---

## Public IP Configuration

A **Standard SKU Public IP Prefix (/30)** is deployed across availability zones.

| Resource | IP Address | Purpose |
|-------|-----------|---------|
| nfazpalo mgmt IP | 172.211.144.251 | Firewall management |
| nfpip1 | 20.126.161.68 | Load Balancer frontend |
| nfpip2 | 20.126.161.69 | Load Balancer frontend |
| nfpip3 | 20.126.161.70 | Load Balancer frontend |

Additional public IPs are defined but unused in this specific template version.

---

## Security Configuration Analysis

### Network Security Groups

#### Allow-All NSG
Applied to:
- Public subnet
- Private subnet

Rules:
- Allow all inbound traffic
- Allow all outbound traffic

This design intentionally delegates **all traffic filtering responsibility** to the Palo Alto NGFW.

---

#### DefaultNSG (Management Subnet)

Applied to:
- Mgmt subnet

Inbound rules:
1. **DenyAnyCustomAnyInbound (Priority 100)** ÔÇô *Action: Allow*
2. Allow-Intra (Priority 101)
3. Default-Deny (Priority 200)

### Security Assessment
This configuration is **highly permissive** and presents multiple risks:

- The management subnet effectively allows unrestricted inbound access
- The NSG rule naming does not reflect actual behavior
- Azure-native network controls are bypassed in favor of firewall-only enforcement

This posture is acceptable **only for lab or evaluation environments**.

---

## Architectural Observations

### Centralized Security Model
All security enforcement relies on the Palo Alto NGFW. This simplifies policy management but increases the impact of firewall misconfiguration or failure.

### Forced Tunneling
The route table implementation ensures strict inspection control and prevents egress bypass.

### Lack of High Availability
- Single NGFW instance
- Single availability zone
- No failover or redundancy  
This creates a **single point of failure**.

### Permissive Management Access
The management subnet NSG contains a high-priority allow-all rule, exposing the firewall management plane to unnecessary risk.

### IP Forwarding
Enabled correctly on firewall interfaces to allow traffic forwarding between subnets.

---

## Architectural Intent
This deployment demonstrates:
- Forced-tunnel NGFW design in Azure
- Centralized Layer 7 security enforcement
- Azure Load Balancer integration with VM-Series firewalls
- The trade-offs between simplicity and resiliency
- Identification of security risks through architectural review

---

[Return to Migration & Cloud Modernisation](../README.md)  
[Return to Solutions Architecture](../../README.md)  
[Return to Root README](../../../README.md)


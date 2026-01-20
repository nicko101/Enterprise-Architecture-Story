# Azure Hybrid Networking & Private Service Projection  
**Tenant-to-Tenant Private Link – Business-Critical Incident Resolution**

---

## Overview

This repository documents a **business-critical tenant-to-tenant Azure Private Link incident**
impacting a high-value customer, where **every minute of downtime resulted in direct financial
and operational impact**.

The supplier initially attributed the issue to the consumer environment
(routing, DNS, firewalls).  
To break escalation deadlock — including live calls with Microsoft —
a **production-accurate reference lab** was deployed to isolate fault domains.

This lab conclusively demonstrated:

1. **DNS was the initial blocker**
2. After DNS was resolved, the **final blocker was the supplier’s Private Link Service /
   Load Balancer configuration**

---

## High-Level Architecture

![Azure Hybrid Networking & Private Service Projection](resources/slides/pls/pls.png)

---

## Dual-Environment Topology

![Dual Environment Topology](resources/slides/pls/dual.png)

### Consumer Hub (North Europe)
- Hybrid connectivity entry point
- Enterprise DNS integration
- Private Endpoint consumer

### Provider Service (West Europe)
- Isolated application VNet
- Load-balanced backend service
- Private Link Service projection

---

## Consumer Hub – Network Foundation

![Consumer Hub Network Foundation](resources/slides/pls/consumer-hub.png)

The consumer hub (`vnet-hub`) provides:
- Dedicated `GatewaySubnet`
- VM workload subnet
- Dedicated Private Endpoint subnet
- Custom DNS aligned with on-prem infrastructure

---

## Hybrid Connectivity Bridge

![Hybrid Connectivity Bridge](resources/slides/pls/bridge.png)

An IPsec IKEv2 VPN extends the on-prem environment into Azure, enabling:
- Real client traffic paths
- Enterprise DNS forwarding chains
- Accurate Private Endpoint validation

---

## Provider Environment – Service Isolation

![Provider Environment](resources/slides/pls/workload.png)

The provider tenant hosts an isolated workload VNet with no address overlap
or peering with the consumer environment.

---

## Traffic Distribution & Load Balancing

![Traffic Distribution & Load Balancing](resources/slides/pls/loadbalancer.png)

A **Standard Azure Load Balancer** fronts the provider workload and is referenced
by the Private Link Service.

At this stage, connectivity was still failing — proving the issue was **not**
routing, VPN, or firewall related.

---

## Projecting the Service – Private Link Service

![Private Link Service Projection](resources/slides/pls/project-pls.png)

The Private Link Service:
- Wraps the Load Balancer frontend
- Abstracts provider IP space
- Restricts visibility to the consumer subscription

This component was later confirmed to be misconfigured in the supplier tenant.

---

## Private Endpoint Integration (Consumer Side)

![Private Endpoint Integration](resources/slides/pls/private-int.png)

The consumer Private Endpoint:
- Terminates in the consumer VNet
- Presents a local RFC1918 IP
- Traverses the Microsoft backbone only

---

## Root Cause Analysis

### Initial Blocker — DNS

![Private DNS Resolution](resources/slides/pls/private-dns.png)

Findings:
- Private DNS zones existed but were not correctly linked
- Enterprise DNS forwarders could not resolve `privatelink.*`
- Resolution was not traversing the Microsoft backbone

Resolution:
- Manually created required Microsoft Private DNS zones
- Corrected on-prem → Azure → Private DNS forwarding paths

---

### Final Blocker — Private Link Service / Load Balancer

With DNS resolved, traffic still failed.

This conclusively proved:
- Consumer routing was correct

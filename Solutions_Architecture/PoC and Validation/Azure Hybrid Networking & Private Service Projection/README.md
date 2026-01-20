# Azure Hybrid Networking & Private Service Projection  
**Tenant-to-Tenant Private Link – Business-Critical Incident Resolution**

---

## Overview

This repository documents a **business-critical tenant-to-tenant Azure Private Link incident** impacting a high-value customer, where **every minute of downtime resulted in direct financial and operational cost**.

The supplier initially attributed the issue to the consumer environment (routing, DNS, firewalls).  
To break escalation deadlock — including multiple Microsoft support calls — a **production-accurate reference lab** was deployed to isolate the fault domains.

This lab conclusively demonstrated:

1. **DNS was the initial blocker**
2. After DNS was resolved, the **final blocker was the supplier’s Private Link Service / Load Balancer configuration**

---

## High-Level Architecture

![Azure Hybrid Networking & Private Service Projection](resources/slides/pls/pls.png)

This architecture demonstrates secure, private service consumption across **tenants and regions**, using Azure Private Link over the Microsoft backbone — with no public internet exposure.

---

## Dual-Environment Topology

![Dual Environment Topology](resources/slides/pls/dual.png)

The design is intentionally split to allow clean fault isolation.

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
- VM subnet for consumer workloads
- Dedicated Private Endpoint subnet
- Custom DNS servers aligned with on-premises resolution

This ensured the lab matched **real production DNS behavior**, not Azure-only defaults.

---

## Hybrid Connectivity Bridge

![Hybrid Connectivity Bridge](resources/slides/pls/bridge.png)

The North Europe hub extends the on-premises network into Azure via an **IPsec IKEv2 VPN**, enabling:

- Real client traffic paths
- Enterprise DNS forwarding chains
- Accurate validation of Private Endpoint access

This removed all ambiguity around routing and firewall responsibility.

---

## Secure Management & Consumer Workloads

![Secure Management & Consumer Workloads](resources/slides/pls/workload.png)

Consumer workloads are accessed securely using Azure Bastion, avoiding direct exposure of management ports while maintaining operational access during troubleshooting.

---

## Traffic Distribution & Load Balancing

![Traffic Distribution & Load Balancing](resources/slides/pls/loadbalancer.png)

A **Standard Azure Load Balancer** fronts the provider workload and is referenced by the Private Link Service.

At this stage, **connectivity was still failing**, even though:
- Routing was correct
- Firewalls were not blocking traffic

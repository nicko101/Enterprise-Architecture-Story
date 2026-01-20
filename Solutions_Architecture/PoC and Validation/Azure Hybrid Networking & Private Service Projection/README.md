# Azure Hybrid Networking & Private Service Projection  
**Tenant-to-Tenant Private Link – Business-Critical Incident Resolution**

---

## Overview

This repository documents a **business-critical tenant-to-tenant Azure Private Link incident** impacting a high-value customer, where **every minute of downtime resulted in direct financial and operational cost**.

The supplier initially attributed the issue to the consumer environment (routing, DNS, firewalls).  
To break escalation deadlock — including live calls with Microsoft — a **production-accurate reference lab** was deployed to isolate the fault domains.

This lab conclusively demonstrated:

1. **DNS was the initial blocker**
2. After DNS was resolved, the **final blocker was the supplier’s Private Link Service / Load Balancer configuration**

---

## High-Level Architecture

![Azure Hybrid Networking & Private Service Projection](slides/pls/pls.png)

---

## Dual-Environment Topology

![Dual Environment Topology](slides/pls/dual.png)

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

![Consumer Hub Network Foundation](slides/pls/consumer-hub.png)

The consumer hub (`vnet-hub`) includes:

- Dedicated `GatewaySubnet`
- VM subnet for consumer workloads
- Dedicated Private Endpoint subnet
- Custom DNS servers aligned with on-prem resolution

This ensured the lab matched **real production DNS behavior**.

---

## Hybrid Connectivity Bridge

![Hybrid Connectivity Bridge

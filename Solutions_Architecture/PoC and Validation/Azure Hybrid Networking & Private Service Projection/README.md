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
- **DNS was the initial blocker**
- After DNS was resolved, the **final blocker was the supplier’s Private Link Service / Load Balancer configuration**

---

## High-Level Architecture

![Azure Hybrid Networking & Private Service Projection](resources/slides/pls/pls.png)

---

## Dual-Environment Topology

![Dual Environment Topology](resources/slides/pls/dual.png)

---

## Consumer Hub – Network Foundation

![Consumer Hub Network Foundation](resources/slides/pls/consumer-hub.png)

---

## Hybrid Connectivity Bridge

![Hybrid Connectivity Bridge](resources/slides/pls/bridge.png)

---

## Provider Environment – Service Isolation

![Provider Environment](resources/slides/pls/workload.png)

---

## Traffic Distribution & Load Balancing

![Traffic Distribution & Load Balancing](resources/slides/pls/loadbalancer.png)

---

## Projecting the Service – Private Link Service

![Private Link Service Projection](resources/slides/pls/project-pls.png)

---

## Private Endpoint Integration (Consumer Side)

![Private Endpoint Integration](resources/slides/pls/private-int.png)

---

## Root Cause Analysis

### Initial Blocker — DNS

![Private DNS Resolution](resources/slides/pls/private-dns.png)

Private DNS zones existed but were not correctly linked to enterprise DNS forwarders,
preventing resolution of `privatelink.*` records over the Microsoft backbone.

---

### Final Blocker — Private Link Service / Load Balancer

Once DNS was resolved, traffic still failed — conclusively proving the issue was isolated
to the supplier’s **Private Link Service / Load Balancer configuration**.

---

## Resource Bill of Materials

![Resource Bill of Materials](resources/slides/pls/resources.png)

---

## Architectural Summary

![Architectural Summary](resources/slides/pls/cross-summary.png)

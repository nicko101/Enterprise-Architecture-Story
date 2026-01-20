# Azure Hybrid Networking & Private Service Projection  
**Tenant-to-Tenant Private Link – Incident Resolution**

---

## Overview

This repository documents a **business-critical tenant-to-tenant Azure Private Link incident** affecting a high-value customer, where service downtime resulted in **direct financial and operational impact**.

The supplier initially attributed the failure to the consumer network (routing, firewalls, Palo Alto).  
To break escalation deadlock and prove correctness, a **production-accurate reference lab** was deployed.

The lab conclusively demonstrated:

1. **DNS was the first blocking layer**
2. Once DNS was fixed, the **final blocker was the supplier’s Private Link Service / Load Balancer configuration**

---

## High-Level Architecture

![Azure Hybrid Networking & Private Service Projection](resources/slides/pls/pls.png)

---

## Dual-Environment Topology

![Dual Environment Topology](resources/slides/pls/dual.png)

### Consumer (North Europe)
- Hybrid hub network
- Enterprise/on-prem DNS integration
- Private Endpoint consumer

### Provider (West Europe)
- Isolated VNet
- Load-balanced backend service
- Private Link Service projection

---

## Consumer Hub – Network Foundation

![Consumer Hub Network Foundation](resources/slides/pls/consumer-hub.png)

---

## Hybrid Connectivity (On-Prem → Azure)

![Hybrid Connectivity Bridge](resources/slides/pls/bridge.png)

---

## Provider Environment – Service Isolation

![Provider Environment](resources/slides/pls/workload.png)

---

## Traffic Distribution & Load Balancing

![Traffic Distribution & Load Balancing](resources/slides/pls/loadbalancer.png)

---

## Service Projection – Private Link Service

![Private Link Service Projection](resources/slides/pls/project-pls.png)

---

## Private Endpoint Integration (Consumer Side)

![Private Endpoint Integration](resources/slides/pls/private-int.png)

---

## Root Cause Analysis

### Initial Blocker — DNS

![Private DNS Resolution](resources/slides/pls/private-dns.png)

**Issue**
- Private DNS zones existed but were not correctly connected
- Enterprise DNS forwarders could not resolve `privatelink.*` zones
- Name resolution did not traverse the Microsoft backbone

**Resolution**
- Manually created required Microsoft Private DNS zones
- Corrected on-prem → Azure → Private DNS forwarding paths

---

### Final Blocker — Private Link Service / Load Balancer

Once DNS was resolved, traffic still failed.

This proved:
- Consumer routing was correct
- Firewalls were not blocking traffic
- Private Endpoint connectivity was functional

The fault was isolated to the **supplier’s Private Link Service / Load Balancer configuration**, which was corrected.

---

## Resource Bill of Materials

![Resource Bill of Materials](resources/slides/pls/resources.png)

---

## Architectural Summary

![Architectural Summary](resources/slides/pls/cross-summary.png)

---

## Key Outcomes

- Consumer network fully exonerated
- DNS identified as the **initial blocker**
- Provider PLS / Load Balancer identified as the **final root cause**
- Supplier corrected configuration
- Business-critical service restored

---

## Why This Matters

This lab demonstrates:

- Tenant-to-tenant Private Link at production scale
- Correct DNS split-horizon design
- Microsoft backbone service consumption
- Structured fault isolation under pressure

This design replicated the **exact production failure mode**.

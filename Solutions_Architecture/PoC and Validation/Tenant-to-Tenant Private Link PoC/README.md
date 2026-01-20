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

![Azure Hybrid Networking & Private Service Projection](./resources/slides/pls/pls.png)

---

## Dual-Environment Topology

![Dual Environment Topology](./resources/slides/pls/dual.png)

---

## Consumer Hub – Netw

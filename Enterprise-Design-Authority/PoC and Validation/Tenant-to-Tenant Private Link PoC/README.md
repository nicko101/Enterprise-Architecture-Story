# Azure Private Link Hybrid Blueprint  
**Tenant-to-Tenant Private Link — Business-Critical Incident Resolution**

---

## Overview

This project documents the resolution of a **business-critical Azure Private Link connectivity incident** impacting a high-value customer, where every minute of downtime resulted in direct financial and operational loss.

Initial escalation focused on the **consumer environment** (routing, VPN, DNS).  
To break the deadlock — including live calls with Microsoft — a **production-accurate reference lab** was built to isolate fault domains and prove root cause.

The lab conclusively demonstrated:

- DNS was the **initial blocking issue**
- After DNS resolution was fixed, the **final blocker** was a **misconfigured Azure Load Balancer rule**
- The issue was **not** VPN, routing, firewall, or Microsoft backbone related

---

## Architecture Blueprint (High-Level)

This diagram establishes the **end-to-end hybrid Private Link model** used throughout the investigation.

![Azure Hybrid Networking & Private Link Architecture](../../../resources/slides/pls/pls.png)

**Key characteristics:**
- On-premises users connect via IPsec VPN
- North Europe acts as the consumer hub
- West Europe hosts the provider service
- All service access is enforced via Azure Private Link

---

## Cross-Region Private Link Connectivity

This view focuses on **how traffic flows across regions and tenants**.

![Cross-Region Private Link](../../../resources/slides/pls/cross-summary.png)

**What this proves:**
- Traffic traverses the **Microsoft backbone only**
- No VNet peering exists between tenants
- The Private Endpoint terminates in the consumer hub
- The Private Link Service projects the provider workload

At this stage, connectivity was **still failing**, confirming the issue was **not network transport**.

---

## DNS Resolution Failure — The First Root Cause

### The DNS Puzzle (On-Premises)

The true initial failure was **name resolution**, not connectivity.

![On-Premises DNS Resolution](../../../resources/slides/pls/on-premdns.png)

**Problem:**
- On-premises DNS servers **cannot resolve Azure Private DNS zones**
- `privatelink.*` records were never returned
- Clients could not reach the Private Endpoint IP

---

## DNS Resolution Strategy (Manual & Conditional)

This diagram represents the **final, working DNS architecture**.

![Custom DNS Zone](../../../resources/slides/pls/customdnszone.png)

**Solution implemented in the lab:**
- Manual forward lookup zones created on-prem
- Private Link FQDNs mapped directly to Private Endpoint IPs
- Split-horizon DNS enforced for hybrid resolution

Once DNS was fixed:
- Name resolution succeeded
- Traffic reached the Private Endpoint
- A second, deeper issue surfaced — confirming DNS was only the **first blocker**

---

## Final Root Cause — Load Balancer Configuration

With DNS resolved, traffic successfully traversed:

- On-premises → VPN
- Hub VNet → Private Endpoint
- Microsoft backbone → Provider tenant

The remaining failure was isolated to the **provider-side Load Balancer**:

- Backend SQL service was hosted on a VM behind a Standard Load Balancer
- A **NAT rule** was configured instead of a **Load Balancing rule**
- This prevented correct traffic distribution behind the Private Link Service

Once corrected, end-to-end Private Link connectivity succeeded.

---

## Key Takeaways

- Private Link failures are frequently **DNS-first problems**
- Connectivity does not equal reachability
- Load Balancer rule types are **critical** behind Private Link Service
- Building a faithful lab can resolve incidents faster than vendor escalation

---

## Why This Project Matters

This PoC demonstrates:

- Real-world Azure Private Link troubleshooting
- Hybrid DNS design and split-horizon resolution
- Tenant-to-tenant fault isolation under production pressure
- Engineering-led incident resolution backed by evidence

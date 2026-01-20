# Azure Hybrid Networking & Private Service Projection  
## Tenant-to-Tenant Private Link — Business-Critical Incident Resolution

---

## Overview

This project documents a **business-critical tenant-to-tenant Azure Private Link incident**
impacting a high-value customer, where every minute of downtime resulted in direct
financial and operational loss.

The service provider initially attributed the outage to the consumer environment
(routing, DNS, or firewalls).  
To break escalation deadlock — including live troubleshooting calls with Microsoft —
a **production-accurate reference lab** was deployed to isolate fault domains.

This lab conclusively demonstrated:

- DNS was the **initial blocker**
- After DNS resolution was corrected, traffic still failed
- The **final root cause** was a misconfigured **Private Link Service / Load Balancer**
  in the provider tenant

---

## Complete Architecture

![Complete Architecture](resources/slides/pls/complete-architecture.png)

This diagram represents the **end-to-end traffic path**:

- On-premises clients
- Hybrid IPsec VPN to Azure Hub (North Europe)
- Private Endpoint termination
- Microsoft backbone traversal
- Provider Private Link Service
- Standard Load Balancer
- Backend workload (SQL / VM)

---

## Cross-Region Private Link Connectivity

![Cross-Region Connection](resources/slides/pls/cross-region.png)

Key characteristics:

- No VNet peering
- No address overlap exposure
- Traffic remains entirely on the **Microsoft backbone**
- Consumer only sees a **local RFC1918 IP**

---

## Publishing the Provider Service

![Publishing the Service](resources/slides/pls/publishing-service.png)

The provider environment exposes the workload via:

- Standard Azure Load Balancer
- Private Link Service
- Subscription-level visibility controls

At this stage, connectivity still failed — proving the issue was **not** routing,
VPN, or firewall related.

---

## DNS Resolution — Azure Side

![Azure Private DNS](resources/slides/pls/private-dns.png)

Azure Private DNS zones were correctly created for the Private Endpoint namespace.
However, **Azure Private DNS is not authoritative outside Azure**.

This led to the initial outage condition.

---

## DNS Resolution — On-Premises (Split-Horizon)

![On-Prem DNS](resources/slides/pls/on-premdns.png)

### The Challenge
On-premises DNS servers **cannot resolve Azure Private DNS zones**.

### The Solution
A split-horizon approach was implemented using:

- Manual forward lookup zones  
- Conditional forwarding (where applicable)  
- Static A-record mapping  

The service FQDN was explicitly mapped to the Private Endpoint IP.

---

## Custom DNS Zone (On-Prem Validation)

![Custom DNS Zone](resources/slides/pls/customdnszone.png)

This confirmed:

- Correct name resolution to the Private Endpoint IP
- Successful DNS traversal across the hybrid boundary
- DNS was **no longer the blocking factor**

---

## Root Cause Analysis

### Initial Blocker — DNS
- Private DNS zones existed but were not reachable by enterprise DNS
- Name resolution failed for `privatelink.*` records

### Final Blocker — Private Link Service / Load Balancer
- After DNS resolution was corrected, traffic still failed
- The issue was isolated to the **provider tenant**
- Misconfiguration within the Private Link Service / Load Balancer prevented service access

---

## Key Takeaways

- Private DNS Zones are **optional**, not mandatory
- Private Endpoints function as long as FQDN → IP resolution is correct
- DNS resolution success does **not** guarantee Private Link Service correctness
- A consumer-side lab is an effective way to prove provider-side faults

---

## Why This PoC Matters

This project demonstrates **real-world Azure troubleshooting**:

- Multi-tenant isolation
- Hybrid DNS realities
- Escalation-grade fault isolation
- Evidence-based root cause analysis

This is not a theoretical design — it is a **validated incident resolution model**.

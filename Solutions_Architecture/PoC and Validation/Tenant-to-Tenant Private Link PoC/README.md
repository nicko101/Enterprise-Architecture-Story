# Azure Hybrid Networking & Private Service Projection  
## Tenant-to-Tenant Private Link — Incident Root Cause Analysis

---

## Overview

This project documents a **business-critical tenant-to-tenant Azure Private Link incident** impacting a production workload, where prolonged downtime resulted in direct operational and financial impact.

The supplier initially attributed the issue to the consumer environment (DNS, routing, firewall).  
To break escalation deadlock — including live troubleshooting with Microsoft — a **production-accurate reference lab** was deployed to isolate fault domains.

The lab conclusively demonstrated:

- **DNS was the initial blocker**
- Once DNS was resolved, the **final root cause was a misconfigured Azure Load Balancer rule** in the provider environment

---

## Complete End-to-End Architecture

![Complete Architecture](../../../resources/slides/pls/cross-summary.png)

This diagram represents the **validated, working end-to-end design**, including:

- On-premises clients and DNS
- Hybrid VPN connectivity
- Consumer hub VNet
- Private Endpoint termination
- Provider-side service exposure via Private Link
- Backend SQL service hosted on a VM

---

## Hybrid Connectivity Foundation

![Hybrid Bridge](../../../resources/slides/pls/bridge.png)

An **IPsec IKEv2 Site-to-Site VPN** extends the on-premises environment into Azure, enabling:

- Real client traffic paths
- Enterprise DNS resolution
- Accurate Private Endpoint testing under production conditions

---

## Consumer Hub — North Europe

![Consumer Hub](../../../resources/slides/pls/consumer-hub.png)

The consumer hub VNet provides:

- Dedicated `GatewaySubnet`
- VM workload subnet
- Dedicated Private Endpoint subnet
- Custom DNS configuration aligned with on-premises resolvers

---

## DNS Resolution Strategy (Manual & Conditional)

### Azure-Side DNS

![Azure Private DNS](../../../resources/slides/pls/private-dns.png)

Azure Private DNS is used to resolve `privatelink.*` records **within the hub VNet** via Private DNS Zone linking.

> **Important:**  
> Azure Private DNS is **recommended but not strictly required**.  
> A manual DNS record pointing directly to the Private Endpoint IP is technically valid and was used during troubleshooting.

---

### On-Premises DNS (Split-Horizon)

![On-Prem DNS](../../../resources/slides/pls/on-premdns.png)

On-premises DNS servers cannot natively resolve Azure Private DNS zones.

Resolution was achieved using:

- Manual forward lookup zones or conditional forwarders
- Explicit A-record mapping of the PaaS FQDN to the Private Endpoint IP

This misalignment was the **initial confirmed blocker** in the incident.

---

## Load Balancer Misconfiguration — Root Cause

![Load Balancer](../../../resources/slides/pls/loadbalancer.png)

After DNS resolution was corrected, connectivity still failed.

Root cause analysis confirmed:

- The backend VM hosts the **SQL service**
- Traffic successfully reached the Private Link Service
- The **Azure Load Balancer was configured with a NAT rule instead of a Load Balancing rule**
- This prevented correct backend service distribution

Correcting the Load Balancer rule immediately restored **end-to-end connectivity**.

---

## Key Lessons Learned

- Private Link incidents are frequently **DNS-related first**
- A functional Private Endpoint does **not guarantee backend reachability**
- Load Balancer rule selection (NAT vs LB) is a **critical dependency**
- Production-accurate lab replication is an effective escalation-breaking technique

---

## Outcome

This project demonstrates:

- Structured fault-domain isolation
- Hybrid DNS troubleshooting under enterprise constraints
- Deep understanding of Azure Private Link and Load Balancer behavior
- Real-world incident resolution methodology suitable for production environments

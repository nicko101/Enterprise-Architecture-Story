# PoC: Migrating to Azure Internal Load Balancer (ILB) for NVA High Availability

## Overview
This Proof of Concept (PoC) documents the architectural transition from pointing User Defined Routes (UDRs) directly at a Palo Alto NVA interface to utilizing an **Azure Internal Load Balancer (ILB)** as the next hop. 

The primary goal of this design is to enable a "Load Balancer Sandwich," allowing for seamless failover between firewall nodes and providing a single, resilient Virtual IP (VIP) for internal traffic.

---

## Architectural Evolution

### Phase 1: Direct NVA Routing (Initial Test)
* **Logic**: Spoke VNet UDR pointed directly to the Private/Trust NIC of the Palo Alto (`172.18.2.4`).
* **Outcome**: Successful connectivity, but no high availability. If the NVA is rebooted, the path is severed.

### Phase 2: Internal Load Balancer Integration (Current PoC)
* **Logic**: The UDR is updated to point to the ILB Frontend IP (`172.18.2.5`). 
* **Mechanism**: The ILB uses **HA Ports** to forward all TCP/UDP traffic to the backend NVA pool.
* **Benefit**: Decouples the routing from the specific VM instance, allowing for NVA maintenance without network downtime.



---

## Technical Challenges & Validations

### 1. Resolving the "None" Health State
During initial deployment, the ILB may show a Health Probe status of **None** or **Unknown**. This state prevents any traffic from being forwarded to the NVA.

* **Discovery**: Analysis of flow logs indicated the NVA was receiving the probe but the response was failing.
* **Root Cause**: Asymmetric routing of the Azure "Wire IP" (`168.63.129.16`). The NVA was attempting to respond to the probe via the Untrust/Egress interface.
* **The Fix**: Implemented a `/32` static route for `168.63.129.16` pointing back to the Trust Subnet Gateway.

### 2. High Availability Ports (HA Ports)
To support the Palo Alto's role as a transit gateway, the ILB must handle traffic across all ports.
* **Validation**: The ARM template explicitly defines a load balancing rule with `protocol: "All"`, `frontendPort: 0`, and `backendPort: 0`.
* **Verification**: Testing with `curl -I http://www.google.com` confirmed that traffic is successfully balanced and logged by the NVA once the health probe transitioned to **Healthy**.



---

## Deployment Artifacts

* **ARM Template**: [template.json](./template.json) - Contains the full Hub-and-Spoke definition, including the ILB, NVA, and Route Tables.
* **Health Probe Configuration**: 
    * **Protocol**: TCP
    * **Port**: 443
    * **NVA Requirement**: An Interface Management Profile must be applied to the Trust interface to permit the Azure Wire IP.

---

## Key Integration Pillar: The Unified Security Fabric
This PoC is a prerequisite for the **ClearPass Intune Extension** and **NDES SCEP** migrations. By securing the internal routing path via an ILB, we ensure that critical identity and certificate traffic remains resilient during firewall updates or failover events.

[Return to Solutions Architecture README](../../../README.md)

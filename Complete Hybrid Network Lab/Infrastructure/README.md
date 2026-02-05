# Infrastructure Architecture – Virtualization and Compute Fabric

## Purpose
This infrastructure provides the **execution and validation platform** for the architectural PoCs documented throughout this repository. It is intentionally designed to support hybrid networking, identity, and security validation across on-premises and Azure environments.

The lab is not presented as a standalone project, but as an **evidence platform** used to validate real-world architecture decisions under controlled conditions.

---

## Infrastructure Overview
The environment combines a Proxmox VE virtualization cluster with a GNS3-based network simulation fabric. This approach enables realistic multi-vendor network scenarios while maintaining tight integration with physical hardware and Azure connectivity.

**Primary goals:**
- Validate hybrid routing and inspection models
- Simulate enterprise network topologies
- Test identity-driven access and Zero Trust controls
- Observe failure, routing symmetry, and inspection behaviour

---

## Core Infrastructure Components

### 1. Virtualization Fabric (Proxmox VE)
The lab is hosted on a Proxmox VE cluster providing resilient compute for security and identity services.

- **Hypervisor Management:** KVM virtual machines and Linux containers orchestrated centrally
- **Software-Defined Networking:** Linux Bridges and Open vSwitch (OVS) for flexible traffic steering
- **Storage:** ZFS-backed virtual disks to support snapshotting and rollback during PoC testing

This layer represents the physical and virtual foundation of the lab.

---

### 2. Network Simulation Fabric (GNS3)
GNS3 is integrated directly into the virtualization layer to simulate complex enterprise network topologies.

- **Multi-Vendor Simulation:** Cisco IOSv, ArubaOS-CX, and Palo Alto PAN-OS appliances
- **Hybrid Bridging:** GNS3 Cloud nodes used to bridge simulated networks into the Proxmox fabric and onward to Azure via VPN
- **Topology Validation:** Enables controlled testing of routing, segmentation, and inspection paths

This allows architectural assumptions to be tested without simplifying real-world behaviour.

---

### 3. Compute and Platform Services
The lab hosts core services required to validate identity-centric and Zero Trust designs.

- **Identity Services:** Active Directory Domain Services (AD DS)
- **Policy Enforcement:** Aruba ClearPass Policy Manager
- **Administrative Access:** Hardened Linux jump hosts for controlled management access

These services mirror production patterns without unnecessary duplication.

---

## Operational Standards

### High Availability
The environment is designed for resilience during testing:
- Snapshot-based rollback for rapid recovery
- Controlled failure testing of routing and inspection paths

### Security and Management Isolation
- Management interfaces are isolated on a dedicated, non-routable VLAN
- Administrative access is authenticated and logged via the central security plane

---

## Validation Usage
This infrastructure is used to validate:
- Hybrid routing behaviour (Gateway vs NVA)
- VPN termination and inspection symmetry
- Identity-driven access enforcement
- Failure scenarios and HA constraints

Functional success is demonstrated when traffic flows correctly from simulated clients, through the security and routing stack, and into Azure VNets in alignment with documented PoCs.

---

## Relationship to Architecture Documentation
This infrastructure underpins and validates designs documented in:
- **Azure Hybrid Network Architecture – Palo Alto NVA**
- **Private Endpoint DNS Architecture**
- **Tenant-to-Tenant Private Link PoC**
- **Identity and Access PoCs**

The lab exists solely to support and validate these architectural outcomes.

---

⬅ Return to root README

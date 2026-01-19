# Solutions Architecture: Hybrid Zero Trust Environment

## Purpose
This section documents the architectural design and composition of the hybrid Zero Trust environment. It focuses on how identity, trust, networking, and security components are assembled into a coherent system, and why specific architectural decisions were made.

**Project Status:** Active engineering build. Architectural designs and content are updated as new modules are validated and integrated.

---

## High-Level Architecture Context

[![Solutions Architecture Overview](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/summary.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/summary.png)
*Figure 1: High-level solution architecture detailing the relationship between identity providers, enforcement points, and hybrid cloud resources.*

This view establishes architectural context. Logical, physical, and trust-flow diagrams are provided within the relevant sub-sections below to demonstrate operational detail.

---

## Architecture Domains

### Identity and Trust Plane
Identity is treated as the primary control plane for access decisions.
* **Active Directory**: Provides authoritative identity for on-premises services.
* **Microsoft Entra ID**: Acts as the cloud identity and policy enforcement layer.
* **Multi-tier PKI**: Enables device and service trust through automated certificate lifecycle management via SCEP/NDES.

### Access and Enforcement Plane
Access enforcement is context-aware rather than network-location-based.
* **Identity Validation**: Network access is granted only after successful identity and posture validation.
* **Aruba ClearPass**: Enforces Network Access Control (NAC) decisions using RADIUS and 802.1X.
* **Microsoft Intune**: Provides real-time device compliance signals to the enforcement layer.

### Connectivity and Routing Plane
Hybrid connectivity is designed to mimic an enterprise WAN architecture.
* **Secure Tunneling**: Site-to-site IPsec connectivity utilizing IKEv2 and AES-256 encryption.
* **Routing Strategy**: Implementation of predictable path selection between local Edge infrastructure and Azure Virtual Network Gateways.
* **Traffic Segmentation**: Clear separation between management, control plane, and data plane traffic.

### Security and Segmentation Plane
Security enforcement is centralized and policy-driven.
* **Layer 7 Inspection**: Palo Alto NGFW provides App-ID based policy enforcement and threat prevention.
* **SSL Decryption**: Applied selectively to inspect encrypted traffic based on trust and risk profiles.
* **Zone-Based Defense**: Explicit segmentation between user, server, DMZ, and cloud zones.

---

## Design Principles
* **Identity First**: Network location does not imply trust.
* **Least Privilege**: Access is explicitly granted based on role, not implied by connection.
* **Separation of Concerns**: Identity, access, routing, and enforcement operate as distinct logical layers.
* **Operational Realism**: Designs reflect enterprise constraints, security requirements, and failure modes.

---

## Solutions Navigation

| Section | Focus |
| :--- | :--- |
| [Integration-Security-and-Operations](./Integration-Security-and-Operations/) | Multi-vendor orchestration and operational visibility. |
| [Migration-Cloud-Modernisation](./Migration-Cloud-Modernisation/) | Strategy for transitioning legacy workloads to hybrid cloud. |
| [PoC and Validation](./PoC%20and%20Validation/) | Engineering tests, routing verification, and protocol analysis. |

---
[Return to Root README](../README.md)

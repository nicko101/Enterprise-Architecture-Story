# Security Engineering and Identity Governance

## Overview
This directory documents the implementation of the Zero Trust security fabric. The architecture moves beyond traditional perimeter defense by treating identity as the primary control plane. Every connection request—whether from inside or outside the physical network—is explicitly authenticated and authorized before access is granted.

[![Architecture Summary](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/summary.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/summary.png)
*Figure 1: High-level solution architecture detailing the relationship between identity providers, enforcement points, and hybrid cloud resources.*

---

## Operational Workflow
Authentication requests follow a specific sequence to ensure zero-trust compliance. This process validates device health via the cloud before granting local network access.

[![Zero Trust Workflow](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/images/Zero-Trust-Workflow.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/images/Zero-Trust-Workflow.png)
*Figure 2: Technical workflow detailing the handshake between the network access control layer (Aruba ClearPass) and the Microsoft Graph API for compliance verification.*

---

## Core Security Pillars

### 1. Identity Governance (Hybrid Entra ID)
* **Hybrid Synchronization**: Integration of on-premises Active Directory with Microsoft Entra ID.
* **Conditional Access**: Enforcement of modern authentication and MFA for high-risk resources.
* **Role-Based Access Control (RBAC)**: Implementation of least privilege across local and cloud management planes.

### 2. Trust and Certificate Lifecycle (Multi-Tier PKI)
* **Root and Intermediate CA**: Deployment of a secure, offline-capable Root CA and an online Intermediate CA.
* **Automated Enrollment**: Utilization of SCEP/NDES to automate the delivery of device certificates to managed endpoints via Microsoft Intune.

### 3. Network Access Control (Aruba ClearPass)
* **Context-Aware Enforcement**: ClearPass orchestrates access decisions based on user identity and real-time compliance status.
* **802.1X Orchestration**: Implementation of EAP-TLS to ensure only hardware with valid certificates can join the fabric.

### 4. Edge Security (Palo Alto NGFW)
* **App-ID Policy Enforcement**: Transition from port-based rules to application-aware security policies.
* **SSL Forward Proxy**: Selective decryption of outbound traffic to protect against encrypted threats and data exfiltration.

---

## Technical Validation
The security posture is continuously validated through:
* **Real-time Telemetry**: Monitoring of authentication logs and policy hits within the ClearPass Access Tracker.
* **Threat Observability**: Application-layer visibility and threat logging on the Palo Alto Edge.
* **Device Health Check**: Verification that only "Compliant" devices are permitted on the internal network.

---
[Return to Root README](../../README.md)

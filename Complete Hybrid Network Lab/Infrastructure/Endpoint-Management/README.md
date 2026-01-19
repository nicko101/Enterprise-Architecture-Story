# Infrastructure Layer — Hybrid Identity & Trust Services

## Overview
This section documents the **core infrastructure services** that underpin the Hybrid Zero Trust environment.  
These components provide identity authority, certificate-based trust, endpoint enrollment, and legacy integration required to support secure access across on-premises and Azure-hosted resources.

This layer is intentionally designed as **foundational infrastructure**, separating identity, trust, and governance concerns from network transport and enforcement.

**Project Status:** Active engineering build. Infrastructure components are incrementally validated and integrated into higher-layer security and access workflows.

---

## Infrastructure Domains

The infrastructure layer is composed of the following tightly scoped service domains.

### Active Directory (Hybrid Trust Services)
Active Directory remains the authoritative identity source for legacy workloads and certificate issuance workflows.

Responsibilities include:
- User and computer identity authority
- Service account management
- Certificate enrollment dependencies
- Hybrid integration with Microsoft Entra ID

📂 **Location:**  
`Infrastructure/Active Directory/`

➡️ **Details:**  
[Active Directory README](./Active%20Directory/README.md)

---

### Certificate Services (PKI & SCEP)
Certificate Services provide **device and service trust** across the environment using a multi-tier Public Key Infrastructure.

Architecture:
- Standalone Root Certificate Authority
- Online Intermediate CA
- NDES/SCEP for automated certificate delivery

This enables:
- EAP-TLS for wired and wireless access
- Machine authentication
- Secure service-to-service communication

📂 **Location:**  
`Infrastructure/Certificate-Services/`

➡️ **Details:**  
- [PKI Architecture](./Certificate-Services/PKI/README.md)  
- [SCEP / NDES Integration](./Certificate-Services/SCEP/README.md)

---

### Endpoint Management (Microsoft Intune)
Microsoft Intune provides the **cloud-based endpoint management plane** for all managed devices.

Key capabilities:
- Azure AD / Entra ID join and enrollment
- Certificate delivery via SCEP
- Compliance enforcement
- Device posture signaling to network enforcement systems

Intune acts as a **real-time compliance authority** rather than a passive MDM platform.

📂 **Location:**  
`Infrastructure/Endpoint-Management/`

➡️ **Details:**  
[Endpoint Management README](./Endpoint-Management/README.md)

---

### Identity Plane (Microsoft Entra)
Microsoft Entra ID serves as the **cloud identity control plane** for the hybrid environment.

Functions include:
- Identity synchronization from Active Directory
- Conditional Access policy enforcement
- Application identity and access governance
- Device registration and trust evaluation

📂 **Location:**  
`Infrastructure/Entra/`

➡️ **Details:**  
[Microsoft Entra README](./Entra/README.md)

---

### Identity Services (RBAC & Governance)
This domain documents how identity is **consumed and enforced** across the environment.

Includes:
- Role-Based Access Control (RBAC)
- Delegated administration models
- Evidence artifacts for validation and auditing

📂 **Location:**  
`Infrastructure/Identity-Services/`

➡️ **Details:**  
[Identity Services README](./Identity-Services/README.md)

---

## Design Principles

The infrastructure layer adheres to the following principles:

- **Identity First**  
  Authentication and authorization precede network access.

- **Certificate-Based Trust**  
  Device and service identity relies on cryptographic trust, not passwords.

- **Separation of Concerns**  
  Identity, certificate services, enforcement, and routing are independently managed.

- **Hybrid by Design**  
  Cloud-native services are integrated without breaking legacy dependencies.

- **Operational Realism**  
  Designs reflect real enterprise constraints, failure modes, and recovery paths.

---

## Role in the Overall Architecture

This infrastructure layer directly supports:

- Network Access Control (ClearPass)
- Zero Trust access enforcement
- Secure hybrid routing and segmentation
- Cloud and on-prem workload protection

No access decision is made without a dependency on **one or more components in this layer**.

---

## Navigation

- ↑ Return to **Hybrid Network Lab Root**  
  ../README.md

- ⌂ Return to **Repository Home**  
  ../../README.md

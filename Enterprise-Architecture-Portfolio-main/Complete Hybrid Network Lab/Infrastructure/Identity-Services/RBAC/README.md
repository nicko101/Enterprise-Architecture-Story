
# 🔑 Identity Services & RBAC Governance

## 📖 Overview
This directory documents the foundational identity services and **Role-Based Access Control (RBAC)** frameworks used to secure the hybrid ecosystem. By centralizing identity as the modern security perimeter, the lab ensures that every access request is verified based on identity, device posture, and granular permissions.

## 📂 Core Governance Components

### 🛡️ Service Principal Governance (App Registrations)
The lab utilizes specific App Registrations in Entra ID to grant on-premises security tools secure, machine-to-machine (M2M) access to the Microsoft Graph API.
* **CPPM-Native-Lookup**: Enables Aruba ClearPass to query real-time user and device metadata from Entra ID for access decisions.
* **ndes-proxy**: Bridges the on-premises NDES/SCEP server to Microsoft Intune for automated certificate deployment.
* **Least Privilege**: Each application is restricted to the minimum necessary Graph API scopes (e.g., `User.Read.All`), ensuring a zero-trust footprint.

### 🤝 Hybrid Identity Handshake
The environment maintains a unified identity plane by synchronizing the local Active Directory forest with Entra ID.
* **Identity Correlation**: Utilizes Entra Connect to map on-premises SIDs to cloud identities, allowing for consistent policy enforcement across the hybrid fabric.
* **SSO Readiness**: Prepares the infrastructure for certificate-based Single Sign-On (SSO) for services like the Palo Alto GlobalProtect VPN.

### 🏗️ On-Premises Administrative Tiering
Documentation of the identity segmentation model used to prevent lateral movement within the lab core.
* **OU Governance**: Administrative and service accounts are isolated into dedicated Organizational Units (OUs) to apply targeted Group Policy Objects (GPOs).
* **Identity Context (User-ID)**: User identity is shared with the Palo Alto NGFW via the User-ID agent to allow for granular, role-based firewall policies.

## 🖼️ Operational Evidence
![Entra App Registrations](./evidence/entra-app-registrations.png)
*Figure 1: Inventory of modern identity integrations via Entra App Registrations.*

![PKI Health Status](./evidence/pki-health-view.png)
*Figure 2: Validation of the Enterprise PKI health, the root of trust for all lab identities.*

---

## 🛠️ Design Principles
1. **Least Privilege (M2M)**: Service principals are granted only the specific API permissions required for their specific function to minimize the security blast radius.
2. **Identity-Driven Access**: Network access decisions are based on the user's role and device health, rather than static credentials.
3. **Audit Readiness**: All identity-based events are logged across the hybrid fabric for centralized monitoring and forensic analysis.

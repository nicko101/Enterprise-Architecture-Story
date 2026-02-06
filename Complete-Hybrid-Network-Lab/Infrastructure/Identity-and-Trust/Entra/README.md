# 🆔 Entra ID & Identity Governance

## 📖 Overview
This directory documents the cloud identity foundation of the hybrid lab. Entra ID serves as the authoritative identity provider (IdP), enabling Secure Single Sign-On (SSO) and providing the API interface for critical cross-platform security integrations.

## 📂 Core Governance Components

### 🚀 App Registrations
The lab utilizes specific App Registrations to grant the on-premises security stack secure access to the Microsoft Graph API and other cloud resources.

**Current Active Registrations:**
* **CPPM-Native-Lookup**: Allows Aruba ClearPass to query Entra ID for real-time user attributes and device compliance status.
* **Palo Alto Networks - GlobalProtect**: Facilitates SAML-based SSO for the GlobalProtect VPN, ensuring modern authentication is enforced before granting remote access.
* **ndes-proxy**: Supports the modernization of PKI by bridging on-premises SCEP requests to the cloud management plane.
* **ConnectSyncProvisioning**: The identity anchor for Entra Connect, ensuring seamless synchronization between the local Active Directory and the cloud.

### 🛡️ Enterprise Applications & SSO
Documentation of the service principals used to manage user consent and conditional access.
* **Centralized Auth**: All lab administrative interfaces are protected by Entra ID SSO where possible to ensure a unified identity perimeter.
* **Security & Secrets**: Active monitoring of Client Secrets and Certificates ensures zero-day rotation and credential hygiene.

## 🖼️ Operational Evidence
![Entra App Registrations](./Entra.png)
*Figure 1: Production inventory of App Registrations supporting hybrid security and synchronization services.*

---

## 🛠️ Configuration Standards
All Entra ID objects follow these Zero-Trust principles:
1. **Least Privilege**: App permissions are restricted to the minimum necessary Graph API scopes required for functionality.
2. **Identity Correlation**: Cloud identities are mathematically mapped to on-premises SIDs via Entra Connect for consistent policy enforcement across the hybrid fabric.
3. **MFA Enforcement**: Every application in this inventory is subject to Conditional Access policies requiring modern, certificate-based authentication.

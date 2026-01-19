# Infrastructure Layer — Hybrid Identity & Trust Services

## Overview
This section documents the core infrastructure services that underpin the Hybrid Zero Trust environment.
These components provide identity authority, certificate-based trust, endpoint enrollment, and legacy integration required to support secure access across on-premises and Azure-hosted resources.

This layer is intentionally designed as foundational infrastructure, separating identity, trust, and governance concerns from network transport and enforcement.

---

## Infrastructure Domains

### Active Directory (Hybrid Trust Services)
Active Directory remains the authoritative identity source for legacy workloads and certificate issuance workflows.

➡️ Details:  
[Active Directory README](./Active%20Directory/README.md)

---

### Certificate Services (PKI and SCEP)
Provides device and service trust using a multi-tier Public Key Infrastructure.

➡️ Details:  
- [PKI Architecture](./Certificate-Services/PKI/README.md)  
- [SCEP / NDES Integration](./Certificate-Services/SCEP/README.md)

---

### Endpoint Management (Microsoft Intune)
Provides cloud-based endpoint enrollment, compliance enforcement, and device governance.

➡️ Details:  
[Endpoint Management README](./Endpoint-Management/README.md)

---

### Identity Plane (Microsoft Entra)
Provides cloud identity, conditional access, and device registration.

➡️ Details:  
[Microsoft Entra README](./Entra/README.md)

---

### Identity Services (RBAC and Governance)
Documents how identity is consumed and enforced across the environment.

➡️ Details:  
[Identity Services README](./Identity-Services/README.md)

---

## Navigation

- Return to Hybrid Network Lab Root  
  ../README.md

- Return to Repository Home  
  ../../README.md

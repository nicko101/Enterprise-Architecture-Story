# Infrastructure Layer — Hybrid Identity and Trust Services

## Overview
This section documents the core infrastructure services that underpin the Hybrid Zero Trust environment. These components provide identity authority, certificate-based trust, endpoint enrollment, and legacy integration required to support secure access across on-premises and Azure-hosted resources.

This layer is designed as foundational infrastructure, separating identity, trust, and governance from network transport and enforcement.

---

## Infrastructure Domains

### Active Directory (Hybrid Trust Services)
Active Directory provides the authoritative identity source for legacy workloads and certificate issuance workflows.

Location:  
[Active Directory](./Active%20Directory/)

---

### Certificate Services (PKI and SCEP)
Certificate Services provide device and service trust using a multi-tier Public Key Infrastructure.

Locations:  
[PKI](./Certificate-Services/PKI/)  
[SCEP and NDES](./Certificate-Services/SCEP/)

---

### Endpoint Management (Microsoft Intune)
Microsoft Intune provides the cloud-based endpoint management plane for device enrollment, compliance enforcement, and configuration governance.

Location:  
[Endpoint Management](./Endpoint-Management/)

---

### Identity Plane (Microsoft Entra)
Microsoft Entra ID provides cloud identity, device registration, and policy scope for enrollment and access control.

Location:  
[Entra](./Entra/)

---

### Identity Services (RBAC and Governance)
Documents how identity is consumed and enforced across the environment, including delegated administration and governance controls.

Location:  
[Identity Services](./Identity-Services/)

---

## Navigation

Return to Hybrid Network Lab Root:  
../README.md

Return to Repository Home:  
../../README.md

# Endpoint Management — Microsoft Intune Integration

## Overview
This section documents the endpoint management and compliance control plane for the hybrid Zero Trust lab environment. Microsoft Intune is used as the authoritative cloud-based management platform for both cloud-only and hybrid-joined endpoints.

Endpoint management is designed around a **compliance-first access model**, where device health and identity directly influence authentication, authorization, and network access decisions across the environment.

This layer integrates tightly with identity services, certificate infrastructure, and network enforcement components to ensure consistent, policy-driven endpoint governance.

---

## Management Scope

Endpoint Management in this environment covers:

- Cloud-managed Windows endpoints (Microsoft Entra ID joined)
- Hybrid identity-aware devices (Active Directory + Entra ID)
- Certificate-based device identity for network access
- Continuous compliance evaluation and enforcement

All managed endpoints participate in the Zero Trust control flow and are treated as untrusted until validated.

---

## Core Capabilities

### Device Enrollment
Devices are onboarded using standardized enrollment methods:

- Microsoft Entra ID Join for cloud-native devices
- Hybrid scenarios supported where required
- Enrollment establishes device identity and management authority

---

### Certificate-Based Device Identity
Endpoint identity is established using certificates rather than passwords.

- Certificates are issued via SCEP from the on-premises NDES infrastructure
- Intune delivers and manages certificate profiles
- Certificates are used for EAP-TLS authentication and device trust validation

This ensures strong, non-replayable device identity across wired, wireless, and VPN access.

---

### Compliance and Posture Evaluation
Intune continuously evaluates endpoint health and configuration state.

Typical compliance signals include:

- Disk encryption status (BitLocker)
- Operating system version and patch level
- Antivirus and security feature state
- Device integrity and enrollment status

Compliance state is treated as dynamic and authoritative.

---

### Network Access Enforcement Integration
Endpoint compliance is integrated with network enforcement platforms.

- Compliance state is consumed by Aruba ClearPass
- Non-compliant devices are denied or restricted at the network edge
- Enforcement decisions are made in real time based on device posture

This removes static trust assumptions and enforces continuous verification.

---

### Application Deployment and Configuration
Endpoints are standardized through centralized policy and application delivery.

- Core applications and security agents are deployed automatically
- Configuration profiles enforce baseline security settings
- PowerShell scripts are used for configuration consistency where required

---

## Architectural Role

Endpoint Management acts as the **device trust authority** within the architecture:

- Identity confirms who the user is
- Certificates confirm what the device is
- Compliance confirms whether the device is allowed access

This layer ensures that access decisions are not based solely on credentials or network location.

---

## Related Architecture Components

### Certificate Services
Provides device identity and certificate lifecycle management.

- PKI (Root and Intermediate CAs):  
  ../Certificate-Services/PKI/README.md

- SCEP / NDES:  
  ../Certificate-Services/SCEP/README.md

---

### Identity Plane (Microsoft Entra)
Provides user and device identity, enrollment authority, and policy scope.

../Entra/README.md

---

### Active Directory (Hybrid Trust Services)
Supports certificate issuance and legacy identity integration where required.

../Active Directory/README.md

---

## Summary
Microsoft Intune provides the foundation for endpoint trust, compliance enforcement, and configuration governance in this hybrid Zero Trust environment.

By integrating certificate-based identity, continuous compliance evaluation, and real-time network enforcement, this layer ensures that endpoint access is earned, validated, and continuously re-evaluated rather than implicitly trusted.

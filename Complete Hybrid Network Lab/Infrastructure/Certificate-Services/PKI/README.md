# PKI Engineering: Trust and Identity Fabric

## Overview
This section documents the Public Key Infrastructure (PKI) designed to provide the root of trust for the entire hybrid environment. By utilizing a multi-tier hierarchy, the system ensures that every device, user, and service can be explicitly verified via cryptographic certificates.

---

## Technical Architecture

[![PKI Hierarchy Overview](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/pki.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/pki.png)
*Figure 1: Two-tier PKI hierarchy featuring an Offline Root CA and an Online Issuing CA.*

### 1. Multi-Tier Hierarchy
* **Offline Root CA**: Acts as the ultimate anchor of trust. It remains powered off and disconnected from the network to protect the private key, only being utilized to sign the Intermediate CA's certificate.
* **Issuing (Intermediate) CA**: An online server responsible for the day-to-day issuance and revocation of certificates for users, computers, and network appliances.

---

## Certificate Policy and Governance

To automate the delivery of trust, specific certificate templates are engineered to support modern authentication protocols like 802.1X (EAP-TLS) and SCEP/NDES for mobile device management.

[![Certificate Templates](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/PKItemplate.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/PKItemplate.png)
*Figure 2: Engineering view of the Active Directory Certificate Services (AD CS) templates used for automated device enrollment.*

### Key Implementation Details
* **SCEP / NDES Integration**: Implementation of the Network Device Enrollment Service (NDES) to allow Microsoft Intune to securely request certificates on behalf of managed endpoints.
* **CRL Distribution Points (CDP)**: High-availability web-based CRL points to ensure real-time revocation checking is available to all security enforcement points (Palo Alto, Aruba ClearPass).
* **Auto-Enrollment**: Group Policy-driven enrollment for domain-joined Windows assets to ensure zero-touch identity delivery.

---

## Operational Validation
Successful PKI deployment is confirmed by:
* **Trust Propagation**: Successful installation of the Root CA certificate in the Trusted Root store across all hybrid assets.
* **Handshake Verification**: Validation of EAP-TLS authentication sessions within the Aruba ClearPass Access Tracker.
* **Certificate Lifecycle**: Monitoring of expiration and successful automated renewal of service-level certificates.

---
[Return to Security Overview](../README.md) | [Return to Root](../../../README.md)

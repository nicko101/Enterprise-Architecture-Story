# PKI and SCEP Engineering: Trust and Identity Fabric

## Overview
This section documents the Public Key Infrastructure (PKI) designed to provide the root of trust for the entire hybrid environment. By utilizing a multi-tier hierarchy, the system ensures that every device, user, and service can be explicitly verified via cryptographic certificates.

---

## Technical Architecture

[![PKI Hierarchy Overview](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/pki.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/pki.png)
*Figure 1: Two-tier PKI hierarchy featuring an Offline Root CA and an Online Issuing CA.*

### Multi-Tier Hierarchy
* **Offline Root CA**: Acts as the ultimate anchor of trust. It remains protected to ensure private key integrity.
* **Issuing (Intermediate) CA**: An online server responsible for day-to-day issuance and revocation for users, computers, and appliances.

---

## SCEP (Simple Certificate Enrollment Protocol)

### Purpose
This directory documents the automated certificate enrollment workflow used to provision device certificates for Intune-managed endpoints. SCEP enables zero-touch device identity and is a key enabler of Zero Trust network access.

### Architectural Role
Within this lab, SCEP provides:
* **Automated machine certificate enrollment** via Microsoft Intune.
* **Secure authentication** of devices using EAP-TLS.
* **Certificate lifecycle management** without manual intervention.



[![SCEP Enrollment Flow](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/ndes-scep.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/ndes-scep.png)
*Figure 2: Engineering workflow of SCEP enrollment via NDES and Microsoft Intune.*

### Enrollment Flow
1. **Device Enrollment**: Device enrolls into Microsoft Intune.
2. **Certificate Request**: Intune requests a certificate via NDES.
3. **Validation**: NDES validates the request against Active Directory.
4. **Issuance**: Certificate is issued by the Issuing CA.
5. **Authentication**: Device uses the certificate for network authentication.

### Integration Points
* **Microsoft Intune**: Policy-driven certificate deployment.
* **NDES**: Enrollment proxy and authentication bridge.
* **Active Directory Certificate Services**: Certificate issuance.
* **ClearPass**: Device authentication and posture validation.

---

## Certificate Policy and Governance

[![Certificate Templates](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/PKItemplate.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/PKItemplate.png)
*Figure 3: Active Directory Certificate Services (AD CS) templates used for automated device enrollment.*

### Scope and Design Notes
* Focused on device (machine) certificates.
* Certificate renewal handled automatically.
* Authentication relies on existing AD trust.

### Status
* SCEP enrollment operational.
* Device certificates successfully issued.
* Validated via EAP-TLS authentication.

---
[Return to Security Overview](../README.md) | [Return to Root](../../../README.md)

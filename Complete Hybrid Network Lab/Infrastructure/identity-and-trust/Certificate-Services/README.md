# Certificate Services: Automated Trust and PKI

## Overview
This directory documents the implementation of the Private Key Infrastructure (PKI) and the automated certificate lifecycle management. This service is the cryptographic foundation for the lab, enabling secure 802.1X authentication, SSL decryption, and identity-based access control.

---

## PKI Hierarchy and Implementation

### Two-Tier Microsoft CA
The environment utilizes a standardized two-tier architecture to balance security with operational flexibility:
* Offline Root Authority: A standalone Root CA used exclusively to issue the certificate for the subordinate authority, ensuring the integrity of the trust anchor.
* Enterprise Issuing Authority: An online subordinate CA that handles real-time certificate requests, CRL distribution, and template management.

### SCEP and NDES Integration
To support Zero Trust for mobile and non-domain joined devices, the environment utilizes automated enrollment protocols:
* Network Device Enrollment Service (NDES): Acts as the proxy between Microsoft Intune and the Issuing CA.
* Simple Certificate Enrollment Protocol (SCEP): Facilitates the automated delivery of unique device certificates to managed endpoints without manual intervention.

---

## Operational Evidence

[![SCEP and NDES Automated Enrollment](../../../resources/slides/SCEP.png)](../../../resources/slides/SCEP.png)
*Figure 1: Technical workflow of the SCEP enrollment process via NDES and Microsoft Intune.*

---

## Engineering Standards

### Certificate Templates
Security is enforced by utilizing specific templates for different use cases:
* Authentication: Unique templates for EAP-TLS and EAP-TEAP, ensuring that only devices with a valid certificate from the internal authority can access the network fabric.
* SSL Decryption: High-assurance subordinate CA certificates used by the Palo Alto NGFW for real-time traffic inspection.

### Security and Maintenance
* Key Protection: Implementation of strong cryptographic providers and key lengths (RSA 2048-bit or higher).
* CRL Availability: High-availability configuration for Certificate Revocation List (CRL) distribution points to ensure authentication flows are not interrupted.

---
[Return to Infrastructure](../) | [Return to Lab Root](../../README.md)

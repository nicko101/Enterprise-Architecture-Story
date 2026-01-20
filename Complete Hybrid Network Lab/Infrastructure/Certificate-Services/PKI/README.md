# PKI Engineering: Trust and Identity Fabric

## Overview
This section documents the Public Key Infrastructure (PKI) designed to provide the root of trust for the entire hybrid environment. 

---

## Technical Architecture

![PKI Hierarchy](../../../resources/images/Cloud%20PKI%20Intune%20Deployment-modern-pki.png)

### 1. Multi-Tier Hierarchy
* **Offline Root CA**: Ultimate anchor of trust.
* **Issuing CA**: Online server for day-to-day certificate issuance.

### 2. Certificate Policy and Governance
![AD CS Templates](../../../resources/images/Cloud%20PKI%20Intune%20Deployment-checklist.png)


---

## Key Implementation Details
* **Advanced PoC**: [Secure NDES via Azure App Proxy](../../../../Solutions_Architecture/PoC%20and%20Validation/PKI-App%20Proxy-NDES/)
* **CRL Distribution Points (CDP)**: Ensures real-time revocation checking.

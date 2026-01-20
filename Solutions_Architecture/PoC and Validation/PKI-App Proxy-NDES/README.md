
# PoC: Secure NDES/SCEP via Azure App Proxy

## Project Artifacts
The following technical resources document the design, validation, and executive briefing for this modernization project:
* **Technical Slide Deck**: [Cloud PKI & Intune Deployment Briefing (PDF)](../../../resources/slidedecks/Cloud_PKI_Intune_Deployment.pdf)
* **Modern PKI Diagram**: [Architecture Flow](#modern-pki-architecture)
* **Deployment Checklist**: [Engineering Validation Checklist](#deployment-checklist-and-validation)

---

## Overview
This Proof of Concept (PoC) documents the modernization of the **Network Device Enrollment Service (NDES)**. Traditional NDES deployments require exposing an internal web server to the internet via a DMZ. This architecture replaces that legacy risk with **Azure Application Proxy**, providing a secure, outbound-only connection for certificate enrollment.

---

## Modern PKI Architecture
The following diagram illustrates the transition from legacy, internet-exposed NDES to a modern, proxy-integrated flow. This architecture ensures that the **Issuing CA** and **NDES Role** remain deep within the private network.

[![Modern PKI Architecture](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/images/Cloud%20PKI%20Intune%20Deployment-modern-pki.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/images/Cloud%20PKI%20Intune%20Deployment-modern-pki.png)
*Figure 1: High-level architectural flow of Intune SCEP requests through the Azure Application Proxy.*



---

## Technical Strategy
The goal of this deployment is to allow Microsoft Intune to deliver SCEP profiles to mobile and remote endpoints without direct internet exposure of internal PKI services.

### Key Components
* **Azure App Proxy Connector**: Installed on-premises to establish an authenticated outbound tunnel to Azure.
* **External URL**: Provides a secure endpoint for Intune to communicate with the SCEP service.
* **Passthrough Pre-authentication**: Required for the SCEP protocol to allow direct communication with the NDES service while maintaining the proxy's security benefits.

---

## Deployment Checklist and Validation
The engineering checklist below was used to validate the end-to-end configuration, ensuring that the Intune SCEP profiles correctly map to the NDES server's requirements.

[![Deployment Checklist](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/images/Cloud%20PKI%20Intune%20Deployment-checklist.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/images/Cloud%20PKI%20Intune%20Deployment-checklist.png)
*Figure 2: Technical validation checklist for the Cloud PKI and Intune SCEP deployment.*

---

## Engineering Highlights

### Security Posture
* **Outbound-Only Connectivity**: By using the App Proxy connector, the corporate firewall requires zero inbound port openings (80/443), significantly reducing the attack surface.
* **Attack Surface Reduction**: The NDES server is no longer placed in a DMZ, keeping the PKI infrastructure entirely within the internal security zone.

### SCEP Enrollment Flow
1. **Intune Policy**: Managed device receives the SCEP configuration profile.
2. **Proxy Request**: The device attempts to contact the SCEP URL via the Azure App Proxy External URL.
3. **Connector Forwarding**: The on-premises connector pulls the request from Azure and forwards it to the local NDES server.
4. **CA Issuance**: The Issuing CA verifies the challenge and issues the machine certificate.

---

## PoC Artifacts
* **Configuration Overview**: Generated via NotebookLM analysis of internal SCEP profiles.
* **PKI Hierarchy Details**: [View Root CA and Issuing CA Design](../../../Complete%20Hybrid%20Network%20Lab/Security/PKI/)

---
[Return to Solutions Architecture](../../README.md) | [Return to Root](../../../README.md)

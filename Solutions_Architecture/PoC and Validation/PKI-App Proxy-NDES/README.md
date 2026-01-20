# PoC: Secure NDES/SCEP via Azure App Proxy

## Project Artifacts
The following resources document the design, validation, and executive briefing for this modernization project:
* **Technical Slide Deck**: [Cloud PKI & Intune Deployment Briefing (PDF)](../../../resources/slidedecks/Cloud_PKI_Intune_Deployment.pdf)
* **Architecture Mapping**: [Modern PKI Flow Visual](#modern-pki-architecture)
* **Configuration Design**: [Intune SCEP Template](#scep-profile-configuration-intune)
* **Operational Proof**: [Success Confirmation](#operational-success--validation)

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

### SCEP Profile Configuration (Intune)
The SCEP profile within Microsoft Intune is the "instruction set" for the endpoint. The following configuration captures the integration with the Azure App Proxy External URL and the specific certificate attributes required for hybrid trust.

![Intune SCEP Template Configuration](../../../resources/slides/SCEP.png)
*Figure 2: Intune SCEP Profile Template showing the External App Proxy URL and Certificate Authority attributes.*

---

## Operational Success & Validation
Final validation is confirmed by the successful delivery of the SCEP certificate to the managed endpoint. The following confirmation from the Intune portal verifies that the policy has been successfully applied and the device has received its identity certificate via the secure App Proxy tunnel.

![Intune SCEP Success Confirmation](../../../resources/slides/scp_proof.png)
*Figure 3: Intune Portal confirmation showing successful certificate delivery and profile compliance.*

---

## Technical Verification & Logs
To ensure the integrity of the enrollment process, validation was performed across three distinct layers:

### 1. Endpoint Layer (Windows Event Viewer)
Verified on the client device under `Applications and Services Logs > Microsoft > Windows > DeviceManagement-Enterprise-Diagnostics-Provider > Admin`:
* **Event ID 36**: "Certificate request generated successfully."
* **Event ID 309**: "Certificate delivered successfully to the device."

### 2. Network Layer (IIS Logs)
On the NDES server, IIS logs (`%SystemDrive%\inetpub\logs\logfiles\w3svc1`) were monitored for the following:
* **HTTP Status 200**: Confirms successful GET/POST requests for `mscep.dll`.

### 3. Proxy Layer (Entra Private Network Connector)
Verified via the Connector logs under `Applications and Services Logs > Microsoft > Microsoft Entra private network > Connector > Admin`:
* Confirmed authenticated outbound sessions mapping the external Intune request to the internal NDES IP.

---

## Engineering Highlights
* **Zero Inbound Ports**: The corporate firewall requires no inbound ports (80/443), as the App Proxy connector uses an outbound-only authenticated tunnel.
* **PKI Isolation**: The Issuing CA and NDES role are kept entirely within the internal network, meeting the highest "Zero Trust" security standards.

---
[Return to Solutions Architecture](../../README.md) | [Return to Root](../../../README.md)

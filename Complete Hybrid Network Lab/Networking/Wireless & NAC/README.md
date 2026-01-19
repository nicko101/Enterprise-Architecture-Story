

# 🔑 Network Access Control: LAB Onboarding Profile

## 📖 Overview
This section documents the technical configuration of the **LAB Onboarding** network profile. This profile serves as the secure entry point for the NF-CloudLab, utilizing an **802.1X Enterprise** framework to ensure that only trusted, managed devices can gain access to the hybrid fabric.

---

## 🖼️ Technical Anatomy & Workflow
The diagram below illustrates the automated trust lifecycle, showing how various operating systems utilize certificate-based authentication to achieve a trusted state.

<div align="center">
  <img src="../../../docs/diagrams/LAB%20Onboarding%20Network%20Configuration%20Details.png" alt="LAB Onboarding Anatomy" width="950">
  <p><i>Figure 1: Onboarding Logic - Trust Configuration and OS-Specific EAP Protocols</i></p>
</div>



---

## 📋 Technical Implementation Details

### 🛡️ Security Framework: 802.1X Enterprise
* **Encryption**: Employs **WPA2 with AES** to secure the wireless data plane.
* **Authentication**: Enforces **802.1X** standards to verify device identity before any network traffic is permitted.

### 🔐 OS-Specific Authentication Protocols
* **EAP-TLS (Modern OS)**: Windows, macOS, iOS, Android, and Ubuntu are strictly required to use certificate-based authentication (EAP-TLS).
* **Trust Automation**: The profile automatically pushes the required root CA and server certificates to clients to eliminate manual "trust" prompts.
* **PEAP (Legacy)**: Support for **PEAP with MSCHAPv2** is maintained exclusively for legacy OS X versions.

### 🌐 Network Orchestration
* **Dynamic IP Assignment**: Authenticated clients are placed into the `182.166.X.X` subnet via DHCP.
* **DNS Integration**: The onboarding profile ensures clients are pointed to internal lab DNS servers for split-brain name resolution.

---

## 🏛️ Architectural Role
This onboarding profile acts as the "Zero Trust" gatekeeper. By integrating with **Infrastructure/Certificate-Services** (SCEP/NDES) and **Endpoint-Management** (Intune), the lab ensures that a device must be both **Identified** (Certificate) and **Compliant** (MDM) before the ClearPass NAC grants network access.

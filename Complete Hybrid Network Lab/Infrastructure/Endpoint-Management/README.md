
# 📱 Endpoint Management: Microsoft Intune Integration

## 📖 Overview
This directory documents the cloud-based management plane for all managed endpoints within the hybrid lab. By leveraging Microsoft Intune, the lab enforces a "Compliance-First" security model where device health directly dictates network access privileges.

## 📂 Core Management Pillars

### 📜 Configuration Profiles (SCEP/NDES)
The primary mechanism for distributing identity to endpoints.
* **Identity Distribution**: Automation of certificate deployment using the SCEP protocol via the on-premises NDES server.
* **Wi-Fi/Wired Profiles**: Pre-configured XML profiles that force devices to use TEAP (Tunnel EAP) for 802.1X authentication.

### 🛡️ Compliance Policies
The "Gatekeeper" for the Zero-Trust handshake.
* **Posture Assessment**: Monitors for BitLocker encryption, OS build version, and Antivirus status.
* **ClearPass Integration**: Intune reports this compliance status back to Aruba ClearPass, which then allows or denies network access based on the device's real-time health.

### 🚀 App Deployment & Scripting
Standardization of the software environment across the lab.
* **Base Image**: Automated deployment of core security agents and productivity tools.
* **Proactive Remediation**: PowerShell scripts used to maintain local security configurations.

## 🖼️ Architectural Logic

*Figure 1: The secure connection flow showing the handshake between the Managed Device, Microsoft Intune, and Aruba ClearPass.*

---

## 🛠️ Implementation Highlights
Every endpoint managed in this suite adheres to the following standards:
1. **Zero-Touch Enrollment**: Devices are onboarded via Autopilot or manual Azure AD Join.
2. **Certificate-Based Identity**: No passwords are used for network access; all trust is founded on the Enterprise PKI.
3. **Automated Governance**: Compliance drift results in immediate quarantine via the Palo Alto NGFW and ClearPass policy engine.

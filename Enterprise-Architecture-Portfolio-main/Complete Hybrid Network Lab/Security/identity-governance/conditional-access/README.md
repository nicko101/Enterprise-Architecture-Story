# 📱 Conditional Access: Intune Compliance & Cloud-Identity Handshake

## 📖 Overview
This directory documents the implementation of **Conditional Access** within the NF-CloudLab. Unlike traditional network access, this model requires every device to prove its "Health" and "Compliance" status in Microsoft Intune before being granted admission by the on-premises switching fabric.

## 🖼️ Cloud-to-Ground Compliance Flow
The integration relies on a secure API handshake to verify device state in real-time.

![Integrated Architecture](../../../resources/images/LAB02-Image.png)
*Figure 1: High-level architectural overview showing the role of Microsoft Intune in the secure network fabric.*

## ⚙️ Technical Implementation

### 🔌 ClearPass Intune Extension
The connection is established using the **Aruba ClearPass Intune Extension**, which facilitates the following:
* **Microsoft Graph API Integration**: ClearPass is registered as an application in the Azure tenant with `Device.Read.All` permissions to query the Graph API.
* **Real-Time Polling**: When a device authenticates via 802.1X, ClearPass sends the device's MAC address or Serial Number to Intune to fetch its current compliance attribute.
* **Attribute Mapping**: The "isCompliant" status from the cloud is mapped to a local ClearPass attribute used in enforcement policies.

### 🛡️ Intune Compliance Policies
Endpoints must meet specific "Healthy" criteria defined in the Microsoft Endpoint Manager portal:
* **Encryption Required**: BitLocker must be active and the recovery key backed up to Entra ID.
* **Security Baseline**: System integrity checks, including Secure Boot and active Antivirus/Firewall state.
* **OS Version**: Devices must be running a supported and patched version of Windows 10/11.

## 🔄 The Zero Trust Handshake
The diagram below illustrates Step 4 of the workflow: the moment ClearPass pauses the authentication to verify compliance with the cloud.

![Zero Trust Workflow](../../../resources/images/Zero-Trust-Workflow.png)
*Figure 2: Workflow detailing the compliance check and subsequent policy enforcement.*

---

## 🛠️ Enforcement Logic
1. **Compliant Access**: If Intune returns `isCompliant: true`, ClearPass assigns the **Employee-Role** and the Palo Alto firewall allows full corporate access.
2. **Non-Compliant Quarantine**: If a device fails compliance (e.g., encryption is disabled), ClearPass triggers a **Change of Authorization (CoA)** to move the port to a restricted remediation VLAN.
3. **Automated Remediation**: Once the user fixes the compliance issue, Intune updates the Graph API, and the device is automatically re-authorized on the next periodic poll.

## 📸 Evidence & Validation
> **[Evidence Placeholder]**: *Insert a screenshot of the Azure Portal showing a "Compliant" Windows 11 device.*
>
> **[Evidence Placeholder]**: *Insert a screenshot of the ClearPass 'Endpoint' attributes showing the successfully fetched Intune compliance data.*

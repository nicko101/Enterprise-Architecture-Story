# 🧬 Network Access Control (NAC): Aruba ClearPass Technical Deep-Dive

## 📖 Overview
This directory details the technical configuration of **Aruba ClearPass Policy Manager (CPPM)** within the LAB02 environment. As the primary Policy Decision Point (PDP), ClearPass orchestrates the transition from traditional identity-only authentication to a modern Zero Trust model by integrating local network requests with cloud-managed health data.

## 🔌 The Intune Extension: Bridging Cloud & Ground
The core of this NAC implementation is the **ClearPass Intune Extension**, which acts as the intelligence bridge:
* **Graph API Connectivity**: The extension allows ClearPass to securely query the Microsoft Graph API using OAuth2 credentials.
* **Compliance Handshake**: During every 802.1X authentication attempt, ClearPass pauses the session to poll Intune for the specific "isCompliant" attribute of the requesting device.
* **Dynamic Decision Making**: Authorization is only granted if the Intune Extension returns a positive compliance status, moving beyond static username/password verification.

## ⚙️ Supported Authentication Services
* **802.1X Wired (EAP-TEAP)**: Leverages a secure tunnel to verify both the machine certificate and the user's identity simultaneously.
* **MAC-Authentication**: Provides basic security for IoT devices, with profiles that can still trigger remediation if the device behaves anomalously.

## 🖼️ Technical Policy Workflow
The diagram below illustrates Step 4: the critical moment the Intune Extension performs its compliance check.

![Zero Trust Workflow](../../../resources/images/Zero-Trust-Workflow.png)  
*Figure 1: Packet-level workflow showing the ClearPass engine leveraging the Intune Extension for real-time compliance validation.*

## 🛠️ Enforcement & Remediation
* **Change of Authorization (CoA)**: ClearPass utilizes RADIUS CoA to dynamically disconnect or re-VLAN a device the moment the Intune Extension detects a compliance failure.
* **VLAN Steering**: Successfully authenticated and compliant devices are steered into protected zones (e.g., VLAN 11 - Employee) via Vendor-Specific Attributes (VSAs) sent to the AOS-CX switch.

## 📸 Evidence & Validation
> **[Evidence Placeholder]**: *Insert a screenshot of the ClearPass 'Extensions' page showing the Intune Extension in an 'Active/Running' state.*
>
> **[Evidence Placeholder]**: *Insert a screenshot of an 'Access Tracker' log entry where the 'Attributes' tab shows the 'Intune_Compliance_Status' fetched from the cloud.*

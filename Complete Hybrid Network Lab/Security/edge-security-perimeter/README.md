# 🛡️ Edge Security Perimeter: Palo Alto NGFW Governance

## 📖 Overview
This directory documents the technical implementation of the perimeter security layer, powered by a **Palo Alto Networks PA-VM**. This tier serves as the final **Enforcement Point** in the Zero Trust workflow, ensuring that only authenticated, authorized, and compliant traffic can traverse the boundary between the local lab and external fabrics.

## 🖼️ Perimeter Security Architecture
The Palo Alto firewall acts as the central security hub, integrating with both on-premises identity and cloud governance.

![Inside the LAB02 Secure Network](../../resources/images/LAB02-Image.png)
*Figure 1: High-level architectural overview highlighting the PA-VM as the primary gatekeeper for threat prevention.*

## 📂 Core Security Functions

### 👤 Identity-Based Enforcement (User-ID)
* **Identity Mapping**: Integrates with Active Directory and Aruba ClearPass to map transient IP addresses to specific user identities.
* **Role-Based Policies**: Security rules are applied based on "Who" is requesting access (e.g., IT Admin vs. Guest) rather than just IP addresses.

### 🕵️ Threat Prevention & Visibility
* **Deep Packet Inspection**: Full implementation of Antivirus, Anti-Spyware, and Vulnerability Protection profiles to harden the lab's attack surface.
* **SSL/TLS Decryption**: Decrypts and inspects encrypted traffic to prevent hidden threats while maintaining privacy for sensitive traffic.
* **URL Filtering**: Enforces strict category-based access to the internet to prevent data exfiltration and access to malicious sites.

## 🔄 Zero Trust Integration
Final access is only granted after the device has successfully passed the **Intune Compliance Check** and **ClearPass RADIUS handshake**.

![Zero Trust Workflow](../../resources/images/Zero-Trust-Workflow.png)
*Figure 2: Step 5 of the Modern Network Access Workflow: Final enforcement by the Palo Alto Networks firewall.*

---

## 🛠️ Standards & Best Practices
1. **Explicit Allow Model**: The security policy follows a strict "Deny-All" default stance.
2. **Zone Protection**: Implementation of Flood Protection and Reconnaissance Protection to defend the virtualized network edge.
3. **Continuous Monitoring**: Enhanced logging is enabled on all rules to provide a comprehensive audit trail for security forensics.

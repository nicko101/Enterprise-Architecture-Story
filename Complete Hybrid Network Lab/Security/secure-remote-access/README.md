
# 🌐 Secure Remote Access: GlobalProtect & Cloud SSO

## 📖 Overview
This directory documents the remote connectivity tier of the LAB02 environment. Secure access is provided via **Palo Alto GlobalProtect**, utilizing **SAML 2.0** for Single Sign-On (SSO) with **Microsoft Entra ID**. This ensures that remote users are subjected to the same identity governance and conditional access policies as internal lab users.

## 🖼️ Remote Access Architecture
The GlobalProtect gateway acts as a virtual extension of the internal network, terminating encrypted tunnels directly into the security perimeter.

![Integrated Architecture](../../resources/images/LAB02-Image.png)
*Figure 1: High-level overview showing the GlobalProtect VPN path and its integration with Microsoft Azure for SSO.*

## 📂 Technical Implementation

### 🔐 SAML 2.0 & SSO Integration
* **Identity Authority**: Microsoft Entra ID serves as the Identity Provider (IdP) for all remote authentication requests.
* **Conditional Access**: Entra ID evaluates the device state and user location before allowing the GlobalProtect connection to establish.
* **User Experience**: Users benefit from a seamless passwordless login experience using their primary corporate credentials and modern Multi-Factor Authentication (MFA).

### 🛡️ VPN Gateway & Portal
* **Termination**: Managed by the Palo Alto PA-VM, providing encrypted SSL/IPsec tunnels for remote endpoints.
* **Identity Mapping**: Remote users are automatically mapped to Palo Alto User-ID groups, ensuring internal security policies follow the user regardless of location.
* **Split-Tunneling**: Optimized to route only internal lab traffic through the tunnel, preserving local bandwidth for the user's internet traffic.

## 🔄 The Secure Connection Workflow
Remote access follows a trust validation process identical to the internal Zero Trust model.

![Zero Trust Workflow](../../resources/images/Zero-Trust-Workflow.png)
*Figure 2: The standard compliance and identity check applied to every remote connection.*

---

## 🛠️ Standards & Security Posture
1. **Always-On Connectivity**: (Optional) Configured for managed devices to ensure a persistent management channel for Intune updates.
2. **Encryption**: Tunnels utilize AES-256 and SHA-256 to ensure data integrity across the public internet.
3. **MFA Enforcement**: Every remote session requires a secondary factor, significantly reducing the risk of unauthorized access via credential theft.

## 📸 Evidence & Validation
> **[Evidence Placeholder]**: *Insert a screenshot of the Palo Alto GlobalProtect 'Gateway' status showing an active user session.*
>
> **[Evidence Placeholder]**: *Insert a screenshot of the Azure Entra ID 'Sign-in logs' showing the successful SAML authentication for GlobalProtect.*


# 🔄 Server Role: AD Connect Sync Bridge

## 📖 Overview
This server facilitates the hybrid identity "Global Handshake" by synchronizing on-premises Active Directory objects to Entra ID. It ensures that identity-based security policies remain consistent across the local network and the Azure cloud tenant.

## 🛠️ Configuration & Role
* **Synchronization Service**: Manages the mapping of local SIDs to cloud immutable IDs.
* **Primary Integration**: Authorized via the **ConnectSyncProvisioning** App Registration in Entra ID.
* **Auth Method**: Password Hash Synchronization (PHS) to enable cloud-native auth features like MFA.

## 🖼️ Operational Evidence
*Placeholder: Insert screenshot of the Synchronization Service Manager showing a successful "Export" to Entra ID.*

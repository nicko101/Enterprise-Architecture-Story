# 💾 Storage Services: Hybrid Data Integration

## 📖 Overview
This directory documents the hybrid storage architecture of the lab, focusing on the integration of **Azure Files** with the on-premises Active Directory forest. This implementation enables seamless cloud-tiering while maintaining traditional NTFS permissions and identity-based access.

## 📂 Core Storage Components

### ☁️ Azure File Shares (Domain Joined)
The lab utilizes Azure Storage Accounts joined to the local `lab02` domain to provide scalable, cloud-native file storage.
* **Identity Integration**: Storage accounts are represented as computer objects in Active Directory to facilitate Kerberos authentication.
* **Access Control**: NTFS permissions are managed via traditional Windows File Explorer, synchronized through the hybrid identity bridge.

### 🔄 Azure File Sync (Az File Sync)
A critical service that bridges on-premises file servers with Azure cloud storage.
* **Cloud Tiering**: Frequently accessed files are cached locally for performance, while the full data set is archived in Azure Files.
* **Sync Groups**: Managed under the `Cloud-Compute` tier to ensure high availability of data across different lab locations.

## 🖼️ Operational Evidence
![Azure File Sync Configuration](./evidence/az-file-sync-status.png)
*Figure 1: Validation of the Azure File Sync service and synchronization health.*

---

## 🛠️ Configuration Standards
1.  **Identity-Based Access**: All storage access is governed by Active Directory security groups, ensuring least-privilege access to sensitive data shares.
2.  **Encryption**: Data is encrypted at rest using Platform Managed Keys (PMK) and in transit via SMB 3.0.
3.  **Governance**: Storage accounts are organized within dedicated **Resource Groups** and tagged according to enterprise lifecycle standards.

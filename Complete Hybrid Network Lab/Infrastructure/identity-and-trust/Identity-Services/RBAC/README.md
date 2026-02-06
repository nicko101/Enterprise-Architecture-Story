# Role-Based Access Control: Hybrid Identity Governance

## Overview
This section documents the enforcement of identity-driven security across the hybrid fabric. Access is governed by the principle of least privilege (PoLP), ensuring that users, service principals, and managed identities are granted only the minimum permissions required to perform their functions.

[![RBAC Solutions Overview](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/RBAC.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/RBAC.png)
*Figure 1: High-level solution architecture detailing the relationship between identity providers, enforcement points, and hybrid cloud resources.*

---

## Dual-Layer Access Governance
In this hybrid environment, storage and file access require a two-tier authorization model to bridge cloud identity (Microsoft Entra ID) with on-premises resource management.

### 1. Share-Level Permissions (Azure RBAC)
Azure RBAC acts as the "Gatekeeper" at the storage account level. It determines if an identity has the right to access the resource over the network.
* **Storage File Data SMB Share Reader**: For read-only access to file shares.
* **Storage File Data SMB Share Contributor**: For standard read/write/delete operations.
* **Storage File Data SMB Share Elevated Contributor**: Required for administrators to modify NTFS ACLs.

### 2. File-Level Permissions (NTFS ACLs)
Once the RBAC gate is passed, standard Windows NTFS permissions are enforced at the directory and file level. This allows for granular control (e.g., Finance team seeing only the 'Payroll' folder within a shared drive).

---

## Identity Synchronization Flow
To enable seamless access for on-premises users to cloud-hosted storage (Azure Files), a hybrid identity flow is implemented:

1. **Active Directory (On-Prem)**: Serves as the authoritative source of truth for user identities.
2. **Entra Connect**: Synchronizes identities to Microsoft Entra ID (Azure AD).
3. **Domain Joining**: The Azure Storage Account is joined to the local Active Directory domain.
4. **Kerberos Authentication**: Clients utilize Kerberos tickets from the local Domain Controller to authenticate against the Azure File Share.

---

## Operational Configuration
The following technical artifact shows the engineering view of the role assignments utilized to manage the laboratory environment.

[![RBAC Assignments](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/PKItemplate.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/PKItemplate.png)
*Figure 2: Role assignment configuration in the Azure Portal, showcasing data-plane access for synced hybrid identities.*

### Best Practices Enforced
* **Group-Based Access**: Roles are assigned to Entra ID Groups rather than individual users to simplify lifecycle management.
* **Separation of Planes**: Control Plane access (Owner/Contributor) is strictly separated from Data Plane access (Storage Blob/File Data roles).
* **Least Privilege**: Global Admin privileges are used only for identity configuration, while day-to-day data access relies on scoped data roles.

---
[Return

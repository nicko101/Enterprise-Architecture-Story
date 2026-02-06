
# ☁️ Cloud Compute & Hybrid Integration

## 📖 Overview
This directory documents the Azure-native compute and storage services that form the cloud-side of the hybrid lab environment. These services facilitate seamless data synchronization and disaster recovery between the on-premises GNS3/Proxmox environment and Microsoft Azure.

## 📂 Core Components

### 🔄 Azure File Sync
Demonstrates the integration of local Windows File Servers with Azure Files.
* **Objective**: Centralize file shares in Azure while keeping local performance through caching.
* **Key Feature**: Cloud Tiering to optimize local disk space while maintaining an "infinite" storage backend.

### 🛡️ Recovery Services Vault (RSV)
The foundational pillar for the lab's business continuity and disaster recovery (BCDR) strategy.
* **Azure Backup**: Protecting on-premises server workloads (Domain Controllers, File Servers).
* **Site Recovery**: Orchestration of failover for critical lab infrastructure to Azure VNets.

### 🏷️ Resource Governance
Documentation of the **Resource Group** hierarchy used to manage lab costs and lifecycle.
* **Naming Standards**: Enforcement of enterprise-aligned naming conventions for VNets, RSV, and Sync Groups.

## 🖼️ Operational Evidence
![Azure Recovery Services Vault Status](./RSV/SRV.png)
*Figure 1: Validation of the Recovery Services Vault status within the production Azure tenant.*

---

## 🛠️ Configuration Standards
Every cloud asset in this directory follows these architectural principles:
1. **Least Privilege**: Managed Identities used for service-to-service authentication.
2. **Encryption**: Data encrypted at rest via Platform Managed Keys (PMK).
3. **Connectivity**: All sync traffic traverses the established Site-to-Site VPN tunnel.

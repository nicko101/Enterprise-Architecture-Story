
# 🖥️ Compute Tier: Core Server Infrastructure

## 📖 Overview
This directory documents the virtualized server assets that power the hybrid lab core. Each server is built on a "Service Isolation" model to ensure high availability, security segmentation, and alignment with enterprise production standards.

## 🏗️ Server Inventory & Roles

| Server Role | Purpose | Critical Integration |
| :--- | :--- | :--- |
| **Domain Controllers** | Primary Identity & DNS services for the `lab02` forest. | User-ID (Palo Alto). |
| **AD Connect** | Identity synchronization bridge to Entra ID. | ConnectSyncProvisioning. |
| **Root-CA** | Offline Root Certificate Authority; the anchor of trust. | Enterprise PKI. |
| **Issuing CA** | Online Subordinate CA for automated certificate lifecycle management. | 802.1X Authentication. |
| **NDES-Server** | SCEP/NDES bridge for mobile and network device enrollment. | ndes-proxy (Entra ID). |

---

## 🛠️ Compute Standards
1. **Hardened Baselines**: Servers are deployed with minimal role-based footprints to reduce attack surface.
2. **Hybrid Integration**: Native agents (AD Connect, NDES Proxy) link on-premises compute directly to the Azure management plane.
3. **Health Monitoring**: Service health is validated via tools such as `pkiview` to ensure continuous uptime for security services.

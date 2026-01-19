# 🌐 Network Infrastructure: Hybrid Connectivity Fabric

## 📖 Overview
This directory contains the core networking artifacts for the NF-CloudLab. The architecture utilizes a "Hub-and-Spoke" model to bridge on-premises hardware with a Pay-As-You-Go Azure tenant, prioritizing secure tunneling and predictable path selection.

## 🖼️ Network Topology & Routing Flow
The hybrid fabric utilizes a **Route-Based Site-to-Site VPN** between the on-premises Palo Alto NGFW and the Azure Virtual Network Gateway.



### 🛡️ Hybrid Connectivity Validation
![Palo Alto S2S VPN Status](../resources/screenshots/Azure-s2s.png)
*Figure 1: Validation showing the active Phase 1 (IKE) and Phase 2 (IPsec) tunnels.*

## 📋 Table of Contents

### ☁️ Cloud Networking
* **VNet Fabric**: Implementation of Virtual Networks and Subnetting within the Azure tenant.
* **VPN Gateway**: Configuration of the Azure Virtual Network Gateway for static IPsec termination.
* **DNS**: Private DNS Resolver integration for seamless cross-premises name resolution.

### 🏢 On-Prem_Switching
* **VLAN Architecture**: Segmentation of Management, Corporate, Guest, and IoT traffic.
* **Trunking**: Layer 2 configurations connecting the Proxmox virtual host to the GNS3 fabric.

### 📶 Wireless & NAC
* **ClearPass**: Identity authority for Corporate (EAP-TLS) and Guest SSIDs.
* **RADIUS Handshake**: Technical integration between the fabric and the ClearPass policy engine.

### 🛡️ Edge Security & Routing
* **Palo Alto NGFW**: Termination point for the hybrid tunnel using IKEv2 and AES-256 encryption.
* **Static Routing Strategy**: Implemented static routes for predictable traffic flow, replacing the legacy BGP configuration to optimize for the Pay-As-You-Go tenant requirements.

### 🖥️ Virtualisation Fabric
* **Proxmox SDN**: Virtual bridge configurations, including specialized EAPOL forwarding masks.

---

## 🖼️ Operational Evidence
![Proxmox Bridge Configuration](../Infrastructure/Virtualisation/evidence/proxmox-bridge-config.png)
*Figure 2: Validation of the 'group_fwd_mask' setting on vmbr25, enabling EAPOL packet forwarding for 802.1X NAC.*

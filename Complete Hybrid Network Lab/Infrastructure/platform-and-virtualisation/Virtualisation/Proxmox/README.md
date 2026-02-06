# 💠 Proxmox VE: Hypervisor & Hybrid Gateway

## 📖 Overview
This directory documents the technical configuration of the Proxmox VE cluster, which serves as the primary compute, Layer 2 bridging, and **Hybrid Connectivity** authority for the environment. 

## 📂 Technical Proof of Implementation

### 🛡️ Hybrid Connectivity: Palo Alto Site-to-Site VPN
The primary gateway for the hybrid fabric is a Palo Alto NGFW VM hosted on Proxmox. This node terminates a persistent IPsec tunnel to the Azure Virtual Network Gateway, facilitating secure identity and storage synchronization between the on-premises lab and the Azure tenant.

* **IPsec Termination**: The Palo Alto VM manages the IKE Gateways and Crypto Profiles required for the Azure handshake.
* **Hybrid Routing**: Facilitates the path for AD Connect and Azure File Sync to reach the cloud tenant securely.

### 🌉 Hypervisor Protocol Transparency (vmbr25)
Standard Linux bridges drop 802.1X (EAPOL) frames by default. This hypervisor is specifically hardened to forward these frames, ensuring authentication traffic from GNS3-hosted switches can reach the Aruba ClearPass Policy Manager.

* **The Command**: `post-up /bin/sh -c 'echo 8 > /sys/class/net/$IFACE/bridge/group_fwd_mask'`.
* **The Result**: Explicit forwarding of the 802.1X multicast group MAC `01:80:c2:00:00:03`.

## 🖼️ Operational Evidence

### 🌐 VPN Tunnel Status
![Palo Alto S2S VPN Status](../../../resources/screenshots/Azure-s2s.png)
*Figure 1: Validation from the Palo Alto WebUI showing the Azure IPsec tunnel and IKE Gateways in an 'Active/Green' state.*

### 🌉 Bridge Configuration
![Proxmox Bridge Configuration](./evidence/proxmox-bridge-config.png)
*Figure 2: CLI validation of the 'group_fwd_mask' setting on vmbr25, enabling EAPOL packet forwarding.*

---

## 🛠️ Performance & Standards
1. **L2 Frame Fidelity**: Maintains original vendor protocol headers (Cisco/Aruba) across virtual boundaries to ensure authentic NAC behavior.
2. **Hybrid Path Integrity**: Optimized host environment to support encapsulated IPsec traffic for high-throughput cloud operations.
3. **VM Lifecycle Governance**: Automated snapshotting of critical identity nodes (DCs, Root CA) ensures rapid recovery and environment stability.

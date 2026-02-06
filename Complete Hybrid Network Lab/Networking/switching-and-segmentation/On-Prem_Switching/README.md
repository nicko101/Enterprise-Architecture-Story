
# 🏢 On-Premises Switching: The NAC Enforcement Tier

## 📖 Overview
This directory documents the configuration of the local switching fabric, powered by **Aruba AOS-CX**. This tier serves as the **Authenticator** within the Zero Trust framework, leveraging RADIUS and TACACS+ to secure the physical and virtual entry points of the LAB02 environment.

## 📂 Core Configuration Pillars

### 🔐 Identity & Access Control (AAA)
The switch is deeply integrated with **Aruba ClearPass** for both management and network access.
* **802.1X & MAC-Auth**: Interfaces 1/1/2 through 1/1/5 are configured for dual-mode authentication, ensuring every device—from managed laptops to headless APs—is identified before admission.
* **RADIUS Dynamic Authorization**: Enabled to allow ClearPass to send **Change of Authorization (CoA)** packets, enabling real-time session termination or VLAN reassignment.
* **TACACS+ Management**: Administrative access is centralized via TACACS+, providing a full audit trail of commands executed on the fabric.

### 🌉 VLAN & Trunking Architecture
The switch manages a multi-VLAN environment to maintain strict broadcast domain separation.
* **Uplink (1/1/1)**: A 802.1Q trunk to the Palo Alto NGFW, carrying critical segments including Management (VLAN 10), Employee (VLAN 11), and CPPM (VLAN 60).
* **DHCPv4 Snooping**: Implemented on the Employee VLAN (VLAN 11) to prevent rogue DHCP servers and maintain IP source integrity.

### 🛡️ Critical & Local Roles
To ensure availability during a RADIUS timeout or server failure, specialized roles are implemented:
* **Critical Role**: Dynamically moves ports to VLAN 20 if ClearPass becomes unreachable, ensuring basic connectivity for essential services.
* **LUR (Local User Roles)**: Defines role-based VLAN assignments (BYOD, Helpdesk, Employee) locally on the switch for rapid enforcement.

## 🖼️ Operational Flow: The NAC Handshake
The diagram below illustrates how this switch interacts with ClearPass and the Palo Alto firewall to secure a connection.

![Zero Trust Workflow](../../resources/images/Zero-Trust-Workflow.png)
*Figure 1: The AOS-CX switch acts as the Authenticator, intercepting EAP-TEAP requests and forwarding them to ClearPass.*

---

## 🛠️ Technical Evidence (CLI Validation)
> **Host**: `AOS-CX-01`  
> **Primary RADIUS**: `cppm.duckdns.org`  
> **Key Security Feature**: `radius dyn-authorization enable` — Critical for active session management and remediation.

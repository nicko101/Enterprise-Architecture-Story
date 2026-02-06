# Technical Implementation: Complete Hybrid Network Lab

## Executive Summary
This directory serves as the definitive "As-Built" documentation for a production-grade hybrid engineering environment. The lab simulates a full-stack enterprise modernization journey, bridging on-premises legacy infrastructure with cloud-native Azure services. 

The environment is designed to validate a Zero-Trust security posture, focusing on certificate-based identity, dynamic routing, and automated endpoint governance across a multi-vendor ecosystem.

---

## Architectural Pillars

### 1. Networking and Perimeter Security
The network core is built on a multi-vendor fabric designed for high availability and granular traffic control.
* **Core Routing**: A virtualized Cisco core handles enterprise-grade routing protocols and site-to-site connectivity.
* **Edge Enforcement**: Aruba AOS-CX switching provides the physical and virtual edge, facilitating 802.1X and MAC-Authentication.
* **Perimeter Defense**: Palo Alto VM-Series Firewalls act as the primary security boundary and central routing authority, managing extensive sub-interface segmentation and enforcing protection against external threats.



### 2. Identity and Access Governance
The identity fabric integrates legacy directory services with modern cloud control planes to ensure "explicit verification" for all users and devices.
* **Hybrid Identity Bridge**: Bidirectional synchronization between local Active Directory and Microsoft Entra ID.
* **NAC Orchestration**: Aruba ClearPass (CPPM) serves as the central policy engine, performing real-time posture checks via Microsoft Intune integration.
* **Modernized Enrollment**: Implementation of NDES and SCEP via Azure App Proxy allows secure, internet-based certificate delivery to managed endpoints without exposing internal PKI roles.

### 3. Compute and Resource Management
* **Hypervisor Foundation**: Proxmox Virtual Environment provides the Type-1 hypervisor foundation, hosting redundant domain controllers, security appliances, and Linux workloads.
* **Operational Stability**: The environment is tuned for high-density operation, utilizing over 32 GB of RAM to sustain concurrent security, identity, and routing services.

---

## Operational Capabilities

### Dynamic Hybrid Routing
The lab utilizes BGP (Border Gateway Protocol) to automate route exchange between the on-premises security boundary and the Azure VPN Gateway. This ensures that cloud and on-premises environments maintain synchronized routing tables without manual intervention.



### Certificate-Based Security
Trust is established through a multi-tier PKI hierarchy. Every managed device is issued a unique identity certificate, which is used for:
* **EAP-TLS Authentication**: Secure network access for wired and wireless clients.
* **GlobalProtect VPN**: Certificate-based pre-logon and user authentication for remote access.
* **Service Identity**: Encrypted communication between internal infrastructure components.

---

## Lab Exploration Guide

### Infrastructure & Platform
* **[Active Directory](./Infrastructure/Active-Directory/)**: Local identity and group policy management.
* **[Certificate-Services](./Infrastructure/Certificate-Services/PKI/)**: PKI hierarchy, NDES, and SCEP architecture.
* **[Virtualization](./Infrastructure/Virtualization/)**: Proxmox host configuration and virtual machine management.

### Networking & Security
* **[Routing](./Networking/Routing/)**: Edge and cloud routing, BGP peering, and Virtual WAN.
* **[Network Access Control](./Security/Network-Access-Control/)**: Aruba ClearPass policy logic and Intune integration.
* **[Firewall Governance](./Security/Firewall/)**: Palo Alto security profiles, zone protection, and SSL decryption.

---
**Author**: Nick Fennell
**Focus**: Hybrid Connectivity, Zero-Trust Identity, and Network Security Engineering.

[Return to Root README](../README.md)

# Technical Implementation: Complete Hybrid Network Lab

## Lab Overview
This directory serves as the technical "As-Built" documentation for the implementation of this hybrid environment. It captures the configuration logic and security policies required to maintain a functional security fabric across on-premises and cloud-native services.

[![Anatomy of a Hybrid Security Lab](../resources/slides/anatomy.png)](../resources/slides/anatomy.png)
*Figure 1: Anatomy of the Hybrid Lab — hardware utilization, virtualized network core, and security orchestration.*

---

## Technical Architecture Components

### Compute and Virtualization
* **Proxmox Virtual Environment**: Serving as the primary type-1 hypervisor hosting the enterprise infrastructure.
* **Resource Management**: Active monitoring of compute resources, with the primary host utilizing over 32 GB of RAM for lab operations.

### Network and Perimeter Security
* **Virtualized Core**: A Cisco-based network core built on GNS3 to simulate enterprise routing environments.
* **Perimeter Defense**: Palo Alto Firewalls providing active threat inspection and protection against TCP flood attacks from untrusted zones.

### Identity and Access Governance
* **Cloud-Native Identity**: Centralized identity management utilizing Microsoft Azure and SAML for Single Sign-On (SSO).
* **Device Compliance**: Integration of Microsoft Intune via Graph API to enforce network access policies based on device health.
* **Automated Trust**: Utilization of SCEP profiles to automatically push certificates from a local authority to managed endpoints.

---

## Lab Exploration Guide

### Infrastructure
* [Active Directory](./Infrastructure/Active%20Directory): Local identity and group policy management.
* [Certificate-Services](./Infrastructure/Certificate-Services): PKI hierarchy and NDES/SCEP configuration.

### Networking
* [Edge Security and Routing](./Networking/Edge%20Security%20%26%20Routing): BGP configuration and VLAN segmentation.
* [Cloud Networking](./Networking/Cloud%20Networking): Azure VPN Gateway and Virtual WAN integration.

### Security
* [Network Access Control](./Security/identity-governance/network-access-control): Aruba ClearPass policy logic and service orchestration.

---
[Return to Root README](../README.md)

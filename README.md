# Hybrid Zero Trust Architecture: Engineering Portfolio

## Overview
This repository documents the engineering and governance of a multi-vendor hybrid environment. The project serves as a functional staging area to bridge on-premises virtualization with Microsoft Azure, creating a unified security fabric centered on identity-based policy enforcement and automated trust delivery.

**Project Status:** Active engineering build. Documentation and technical artifacts are updated regularly as new integration modules are validated.

[![Key Integration Framework](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/keyintgration.png)](https://raw.githubusercontent.com/nicko101/Enterprise-Architecture-Portfolio/main/resources/slides/keyintgration.png)
*Figure 1: Full-stack integration summary across Identity, Trust, Access, Perimeter, and Hybrid Cloud.*

---

## Solutions Architecture
The environment is built on a modular architecture designed to scale and adapt to enterprise-grade security requirements. It spans four primary domains:

* **Identity and Trust Plane**: Managed via a two-tier PKI (Root and Intermediate CA) and Microsoft Entra ID synchronization.
* **Access and Enforcement Plane**: Context-aware admission utilizing Aruba ClearPass policy orchestration and Microsoft Intune device compliance.
* **Connectivity and Routing Plane**: Hybrid transit utilizing secure IPsec tunneling and predictable path selection between local Edge and Azure Virtual Network Gateways.
* **Infrastructure and Hosting Plane**: High-availability compute via a Proxmox virtualization cluster and GNS3-simulated network fabrics.

---

## Portfolio Navigation
These links are verified to match the repository structure. Clicking these will lead reviewers directly to the technical deep-dives.

| Category | Deep-Dive Links |
| :--- | :--- |
| **Integrated Lab Environment** | [Complete Hybrid Network Lab](./Complete%20Hybrid%20Network%20Lab/) |
| **Architecture Playbook** | [Solutions Architecture](./Solutions_Architecture/) |
| **Technical Assets** | [Image Resources](./resources/images/) \| [Architectural Slides](./resources/slides/) |

---

## Technical Standards and Validation
Functional success is proven through operational evidence:
* **Traffic Observability**: Real-time logs from Palo Alto and ClearPass telemetry.
* **Compliance Reporting**: Live status dashboards from Microsoft Intune and Azure Backup status.
* **Engineering Documentation**: Repeatable deployment templates for hybrid infrastructure.

---
Engineering Portfolio | Focused on Resilient Security Architectures

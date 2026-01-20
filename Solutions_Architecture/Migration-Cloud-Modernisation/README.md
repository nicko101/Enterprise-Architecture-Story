# Cloud Migration & Modernisation

## Overview
This directory documents a series of **cloud migration and modernisation initiatives** focused on transitioning legacy, on-premises infrastructure into **secure, scalable, and cloud-native Azure architectures**.

Rather than isolated proofs of concept, each sub-project represents a **practical modernization pattern** commonly encountered in enterprise environments—covering networking, identity, security, connectivity, and platform services.

The designs prioritise:
- **Security-first architecture**
- **Hybrid compatibility**
- **Incremental migration strategies**
- **Operational realism**

All solutions are implemented and validated in a live Azure environment.

---

## Objectives
- Modernise legacy infrastructure using **Azure-native services**.
- Reduce operational complexity while improving security posture.
- Enable hybrid coexistence during migration phases.
- Demonstrate real-world enterprise migration patterns.
- Provide production-aligned reference architectures.

---

## Solution Categories
Each subdirectory below contains a **self-contained architecture**, including design rationale, configuration details, and validation outcomes.

### 🔐 Identity & Access Modernisation
* **[App Proxy (NDES)](./App%20Proxy%20(NDES)/)**: Secure certificate enrollment via Azure App Proxy.
* **[Azure P2S VPN (Internal PKI)](./Azure%20P2S%20VPN%20(Internal%20PKI)/)**: Certificate-based remote access using OpenVPN and internal CA trust.
* **[ClearPass (On-Prem to Azure)](./ClearPass%20(On-Prem%20to%20Azure)/)**: Extending enterprise NAC and identity enforcement into Azure.
* **[ClearPass Intune Integration](./ClearPass%20Intune%20Integration/)**: Device-based access control using Intune compliance.
* **[GPO to Intune Profiles](./GPO%20to%20Intune%20Profiles/)**: Migrating legacy Group Policy to cloud-native management.

### 🌐 Networking & Connectivity
* **[Hybrid Connectivity (S2S VPN Gateway)](./Hybrid%20Connectivity%20(S2S%20VPN%20Gateway)/)**: BGP-enabled site-to-site connectivity for hybrid routing.
* **[Azure Firewall (Hub-and-Spoke Governance)](./Azure%20Firewall%20(Hub-and-Spoke%20Governance)/)**: Centralised security for spoke VNets.
* **[Azure Connectivity (Load Balancing)](./Azure%20Connectivity%20(Load%20Balancing)/)**: Azure Load Balancer design and traffic distribution.
* **[Azure Application Gateway (L7 & Host-Based Routing)](./Azure%20Application%20Gateway%20(L7%20&%20Host-Based%20Routing)/)**: Layer 7 ingress control and TLS termination.

### ☁️ Platform & Infrastructure Modernisation
* **[Azure Deployment: Palo Alto Networks NGFW (VM-Series)](./Azure%20Deployment:%20Palo%20Alto%20Networks%20NGFW%20(VM-Series)/)**: Advanced NVA security enforcement in Azure.
* **[Azure Storage (DFS to Files)](./Azure%20Storage%20(DFS%20to%20Files)/)**: Modernising legacy DFS using Azure Files.
* **[Azure Private Endpoints](./Azure-Private-Endpoints/)**: Removing public exposure of PaaS services via Private Link.

---

## Architecture Principles
* **Hybrid-First Design**: Ensuring on-premises and cloud environments coexist seamlessly during migration.
* **Zero Trust Alignment**: Identity, certificates, and private connectivity are prioritised over perimeter trust.
* **Incremental Modernisation**: Moving services independently to avoid "big bang" failure points.
* **Operational Clarity**: Designs prioritising DNS, routing, and long-term supportability.

---

## Author
**Nick Fennell** *Cloud & Network Engineer* Specialising in Hybrid Connectivity, Security, and Azure Architecture

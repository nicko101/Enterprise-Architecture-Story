# Security Architecture: Hybrid Zero Trust Implementation

## Project Overview
This section details the security architecture for the NF-CloudLab environment. The implementation represents a transition from legacy perimeter-based security to a Zero Trust model where identity serves as the primary enforcement boundary. Access is never granted by default; it is verified through continuous authentication and device-health validation.

## Core Implementation Capabilities
The following capabilities have been successfully integrated and verified in the current production environment:

* **Identity Governance**: Bidirectional synchronization between on-premises Active Directory and Microsoft Entra ID.
* **PKI Infrastructure**: Automated certificate issuance via NDES/SCEP for passwordless authentication.
* **Network Access Control**: Implementation of EAP-TEAP for machine/user chaining, integrated with real-time Intune compliance polling.
* **Perimeter Security**: Palo Alto PA-VM deployment utilizing SSL Forward Proxy decryption and User-ID mapping.
* **Hybrid Connectivity**: BGP-routed IPsec site-to-site tunnels connecting local Proxmox infrastructure to Azure Virtual WAN.

## Technical Documentation Hierarchy
Documentation is organized by technical function to reflect a modular security posture:

1. **[Identity Governance](./identity-governance)**: Detailed configurations for NAC, certificate automation, and cloud-compliance integration.
2. **[Edge Security Perimeter](./edge-security-perimeter)**: Security policy governance, threat prevention profiles, and SSL inspection.
3. **[Secure Remote Access](./Secure%20Remote%20Access)**: GlobalProtect VPN configurations utilizing SAML 2.0 and Azure MFA.

## Operational Workflow
Authentication requests follow a specific sequence to ensure zero-trust compliance.

![Zero Trust Workflow](../resources/images/Zero-Trust-Workflow.png)
*Figure 1: Packet-level workflow detailing the handshake between ClearPass and the Microsoft Graph API.*

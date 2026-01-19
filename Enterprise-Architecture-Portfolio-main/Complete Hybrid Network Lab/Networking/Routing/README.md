# Routing: Hybrid Perimeter Governance

## Overview
This directory documents the configuration of the perimeter security and hybrid routing fabric. The edge is powered by a Palo Alto Networks PA-VM, which serves as the primary gateway for threat prevention, identity-based enforcement, and encrypted cloud connectivity.

[![BGP and Hybrid Routing Architecture](../../../resources/slides/bgp.png)](../../../resources/slides/bgp.png)
*Figure 1: Architectural view of the hybrid routing fabric and BGP peering sessions.*

---

## Core Technical Components

### Hybrid Connectivity (S2S VPN)
* **BGP Dynamic Routing**: Implementation of Border Gateway Protocol to automate route propagation between the local edge and Azure Virtual WAN.
* **IPsec Tunneling**: Secure transport utilizing IKEv2 and AES-256 encryption for cross-premises data flow.

### Threat Prevention and Security Profiles
* **Identity-Aware Policies**: Leverages User-ID data from Active Directory and ClearPass to apply rules based on user roles.
* **SSL/TLS Decryption**: Inspects encrypted traffic to prevent hidden threats while maintaining privacy for sensitive traffic categories.
* **Security Profiles**: Implementation of Antivirus, Anti-Spyware, and Vulnerability Protection to harden the attack surface.

### Secure Remote Access
* **GlobalProtect VPN**: Provides secure access for remote clients, utilizing SAML authentication for Single Sign-On (SSO) with Microsoft Azure.

---

## Directory Contents
* **configs/**: Technical exports and screenshots of Security Policies, NAT Rules, and Virtual Router settings.
* **VPN/**: Detailed Phase 1 (IKE) and Phase 2 (IPsec) parameters used for the Azure handshake.

---

## Engineering Standards
* **Zero-Trust Posture**: The security policy is built on an "Explicit Allow" model; all other traffic is dropped by default.
* **Dynamic Path Selection**: Utilization of BGP ensures high availability and automated route propagation within the hybrid fabric.
* **Identity-to-IP Mapping**: Ensures firewall logs show human-readable usernames instead of transient IP addresses, significantly improving audit trails.

---
[Return to Networking](../) | [Return to Lab Root](../../README.md)

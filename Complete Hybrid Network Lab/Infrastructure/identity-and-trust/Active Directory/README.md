# Active Directory: Hybrid Identity Source of Truth

## Overview
This directory documents the configuration of the on-premises identity core. Active Directory serves as the primary authority for user authentication, group policy enforcement, and internal name resolution, forming the foundation of the Zero Trust identity model.

---

## Domain Architecture and Trust

### Forest and Domain Structure
The environment is built on a modern functional level to support advanced security features such as Protected Users and Kerberos hardening.
* Domain Controllers: Redundant virtual machines hosted on Proxmox to ensure identity availability during maintenance cycles.
* DNS Integration: Integrated zones providing service location (SRV) records for site-aware authentication.

### Entra ID Synchronization
To bridge the gap between on-premises and cloud resources, identity data is synchronized to Microsoft Entra ID.
* Synchronization Engine: Implementation of Entra Connect utilizing Password Hash Synchronization (PHS) for seamless user transitions.
* Attribute Mapping: Filtering and mapping specific organizational units to minimize the cloud attack surface.

---

## Operational Evidence

[![Hybrid Active Directory and Sync Architecture](../../../resources/slides/hybrid-ad.png)](../../../resources/slides/hybrid-ad.png)
*Figure 1: Technical diagram of the Active Directory forest structure and the synchronization bridge to Microsoft Entra ID.*

---

## Engineering Standards

### Organizational Unit (OU) Design
The directory follows a standardized OU hierarchy designed for granular Group Policy application:
* Tiered Administration: Separation of administrative accounts from standard user accounts to prevent credential harvesting.
* Managed Endpoints: Dedicated containers for servers and workstations to facilitate automated certificate enrollment via SCEP.

### Group Policy Baselines
Security is enforced through central policy management:
* Authentication Hardening: Disabling legacy protocols (SMBv1, LLMNR) across the entire forest.
* Password Policy: Enforcement of complex passphrases and account lockout thresholds to mitigate brute-force attempts.

---
[Return to Infrastructure](../) | [Return to Lab Root](../../README.md)

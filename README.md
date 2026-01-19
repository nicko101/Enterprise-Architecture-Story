# Hybrid Zero Trust Architecture: Engineering & Architecture Portfolio

## Overview
This repository documents the **design, engineering, and governance** of a multi-vendor **Hybrid Zero Trust environment**.

It serves as a functional, production-aligned portfolio demonstrating how on-premises virtualization is integrated with Microsoft Azure to form a **unified, identity-centric security fabric**.

This is **not a proof of concept**.  
All components have been **designed, implemented, integrated, and validated** to reflect real-world enterprise constraints.

**Project Status:** Active engineering build. Documentation and technical artifacts are updated as integrations are validated.

---

## System Capabilities

[![Key Integration Framework](resources/slides/keyintgration.png)](resources/slides/keyintgration.png)  
*Figure 1: Full-stack integration across Identity, Trust, Access, Perimeter Security, and Hybrid Cloud connectivity.*

---

## Engineering Overview
This portfolio represents **hands-on engineering with architectural ownership**, not theoretical design.

I acted as both **solution architect and implementing engineer**, defining the target-state architecture while also building and integrating the underlying identity, networking, security, and trust components across on-premises and Azure environments.

The work spans the full lifecycle:

- **Architecture & Design** — Translating Zero Trust principles into a deployable hybrid architecture
- **Implementation** — Building identity services, PKI, networking, and security controls
- **Integration** — Multi-vendor integration across Microsoft, Palo Alto, and Aruba platforms
- **Validation** — Functional testing of routing, authentication, access enforcement, and inspection
- **Iteration** — Refinement based on operational behavior and failure scenarios

---

## Architecture Overview
The architecture is designed as a **layered Zero Trust system**, where **identity is the primary control plane** and network location provides no implicit trust.

The environment is composed of clearly separated architectural layers:

### Identity Plane
Hybrid identity using on-premises Active Directory synchronized with Microsoft Entra ID as the authoritative identity source.

### Trust Plane
Certificate-based trust delivered via a multi-tier PKI  
(Offline Root CA → Issuing CA → NDES), enabling EAP-TLS, device authentication, and secure service communication.

### Access & Enforcement Plane
Context-aware access enforcement using Aruba ClearPass and Microsoft Intune, combining identity, device posture, and compliance state.

### Connectivity Plane
Hybrid routing fabric connecting on-premises infrastructure to Azure using secure site-to-site IPsec tunnels.

### Security Perimeter Plane
Centralized traffic inspection, policy enforcement, and threat prevention delivered by a next-generation firewall.

This separation ensures **authentication, authorization, and enforcement are decoupled**, enabling scalable policy control and operational resilience.

---

## Core Engineering Pillars

### Hybrid Routing & Connectivity
- Site-to-site IPsec connectivity between on-premises and Azure
- Deterministic routing for hybrid transit
- Controlled route propagation across peered environments

### Unified Identity & Trust
- Hybrid identity with Entra ID
- Certificate lifecycle management using internal PKI
- Certificate-based authentication for devices and services

### Access Control & Security
- Zero Trust network admission using ClearPass and Intune
- Identity- and application-aware firewall enforcement
- SSL inspection and threat prevention at the edge

---

## Portfolio Navigation

| Domain | Description |
|------|------------|
| **Complete Hybrid Network Lab** | End-to-end integrated lab environment and validation |
| **Solutions Architecture** | Target-state architecture, design rationale, and structure |
| **Migration & Cloud Modernisation** | Controlled transition from legacy on-premises to hybrid cloud |
| **Security & Identity** | Identity governance, PKI, NAC, and enforcement controls |
| **Resources** | Diagrams, architectural slides, and supporting artifacts |

---

## Technical Standards & Validation
Functional success is demonstrated through **operational evidence**, not diagrams alone:

- **Traffic Observability** — Firewall and NAC telemetry
- **Access Enforcement** — Verified identity- and posture-based decisions
- **Operational Integrity** — Repeatable configuration patterns and tested failure modes

---

## Scope & Intent
This repository exists to:

- Demonstrate **real-world Hybrid Zero Trust architecture**
- Showcase **hands-on engineering with architectural accountability**
- Provide traceable evidence of **design decisions and implementation logic**
- Serve as a **living reference** as integrations evolve

---

**Engineering & Architecture Portfolio — Focused on Identity-Centric, Resilient Security Systems**

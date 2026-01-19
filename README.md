# Hybrid Zero Trust Architecture: Engineering & Architecture Portfolio

## Overview
This repository documents the **design, engineering, and governance** of a multi-vendor **Hybrid Zero Trust environment**.

It serves as a production-aligned engineering portfolio demonstrating how on-premises virtualization integrates with Microsoft Azure to form a **unified, identity-centric security fabric**.

This is **not a proof of concept**.  
All components have been **designed, implemented, integrated, and validated** to reflect real enterprise constraints, operational realities, and failure scenarios.

**Project Status:** Active engineering build. Documentation evolves as integrations are validated.

---

## System Capabilities (Architecture Overview)

[![Architecture Summary & Key Integrations](resources/slides/keyintgration.png)](resources/slides/keyintgration.png)  
*Figure 1: End-to-end integration across Identity, Trust, Access Enforcement, Security Perimeter, and Hybrid Cloud connectivity.*

---

## Engineering Overview
This portfolio represents **hands-on engineering with architectural accountability**, not theoretical design.

I acted as both **solution architect and implementing engineer**, defining the target-state architecture while also building and integrating the underlying identity, networking, security, and trust components across on-premises and Azure environments.

The engineering lifecycle covered includes:

- **Architecture & Design** — Translating Zero Trust principles into a deployable hybrid architecture
- **Implementation** — Identity services, PKI, routing, firewalling, and enforcement controls
- **Integration** — Multi-vendor alignment (Microsoft, Palo Alto, Aruba)
- **Validation** — Routing tests, authentication flows, policy enforcement, telemetry
- **Iteration** — Refinement based on operational behavior and failure scenarios

---

## Architecture Model
The environment is built as a **layered Zero Trust system**, where **identity is the primary control plane** and network location provides no implicit trust.

### Identity Plane
Hybrid identity using on-premises Active Directory synchronized with Microsoft Entra ID as the authoritative identity source.

### Trust Plane
Certificate-based trust delivered via a multi-tier PKI  
(Offline Root CA → Issuing CA → NDES), enabling EAP-TLS, device authentication, and secure service communication.

### Access & Enforcement Plane
Context-aware access enforcement using Aruba ClearPass and Microsoft Intune, combining identity, device posture, and compliance signals.

### Connectivity Plane
Hybrid routing fabric connecting on-premises infrastructure to Azure using secure site-to-site IPsec tunnels.

### Security Perimeter Plane
Centralized traffic inspection, policy enforcement, and threat prevention delivered by a next-generation firewall.

This separation ensures **authentication, authorization, and enforcement are decoupled**, enabling scalable policy control and operational resilience.

---

## Repository Structure (Start Here)

This repository is intentionally organised into **two primary engineering domains**, supported by a shared resources library.

### 1) Integrated Engineering Build (As-Built)
**[Complete Hybrid Network Lab](./Complete%20Hybrid%20Network%20Lab/)**  
The implementation layer.  
Contains the full end-to-end hybrid lab build, including infrastructure, networking, security controls, and validation evidence.

### 2) Architecture & Modernisation Playbook
**[Solutions_Architecture](./Solutions_Architecture/)**  
The design layer.  
Contains target-state architecture, solution blueprints, integration patterns, and migration / modernisation modules.

### Shared Assets
**[resources](./resources/)**  
Architectural diagrams, slides, screenshots, and supporting technical media used throughout the documentation.

```text
/
├── Complete Hybrid Network Lab/
├── Solutions_Architecture/
├── resources/
└── README.md

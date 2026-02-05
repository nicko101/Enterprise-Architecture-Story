# Azure Hybrid Network Architecture – Palo Alto NVA  
## Migration, Inspection, and Validated Split-Transit Design

## Overview
This project documents the architecture, delivery, and validation of a hybrid on-premises to Azure network migration using Palo Alto Networks NVAs as the primary inspection and enforcement layer.

The work originated from a real production requirement to migrate an on-premises site away from a legacy Azure VPN Gateway–centric model and toward a more scalable, inspection-driven architecture. The engagement captures both the **in-scope delivery under time constraints** and a **validated proof of concept (PoC)** addressing longer-term architectural limitations identified during implementation.

This repository is structured to reflect a Professional Services–style engagement, including scope definition, design intent, delivery outcomes, constraints, risks, and dependency boundaries.

---

## Project Objectives
- Migrate a new on-premises site to Palo Alto NVA–based connectivity
- Enable secure internet breakout via Azure
- Ensure inspection of on-premises ↔ Azure traffic for the new site
- Maintain operational stability under strict delivery timelines
- Identify and validate a scalable long-term routing architecture

---

## Delivered Outcome (In Scope)
The production delivery achieved:
- Direct on-premises to Palo Alto NVA VPN connectivity for the new site
- Centralised inspection of on-premises ↔ Azure traffic
- Internet breakout via the NVA without reliance on GatewaySubnet routing changes
- Minimal impact to legacy site connectivity

This delivery met all agreed success criteria within the defined scope.

---

## Architectural Findings
During implementation, several platform and design constraints were identified:
- Standalone NVA deployments behind an external load balancer support only a single active VPN tunnel
- Load balancer health probes introduce HA and failover limitations
- VPN Gateway–sourced traffic bypasses NVA inspection by default unless explicitly routed

These findings highlighted scalability and consistency risks in tunnel-anchored inspection models.

---

## Validated PoC – Split-Transit Routing Model
In response, a **dependency-aware split-transit routing model** was designed and validated via PoC.

Key characteristics:
- VPN termination anchored at the Azure VPN Gateway
- Gateway-level routing used to steer traffic to the Palo Alto NVA
- Removal of NVA VPN tunnels for Azure ↔ on-premises traffic
- Retention of NVA VPN tunnels exclusively for on-premises internet breakout
- Improved HA characteristics and routing symmetry

⚠️ **This model has been validated via PoC only and has not been implemented in production.**

---

## Scope and Ownership
- Azure-side architecture, routing design, and validation are documented here
- On-premises VPN changes and additional tunnels fall outside current ownership
- Any production rollout of the PoC architecture would require a formally scoped Professional Services engagement

---

## Repository Structure
This project is organised as a structured engagement lifecycle:

- **00 – Context and Scope**: Background and scope boundaries  
- **01 – Original Design Intent**: Initial architecture assumptions  
- **02 – In-Scope Delivery**: Production changes delivered  
- **03 – Constraints and Pivot**: Design adjustments under pressure  
- **04 – Constraints and Risks**: Identified architectural limitations  
- **05 – Validated PoC – Split Transit**: Tested future-state design  
- **06 – Dependencies and Ownership**: Responsibility boundaries  
- **07 – Reference and Blueprints**: External reference material  

---

## Status
- **Production Delivery**: Completed (in scope)
- **Split-Transit Architecture**: Designed and validated via PoC
- **Further Implementation**: Not undertaken

---

## Audience
This documentation is intended for:
- Azure networking and cloud engineers
- Professional Services consultants
- Solution and network architects
- Technical stakeholders reviewing hybrid cloud designs

# Validated PoC – Split-Transit Routing Model

## Objective
To validate a routing architecture that decouples VPN termination from traffic inspection while preserving security controls and improving HA characteristics.

## PoC Design
The validated model introduces:
- A new on-premises → Azure VPN Gateway tunnel
- Gateway-level routing to steer traffic toward the Palo Alto NVA
- Elimination of NVA VPN tunnels for Azure ↔ on-premises traffic
- Retention of NVA VPN tunnels solely for on-premises internet breakout

## Validation Outcome
The PoC confirmed:
- Symmetric routing behaviour
- Removal of load balancer tunnel constraints
- Centralised inspection of hybrid traffic flows

## Status
This model was **designed and validated via PoC only**.  
It has **not been implemented in the production environment**.

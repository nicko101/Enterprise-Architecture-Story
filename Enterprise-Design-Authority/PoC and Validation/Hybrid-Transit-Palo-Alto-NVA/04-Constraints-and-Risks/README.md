# Constraints and Risks

## Architectural Limitations
The implemented delivery model highlighted several inherent limitations:
- Tunnel-anchored inspection reduces flexibility and scalability
- Load balancer–based HA does not provide symmetric failover for VPN termination
- Gateway-sourced traffic from legacy sites bypasses centralised inspection by default

## Risk Summary
- Inconsistent inspection posture across sites
- Increased operational complexity as additional sites are onboarded
- Tight coupling between VPN termination and inspection paths

These risks informed the subsequent design and validation of an alternative routing model.

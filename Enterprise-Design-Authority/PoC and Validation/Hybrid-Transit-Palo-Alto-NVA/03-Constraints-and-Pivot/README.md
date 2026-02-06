# Constraints and Design Pivot

## Identified Constraints
During implementation, the following platform constraints became evident:
- Standalone Palo Alto NVA deployment behind an external load balancer
- Only a single active VPN tunnel could be terminated at any given time
- Traffic steering relied on load balancer health probes

## Impact
These constraints limited true active/active behaviour and introduced operational fragility under failure conditions.

## Design Pivot
To meet delivery timelines and maintain stability, the design pivoted away from Gateway-anchored routing toward a direct NVA VPN model for the new site.

This pivot was tactical and intentionally limited to in-scope requirements.

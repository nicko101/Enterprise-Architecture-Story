# Original Design Intent

## Intended Architecture
The initial design intent was to onboard the new on-premises site using the existing Azure VPN Gateway, maintaining architectural consistency with the legacy site.

Traffic from the new site would:
- Terminate on the Azure VPN Gateway
- Be routed onward to spoke VNets
- Require custom routing to ensure inspection via the Palo Alto NVA

## Design Assumptions
- GatewaySubnet User Defined Routes (UDRs) would be required to steer traffic to the NVA
- The VPN Gateway would remain the primary transit point for hybrid connectivity

This design represented the most conservative extension of the existing architecture prior to implementation.

# Context and Scope

## Background
This engagement originated from a requirement to migrate an on-premises site to a Palo Alto NVA–based inspection model within Azure. The legacy environment utilised an Azure VPN Gateway for site-to-site connectivity, with traffic transiting directly to spoke VNets.

## Original Scope
The agreed scope for this work was limited to:
- Migrating the new on-premises site to Palo Alto NVA–based connectivity
- Enabling internet breakout via the NVA
- Ensuring on-premises to Azure traffic inspection for the new site

## Out of Scope
The following items were explicitly out of scope:
- Re-architecting legacy site connectivity
- Modifying existing VPN Gateway routing for other sites
- Changes to on-premises VPN configurations outside the new site

This directory establishes the baseline context against which all subsequent design decisions and validations were made.

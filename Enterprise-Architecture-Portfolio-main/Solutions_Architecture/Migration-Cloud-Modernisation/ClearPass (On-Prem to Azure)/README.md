🛡️ Case Study: Hybrid Auth Bridge — On-Prem Aruba AOS-S to Azure ClearPass
📖 Executive Summary
This project documents the architectural design and deployment of an enterprise Network Access Control (NAC) bridge. It enables physical on-premises Aruba AOS-S switches to authenticate local users against a cloud-hosted Aruba ClearPass Policy Manager (CPPM) v6.11.1 appliance. By leveraging a BGP-enabled Site-to-Site VPN, the solution ensures resilient, encrypted, and low-latency authentication paths across the hybrid fabric.

🏗️ Technical Architecture: The "Auth-Path"
The core of this solution is the established "Auth-Path" that allows RADIUS and TACACS+ packets to transit from the local edge to the Azure Hub.

🛠️ Core Infrastructure (Verified by IaC)
The NAC Engine (nf-cppm): An official Aruba ClearPass virtual appliance (6.11.1) deployed as a Marketplace resource. It is assigned a static internal IP of 172.16.0.4, acting as the primary RADIUS target for the local network.

Dynamic Routing (BGP): To ensure high availability of authentication services, the IPsec VPN utilizes BGP for dynamic route propagation:

Azure Hub ASN: 65515 (Virtual Network Gateway: ng-vpngw).

On-Prem Edge ASN: 65010 (Local Network Gateway: nf-lng).

Local Network Entry: The on-premises tunnel endpoint is located at 37.228.231.33, facilitating the secure entry of authentication traffic from local switch hardware.

🛰️ Solution Logic: Aruba AOS-S Switch Authentication
The deployment specifically addresses the challenge of moving identity services to Azure without sacrificing the security of the local physical port.

Switch Configuration: Physical AOS-S switches are configured with the ClearPass internal IP (172.16.0.4) as the RADIUS/TACACS+ target.

BGP Adjacency: The BGP session ensures that if a primary network link fails, authentication traffic is dynamically re-routed to maintain access control integrity.

Encapsulated Auth: Every user credential and authentication attribute is encrypted within the IPsec tunnel, ensuring Zero Trust principles are maintained between the on-premises site and the Azure cloud.

Policy Enforcement: ClearPass processes requests based on local Active Directory or internal databases, returning dynamic VLAN assignments and port-security commands back to the physical switch.

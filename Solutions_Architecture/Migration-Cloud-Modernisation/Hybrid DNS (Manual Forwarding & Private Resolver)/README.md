Azure Network Architecture Briefing
Executive Summary
This document provides a comprehensive analysis of an Azure cloud infrastructure deployment, as defined by an Azure Resource Manager (ARM) template. The architecture implements a sophisticated and complex network topology centered around a hybrid hub-and-spoke model. It combines a traditional VNet hub, complete with a dedicated VPN Gateway for Site-to-Site connectivity, with a modern Azure Virtual WAN (vWAN) and Virtual Hub to facilitate Point-to-Site access and scalable VNet integration.
Key components of this deployment include multiple Virtual Networks (VNets) for hub, spoke, and private endpoint services, all interconnected via VNet peering. Hybrid connectivity to an on-premises location is established through an IPsec/IKEv2 VPN tunnel. Remote user access is enabled via a Point-to-Site (P2S) OpenVPN gateway configured on the Virtual Hub. The environment hosts a Linux virtual machine, a storage account secured with a Private Endpoint, and an Azure Bastion host for secure administrative access. A notable feature is the advanced DNS configuration, which leverages a DNS Private Resolver with forwarding rules to manage name resolution for private endpoints.
A critical security finding is the configuration of the primary Network Security Group (NSG). Its rules are excessively permissive, allowing all inbound and outbound traffic from any source to any destination. This configuration presents a significant security vulnerability by failing to enforce the principle of least privilege, a foundational element of secure network design.
Core Network Topology
The foundation of the infrastructure is a dual-approach hub-and-spoke network design, utilizing both VNet Peering and a Virtual WAN hub.
Hub-and-Spoke Virtual Network Architecture
Three distinct Virtual Networks (VNets) are defined to segment the environment, all located in the westeurope region.
VNet Name
Address Space
Primary Role and Key Subnets
nf-hub-vnet
172.16.0.0/16
Central Hub: Hosts shared services. Contains GatewaySubnet (172.16.1.0/27) for the VPN Gateway and two dedicated subnets for the DNS Resolver.
nf-vnet-spoke1
172.17.0.0/16
Workload Spoke: Hosts the testvm virtual machine. Contains a default subnet (172.17.0.0/24) and an AzureBastionSubnet (172.17.1.0/26).
nf-endpoint-vnet
172.19.0.0/16
Services Spoke: Dedicated to hosting private endpoints, specifically for the nfstore1 storage account within its default subnet (172.19.0.0/24).
VNet Peering Configuration
The VNets are interconnected via VNet peering to enable direct communication:
• Hub-to-Spoke Peering: The nf-hub-vnet is peered with both nf-vnet-spoke1 and nf-endpoint-vnet.
    ◦ The hub-side peering connections (hub-spoke, endpoint-peer) are configured with allowGatewayTransit set to true, enabling the spoke VNets to use the hub's VPN gateway for on-premises connectivity.
• Spoke-to-Hub Peering: The spoke-side peering connections (spoke-hub, hub-endpoint) have useRemoteGateways set to true, directing traffic destined for on-premises networks through the hub's gateway.
Virtual WAN Implementation
In addition to the VNet hub, a Standard SKU Azure Virtual WAN (nf-vwan) is deployed.
• Virtual Hub (nf-vhub1): A Virtual Hub is provisioned within the vWAN, also using the Standard SKU. It is configured with an address prefix of 172.17.0.0/24 and is the central point for P2S VPN connectivity and VNet connections within the vWAN fabric.
• VNet Connections: The hub connects to nf-vnet1 and spoke1 (referenced via external resource IDs), integrating them into the vWAN topology. The connection to spoke1 includes a static route for 0.0.0.0/0 pointing to 172.17.0.4, enabling internet-bound traffic control.
Hybrid and Remote Connectivity
The architecture provides robust connectivity options for both on-premises sites and remote users.
Site-to-Site (S2S) VPN Connection
A standard IPsec VPN tunnel connects the Azure environment to an on-premises location.
• Azure VPN Gateway: A VpnGw2AZ SKU Virtual Network Gateway (nf-vpngw) is deployed in the GatewaySubnet of the nf-hub-vnet. It is associated with the public IP address 51.138.69.160.
• Local Network Gateway: An on-premises site is represented by a Local Network Gateway (nf-LGW) with a public gateway IP of 37.228.233.24. It defines the on-premises address spaces as 192.168.0.0/24 and 10.0.0.0/8.
• Connection: An IPsec connection resource (onprem-az) using the IKEv2 protocol formally establishes the tunnel between the Azure VPN Gateway and the Local Network Gateway.
Point-to-Site (P2S) VPN Configuration
Remote user access is handled by a P2S VPN Gateway deployed as part of the Virtual Hub.
• P2S VPN Gateway: A gateway with a scale unit of 1 is attached to nf-vhub1.
• Server Configurations: Two VPN server configurations are defined, supporting both OpenVPN (named P2S) and IkeV2 (named ike2). Both configurations rely on certificate-based authentication, with root CA public certificate data embedded in the template. The deployed P2S Gateway utilizes the P2S (OpenVPN) configuration.
• Client Address Pool: Remote clients connecting via P2S VPN are assigned IP addresses from the 10.110.0.0/24 address pool.
• Custom DNS: The P2S configuration pushes a custom DNS server (192.168.0.240) to connecting clients.
DNS Resolution Architecture
A DNS Private Resolver is deployed to facilitate name resolution across the hybrid environment, particularly for private endpoints.
• DNS Private Resolver (nf-dns): Deployed within the nf-hub-vnet, this service provides centralized DNS capabilities.
    ◦ Inbound Endpoint: Located in the dns-resolve subnet (172.16.3.0/24) with a static private IP of 172.16.3.4. This endpoint listens for DNS queries from on-premises and other VNets.
    ◦ Outbound Endpoint: Located in the dns-resolve-out subnet (172.16.2.0/24), used for forwarding DNS queries from Azure to external resolvers (e.g., on-premises DNS servers).
• Private DNS Zone: A Private DNS Zone for privatelink.file.core.windows.net is created to handle resolution for the storage account's private endpoint. It is linked to both nf-hub-vnet and nf-vnet-spoke1, allowing resources in those networks to resolve records within the zone. An A record for nfstore1 points to the private endpoint IP 172.19.0.4.
• Forwarding Rulesets: Two rulesets (dns-rules and on-prem-azure) are configured. Both contain a rule that forwards any query for the domain privatelink.file.core.windows.net. to the DNS Private Resolver's inbound endpoint IP (172.16.3.4).
Deployed Azure Resources and Services
The infrastructure supports various workloads and management services.
• Compute: A Standard_D2s_v3 virtual machine named testvm is deployed in the nf-vnet-spoke1. It runs the Canonical ubuntu-24_04-lts image and is configured with a 30 GB OS disk.
• Storage: A Standard_LRS General Purpose V2 storage account (nfstore1) is provisioned.
    ◦ Private Endpoint: Access to the storage account's file service is secured via a private endpoint (nfstore1). This endpoint is located in the nf-endpoint-vnet and is assigned the private IP 172.19.0.4. Its DNS record is managed by the privatelink.file.core.windows.net private zone.
• Secure Management: An Azure Bastion host (nf-vnet-spoke1-bastion) with a Basic SKU is deployed into the dedicated AzureBastionSubnet. It is associated with the public IP address 65.52.139.98 and enables secure RDP/SSH access to the testvm without exposing any ports on the VM to the public internet.
Security Configuration Analysis
The security posture of the deployed environment is a critical consideration.
Network Security Group (NSG)
A single Network Security Group (nf-nsg) is defined and associated with the network interface of the testvm. Its security rules represent a significant security risk.
Rule Name
Priority
Direction
Protocol
Source
Destination
Port
Action
AllowAnyCustomAnyInbound
100
Inbound
Any (*)
Any (*)
Any (*)
Any
Allow
AllowAnyCustomAnyOutbound
100
Outbound
Any (*)
Any (*)
Any (*)
Any
Allow
Analysis: This configuration allows all traffic from any source to any destination, both inbound and outbound. It effectively disables network-level traffic filtering for the associated resource, violating the principle of least privilege. This permissive setup should be replaced with specific rules that only allow necessary traffic for the workload.
Public IP Addresses
The deployment provisions two static, Standard SKU public IP addresses:
• 51.138.69.160: Assigned to the nf-vpngw Virtual Network Gateway for the Site-to-Site VPN connection.
• 65.52.139.98: Assigned to the nf-vnet-spoke1-bastion Bastion Host for secure administrative access.

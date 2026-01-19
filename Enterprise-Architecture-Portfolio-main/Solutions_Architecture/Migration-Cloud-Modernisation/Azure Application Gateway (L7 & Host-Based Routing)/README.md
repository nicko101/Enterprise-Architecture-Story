# Migration - Azure Application Gateway (L7 & Host-Based Routing)

## 🎯 Project Objective
The objective of this module is to architect a centralized ingress point for web traffic using a **Standard_v2 Azure Application Gateway**. This configuration demonstrates advanced **Layer 7 load balancing** capabilities, allowing a single gateway to route traffic to multiple backend services based on the incoming HTTP host header.

## 🖼️ Architecture: The L7 Edge
This solution is deployed within a **Hub-and-Spoke** network topology, providing a governed entry point for external traffic before it reaches the backend application spokes.

* [cite_start]**Host-Based Routing**: Traffic is programmatically directed to specific backend pools based on whether the request is for `vm1.nginx.local` or `vm2.lab01.local`[cite: 33].
* [cite_start]**Network Integration**: The gateway is deployed into a dedicated subnet (`10.14.0.0/24`) with peering to a central hub (`172.16.0.0/16`)[cite: 33].
* [cite_start]**Hybrid Reachability**: The architecture includes a **VPN Gateway** (`nf-vpngw`) to allow for secure management and connectivity back to on-premises environments[cite: 33].
* [cite_start]**Enterprise Scalability**: Configured with **Autoscale** (1-10 units) to handle variable enterprise workloads[cite: 33].

## 📊 Routing Logic Summary
| Listener | Hostname | Backend Target | Priority |
| :--- | :--- | :--- | :--- |
| `nf-list1` | `vm1.nginx.local` | `172.16.0.4` | 1 |
| `list2` | `vm2.lab01.local` | `172.16.0.5` | 2 |

## 🛠️ Implementation Details (ARM)
The included deployment template (`applicationGateways_nf_appgw.json`) automates the following:
* [cite_start]**Public IP**: Standard SKU static IP with Zone Redundancy[cite: 33].
* [cite_start]**NSG Security**: Lockdown of backend subnets with specific SSH/HTTP allow rules[cite: 33].
* [cite_start]**Resource Governance**: Tagged with `Creator: Nick Fennell` and `DateCreated: 06/20/2025` for enterprise lifecycle management[cite: 33].

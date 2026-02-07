# 📂 Packet Life: Forensic Timeline & Validation

### 🏛️ Project Purpose
This repository is the **forensic companion** to the Enterprise Portfolio. It documents the 'Packet Life Walk'—the binary proof of the 'Identity-to-Cloud' data plane, validating every state change a packet undergoes from the edge to the NVA.

### 🗺️ Forensic Timeline (The Walk)

#### **Step 1: Identity Admission**
* **Location**: /Stage-1-Edge-Identity
* **Validation**: Implementation of **TEAP (EAP-TLS)** via Aruba ClearPass for chained Machine + User authentication.

#### **Step 2: L3 Mapping & Provisioning**
* **Location**: /Stage-2-IP-Allocation
* **Validation**: Forensic matching of MAC \c:24:11:fb:cb:c4\ to the **10.0.11.17** DHCP lease on the Palo Alto PA-VM.

#### **Step 3: Tunnel Steering (PBF)**
* **Location**: /Stage-3-Data-Plane-Steering
* **Validation**: Proves traffic for \8.8.8.8\ was explicitly steered into **tunnel.200** for encrypted egress.

#### **Step 4: Cloud Ingress Forensics**
* **Location**: /Stage-4-Cloud-Ingress
* **Artifact**: \save.pcap\.
* **Validation**: Raw packet analysis proving arrival at the Azure NVA after decapsulation.

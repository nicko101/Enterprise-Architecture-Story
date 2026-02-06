# 📱 Server Role: Network Device Enrollment Service (NDES)

## 📖 Overview
The critical gateway between Microsoft Intune and the on-premises PKI. This server utilizes the SCEP protocol to automate the distribution of certificates to mobile devices and workstations managed in the cloud.

## 🛠️ Configuration & Role
* **Primary Integration**: Managed via the **ndes-proxy** App Registration in Entra ID.
* **Intune Certificate Connector**: Hosts the active connector that facilitates real-time communication between Microsoft Intune and the local Certificate Authority.
* **NDES Service**: Runs the Network Device Enrollment Service role to handle incoming SCEP requests.
* **Protocol**: Simple Certificate Enrollment Protocol (SCEP).
* **Network Flow**: Receives enrollment requests from Intune-managed devices and proxies them to the Issuing CA for automated fulfillment.

## 🖼️ Operational Evidence
![Intune Certificate Connector Status](./evidence/intune-connector-status.png)
*Figure 1: Validation of the Intune Certificate Connector and NDES services in a 'Running' state on the NDES-Server.*

---

## 🛡️ Security Posture
1. **Least Privilege**: The NDES service account is granted only 'Enroll' permissions on the specific SCEP certificate template.
2. **Proxy Security**: Traffic is secured via the Entra ID Application Proxy (ndes-proxy), ensuring the internal server is never directly exposed to the internet.
3. **Chain of Trust**: All certificates issued are signed by the Issuing CA and anchored to the Offline Root CA.

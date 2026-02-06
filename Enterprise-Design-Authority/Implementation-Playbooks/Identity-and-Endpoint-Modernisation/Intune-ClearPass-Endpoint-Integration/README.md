# ClearPass Intune Integration: Cloud-Native Authentication Bridge

### **End-to-End Demo**
This playbook documents a production-verified transition from legacy on-premises authentication to a **Cloud-First Trust Model**. It demonstrates how a physical network (RADIUS) can consume real-time device health telemetry from **Microsoft Intune** to make dynamic access decisions.

### **Function**
* **Cloud-Auth Bridge**: Replaces the dependency on on-premises Domain Controllers by using the **ClearPass Intune Extension**.
* **Identity Correlation**: Correlates the **Certificate Common Name (CN)**containing the Intune Device IDwith real-time API telemetry.
* **Dynamic Enforcement**: Performs an API lookup to Intune; "Compliant" devices are authorized for secure production access, while "Non-Compliant" devices are restricted.

### **Execution Highlights**
* **SCEP Profile Design**: Configuration of the certificate payload to include the unique **Device Serial/ID** in the CN field.
* **Extension Logic**: Setup of the ClearPass API client to query the Intune \isCompliant\ and \EAS_ID\ attributes.
* **RADIUS Service**: A specialized service template for certificate-based authentication with **Intune Authorization**.

### **Dependencies**
* **Relies Upon**: **\NDES-Outbound-Migration\** for secure certificate issuance.
* **The Next Element**: Connects to **\Integration - Aruba Switching\** for final port/VLAN assignment based on compliance.

---

> [!IMPORTANT]
> **Live Engineering Evidence**: For Access Tracker logs and real-time attribute verification, see the [NDES-SCEP-Publishing-Pattern PoC](../../../PoC%20and%20Validation/NDES-SCEP-Publishing-Pattern).

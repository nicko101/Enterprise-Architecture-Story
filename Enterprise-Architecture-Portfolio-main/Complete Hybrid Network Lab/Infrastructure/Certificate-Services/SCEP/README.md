# 📜 SCEP (Simple Certificate Enrollment Protocol)

## Purpose

This directory documents the **automated certificate enrollment workflow** used
to provision device certificates for Intune-managed endpoints.

SCEP enables **zero-touch device identity** and is a key enabler of Zero Trust
network access.

---

## Architectural Role

Within this lab, SCEP provides:

- Automated machine certificate enrollment via Intune  
- Secure authentication of devices using EAP-TLS  
- Certificate lifecycle management without manual intervention  

---

## Enrollment Flow

1. Device enrolls into Microsoft Intune  
2. Intune requests a certificate via NDES  
3. NDES validates the request against Active Directory  
4. Certificate is issued by the Issuing CA  
5. Device uses the certificate for network authentication  

---

## Integration Points

- **Microsoft Intune** — Policy-driven certificate deployment  
- **NDES** — Enrollment proxy and authentication bridge  
- **Active Directory Certificate Services** — Certificate issuance  
- **ClearPass** — Device authentication and posture validation  

---

## Scope & Design Notes

- Focused on device (machine) certificates  
- Certificate renewal handled automatically  
- Authentication relies on existing AD trust  

---

## Status

✔ SCEP enrollment operational  
✔ Device certificates successfully issued  
✔ Validated via EAP-TLS authentication

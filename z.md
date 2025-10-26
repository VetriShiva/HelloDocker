# Urgent: AURA Card Not Visible – Duplicate Card Creation in Production

**To:** Michael Sefrin, Arivazhagan Meiyazhagan  
**Cc:** Ferhat Bulut, Ahmed Saleh Ghreban Abdlrahman, Wasim Hammouda, Naji Chemaly, IT Platform Integrations  
**From:** Vetri (MAS Solution Architect)  
**Date:** 26-Oct-2025  

---

### Context
A **production issue** has been identified where the customer is **unable to view their AURA card**.  
This leads to **duplicate card creation**, impacting live users. Immediate attention is required.

---

### API Reference
**Endpoint:**  
`https://apis.nbk.kuwait/gateway/CreditCards/1.0/CreditCardDetails`

**Sample Request:**
```json
{
  "header": {
    "country": "KW",
    "branchId": "14170",
    "sourceSystemId": "DvpApp",
    "userId": "APPACPVOP",
    "clientIp": "94.129.181.7",
    "deviceId": "4fc80564a552487496063016cc680e154"
  },
  "getCreditCardRequestBody": {
    "inquireMode": "list"
  }
}
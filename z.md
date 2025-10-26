# Subject: Urgent: AURA Card Not Visible – Duplicate Card Creation in Production

**To:** Michael Sefrin, Arivazhagan Meiyazhagan  
**Cc:** Esmail Bullo, Ahmed Selim, Wadah Hammoud, Naji Chemaly, IT Platform-Integrations  
**From:** Vetri  

---

Dear Michael & Arivazhagan,

Kindly check on the below **production issue** — the customer is unable to view their **AURA card**, which is leading to **duplicate card creation**.  
This is impacting live users and requires **immediate attention**.

### API
`https://apis.nbk.kuwait/gateway/CreditCards/1.0/CreditCardDetails`

### Sample Request
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
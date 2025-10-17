# RCA – Design Gap Between MAS and ESB on Online vs Offline Mode for Aura Card

**Date:** 2025-10-17  
**Prepared by:** Vetri – Weyay Digital Banking Team  
**Category:** Design Gap / Integration Behavior  
**Status:** Open – Pending ESB Alignment  

---

## 🧩 Summary

During the RCA for Aura card inquiry and 3DS pending authorization discrepancies, a **design gap** was identified between **MAS** (Mobile Application Server) and **ESB** (Enterprise Service Bus) regarding **online and offline mode handling**.

Currently:
- MAS implements **online mode** as per the original ESB design.
- **Offline mode** fallback exists on MAS side, but **balance inquiry is not yet active** from ESB side.
- This causes inconsistent behavior when online services are unavailable.

---

## 🔍 Background

| Phase | Description | Status |
|-------|--------------|---------|
| **PR1** | MAS implemented **online mode** only. | ✅ Completed |
| **PR2** | MAS implemented **fallback to offline mode** (based on ESB suggestion). | ⚠️ Implemented but no balance data returned |
| **Observation** | ESB’s offline mode query currently doesn’t provide balance details. | ❌ Not available |

Offline mode was expected to behave similar to digital card flow, where T-1 balance is available within the same day.  
However, in the current flow, **no balance data is returned** when online inquiry fails, leading to inconsistent user experience.

---

## ⚙️ Observed Behavior

- When the **online inquiry fails**, MAS triggers fallback to **offline mode**, but receives **no balance response**.  
- The **mobile app** then displays incomplete or empty data to the customer.  
- Example scenario:
  - During **3DS pending authorizations**, Aura inquiry fails when offline mode is triggered.
  - **TEMS** implemented missing logic recently, but offline balance remains unavailable.
- Some customers temporarily appear to hold **two Aura cards** due to duplicate inquiries under high load.

---

## 📈 Impact

| Impact Area | Description |
|--------------|-------------|
| **Customer Experience** | Aura card balance not shown or inconsistent under fallback condition. |
| **System Load** | Duplicate inquiry calls increase load on MAS and ESB under stress. |
| **Regression Risk** | Offline path differs from digital card flow, reducing test coverage accuracy. |

---

## ✅ Recommendations / Next Steps

1. **ESB to confirm** the plan or timeline for enabling offline balance availability.
2. **Joint validation** between MAS, ESB, and UAT teams to ensure data consistency across both modes.
3. **Regression coverage** to include online ↔ offline switch scenarios under simulated load.
4. **MAS** to align fallback logic once ESB confirms offline mode readiness.

---

## 🤝 Collaboration Notes

- UAT team to assist with regression testing of fallback and 3DS authorization edge cases.
- Once ESB confirms offline mode response readiness, MAS will finalize production fallback design.
- Future design should ensure **same-day availability** of offline balance (like digital card behavior).

---

## 📎 References

- [PR1 – Online Mode Implementation](https://dev.azure.com/mcs.nbkbcloud/Weyay/_git/weyay-server/pullrequest/68157)
- [PR2 – Offline Mode Fallback Logic](https://dev.azure.com/mcs.nbkbcloud/Weyay/_git/weyay-server/pullrequest/71441)
- Related Incidents:  
  `INC879086`, `INC879105`, `INC879403`, `INC880489`, `INC879038`

---

**Prepared by:**  
**Vetri**  
_Weyay Digital Banking Team_
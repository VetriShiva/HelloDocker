# Subject: MOM – AURA Card Inquiry Flow and System Optimization Discussion

**Date:** 26-Oct-2025  
**Attendees:** MAS Team, ESB Team, Business Representatives  
**Prepared by:** Vetri (MAS Solution Architect)

---

## 📝 Discussion Summary

We discussed the AURA Card inquiry flow and identified action points to address the design gap between **online (VMX)** and **offline (ESB)** modes while ensuring business requirements for **live balance visibility** and **duplicate card prevention** are met.

---

## ✅ Agreed Action Points

1. **Offline Inquiry Disabled in MAS**
   - Offline mode inquiry has been **removed from MAS** to align with business expectations for **live balance visibility**.
   - MAS will continue to use **online mode (VMX)**.
   - In case of failures, the system should handle gracefully without allowing **duplicate card creation**.

2. **Home Screen Navigation from Cache**
   - Subsequent navigations between screens on the home page should retrieve card details from **cache** instead of repeated backend calls.

3. **Cache Eviction Policy**
   - Introduce an **eviction TTL of 7 minutes** (aligned with mobile session timeout).
   - Evict cache entries immediately after any **payment transaction** within the mobile app.

4. **Error Handling and Validation**
   - If the inquiry call fails, MAS should **throw a controlled error back to the mobile app**.
   - In this case, **card creation should be blocked** to prevent duplicates until the next valid inquiry is successful.

---

## 🧭 Next Steps

- MAS to finalize implementation approach and share ETA.  
- ESB to validate handling capacity for peak login scenarios.  
- Business to confirm final behavior for offline fallback if VMX is unavailable.

---

**Best regards,**  
**Vetri**  
_MAS Solution Architect_
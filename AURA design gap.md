# Design Gap Observation – AURA Card Inquiry (Online vs Offline Mode)

**To:** ESB / Integration Team  
**From:** Vetri  
**Date:** 21-Oct-2025  
**Subject:** Design Gap Observation – AURA Card Inquiry (Online vs Offline Mode)

---

## Summary

As per my understanding, this appears to be a **design gap from ESB** related to the AURA card inquiry process.

---

## Implementation Details

- **PR 1:** Implemented **only online mode**  
  🔗 [View PR 1](https://dev.azure.com.mcas.ms/nbkcloud/Weyay/_git/dvp-server/pullrequest/6815)

- **PR 2:** Implemented **fallback mechanism (online → offline)** based on ESB’s suggestion  
  🔗 [View PR 2](https://dev.azure.com.mcas.ms/nbkcloud/Weyay/_git/dvp-server/pullrequest/7944)

---

## Identified Design Gap

- The **offline mode** does not return balance information, as mentioned in ESB’s previous communication.  
- Later, ESB indicated that an **offline query** would be implemented, but this is still pending.  
- As per the design, **digital card details should be available on the same day**, which is not currently happening.

---

## Current Impact

- **AURA inquiry fails in offline mode** due to missing offline implementation.  
- This feature was not implemented in the initial release and remains pending.  
- Consequently, **balance information is unavailable** even for recently onboarded users.

---

## Next Steps

Kindly review and advise on the next steps from ESB’s end to ensure consistent behavior across both **online and offline modes**.

---

**Regards,**  
**Vetri**
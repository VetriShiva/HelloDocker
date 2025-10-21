# **Subject:** High Volume of Unnecessary MAS Calls During Logout and Session Timeout Scenarios

Dear Team,

We have observed that while the **home screen login flow is performing as expected**, the **mobile client is triggering multiple asynchronous MAS API calls during logout, session timeout, and forced logout scenarios**.

These additional calls are **not required** for proper logout handling and are introducing **unnecessary load on MAS**, especially during peak usage hours.  
This is a **serious concern**, as MAS is concurrently handling high-priority **payment and transactional requests**.  
Such redundant calls can lead to queue congestion, delayed responses, and degraded customer experience.

We kindly request the mobile team to **review and optimize the logout and session handling flows** to ensure:
- Only a single, necessary logout request is sent to MAS.  
- No background or redundant async calls are executed post logout or session expiry.  
- Forced logout scenarios gracefully clear local sessions without backend invocation where possible.

Our team is available to assist in reviewing impacted endpoints and recommend prioritization or rate-limiting strategies to maintain MAS stability.

---

## 🔍 **Technical Observations**

| **Metric** | **Observation** | **Impact** |
|-------------|------------------|-------------|
| **403 Error Count** | Certain devices show 5–10× higher 403 counts during logout/session timeout | Indicates repeated invalid or expired token calls |
| **API Paths Involved** | `/authorization/v4/logout/force/webview`, `/authorization/v4/login/biome`, `/authorization/v4/login/password` | Redundant invocation patterns |
| **Pattern Identified** | Multiple async MAS calls triggered in parallel during logout or session expiry | MAS thread utilization spike during peak hours |
| **Platform Impact** | Observed on both iOS and Android clients | Client-side implementation behavior, not platform-specific |
| **Risk** | Queue congestion leading to delay in **payment API** execution | Potential transaction delay and degraded user experience |

---

## ✅ **Next Steps / Recommendations**

1. Optimize or suppress redundant MAS calls during logout and timeout flows.  
2. Introduce a debounce or retry guard in the client SDK for 403 handling.  
3. Review MAS API priority configuration to deprioritize non-critical calls.  
4. Validate post-fix behavior in UAT under peak-load simulation.  

---

**Best regards,**  
**Vetri**  
*Solution Architect – MAS Integration*
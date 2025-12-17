### Root Cause – Aura Card expDate Issue

Root cause identified as a data-related issue.  
Both customers are on the same app version, and the cards were created on the same date.  
However, Customer A (CMNO: 117409456) has expDate = NULL, whereas Customer B has a valid expDate.

Due to the missing expDate and timestamp difference, the application interpreted the case as no active card,
which resulted in duplicate card creation.

Kindly double-check our findings from your side and advise if you observe any discrepancy.


```
Subject: RE: Weyay Aura Multiple – PP Active Cards – As of 08 Dec 2025
Dear Suresh Reddy Katakam,
Thanks for the details.
Root Cause – Aura Card expDate Issue
The root cause has been identified as a data-related issue.
Both customers are on the same app version, and the cards were created on the same date.
However, Customer A (CMNO: 117409456) has expDate = NULL, whereas Customer B (CMNO: 117590169) has a valid expDate.
Due to the missing expDate and timestamp difference, the application interpreted the scenario as no active card, which resulted in duplicate card creation.
Kindly double-check our findings from your side and let us know if you observe any discrepancy.
Below are the relevant extracts from our production logs for reference:
Customer A (CMNO: 117409456)
Timestamp: 2025-12-15 22:52:21.398
expDate: NULL
issDate: 2025-12-15T00:00:00+0300
Customer B (CMNO: 117590169)
Timestamp: 2025-12-15T11:03:32.521+0300
expDate: 2030-12-31T00:00:00+0300
issDate: 2025-12-15T00:00:00+0300
Regards,
[Your Name]
```
### Root Cause – Aura Card expDate Issue

Root cause identified as a data-related issue.  
Both customers are on the same app version, and the cards were created on the same date.  
However, Customer A (CMNO: 117409456) has expDate = NULL, whereas Customer B has a valid expDate.

Due to the missing expDate and timestamp difference, the application interpreted the case as no active card,
which resulted in duplicate card creation.

Kindly double-check our findings from your side and advise if you observe any discrepancy.
Here’s a leadership-style summary version — concise, factual, and polished for senior management:
Subject: RCA Summary – Aura Customer vs Active Login vs Fiserv Inquiry
Hi [Manager Name], [Tech Lead Name],
As part of the RCA, we compared the Aura customer base with actual login and Fiserv inquiry counts:
Aura registered customers: 14,000
Active logged-in users: 68,868
Fiserv inquiry count: 278,526
This indicates nearly a 5x higher inquiry volume compared to the total Aura customers. The investigation suggests that duplicate Aura records were created under heavy load on Fiserv, which operates on an MQ-based architecture rather than standard HTTP.
Recommendation:
To prevent recurrence, MAS recommends introducing mobile client-side controls to reduce redundant or repeated inquiries.
I identified this discrepancy during the review; please validate the findings and advise on next steps.
Best regards,
Vetri
Would you like me to include a “Next Action Plan” section (e.g., fix ownership, monitoring, or timeline) to make it presentation-ready for leadership updates?


Here’s your final combined email — a strong, leadership-level version that includes the RCA summary, API-level suggestions, and governance control message — polished and ready to send 👇
Subject: Home Screen API Overuse Impacting MAS, ESB & CBG — Strict Approval Control Required
Hi [Manager Name], [Tech Lead Name],
As part of the RCA, we analyzed the Aura customer data against active login and Fiserv inquiry counts:
Aura registered customers: 14,000
Active logged-in users: 68,868
Fiserv inquiry count: 278,526
This shows that the Fiserv inquiry volume is nearly 5x higher than the active login count, indicating redundant or duplicate API calls from the mobile client.
The investigation suggests this behavior is caused by duplicate Aura records generated under heavy load on Fiserv, which operates on an MQ-based setup rather than a standard HTTP interface.
Recommendation:
MAS has recommended implementing mobile client-side controls to minimize unnecessary requests and stabilize load across all layers.
API-Level Suggestions (Home Screen Optimization):
Below are the key recommendations identified to reduce redundant Fiserv calls and improve overall performance:
whatsapp/v1/cards – Limit to one MAS request per login
referral/v1/status – Limit to one MAS request per login
products/v3/portfolio/widgets – Move to sync call to avoid repeated load
products/v1/card/prepaid/details – Include within existing sync call
authorization/v1/user/details – Combine within sync call instead of per session
notification/v2/card/authorizations – Trigger only if user missed push notification
Governance & Next Steps:
We need to enforce a strict process — any new API addition to the home screen must be reviewed and approved by Ferhat before implementation.
Uncontrolled API usage is overloading MAS, ESB, and CBG layers, similar to executing heavy SQL queries repeatedly in production.
This governance step is essential to protect system stability, performance, and downstream service reliability.
I identified these discrepancies during the RCA; please review and share your comments or additional inputs.
Best regards,
Vetri
Would you like me to make a short version (2-paragraph) of this for leadership WhatsApp or Teams update as well?


Here’s the same message in short bullet-point format, friendly and firm:
Subject: Home Screen API Categorization & Control
Hi Team,
To improve performance and reduce MAS/Fiserv load:
Per Session List – Call once per login (Best for You, What’s Next, 3DS Pending Authorization)
Pull to Refresh List – Call only on manual refresh (for latest updates)
Always Call – Real-time data (Balance, etc.)
Let’s follow this structure for all existing and new home screen APIs to keep performance consistent and avoid unnecessary load.
Best,
Vetri


Here’s the short, bullet-point version for the MAS backend APIs — clear, friendly, and firm:
Subject: MAS Backend API Categorization & Control
Hi Team,
To optimize MAS performance and prevent redundant load from mobile:
Per Session List – Call once per login (Best for You, What’s Next, 3DS Pending Authorization)
Pull to Refresh List – Call only on manual refresh (for latest updates)
Always Call – Real-time APIs (Balance, etc.)
Let’s apply the same categorization for all MAS backend APIs used by the mobile home screen to maintain system stability and avoid unnecessary MQ load.
Best,
Vetri


Here’s the short, bullet-point version for the MAS backend APIs — clear, friendly, and firm:
Subject: MAS Backend API Categorization & Control
Hi Team,
To optimize MAS performance and prevent redundant load from mobile:
Per Session List – Call once per login (Best for You, What’s Next, 3DS Pending Authorization)
Pull to Refresh List – Call only on manual refresh (for latest updates)
Always Call – Real-time APIs (Balance, etc.)
Let’s apply the same categorization for all MAS backend APIs used by the mobile home screen to maintain system stability and avoid unnecessary MQ load.
Best,
Vetri


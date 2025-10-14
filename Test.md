Got it 👍 I can clearly see the content on your screen — it’s your email draft titled:
“RE: Weyay – ESB API Calls”,
subject line referencing Home Screen API Overuse Impacting MAS, ESB & CBG – Strict Approval Control Required.
Here’s a professional and polished version of your email with stronger structure, clearer flow, and executive-friendly tone while keeping your original meaning intact:
Subject: Home Screen API Overuse Impacting MAS, ESB & CBG – Strict Approval & Control Required
Dear Team,
As part of the RCA, we analyzed the Aura customer data against active login and Fiserv inquiry counts for the day of 04 Oct 2025:
Aura registered customers: 14,045 (from Business)
Active logins: 68,867 (from Business)
Fiserv inquiry count: 278,526 (from ESB)
This shows that the Fiserv inquiry volume is nearly 5× higher than the active login count, indicating a high number of duplicate API calls from the mobile app.
Our investigation suggests this behavior is caused by duplicate Aura and Fiserv calls made via home screen APIs.
Recommendations
1. MAS to implement stricter controls on home screen API configurations to minimize unnecessary load across internal systems (MAS, ESB, CBG).
Home Screen API Categorization & Controls:
Per Session Load: Call once per session (e.g., “Best for You”, “What’s Next”, “3DS Pending Auth”).
Pull to Refresh Load: Call only on user-initiated refresh actions.
Always Call List: Only for balance or real-time data (justified APIs).
All new APIs: Require prior MAS approval before inclusion on the home screen.
2. API Level Enhancements:
Implement correlation IDs for each session and API flow.
Apply session-level throttling and F5/ESB resilience to prevent DoS-type spikes.
Reference: [PRODUCTION BUG 154908] for related case.
Governance & Next Steps
To ensure a stricter approval process:
Any new or modified APIs on the home screen must undergo review and approval from MAS / CBG / Integration teams before deployment.
Uncontrolled API usage should be flagged immediately for assessment before going live.
This process must be followed in all UAT and production environments.
This governance is essential to protect system stability, performance, and ensure smoother integration across Weyay channels.
Summary:
We identified these discrepancies during the RCA of PRODUCTION BUG 154908. Please review and share any additional inputs.
Regards,
[Your Name]
Solution Architect – Weyay Channels
Would you like me to make it sound friendlier and shorter (for a wider audience) or stronger and firmer (for senior management / escalation)? I can tailor the tone accordingly.
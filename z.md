# Subject: RE: Urgent: AURA Card Not Visible – Duplicate Card Creation in Production

Dear Mahendra,

Thank you for your feedback.

We’ve identified a **design gap between online and offline modes**. As per business requirements, customers must always view **live AURA balance and card details**. However, in **offline mode**, the **available balance is not returned** and only **T-1 balance data** is available.

To meet this expectation, we continue to use **online (VMX) mode** for AURA inquiries.  
During **peak login hours** (e.g., campaigns or new feature promotions), login volume may increase significantly. Since each login verifies the **AURA card status and balance**, we’d like to confirm the **maximum concurrent login count and API throughput ESB can safely support** under load.

Your guidance on this will help us ensure a **stable and real-time customer experience**.

Best regards,  
**Vetri**  
_MAS Solution Architect_
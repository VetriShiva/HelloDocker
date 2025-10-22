# Home Screen Optimization – BR / Story Request for Performance Improvement

To: Business Team, Product Owner  
Cc: ESB Integration Team, CBG Backend Team  

---

## Context
As part of the recent RCA, we identified that the home screen currently invokes multiple backend APIs simultaneously, which has started impacting overall system performance — particularly on MAS, ESB, and CBG (shared backend services).

A few key features contributing to the high load are:
- What's Next  
- Best for You  
- Available Balance  
- 3D Pending Authorizations  

Previously, these widgets were static, but with recent enhancements they are now dynamically fetched from ESB, leading to:
- Increased number of backend calls per user session  
- Higher response time and load on shared systems  
- Occasional timeouts and delayed data refresh on the client side  

---

## Proposed Next Steps
To address this, we propose creating a Business Requirement (BR) or User Story focused on home screen API optimization, with the following goals:

1. Categorize API calls based on data freshness and importance  
   (e.g., per session, on refresh, real-time)  
2. Introduce caching and smart invalidation logic to reduce redundant ESB calls  
3. Optimize orchestration sequence for dependent services to minimize wait time  
4. Collaborate with Business to confirm which features can be refreshed less frequently without impacting the user experience  

---

## Expected Outcome
- Reduction in MAS–ESB–CBG traffic  
- Improved home screen load time for end users  
- Enhanced system stability and better resource utilization  

---

## Next Action
Kindly review this proposal and let's plan a discussion to define the scope and prioritization for the optimization BR or Story.  

---

Best regards,  
MAS Solution Architect Team  
Weyay Digital Banking


# Home Screen Optimization – BR / Story Request for Performance Improvement

To: Business Team, Product Owner  
Cc: ESB Integration Team, CBG Backend Team, BAT Team  

---

## Context
As part of the recent RCA, we identified that the home screen currently invokes multiple backend APIs simultaneously, which has started impacting overall system performance — particularly on MAS, ESB, and CBG (shared backend services).

A few key features contributing to the high load are:
- What's Next  
- Best for You  
- Available Balance  
- 3D Pending Authorizations  

Previously, these widgets were static, but with recent enhancements they are now dynamically fetched from ESB, leading to:
- Increased number of backend calls per user session  
- Higher response time and load on shared systems  
- Occasional timeouts and delayed data refresh on the client side  

---

## Proposed Next Steps
To address this, we propose creating a Business Requirement (BR) or User Story focused on home screen API optimization, with the following goals:

1. Categorize API calls based on data freshness and importance  
   (e.g., per session, on refresh, real-time)  
2. Introduce caching and smart invalidation logic to reduce redundant ESB calls  
3. Optimize orchestration sequence for dependent services to minimize wait time  
4. Collaborate with Business to confirm which features can be refreshed less frequently without impacting the user experience  
5. Engage BAT team early to support end-to-end regression testing and ensure stability across all home screen use cases  

---

## Expected Outcome
- Reduction in MAS–ESB–CBG traffic  
- Improved home screen load time for end users  
- Enhanced system stability and better resource utilization  
- Early identification of potential issues through BAT regression support  

---

## Next Action
Kindly review this proposal and let's plan a discussion to define the scope, prioritization, and testing strategy for the optimization BR or Story.  

---

Best regards,  
MAS Solution Architect Team  
Weyay Digital Banking


# Home Screen Optimization – BR / Story Request for Performance Improvement

To: Business Team, Product Owner  
Cc: ESB Integration Team, CBG Backend Team, BAT Team  

---

## Context
As part of the recent RCA, we identified that the home screen currently invokes multiple backend APIs simultaneously, which has started impacting overall system performance — particularly on MAS, ESB, and CBG (shared backend services).

A few key features contributing to the high load are:
- What's Next  
- Best for You  
- Available Balance  
- 3D Pending Authorizations  

Previously, these widgets were static, but with recent enhancements they are now dynamically fetched from ESB, leading to:
- Increased number of backend calls per user session  
- Higher response time and load on shared systems  
- Occasional timeouts and delayed data refresh on the client side  

---

## Proposal (From MAS Solution Architect Team)
This proposal is raised by the **MAS Solution Architect Team** to optimize the home screen API design and improve system performance through a coordinated approach involving Business, ESB, CBG, and BAT teams.

### Key Objectives
1. Categorize API calls based on data freshness and importance  
   (e.g., per session, on refresh, real-time)  
2. Introduce caching and smart invalidation logic to reduce redundant ESB calls  
3. Optimize orchestration sequence for dependent services to minimize wait time  
4. Collaborate with Business to confirm which features can be refreshed less frequently without affecting user experience  
5. Engage BAT team early to support end-to-end regression testing and ensure stability across all home screen use cases  

---

## Expected Outcome
- Reduced MAS–ESB–CBG traffic  
- Improved home screen load time for end users  
- Better stability and performance across environments  
- Early detection of issues through structured BAT regression testing  

---

## Next Action
Requesting Business and Product teams to review this proposal and plan a discussion to define the scope, prioritization, and testing support required for this optimization initiative.  

---

MAS Solution Architect Team  
Weyay Digital Banking
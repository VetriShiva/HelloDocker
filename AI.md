Prompt Engineering Frameworks – Complete Reference

This document lists commonly used prompt engineering frameworks with clear structure and examples. It is intended for engineers, architects, and AI practitioners.


---

1. RTF – Role, Task, Format

Structure

Role
Task
Format

Example

Role: You are a senior solution architect.
Task: Explain RAG to a junior developer.
Format: Bullet points with a simple example.


---

2. TAG – Task, Action, Goal

Structure

Task
Action
Goal

Example

Task: Review a Spring Boot REST API.
Action: Analyze scalability, security, and performance.
Goal: Provide production-ready recommendations.


---

3. RTCF – Role, Task, Context, Format

Structure

Role
Task
Context
Format

Example

Role: Fintech backend architect.
Task: Design a payment retry mechanism.
Context: High traffic, idempotency required.
Format: Architecture steps with pseudocode.


---

4. CRISPE – Context, Role, Input, Steps, Output, Evaluation

Structure

Context
Role
Input
Steps
Expected Output
Evaluation Criteria

Example

Context: Loan management system.
Role: Senior solution architect.
Input: Loan repayment API design.
Steps: Analyze → Identify gaps → Propose fixes.
Output: Improved design.
Evaluation: Scalable, secure, maintainable.


---

5. CO-STAR – Context, Objective, Style, Tone, Audience, Response

Structure

Context
Objective
Style
Tone
Audience
Response Format

Example

Context: Internal engineering training.
Objective: Teach prompt engineering basics.
Style: Practical.
Tone: Friendly.
Audience: Backend developers.
Response: Examples with checklist.


---

6. CARE – Context, Action, Result, Example

Structure

Context
Action
Result
Example

Example

Context: Poor RAG answer quality.
Action: Improve chunking and prompts.
Result: Higher accuracy.
Example: Before vs after prompt.


---

7. IDEA – Input, Design, Execution, Analysis

Structure

Input
Design
Execution
Analysis

Example

Input: Requirement for AI chatbot.
Design: RAG with vector database.
Execution: Retrieval + prompt pipeline.
Analysis: Latency, accuracy, cost.


---

8. PEARL – Purpose, Environment, Audience, Rules, Limitations

Structure

Purpose
Environment
Audience
Rules
Limitations

Example

Purpose: Generate SQL queries.
Environment: SQL Server.
Audience: Backend engineers.
Rules: No DELETE or UPDATE.
Limitations: Read-only access.


---

9. Chain-of-Thought (CoT)

Structure

Request step-by-step reasoning

Example

Explain step by step how EMI is calculated for a loan.

> Note: In production, prefer concise reasoning to reduce cost and risk.




---

10. Few-Shot Prompting

Structure

Instruction
Example 1
Example 2
New Input

Example

Convert text to JSON.

Input: User logged in
Output: {"event": "login"}

Input: Payment failed
Output:


---

11. ReAct – Reason + Act

Structure

Thought
Action
Observation
Final Answer

Example

Thought: Need customer details.
Action: Call customer API.
Observation: Data received.
Final Answer: Customer summary.


---

12. Instruction + Constraints Pattern

Structure

Instruction
Constraints
Output Format

Example

Explain RAG.
Constraints:
- Max 5 bullet points
- Simple language
Output:
- Bulleted list only


---

Summary

Prompt frameworks provide structure, repeatability, and safety when working with LLMs. They serve the same role as API contracts in traditional software engineering.

Recommended usage:

Enterprise & RAG systems: RTCF + Constraints + Few-shot

Learning & mentoring: CO-STAR or CARE

Agents & MCP systems: ReAct + PEARL
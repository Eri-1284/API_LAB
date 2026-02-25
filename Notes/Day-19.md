# Day 19 — BOLA: The Ownership Crisis in API Architecture

## 1. The Core Vulnerability: Resource Ownership
- **Actions:** Analyzed Broken Object Level Authorization (BOLA/IDOR) as the failure to verify if a requester owns the resource they are accessing.
- **Outcome:** Defined BOLA as the ability to manipulate object identifiers (IDs) to access data belonging to others.
- **Strategic Insight:** APIs are uniquely susceptible because they expose raw database identifiers (UUIDs, Emails, IDs) directly in the URL and JSON body, removing the "curtain" of a traditional UI.

---

## 2. The Fallacy: Authentication is Not Authorization
- **Actions:** Established that a "Valid Token" (Authentication) does not equal "Resource Permission" (Authorization).
- **Outcome:** OAuth Context: Even if an API extracts a sub: 101 from a token, it is vulnerable if it allows a request for /api/user/102 to proceed without an ownership check.

---

## 3. BOLA vs. BFLA: Objects vs. Actions
- **Actions:** BOLA (API1): Targets Resources (e.g., GET /user/124 instead of 123). BFLA (API5): Targets Functions (e.g., POST /admin/delete).
- **Outcome:** Clarified the distinction between object-level (BOLA) and function-level (BFLA) authorization failures.

---

## 4. Methodology: The Identifier Swap
- **Actions:** 
  - Baseline: Confirm access to your own resource with a valid token.
  - Identification: Locate IDs in URL paths, query params, JSON bodies, or GraphQL queries.
  - Execution: Modify only the ID; keep headers and tokens identical.
- **Outcome:** A 200 OK containing another user's data confirms a critical breach of the authorization layer.

---

## Summary
- **Actions:** Focused on the structural exposure of IDs in APIs, proving that security fails when backend logic assumes that a valid login grants universal access to all database objects.

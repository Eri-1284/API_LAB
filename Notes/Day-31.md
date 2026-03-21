# Day 31: Turning Recon into Attack Paths

### Lesson Context:
We have reached the final stage of the Reconnaissance phase. Day 31 is about Synthesis. It is the bridge between gathering data and launching an exploit. This session teaches the "Commander’s View"—how a Senior Consultant takes a chaotic list of URLs, parameters, and versions and turns them into a lethal, structured Attack Matrix. This is where we stop looking and start hunting, mapping our findings directly to the OWASP API Top 10.

---

## The Operational Shift: From Data to Strategy
Reconnaissance is a library; Attack Mapping is a battle plan. If you enter the "Testing Phase" without a map, you will waste hours on low-impact bugs while missing the critical BOLA or BFLA vulnerabilities hiding in plain sight.
The Core Objective
The goal of Day 31 is to answer one question: "Given what I know about this API's structure, where is the logic most likely to break?" We move from identifying what is there to predicting how it fails.

---

### Step 1: Building the Master Endpoint Map
Your recon (Days 23–29) should have yielded a "Raw List." Now, we organize it to visualize the attack surface.
The Raw List:
  - /api/v1/login
  - /api/v2/user/{id}
  - /api/v1/orders/{id}
  - /api/internal/admin/stats
  - /api/mobile/v2/profile
  - /api/payment/checkout
  - /api/otp/generate

---

### Step 2: The Classification Framework
Professionals categorize endpoints by their Functional Risk. This allows us to apply the correct "Attack Tool" to the right "Target."
1. Authentication & Identity (The Keys)
  - Endpoints: /api/login, /api/token/refresh, /api/mfa/verify.
  - Attack Vector: Brute force, JWT manipulation, session fixation, and Broken Authentication (API2).

2. Object-Level Endpoints (The Data Vaults)
  - Endpoints: /api/user/{id}, /api/orders/{uuid}, /api/tickets/{id}.
  - Attack Vector: BOLA (API1). These are your #1 priority. If there is an ID in the URL, you test if User A can see User B’s data.

3. Function-Level Endpoints (The Control Room)
  - Endpoints: /api/admin/*, /api/export/csv, /api/user/delete.
  - Attack Vector: BFLA (API5). Can a regular "User" role call an "Admin" function?

4. Workflow & Logic Endpoints (The Factory Floor)
  - Endpoints: /api/cart/checkout $\rightarrow$ /api/pay $\rightarrow$ /api/confirm.
  - Attack Vector: Business Logic Flaws (API6). Can we skip the payment step? Can we submit a negative quantity?

5. High-Resource/Utility Endpoints (The Fuel)
  - Endpoints: /api/search, /api/upload, /api/send-sms.
  - Attack Vector: Unrestricted Resource Consumption (API4). Can we trigger 10,000 SMS messages or crash the DB with a complex search?

---

### Step 3: The Testing Matrix (The Consultant's Deliverable)
A Testing Matrix is a structured table that ensures 100% coverage. It prevents "Pentester Tunnel Vision."
| Target Endpoint | Classification | Primary Attack Vector | Priority |
|----------------------|--------------------|------------------------------|-----------|
| /api/v1/orders/{id} | Object-Based | BOLA (ID substitution) | CRITICAL |
| /api/admin/config | Function-Based | BFLA (Unauthorized access) | HIGH |
| /api/v2/profile | Mobile/Internal | Excessive Data Exposure | MEDIUM | 
| /api/otp/send | Utility | Rate Limiting / SMS Fraud | HIGH |
| /api/v1/pay | Workflow | Logic Bypass (Step skipping) | CRITICAL |

---

### Step 4: Risk-Based Prioritization
We don't have infinite time. We prioritize based on the Impact vs. Probability of the vulnerability.
1.	Administrative & Internal Routes: These offer the "Keys to the Kingdom."
2.	Predictable IDs: Any endpoint using a simple integer (/101) is a higher priority than a random UUID, as it’s easier to enumerate.
3.	Financial Workflows: Anything involving "Money," "Credits," or "Refunds" is a top-tier target for business logic abuse.
4.	Legacy/Shadow APIs: As learned in Day 30, the /v1/ version is likely less protected than the /v3/ version.

---

## The "Red Thread" Connection
Everything you have learned in the last 31 days is now a single thread:
  - Mobile Recon found the /api/mobile/beta route.
  - Parameter Mining found the ?debug=true flag.
  - Asset Management showed that v1 is still live.
  - Mapping now tells you to test: GET /api/mobile/beta/user/101?debug=true for a BOLA exploit that bypasses the modern v3 security.

---

## OWASP API Mapping & Strategy
| Attack Category | Strategy | OWASP Link |
|-----------------------|------------|-----------------|
| BOLA | Swap IDs in Object-Based endpoints. | API1:2023 |
| BFLA | Access Admin endpoints with a User token. | API5:2023 |
| Logic | Change the sequence or values in Workflows. | API6:2023 |
| Asset | Target the discovered legacy versions. | API9:2023 |

---

### Summary
Day 31 is the "Final Exam" of Reconnaissance. It transforms you from a "Data Collector" into a "Security Architect." By classifying endpoints, building a matrix, and prioritizing risk, you ensure that your testing is systematic, professional, and high-impact. You no longer "guess" where the bugs are; you calculate where the logic is weakest.

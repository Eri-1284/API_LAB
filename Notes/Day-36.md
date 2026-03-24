# Day 36: Multi-Stage Vulnerability Chaining Techniques


## I. The Philosophy of the "Kill Chain"
In professional penetration testing, a single bug (like a leaked UUID) is often labeled "Low" or "Informational." However, in the hands of an attacker, that UUID is a primitive—a building block used to unlock the next stage of an attack.
An isolated BOLA (reading a user's email) is an invasion of privacy. A BOLA chained to a weak password reset flow is a total account takeover.

---

## II. Scenario #1: The "Identity Theft" Chain (BOLA + Auth)
This is the most common path to full account compromise. It exploits the "Excessive Data Exposure" of one endpoint to feed a "Broken Authentication" flaw in another.
  1.	Stage 1: The Leak (API3/API1)
    - Action: GET /api/v1/users/102
    - Discovery: The response includes the victim's email and a secret_security_answer.
  3.	Stage 2: The Trigger (API2)
    - Action: POST /api/v1/auth/forgot-password
    - Discovery: The API asks for the security answer found in Stage 1.
  4.	Stage 3: The Takeover
    - Action: Submit the leaked answer and set a new password.

---

## III. Scenario #2: The "Shadow Admin" Chain (BFLA + BOLA)
This chain targets the management layer of the API. It uses a lack of function-level checks to gain access to a tool, and then uses a lack of object-level checks to target specific users.
  - The Entry (BFLA): A standard user discovers they can reach POST /api/internal/set-user-status.
  - The Pivot (BOLA): The attacker notices the endpoint accepts a status string and a target_uid.
  - The Escalation: The attacker sends {"target_uid": "my_id", "status": "admin"}.
Impact: Vertical Privilege Escalation. The user has effectively "promoted" themselves because the internal function trusted the input without verifying the requester's authority.

---

## IV. Scenario #3: The "Financial Siphon" (Logic + Parameter Abuse)
Business logic chains are the most "expensive" bugs because they directly affect the company's bottom line. They often involve manipulating the State of a transaction.

| Step | Action | Vulnerability |
|--------|----------|------------------|
| 01 | POST /api/cart/add | Parameter Mining: Find hidden discount_percent field. |
| 02 | PATCH /api/cart/101 | Logic Abuse: Set discount_percent to 100. |
| 03 | POST /api/order/checkout | Insecure Workflow: API processes the 100% discount without re-validating the coupon code. |

---

## V. Professional Methodology: Building the Attack Tree
To find these chains, a consultant uses Visual Mapping to identify how data flows between endpoints.
  1.	Input/Output Matching: Does Endpoint A return a value (Token, ID, Email) that is required as an input for Endpoint B?
  2.	State Dependency: Does Endpoint B assume that "if the user is here, they must have successfully completed Endpoint A"? (e.g., assuming payment is done because the user called /confirm).
  3.	Cross-Role Probing: If I find an "Admin" ID in a public "User" profile, can I use that ID in a "Support" endpoint?

---

## VI. OWASP API Security Mapping
  - API1 (BOLA) & API3 (Data Exposure): Usually the "Fuel" for the attack.
  - API2 (Broken Auth) & API5 (BFLA): Usually the "Engine" that drives the escalation.
  - API6 (Unrestricted Business Flows): The "Outcome" where the business logic is subverted.

---

## VII. Summary: Comparison of Single vs. Chained Impact

| Single Finding | Impact | Chained Finding | Impact |
|--------------------|-----------|-----------------------|----------|
| BOLA | Can see user's phone number. | BOLA + SMS API | Intercept OTP; Take over account. |
| BFLA | Can see admin dashboard. | BFLA + Admin Update | Change system-wide configs. |
| Logic Flaw | Can add negative items. | Logic + Checkout | Checkout with a negative balance (Cash out). |

---

### Summary 
This advanced module marks the transition from "vulnerability scanning" to "adversarial simulation." Students will learn the methodology of Multi-Step Attack Chaining, where a sequence of seemingly low-impact bugs is orchestrated to achieve a catastrophic result. By analyzing real-world "Kill Chains," practitioners will learn how to pivot through an API's architecture, using the output of one exploit as the input for the next.

# Day 33: Auth + BOLA Chaining (Account Takeover Paths)
### Lesson Context
We are dismantling the most dangerous myth in backend development: "The user is logged in, so they must be safe." Day 33 dives into Lateral Movement within an API environment. We are exploring how a legitimate, low-privileged attacker uses their own valid session as a "mask" to systematically dismantle the accounts of other users. This session bridges OWASP API1 (BOLA), API2 (Broken Authentication), and API5 (BFLA), focusing on the transition from a valid session to a total Account Takeover (ATO).

---

## The Paradox of Authentication
Authentication is merely a digital ID card; it says who you are. Authorization is the set of permissions that says what you can touch.
**The Developer's Fallacy**
Developers often write middleware that checks if a JWT or Session Cookie is present. If it is, the code proceeds to the database query.
  - The Vulnerable Logic: SELECT * FROM users WHERE id = $request_id;
  - The Secure Logic: SELECT * FROM users WHERE id = $request_id AND owner_id = $session_user_id;
If the owner_id check is missing, the API is "Blind." It trusts the token for identity but fails to enforce the boundary of the resource.

---

## The Base Scenario: The "Masked" Request
As a tester, you start with a valid account (User A). You have a legitimate token:
Authorization: Bearer <User_A_JWT>
The Probe:
GET /api/v1/user/101 (Your own ID) $\rightarrow$ 200 OK
GET /api/v1/user/102 (Victim ID) $\rightarrow$ 200 OK
**The Diagnostic:** The server validated your token (Authentication) but failed to check if User A has the right to see User 102's data (Authorization). You have confirmed a BOLA vulnerability. Now, we amplify the impact.

---

## The ATO Kill Chain: From Information to Control
A professional doesn't just "read" data; they look for State-Changing Pivots.

### 1. Identifying Leakage for Exploitation
In the BOLA response for /api/user/102, hunt for "High-Entropy" or "Security-Critical" fields:
 - email: Used for password resets.
 - reset_token: A direct path to ATO.
 - mfa_enabled: Knowing if you need to bypass 2FA.
 - phone_number: For SMS-based hijacking.

### 2. The Profile Update Abuse (The "Email Swap")
The most common path to ATO is changing the victim's email to your own.

  - Request:
  **HTTP**
  ```
  PUT /api/v1/user/102
  Content-Type: application/json
  Authorization: Bearer <User_A_TOKEN>

  { "email": "attacker@evil.com" }
  ```

  - The Logic: If the server accepts this, the victim's account is now linked to the attacker's inbox. The attacker then triggers a "Forgot Password" flow, receives the link, and sets a new password.

### 3. Token Misuse & Resource Decoupling
Sometimes the token contains the user's ID (sub: 101), but the API route relies on the URL parameter (/102). This is Resource Decoupling. The server trusts the "Who" from the token but ignores the "What" in the URL.

---

## Session & Token Pivoting
Session Pivoting is the act of moving from your legitimate session into a new, unauthorized session belonging to a victim.
1.	Modify: Use BOLA to change victim metadata (Email/Phone).
2.	Trigger: Use the "Password Reset" or "Verify Account" API.
3.	Generate: Complete the reset to generate a new valid JWT for the victim's account.
4.	Pivot: You are now logged in as the victim. Your original User A token is no longer needed.

---

## The Impact Matrix: Chaining Outcomes
| Chained Action | Vulnerability Type | Final Impact |
|----------------------|-------------------------|-----------------|
| BOLA + Read Email | API1 + API3 | PII Leak / Targeted Phishing |
| BOLA + Update Email | API1 + Logic | Full Account Takeover |
| BOLA + Reset Token Leak | API1 + API2 | Instant Account Takeover |
| BOLA + Delete User | API1 + API5 | Massive Data Loss / DoS |

---

## Professional Testing Methodology
When performing an Auth-BOLA audit, I follow this structured workflow:
- Step 1: Ownership Verification: Does the API require a token? (Auth check).
- Step 2: Boundary Testing: Does the API return someone else's data if I change the ID? (BOLA check).
- Step 3: Sensitivity Analysis: What fields are returned? Is there anything I can use to reset a password or bypass 2FA?
- Step 4: Write-Access Testing: Can I PUT, PATCH, or POST to a victim's ID? This is where the real damage happens.
- Step 5: Token Integrity: Does the backend verify that the ID in the URL matches the sub (Subject) field in the JWT?

---

## OWASP API Mapping
| Risk ID | Title | Strategic Connection |
|-----------|-------|-----------------------------|
| API1:2023 | BOLA | The core flaw allowing access to the victim's object. |
| API2:2023 | Broken Authentication | Occurs when the reset tokens or session management logic is flawed. |
| API5:2023 | BFLA | When a user accesses "Update" or "Delete" functions they shouldn't own. |

---

### Summary
Day 33 proves that Authentication is not a security blanket. An authenticated attacker is often more dangerous than an unauthenticated one because they have a "legitimate" reason to be communicating with the API. By chaining a valid session with weak authorization checks, we can pivot through a database, hijacking accounts one by one without ever firing a brute-force tool.

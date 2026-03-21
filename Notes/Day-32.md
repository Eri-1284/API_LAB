Day 32: Turning IDOR into Full System Compromise

### Lesson Context:
We are moving from the "What" to the "So What?" Day 32 focuses on Impact Amplification. In a professional engagement, simply finding a Broken Object Level Authorization (BOLA) bug is only half the job. A Senior Consultant's value lies in demonstrating how a "low-stakes" data leak can be chained to achieve Account Takeover (ATO), Financial Fraud, or Systemic Denial of Service. We are mapping this to the intersection of OWASP API1 (BOLA) and API5 (BFLA) to prove that one open window can lead to the entire house being emptied.

---

## Beyond the "Vulnerability Checklist"
Most entry-level bug hunters treat BOLA as a binary state: "I changed the ID, and it worked." They report a "Medium" severity finding and move on.
The Professional Pivot: A Senior Tester treats BOLA as an entry point. We don't just want to see the data; we want to use that data to compromise the application's core logic.
The Evolution of an Attack
1.	Discovery: GET /api/user/101 $\rightarrow$ GET /api/user/102 (Success).
2.	Analysis: What unique identifiers or "secrets" are in the response for 102?
3.	Chaining: Can I use those secrets in a POST, PUT, or DELETE request?

---

## The Anatomy of the "Kill Chain"
To maximize impact, we look for "Actionable Data" in the BOLA response.

### Chain 1: The Password Reset Hijack (BOLA $\rightarrow$ ATO)
This is the "Holy Grail" of BOLA chaining.
  - The Leak: GET /api/v1/users/99 returns the victim’s email AND a hidden temporary_reset_token.
  - The Action: The attacker takes that token to the public /api/v1/password-reset/confirm endpoint.
  - The Result: Full Account Takeover.
  - Impact: Critical.

### Chain 2: The Financial Sabotage (BOLA $\rightarrow$ Logic Abuse)
  - The Leak: An attacker finds they can access /api/v2/orders/555 belonging to a competitor.
  - The Action: They find a secondary endpoint: POST /api/v2/orders/555/cancel.
  - The Logic Gap: If the cancel endpoint only checks if the order_id exists but doesn't verify if the current session owns that order, the attack succeeds.
  - The Result: Mass cancellation of a competitor's revenue.
  - Impact: High/Financial.

---

## BOLA + BFLA: The "Admin-in-the-Middle" Attack
This occurs when a functional endpoint (meant for admins) is exposed to users, and that endpoint lacks object-level checks.
  - Step A (BFLA): A regular user discovers they can reach POST /api/admin/update-permissions.
  - Step B (BOLA): The request body requires a target_user_id.
  **JSON**
  ```
  {
    "target_user_id": 101,
    "new_role": "super_admin"
  }
  ```

  - The Synthesis: By putting their own user_id in the target_user_id field, the user promotes themselves to Admin.

---

## The Professional "Impact Matrix"
When documenting these findings for a GitHub portfolio or a client report, use a transition table to show the "Escalation of Force."
| Initial Discovery | Chained Endpoint | Resulting Impact | Final Severity |
|-----------------------|-------------------------|------------------------|-------------------|
| Read BOLA on /user/me | /api/auth/mfa-disable | Bypassing MFA for any user | Critical |
| Read BOLA on /invoices | /api/payments/refund | Diverting refunds to attacker's  I BAN | Critical |
| Read BOLA on /profile | /api/email/update | Changing victim email to attacker email | High |
| Read BOLA on /docs | None (Read only) | PII Leak (Names/Addresses) | Medium |

---

## Tactical Thinking: The "What Else?" Methodology
When you find a BOLA, apply this 5-step recursive logic:
1.	Read: What sensitive keys (UUIDs, tokens, emails) did I just gain?
2.	Write: Is there a PUT or PATCH version of this endpoint? Can I change the victim's data?
3.	Delete: Can I remove this object? (e.g., deleting a victim’s cloud server).
4.	Act: Does this ID work in "Action" endpoints (e.g., /invite, /share, /pay)?
5.	Pivot: Can this ID be used to enumerate other objects? (e.g., a User ID leads to finding their Order IDs).

---

## OWASP API Mapping
| Risk ID | Title | Chaining Context |
|-----------|-------|------------------------|
| API1:2023 | BOLA | The foundation of the chain. Accessing the object ID. |
| API5:2023 | BFLA | Using BOLA to execute functions that should be restricted by role. |
| API6:2023 | Unrestricted Business Flows | Chaining BOLA into a workflow (like a checkout) to disrupt business rules. |

---

### Summary
Day 32 teaches us that vulnerabilities do not exist in a vacuum. A single IDOR is a spark; chaining it with other endpoints is the fire. As a professional, your goal is to build a narrative of destruction that shows a client exactly how a small oversight in a GET request can lead to a catastrophic data breach.

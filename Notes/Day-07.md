# Day 7 — API Security: JWT Logic & Claim Abuse Analysis

## 1. Vulnerability Surface Assessment
- **Actions:** Evaluated how heavily the API relies on client-side JWT claims instead of server-side session state.
- **Actions:** Prioritized tests around logical claim trust issues rather than deep cryptographic attacks.
- **Outcome:** Confirmed that the main attack vector is the backend’s implicit trust in the integrity and truthfulness of the JWT payload.

---

## 2. Claim-Based Trust Evaluation
- **Actions:** Decoded JWT payloads to isolate high-value identifiers such as `userId`, `role`, and `exp`.
- **Actions:** Analyzed how these claims directly influenced server-side authorization decisions.
- **Outcome:** Showed that blindly trusting client-controlled claims opens direct paths to IDOR and privilege escalation.

---

## 3. Manual Replay & Authorization Probing
- **Actions:** Used a replay workflow to remove, modify, and resend tokens against multiple API endpoints.
- **Actions:** Watched for inconsistent status codes, such as `200 OK` where `401` or `403` should have been returned.
- **Outcome:** Assessed the strength of authorization boundaries and surfaced potential Broken Function Level Authorization (BFLA) weaknesses.

---

## Summary
- **Actions:** Shifted focus from theoretical JWT structure to practical logic-based exploitation and automation using scripting.
- **Outcome:** Reinforced that even cryptographically sound JWTs are insecure if business logic fails to properly validate the context and trustworthiness of token claims.

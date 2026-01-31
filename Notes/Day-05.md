# Day 05 — Step-by-Step API Authentication Auditing

## Phase 1: Identity Discovery
- **Actions:** Captured authenticated traffic in the proxy to identify the token’s format and structure.
- **Actions:** Located the authentication data carried inside the `Authorization` header.
- **Outcome:** Gained clarity on how the client represents identity and where the API expects credentials.

---

## Phase 2: Baseline Enforcement Check
- **Actions:** Tried to access the same protected endpoint in Postman without sending any credentials.
- **Actions:** Observed a `401 Unauthorized` response from the server.
- **Outcome:** Established a secure baseline showing that the endpoint correctly denies unauthenticated requests.

---

## Phase 3: Token Manipulation (Exploitation)
- **Actions:** Sent the authenticated request to a replay tool for controlled experimentation.
- **Actions:** Systematically deleted, truncated, and bit-flipped parts of the token to test whether the server still accepted the request.
- **Outcome:** Measured how strict the server’s token parsing and validation logic is under malformed or tampered inputs.

---

## Phase 4: Finding Interpretation
- **Actions:** Evaluated whether any responses indicated susceptibility to Broken Authentication (OWASP API2:2023).
- **Actions:** Recorded how the server behaved when given expired, dummy, or obviously invalid tokens.
- **Outcome:** Distinguished between robust authentication enforcement and critical logic flaws that could enable bypass or impersonation.

---

## Summary
- **Actions:** Followed a structured “discover → baseline → manipulate → interpret” workflow for API authentication testing.
- **Outcome:** Developed a practical method to assess whether auth controls are securely implemented or vulnerable to common bypass techniques.

# Day 15 — Assessment of Post-Issuance OAuth Token Abuse

## 1. Architectural Analysis: Token vs. Permission
- **Actions:** Evaluated the distinction between authentication (token validity) and authorization (resource access rights).
- **Outcome:** Identified that architectural weaknesses often stem from the assumption that a valid access token inherently implies permission to execute any request.

---

## 2. Forensic Capture and Behavioral Baseline
- **Actions:** Captured an Opaque Access Token through the Authorization Code Flow using Burp Suite.
- **Observations:** Determined that Opaque tokens necessitate a black-box testing approach, focusing on server-side responses rather than claim inspection.

---

## 3. Differential Testing for Authorization Gaps
- **Actions:** Conducted differential testing in Burp Repeater across multiple vectors:
  - Scope Enforcement: Testing the boundaries of the read permission.
  - Audience Isolation: Verifying if tokens are restricted to their intended API resource.
  - Functional Boundaries: Attempting administrative actions with standard user tokens.
- **Outcome:** Demonstrated that a lack of internal validation leads to BFLA and BOLA vulnerabilities.

---

## 4. Alignment with Industry Standards
- **Actions:** Cross-referenced findings with the OWASP API Security Top 10.
- **Outcome:** Validated that these failure modes are primary drivers for API1, API5, and API8, requiring server-side enforcement of scope and object ownership.

---

## Summary
- **Actions:** Completed a comprehensive audit of OAuth token handling, establishing that authorization logic must exist independently of token validation.

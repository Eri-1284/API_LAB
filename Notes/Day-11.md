# Day 11 — OAuth 2.0 Conceptual Mapping

## 1. Role & Responsibility Audit
- **Actions:** Identified the four core OAuth roles: Resource Owner, Client, Authorization Server, and Resource Server (API).
- **Actions:** Verified the "Resource Server" requirement for independent token validation.
- **Outcome:** Defined the API's role as the final gatekeeper that must verify token integrity before serving data.

---

## 2. Token Security Standards
- **Actions:** Categorized token types and their specific usage boundaries (Access vs. Refresh).
- **Actions:** Flagged "Opaque vs. JWT" implementation differences for future testing.
- **Outcome:** Created a baseline for identifying "Red Flag" behaviors, such as APIs accepting refresh tokens or ignoring token scopes.

---

## 3. OAuth vs. JWT Distinction
- **Actions:** Clarified that OAuth is the process (framework) while JWT is the format (container).
- **Outcome:** Eliminated the common developer misconception that using OAuth automatically secures an API without further manual checks.

---

## Summary
- **Actions:** Established a professional mental model of OAuth 2.0 fundamentals.
- **Outcome:** Prepared the technical foundation for upcoming labs focused on OAuth bypasses and logic flaws.

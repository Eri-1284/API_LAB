# Day 17 — Assessment of Single Sign-On (SSO) Attack Surface & Identity Logic

## 1. Conceptual Framework of Externalized Authentication
- *Actions:* Evaluated the shift in attack surface when authentication is delegated to an Identity Provider (IdP).
- *Outcome:* Determined that SSO complicates the security model by introducing complex mapping requirements between external identities and internal authorization structures.

---

## 2. Comparative Analysis of Protocol Implementation
- *Actions:* Distinguished between OAuth 2.0 (Authorization), OpenID Connect (Identity), and SAML (Enterprise Identity).
- *Outcome:* Positioned application-side logic—rather than IdP infrastructure—as the primary target for security assessments.

---

## 3. Systematic Identification of Trust Boundary Risks
- *Actions:* Categorized risks across four domains:
  - Mapping: Account linking and identifier collisions.
  - Permissions: Role assignment and over-privileged defaults.
  - Persistence: Partial logout and silent re-authentication.
  - Validation: Mismatched audience (aud) claims and ID token misuse.
- *Outcome:* Defined SSO as an impact-multiplier for API2 (Broken Auth) and API5 (BFLA).

---

## 4. Defensive & Audit Takeaways
- *Actions:* Developed a testing mental model focused on the "Identity ≠ Permission" principle.
- *Outcome:* Recommended strict separation of identity from authorization and explicit mapping of IdP roles to mitigate risk.

---

## Summary
- *Actions:* Completed a comprehensive mental mapping of SSO vulnerabilities, emphasizing that authorization integrity must be maintained independently of the authentication provider.

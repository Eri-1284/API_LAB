# Day 21 — Systematic Methodology for API Authorization Assessment

## 1. Methodological Overview
- **Actions:** Transitioned from ad-hoc testing to structured matrix-driven audit covering OWASP API1, API3, API5.
- **Outcome:** Established testing across Horizontal (Object Ownership) and Vertical (Functional Privilege) axes.

---

## 2. The Five-Step Authorization Framework
- **Actions:**
  - Phase I (Mapping): Identified User Personas and Functional Endpoints.
  - Phase II (Matrix Construction): Developed cross-functional test matrix tracking Token/Endpoint/Object interactions.
  - Phase III (Horizontal Execution): Tested BOLA by manipulating object identifiers within same privilege tier.
  - Phase IV (Vertical Execution): Tested BFLA using sub-privileged credentials against administrative functions.
- **Outcome:** Created comprehensive framework ensuring systematic coverage of all authorization failure modes.

---

## 3. Token Integrity Layer
- **Actions:** Verified Scope and Audience (aud) claims; evaluated responses to expired/revoked tokens.
- **Outcome:** Confirmed token lifecycle enforcement preventing unauthorized persistence.

---

## 4. Consultant-Level Synthesis
- **Actions:** Determined root causes distinguishing localized controller failures from centralized middleware issues; evaluated global ownership checks at database level.
- **Outcome:** Delivered architectural analysis enabling systemic remediation beyond individual endpoints.

---

## Summary
- **Actions:** Established professional-grade methodology rooted in architectural analysis rather than opportunistic discovery.

# Day 06 — Practical JWT Decoding & Inspection

## 1. JWT Structure Verification
- **Actions:** Decoded live JWTs using Python/CLI tools to reveal header and payload data.
- **Actions:** Observed that token data is stored in a human‑readable, plain‑text format.
- **Outcome:** Practiced rapid JWT inspection as part of active API traffic analysis.

---

## 2. High-Value Claim Identification
- **Actions:** Extracted identity and permission claims such as `userId`, `role`, and `scope`.
- **Actions:** Checked for the presence and correctness of the `exp` (expiration) claim.
- **Outcome:** Isolated the parameters most likely to be abused during authorization bypass attempts.

---

## 3. Security Risk Audit
- **Actions:** Treated JWTs as transparent containers of user state rather than as secure storage.
- **Actions:** Reviewed how the API handled reused, expired, or weakly validated tokens.
- **Outcome:** Identified that improperly validated or reused tokens pose a serious Broken Authentication risk.

---

## Summary
- **Actions:** Applied a structured reconnaissance process to decode and interpret JWT contents.
- **Outcome:** Built the ability to quickly spot insecure claim usage and potential weaknesses in how an API validates and trusts JWT-based user identities.

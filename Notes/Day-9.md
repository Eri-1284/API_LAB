# Day 9 — API Security Assessment: GraphQL Authorization

## 1. Object-Level Access Control Audit
- *Actions:* Performed BOLA testing by manipulating query arguments in Burp Repeater.
- *Outcome:* Confirmed the API fails to validate object ownership, enabling unauthorized data exfiltration across the user database.

---

## 2. Field-Level Permission Analysis
- *Actions:* Attempted to retrieve privileged attributes like isAdmin and role through standard user queries.
- *Outcome:* Exposed lack of granular authorization, causing Excessive Data Exposure of internal administrative flags.

---

## 3. Denial of Service (DoS) Risk Assessment
- *Actions:* Tested the API's handling of high-complexity, deeply nested queries.
- *Outcome:* Identified missing query depth limits, making the API vulnerable to resource exhaustion and service instability.

---

## Summary
- *Actions:* Systematically tested GraphQL authorization boundaries from object to field level.
- *Outcome:* Built a methodology for uncovering GraphQL-specific risks tied to its flexible query structure and missing server-side controls.

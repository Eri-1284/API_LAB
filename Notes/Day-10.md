# Day 10 — Advanced GraphQL: Turning Features into Flaws

## 1. One Request, A Thousand Answers (Batching)
- **Actions:** Used aliasing to pack multiple user queries into a single HTTP request.
- **Actions:** Tested whether rate limiting counts HTTP requests or individual GraphQL operations.
- **Outcome:** Confirmed attackers can exfiltrate data rapidly and quietly by batching—server sees one request, attacker gets many results.

---

## 2. Breaking the Server with "Inception" Queries
- **Actions:** Created deeply nested queries with endless relationships (user → posts → comments → author → posts...).
- **Actions:** Pushed query complexity to test for depth limits and resource caps.
- **Outcome:** Proved that unlimited nesting can overwhelm server resources, causing potential crashes without high traffic volume.

---

## 3. Hiding in Plain Sight with Aliases
- **Actions:** Used aliases to rename repeated sensitive fields, evading duplicate detection.
- **Actions:** Tested whether repeated data requests triggered security alerts or logging.
- **Outcome:** Showed GraphQL aliases enable stealthy data theft if developers don't limit alias usage.

---

## Summary
- **Actions:** Exploited legitimate GraphQL features rather than hunting traditional bugs.
- **Outcome:** Understood that developer "features" often become pentester "backdoors" without proper restrictions.

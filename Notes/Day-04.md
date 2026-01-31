# Day 04 — Manual API Security Assessment Workflow

## 1. Attack Surface Mapping (Proxy)
- **Actions:** Utilized the Burp proxy to intercept and log all traffic between the source and destination.
- **Actions:** Filtered the HTTP history to isolate high‑risk API endpoints and methods.
- **Outcome:** Built a technical map of the application's internal architecture and identified hidden or less obvious entry points.

---

## 2. Granular Manipulation (Repeater)
- **Actions:** Transferred intercepted requests into Repeater for isolated experimentation.
- **Actions:** Iteratively modified headers, query parameters, and JSON bodies.
- **Outcome:** Performed targeted testing of server‑side validation logic without any interference from client‑side scripts.

---

## 3. Targeted Vulnerability Simulations
- **Actions:** Broken Object Level Authorization (BOLA): Tested for insecure direct object references via path mutation.
- **Actions:** Information Disclosure: Reviewed error messages and stack traces for signs of sensitive data leakage.
- **Actions:** Improper Input Handling: Sent unexpected or “overflow” JSON keys to observe server behavior.
- **Outcome:** Validated how well the API enforced authorization checks and sanitized or rejected untrusted input.

---

## 4. Technical Insights & Logic Analysis
- **Actions:** Performed a side‑by‑side comparison of Intercept and Repeater workflows.
- **Actions:** Evaluated the impact of custom headers on the application's business logic and decision paths.
- **Outcome:** Refined the manual testing methodology to maximize efficiency and precision during the exploitation phase of a security audit.

---

## Summary
- **Actions:** Applied a structured, repeatable methodology for manual API pentesting focused on mapping, manipulation, and analysis.
- **Outcome:** Confirmed that response analysis—the server’s reaction to tampered data—is the most reliable indicator of underlying vulnerabilities.
- Day 4 established a strong foundation in Burp Suite usage by focusing on HTTP History analysis, Repeater-based testing, and manual request manipulation. This day marks the transition from basic tool usage to structured pentesting workflows." rephrase this and give me 3-4 options with concepts like Focus on the Why, More Concise, etc

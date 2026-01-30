# Day 2 — Practical API Request Control (Burp Workflow)

## 1. Header Control: Probing Server Reliance
- **Actions:** The `User-Agent` string was replaced with a custom, non-standard identifier.
- **Actions:** A new custom header was successfully injected into the request stream via both Postman and Burp Suite.

Purpose:
To practice manipulating the metadata supplied by the client, specifically to test the server's dependency on or differential treatment of specific header values for security logic.

---

## 2. Parameter Injection: Testing Hidden Functionality
- **Actions:** Custom query parameters were dynamically appended to the request URL, specifically:
  - `test=123` (General parameter probing)
  - `debug=true` (Targeted test for information leakage)

Purpose:
To gain hands-on experience in adding and modifying query parameters to discover and test exposed developer features or backend configuration settings.

---

## 3. Resource ID Testing: BOLA/IDOR Simulation
- **Actions:** The static resource identifier in the endpoint path was arbitrarily mutated from `/posts/1` to an assumed valid ID, `/posts/99`.

Purpose:
To establish the mechanics of manipulating direct object references and simulate the initial step of a Broken Object Level Authorization (BOLA) or IDOR attack.

---

## 4. Multi-Object Testing: Access Control Enumeration
- **Actions:** A series of sequential requests was launched, intentionally targeting a range of distinct object IDs:
  - `/posts/1`, `/posts/2`, `/posts/3`, and `/posts/10`.

Purpose:
To perform an initial simulation of how an attacker enumerates resources and tests the integrity of object-level access controls across multiple related data points.

---

## 5. Burp Suite Results: Workflow Validation
- **Outcome:** Requests were reliably intercepted and held for manual inspection and modification within the Burp proxy tool.
- **Outcome:** All intended changes (headers, path, query) were successfully applied and forwarded to the target server.
- **Outcome:** Server responses were closely monitored to analyze how the modifications affected application behavior.

---

## Summary
Day 2 successfully focused on practical control of API requests using the Burp Suite proxy. This established the foundational skill set necessary for identifying common API security flaws through deliberate request manipulation.

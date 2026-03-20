# Day 29: Rate Limiting & Abuse Testing


## I. The Mechanics of Rate Limiting
Rate Limiting is a traffic management strategy that restricts the number of requests a client can make to an API within a specific temporal window. It is the primary defense mechanism against Unrestricted Resource Consumption.
Core Logic:
An API implementation typically uses an algorithm (such as Token Bucket or Leaky Bucket) to track requests.
The Professional Standard: Rate limiting should be enforced at the API Gateway or Load Balancer level to protect the application servers from even processing the overhead of the malicious requests.

---

## II. High-Priority Targeting: Where Limits are Critical
Not all endpoints require the same level of restriction. Pentesters focus on "High-Value" targets where automation yields the highest reward for an attacker.

| Endpoint Category | Example Path | Risk of No Limit |
|--------------------------|--------------------|---------------------|
| Authentication | POST /api/v1/login | Credential stuffing and password brute-forcing. |
| Communication | POST /api/v1/send-otp | SMS toll fraud and notification flooding. |
| Data Extraction |GET /api/v1/search | Full-database scraping and intellectual property theft. |
| Compute Intensive | POST /api/v1/generate-pdf | Denial of Service (DoS) via CPU/Memory exhaustion. |

---

## III. The Pentester’s Testing Workflow
A systematic approach to testing for rate limiting involves more than just "spamming" the server; it requires observing how the server tracks the client.
1.	Baseline Identification: Send a single valid request and note the headers. Look for X-RateLimit-Limit, X-RateLimit-Remaining, and X-RateLimit-Reset.
2.	Automated Stress Test: Use Burp Intruder or a Python script to send 500 requests to a sensitive endpoint (e.g., /reset-password).
3.	Behavioral Analysis: * Does the server return a 429?
- Does it return a 403 Forbidden?
- Does the response time increase (indicating Throttling)?
4.	Recovery Testing: Once blocked, determine the "Cool-down" period. Does the block lift after exactly 60 seconds, or is it an exponential back-off?

---

## IV. Common Rate Limiting Weaknesses & Bypasses
Attackers often find "leaks" in the rate-limiting logic. As a consultant, you must test for these common implementation failures:
### 1. The IP Rotation Fallacy
If a limit is tied strictly to an IP address, an attacker can use a proxy provider to rotate through thousands of residential IPs, making each request appear unique.
- The Fix: Implement limits based on API Keys, User IDs, or Session Tokens in addition to IP.

### 2. The Versioning/Path Leak
Developers often protect /api/v2/login but forget that the deprecated /api/v1/login is still active and lacks any rate-limiting logic.
- The Test: Find a protected endpoint and attempt to call the same function on /v1/, /beta/, or /mobile/ paths.

### 3. Client-Side Enforcement
The most egregious error is enforcing the "wait 60 seconds" logic in the JavaScript/Mobile UI.
- The Bypass: Simply send the request directly to the API using Burp Suite, bypassing the UI logic entirely.

### 4. Header Manipulation
Some Rate Limiters trust headers like X-Forwarded-For or X-Real-IP to identify the user. An attacker can spoof these headers in every request:
- X-Forwarded-For: 1.1.1.1
- X-Forwarded-For: 1.1.1.2 ... and so on.

---

## V. OWASP API Security Mapping
- API4: Unrestricted Resource Consumption: This is the direct mapping. It covers the lack of limits on CPU, memory, storage, and network bandwidth.
- API6: Unrestricted Business Flows: Rate limiting prevents attackers from abusing a business process (like "Create Account") at a scale that harms the business (e.g., creating 1 million fake accounts).

---

## VI. Final Summary Table
| Test Scenario | Tooling | Successful Outcome (Secure) | 
|-------------------|-----------|-----------------------------------------|
| Brute Force Attempt | Burp Intruder | 429 Too Many Requests after 10 tries. |
| Version Bypass | Burp Repeater | Both /v1 and /v2 enforce the same limit. |
| IP Spoofing | Custom Header Script | Server identifies user by Token, not just IP. |
| Resource Exhaustion | Parallel Requests | Server gracefully throttles or denies over-limit tasks. | 

---

### Summary
This module focuses on the defensive architecture of Rate Limiting and the methodologies used to subvert it. Students will explore how APIs protect themselves from automated exhaustion, brute-force, and scraping. By the end of this session, practitioners will be able to identify "choke points" in an API’s infrastructure and conduct stress tests to determine if the backend can distinguish between legitimate high-volume traffic and malicious automated abuse.

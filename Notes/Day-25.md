# Day 25: Exploiting API9 — Breaking Version Parity & Asset Discovery

## I. The Architectural Necessity of Versioning
In modern software development, APIs are not static. To avoid breaking downstream integrations (mobile apps, third-party partners, or legacy microservices), developers implement versioning. This allows the API to evolve without forcing an immediate upgrade on all clients.
Common versioning patterns include:
- URI-Based: https://api.target.com/v1/resource
- Header-Based: X-API-Version: 2.1 or Accept: application/vnd.example.v1+json
- Parameter-Based: https://api.target.com/resource?version=2

---

## II. The Core Vulnerability: Version Drift
Version Drift is a state where security parity is lost across different versions of the same API. While developers focus their security audits and hardening efforts on the "Current" version, legacy versions often fall into a "maintenance-only" mode, where security patches are neglected.
  - Technical Definition: Version Drift occurs when a security control (e.g., an authorization check or data sanitization logic) is present in version $V_n$ but absent in version $V_{n-1}$, despite both versions interacting with the same underlying database.

---

## III. Professional Discovery & Enumeration Logic
A Senior Consultant does not guess versions randomly. Discovery is a structured process of logical manipulation based on identified patterns.
### 1. Identification of Leaks
- JavaScript Bundles: Search for deprecated endpoint strings in frontend code that may still support "legacy" mode for certain users.
- Mobile Binary Analysis: Decompiling APK/IPA files often reveals hardcoded URLs pointing to /v1/ or /beta/ endpoints that are no longer documented.
- Response Metadata: Analyzing JSON responses for apiVersion or deprecation_date keys.

### 2. Logical Probing Table
When a baseline endpoint is discovered, the following variations must be tested:

| Baseline Endpoint | Logical Variation | Target Environment |
|------------------------|----------------------|--------------------------|
| /api/v2/user/me | /api/v1/user/me | Legacy/Deprecated |
| /api/v2/user/me | /api/v3/user/me | Bleeding Edge/In-Dev |
| /api/v2/user/me | /api/beta/user/me | Public Beta |
| /api/v2/user/me | /api/test/user/me | QA/Staging Leak |

---

## IV. Comparative Analysis: Identifying Security Regressions
The most effective way to exploit versioning issues is through Differential Response Analysis. This involves sending the same request to different versions and comparing the output.

**Case Study: Data Over-exposure**
- Request A: GET /api/v2/profile/101
- Response: {"id": 101, "username": "jdoe"} (Secure/Sanitized)
- Request B: GET /api/v1/profile/101
- Response: {"id": 101, "username": "jdoe", "password_hash": "$2b$12$...", "email": "jdoe@internal.com"} (Vulnerable)

**Common Regressions to Test:**
1.	Authorization (BOLA/BFLA): Does /v1/ check if I own the object, even if /v2/ does?
2.	Rate Limiting: Is the /v1/ endpoint missing the 429 "Too Many Requests" protection found in /v2/?
3.	Input Validation: Does /v1/ allow SQL injection or XSS payloads that are blocked by the /v2/ WAF/Logic?

---

## V. OWASP API Security Mapping: API9
This module maps directly to OWASP API9:2023 - Improper Assets Management.
- Definition: This risk occurs when an organization has a lack of visibility into its API surface, including older versions, unpatched endpoints, and staging environments.
- Exploitation: Attackers target the "weakest link" in the version chain. Even if the current API is secure, the existence of an active, vulnerable v1 endpoint renders the entire system's data at risk.
- Remediation Insight: From a consultancy perspective, the recommendation is not just "patching," but implementing strict Deprecation Policies and decommissioning old versions once they reach End-of-Life (EOL).

---

## VI. The Testing Matrix for Versioned APIs

| Step | Action | Objective |
|-------|----------|-------------|
| 01 | Pattern Recognition | Identify the versioning scheme (URI, Header, or Param). |
| 02 | Vertical Probing | Systematically increment/decrement version numbers. |
| 03 | Diffing | Capture responses from all active versions in Burp Suite. |
| 04 | Security Validation | Re-test known vulnerabilities (BOLA/BFLA) against legacy versions. |

---

### Summary: 
This module dissects the security implications of API lifecycle management, focusing on the "shadow" attack surface created by legacy versions. Students will explore the mechanics of Version Drift, where security patches applied to modern endpoints (e.g., /v3/) fail to propagate to older, active versions (e.g., /v1/). By mastering logical version probing and differential response analysis, practitioners will learn to identify critical regressions that bypass modern security controls.

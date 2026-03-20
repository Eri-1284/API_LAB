# Day 30: Weak Asset Management Practices


## I. Defining Improper Asset Management (IAM)
In the context of API security, Asset Management is the ability to maintain an accurate, real-time inventory of all exposed endpoints, their versions, and their intended consumers. Improper Asset Management (IAM) occurs when the rate of API deployment outpaces the rate of API documentation and decommissioning.
The Professional Definition: IAM is the failure to maintain an up-to-date inventory of hosts and API versions, leading to the exposure of unpatched, unmonitored, or redundant entry points into the infrastructure.

---

## II. The "Asset Leak" Lifecycle
Assets typically "leak" into the public attack surface through three primary channels:
1.	**Version Drift (Legacy Leak):** Deploying /v3 but failing to shut down /v1. While /v3 has MFA and rate limiting, /v1 still allows basic authentication and unlimited requests.
2.	**Environment Leak (Dev/Staging):** Developers accidentally expose dev-api.target.com to the public internet. These environments often have "Debug Mode" enabled and use weaker credentials.
3.	**Third-Party/Mobile Divergence:** Creating a specific API for a partner or a mobile app and forgetting its existence once the project is completed, leaving a "zombie" endpoint active.

---

## III. The Pentester’s Discovery Toolkit
Finding unmanaged assets requires a shift from "deep" testing to "wide" reconnaissance.
### 1. Advanced Subdomain Enumeration
Attackers look for naming conventions that suggest non-production environments.
- **Targets:** test-api., beta-api., old-api., internal-api., stg-api..
- **Logic:** If api.target.com exists, there is a high mathematical probability that dev.api.target.com also exists.

### 2. Pattern-Based Version Probing
Using automated tools to "sweep" for version numbers across identified hosts.
- **Scan Path:** /v0/, /v1/, /v1.1/, /v2-beta/.
- **Verification:** Compare the HTTP headers. If v3 returns Server: Cloudflare but v1 returns Server: Apache/2.4.1 (Unix), you have found an unmanaged legacy asset.
### 3. Documentation "Scavenging"
Searching for forgotten Swagger/OpenAPI UI instances.
- **Common Paths:** /swagger-ui.html, /api-docs, /v1/swagger.json, /redoc.
- **The "Schema Leak":** Even if the API requires auth, the JSON Schema might be public, revealing the entire parameter structure and internal logic of the hidden asset.

---

## IV. The Danger of "Shadow" Functionality
Why do attackers prefer a forgotten /v1 over a modern /v3? Because unmanaged APIs are frequently "Security Exempt."
| Feature | Production API (v3) | Shadow/Legacy API (v1) |
|-----------|---------------------------|--------------------------------|
| Authentication | OAuth2 / JWT | Hardcoded API Keys / Basic Auth |
| Rate Limiting | 100 req/min | Unlimited |
| Logging | SIEM / Audit Logs | Local logs only (or none) |
| Data Sanitization | Strict Filtering | Returns full DB objects (API3) |
| Vulnerabilities | Patched Monthly | Likely vulnerable to BOLA/BFLA |

---

## V. Remediation Advocacy: The "Asset Inventory" Approach
As a Senior Consultant, the recommendation for API9 is never just "delete the old API." It requires a structured governance plan:
- Automated Discovery: Use tools to continuously scan the organization's IP space for new or unauthorized 80/443/8080/8443 ports.
- Version Retirement Policy: Implementing a strict "Sunset Policy" where old versions are blocked by the WAF after a specific date.
- Documentation as Code: Ensuring the API cannot be deployed to production unless an OpenAPI (Swagger) definition is automatically generated and registered.

---

## VI. OWASP API Security Mapping: API9
- API9:2023 Improper Assets Management: This is the flagship category for this module. It highlights that you cannot secure what you do not know exists.
- Integration with API3 (Excessive Data Exposure): Shadow APIs are the #1 cause of API3, as old versions often return "everything" because they were built before modern privacy standards (like GDPR) were implemented.

---

### Summary
This module addresses the systemic risk of Shadow APIs—endpoints that exist outside the organization’s formal security visibility. Students will explore the lifecycle of an API and identify the critical failure points where "Temporary" dev environments or "Deprecated" legacy versions transition into permanent, unmonitored liabilities. This session focuses on the methodology of discovering these forgotten assets and mapping the "Security Gap" between hardened production versions and neglected legacy code.

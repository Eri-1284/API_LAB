# Day 18 — Assessment of Broken Function Level Authorization (BFLA)

## 1. Conceptual Framework
- **Actions:** Evaluated BFLA (OWASP API5) as a breakdown in the authorization layer where authenticated entities execute privileged functional logic.
- **Outcome:** Distinguished BFLA from BOLA by focusing on the action type (Method/Endpoint) rather than the resource instance (Object ID).

---

## 2. Root Cause Analysis in API Architecture
- **Actions:** Analyzed the common architectural failure of delegating authorization to the client-side UI or relying solely on OAuth token validity.
- **Outcome:** Concluded that "Identity Propagation" (SSO/OAuth) does not satisfy the requirement for "Local Authorization" (Backend RBAC).

---

## 3. Systematic Testing Protocol
- **Actions:** Conducted a multi-step audit:
  - Step 1: Establish a baseline with a low-privilege authenticated session.
  - Step 2: Targeted functional endpoints (e.g., /internal/export, /admin/create-user).
  - Step 3: Executed replay attacks via Burp Repeater using the unmodified baseline token.
- **Outcome:** Analyzed response headers and status codes to differentiate between proper enforcement (403) and authorization failure (200/204).

---

## 4. Impact Mapping & Mitigation
- **Actions:** Mapped findings to API5, API1, and API2. Recommended implementation of server-side RBAC, granular scope validation, and logging of all administrative function calls.
- **Outcome:** Highlighted the critical need for backend-driven authorization logic in modern API environments.

---

## Summary
- **Actions:** Completed a detailed assessment of function-level security, emphasizing backend authorization independent of authentication providers.

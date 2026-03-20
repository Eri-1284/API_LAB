# Day 26: Identifying and Mining Hidden API Parameters

## I. The Anatomy of Parameter Mining
Parameter Mining (or Parameter Discovery) is the systematic probing of an API to identify input keys that are not listed in the public schema but are actively processed by the backend logic. In the context of an API, parameters are the steering wheel of the request; hidden parameters are the "secret switches" installed by developers.
**The "Shadow" Input Concept:**
Consider a standard request:
GET /api/v1/user/profile?id=402
While id is the documented parameter, the backend code may contain logic for:
GET /api/v1/user/profile?id=402&debug=true&show_permissions=1&as_role=admin
These hidden inputs often bypass the standard "view" of the data, interacting directly with internal business logic.

---

## II. Why Hidden Parameters Exist: The Developer's Perspective
Hidden parameters are rarely malicious by design. They are usually functional artifacts that were never decommissioned or were intended to be "hidden by obscurity."
- Debugging & Diagnostics: Parameters like ?debug=true or ?log=verbose that output stack traces or internal variable states.
- Administrative Overrides: Parameters used by internal support tools to view a user's account "as an admin" (e.g., ?impersonate=user_id).
- Feature Flags: Parameters that toggle "Beta" features or experimental UI elements (e.g., ?alpha_features=on).
- Mobile-Specific Logic: Parameters that change data density for mobile clients to save bandwidth (e.g., ?compact=false).

---

## III. Sources of Parameter Leaks
Professional pentesters treat the application's own code and traffic as a roadmap to hidden parameters.
### 1. JavaScript Static Analysis (Client-Side)
Frontend developers often hardcode parameter keys in their API call functions. By searching the JS bundles, you can find keys that aren't used in the current UI but remain in the code.
- Key Search Patterns: * grep -iE "(\?|&)[a-zA-Z0-9_]+=" bundle.js
- Keywords: admin, config, state, env, internal, test.

### 2. Response Metadata & Hints
APIs often "hint" at what they can do within their own JSON responses.
- The "Expandable" Pattern:
**JSON**
```{
  "user_id": 101,
  "name": "Alice",
  "_links": { "expandable": ["vault", "history", "audit_logs"] }
}
```
- The Probing Logic: This response strongly suggests the existence of a parameter like ?expand=vault.

### 3. Burp Suite History & Traffic Analysis
Observing how the application communicates during different states (login, checkout, profile update) reveals consistent parameter naming conventions. If the app uses ?user_id=, it might also use ?org_id= or ?tenant_id=.

---

## IV. Dangerous Parameter Categories
When mining, specific types of parameters represent high-value targets due to their likelihood of leading to critical vulnerabilities.
| Parameter Type | Example Key | Potential Security Impact |
|----------------------|------------------|-----------------------------------|
| Logic Toggles | debug, test, internal | Information Exposure: Leaking stack traces or env variables. |
| Data Expansion | expand, include, fields | API3 (Excessive Data Exposure): Revealing PII or password hashes. |
| Identity/Role | role, is_admin, sudo | API5 (BFLA): Unauthorized access to admin functions. |
| Object Referencing | userId, accountGuid, fileId | API1 (BOLA): Accessing resources belonging to other users. |
| Filter/Sort | filter, where, search | Insecure Direct Object Reference: Manipulating DB queries. |

---

## V. Tactical Testing Strategy: A Step-by-Step Workflow
Discovery is only half the battle; validation determines the impact.
1.	Baseline Request: Capture a successful request (e.g., GET /api/v1/me).
2.	Inference: Based on the application's purpose, brainstorm logical keys (e.g., if it's a banking app, try ?include_balance=true).
3.	The "Null-Byte" or "Value Shift": Try the parameter with different truthy/falsy values:
- ?admin=1, ?admin=true, ?admin=yes
- ?debug=v, ?debug=1, ?debug=root
4.	Response Comparison: Use a diffing tool to see if the response size or JSON keys changed. Even a 1-byte difference in response size can indicate a successful parameter hit.

---

## VI. OWASP API Security Mapping
The exploitation of hidden parameters is a "force multiplier" for several OWASP API Top 10 vulnerabilities:
- API1: Broken Object Level Authorization (BOLA): Discovering a hidden ?ownerId= parameter may allow an attacker to view resources they don't own by simply changing the value.
- API3: Excessive Data Exposure: Parameters like ?include=all_details may bypass the API's standard data-masking layers, returning sensitive PII.
- API5: Broken Function Level Authorization (BFLA): Identifying an is_admin=true parameter in a POST request can allow a regular user to perform administrative actions, such as changing other users' passwords.

---

### Summary 
This module focuses on the art of Parameter Mining, the process of discovering undocumented or "hidden" inputs that trigger unintended backend behaviors. Students will learn that API security is often compromised not by what is documented, but by the "ghost" parameters left behind for debugging, administrative convenience, or legacy support. By mastering the systematic identification of these inputs, practitioners can unlock access to internal data structures and bypass high-level authorization controls.

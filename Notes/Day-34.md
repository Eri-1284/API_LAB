# Day 34: BFLA Chaining (Function Abuse)


## I: The Function Level Gap

### 1.1 Authentication is Not a Role Check
Developers often implement a global "IsAuthenticated" check but forget the "IsAuthorizedForFunction" check.
  - The Error: The code verifies the JWT is valid, but it doesn't check if the role claim in that JWT matches the requirements for the specific HTTP method or path.

### 1.2 The "Hidden UI" Fallacy
A primary cause of BFLA is the belief that if a button is hidden in the React/Angular frontend, the backend endpoint is secure.
  - The Reality: A pentester ignores the UI and calls the endpoint directly via Burp Suite. If the backend doesn't re-verify the user's role, the "hidden" function is fully exposed.

---

## II: BFLA Discovery & Escalation Paths

### 2.1 Discovering the "Admin" Surface
Administrative endpoints are rarely linked in a standard user's session. They are found by:
  - JS Analysis: Searching for strings like isAdmin, /admin/, /internal/, or system_config.
  - Path Guessing: Attempting to prefix known routes with /admin or /api/v1/mgmt.
  - Documentation Leaks: Finding internal routes in swagger.json that were meant for the dev team.

### 2.2 The "Self-Promotion" Pivot (Vertical Escalation)
The highest impact BFLA is the ability to modify your own permissions.
  - Target: PATCH /api/users/me or POST /api/admin/roles/assign
  - Payload: {"role": "superuser"} or {"permissions": ["*"]}
  - Result: The attacker converts a low-privilege account into a full Administrator.

---

## III: Technical Protocol - The BFLA + BOLA Chain
The most destructive API vulnerabilities occur when Function Abuse meets Object Abuse.
| Step | Vulnerability | Action |
|--------|------------------|----------|
| 1. Access | BFLA | User 101 calls DELETE /api/admin/user/{id}. |
| 2. Scope | BOLA | User 101 changes {id} from their own to the Admin_ID. |
| 3. Result | Critical Impact | A regular user has successfully deleted the System Administrator. |

---

## IV: High-Impact "Function Abuse" Targets

| Function Category | Example Endpoint | Impact of BFLA |
|--------------------------|-------------------------|----------------------|
| User Management | POST /api/admin/invite | Attacker invites their own "Burner" email as an Admin. |
| System Config | PUT /api/v1/settings/maintenance | DoS: Attacker puts the entire site into maintenance mode. |
| Data Export | GET /api/internal/backup/download | Mass Breach: Full database dump via a forgotten export tool. |
| Log Management | DELETE /api/logs/all | Anti-Forensics: Deleting audit trails to hide other attacks. |

---

## V: Professional Testing Methodology
When testing for BFLA, follow this Role-Switching Protocol:
  1.	Identify Privileged Routes: Use your recon notes to find any endpoint containing admin, internal, setup, or manager.
  2.	The "Low-to-High" Request: Take a request that requires high privileges (e.g., GET /api/admin/stats) and replace the Authorization header with a Standard User's token.
  3.	Method Swapping: Sometimes GET /api/admin/users is blocked, but POST /api/admin/users (to create a user) is improperly secured.
  4.	Parameter Injection: If the function is accessible, try to inject your own userId into parameters meant for administrative overrides.

---

## VI: Audit Checklist: BFLA & Privilege Escalation
  - Role Mapping: Have you identified all available roles (User, Manager, Admin, Support)?
  - Cross-Role Testing: Have you attempted to call every "Admin" endpoint using a "User" token?
  - Hidden Path Search: Have you scanned for /api/v1/admin, /api/internal, and /api/mgmt?
  - Method Permutation: Have you tested PUT, POST, and DELETE on administrative resources?
  - State-Change Analysis: If you can access an admin GET request, can you also perform a POST/PATCH to change system state?
  - Self-Service Abuse: Can you use a PATCH /api/v1/users/me request to change your own role_id?
  - Documentation Audit: Does the public Swagger file list "Internal Only" endpoints that respond to standard user tokens?

---

### Summary
Day 34 focuses on Vertical Privilege Escalation via API5: Broken Function Level Authorization (BFLA). While BOLA (Day 32/33) involves horizontal movement between users of the same rank, BFLA involves a user crossing the "Role Boundary" to execute administrative tasks. We examine how "Hidden" administrative endpoints—often discovered through the recon techniques in Days 24-26—can be chained with BOLA to achieve full system compromise. The goal is to move from "I can see an admin page" to "I can promote myself to Admin."

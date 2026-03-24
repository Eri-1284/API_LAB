# Day 35: OAuth Misuse & Token Abuse


## I. The "Valid Token" Trap
The most dangerous assumption a developer can make is: "If the JWT signature is valid, the request is authorized." In reality, a token is only a "Passport"; the API must still check if that passport has the correct Visa for the specific country (service) and activity (scope) being requested.
The Three Pillars of Token Validation:
  1.	Integrity: Is the signature valid? (Technical check)
  2.	Temporality: Is the exp (expiration) in the future? (Time check)
  3.	Context: Is this token meant for this API and this action? (Logic check) <-- Where most bugs hide.

---

## II. Scope Inflation & Enforcement Failures
Scopes (e.g., read, profile, openid) are intended to limit what an application can do on behalf of a user. Scope Inflation occurs when the backend API ignores these claims and grants "Full Access" as long as any valid token is presented.

The Exploit Scenario:
  - The Token: Issued for a "Read-Only" mobile dashboard (scope: "read").
  - The Attack: The attacker intercepts this token and sends it to a destructive endpoint:
  - DELETE /api/v1/users/account/101
  - The Failure: The API checks that the token is "Valid" but fails to check if the scope claim contains delete or admin permissions.

---

## III. Audience Confusion (Cross-Service Pivot)
The aud (Audience) claim in a JWT defines the intended recipient. If an organization uses a single "Central Auth" for multiple microservices, Service A might incorrectly accept a token meant for Service B.
  - Service A (Low Sensitivity): A weather API.
  - Service B (High Sensitivity): A payment API.
  - The Attack: An attacker obtains a token for the Weather API and "replays" it against the Payment API. Because both trust the same Central Auth, the Payment API grants access if it fails to verify that the aud claim matches its own identifier.

---

## IV. Token Type Confusion: Access vs. ID Tokens
In the OpenID Connect (OIDC) world, there are two distinct tokens:
  1.	Access Token: A "KeyCard" meant for APIs.
  2.	ID Token: A "Business Card" meant for the Frontend to show the user's name/picture.

The Vulnerability: Many APIs incorrectly accept the ID Token as a means of authorization. ID tokens often lack scopes and are not intended for resource access. Using an ID token to call an API often bypasses the strict scope-based logic designed for Access Tokens.

---

## V. Token Replay & Binding Failures
A Token Replay occurs when a token captured from a legitimate session is used by an attacker from a different device/IP.
  - The Gap: Most OAuth implementations are "Bearer" tokens—anyone "bearing" the token gets access.
  - The Defense (DPoP): Demonstrating Proof-of-Possession (DPoP) binds the token to a specific private key on the user's device. If DPoP is missing, the token is a "Universal Key" that can be moved from one machine to another without restriction.

---

## VI. Professional Testing Methodology: The Token Audit
To identify OAuth misuse, use a systematic "Claim-vs-Constraint" approach:
  1.	Claim Inspection: Decode the token (jwt.io) and identify the scope, aud, iss, and exp claims.
  2.	Boundary Testing (Scope): Attempt to perform a POST or DELETE using a token that only has read scopes.
  3.	Audience Swapping: If the organization has multiple APIs (e.g., dev.api.com and prod.api.com), try using a Dev token on the Prod endpoint.
  4.	Temporal Testing: Save a token and try using it 10 minutes after its exp time to check if the backend actually checks the clock.
  5.	Type Swapping: Attempt to call a protected API endpoint using the id_token instead of the access_token.

---

## VII. OWASP API Security Mapping
  - API2: Broken Authentication: Specifically the failure to validate token expiration and the misuse of ID tokens.
  - API5: Broken Function Level Authorization: Using a low-scope token to access a high-privilege function.
  - API1: BOLA: Combining a valid (but misused) token with an arbitrary ID change to access other users' data.

## VIII. Summary: Token Vulnerability Matrix

| Attack Type | Root Cause | Impact |
|-----------------|----------------|-----------|
| Scope Bypass | API ignores scope claim. | Unauthorized "Write" actions. |
| Audience Confusion | API ignores aud claim. | Cross-service lateral movement. |
| Type Confusion | API accepts id_token. | Bypass of specific access controls. |
| Expiration Failure | API ignores exp claim. | Perpetual access via "Zombie" tokens. |

---

### Summary 
This module shifts the focus from "stealing" credentials to the Abuse of Trust. Students will learn that a cryptographically "valid" token (correct signature, not expired) can still be a vehicle for high-impact exploitation if the API fails to validate its intent. We will cover the critical gaps in Scope Enforcement, Audience Confusion, and Token Replay, demonstrating how an attacker can take a limited-access token and "coerce" it into performing administrative or cross-service actions.

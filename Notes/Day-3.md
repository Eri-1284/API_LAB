## 1. POST Request
- **Actions:** Used POST requests to create or modify resources on the server instead of just reading them.
- **Actions:** Sent data in the HTTP body rather than placing it in the URL.
- **Outcome:** Confirmed that POST is designed specifically for operations that change application state.

---

## 2. GET vs POST (Quick Tech View)
- **Actions:** Treated GET as the main method for safe, read-only operations.
- **Actions:** Used POST for non-idempotent write actions such as creating or updating records.
- **Actions:** Observed that GET parameters were placed in the URL query string, while POST data lived in the body (JSON, form-data, etc.).
- **Outcome:** Noted that many modern API security issues tend to concentrate in POST/PUT/PATCH logic where writes occur.

---

## 3. Request Body & JSON
- **Actions:** Used the request body to carry structured payloads destined for the backend.
- **Actions:** Chose JSON as the standard format for the API and populated fields like `title`, `body`, and `userId`.
- **Outcome:** Understood that these fields represent typical post/record properties that map directly to backend data models.

---

## 4. Postman POST Run
- **Actions:** Sent a POST request from Postman to `https://jsonplaceholder.typicode.com/posts`.
- **Actions:** Set the `Content-Type: application/json` header before sending the request.
- **Outcome:** Received an HTTP `201 Created` response along with a server-generated `id`, confirming successful resource creation.

---

## 5. Body Manipulation Tests
### IDOR-style
- **Actions:** Edited the `userId` field to alternate values inside the JSON body.
- **Outcome:** The server’s acceptance of these changes suggested weak or missing ownership checks.

### Mass Assignment
- **Actions:** Injected additional fields such as `isAdmin` and `role` into the payload.
- **Outcome:** The dummy API discarded them, but this mirrored how real applications might bind these fields directly into models via Mass Assignment.

---

## 6. Burp Suite Snapshot
- **Actions:** Intercepted the POST request to view raw HTTP details including headers, JSON body, and content length.
- **Actions:** Modified the payload in-flight before forwarding it to the server.
- **Outcome:** Successfully emulated adversarial requests by changing body data at the proxy level.

---

## 7. Python Automation
- **Actions:** Used `requests.post()` with the `json={...}` parameter in Python scripts.
- **Actions:** Replayed and varied POST requests programmatically.
- **Outcome:** Achieved rapid, repeatable API interaction and basic fuzzing through automation.

---

## 8. Core Pentest Points
- **Actions:** Treated all client-controlled JSON fields as untrusted input requiring scrutiny.
- **Actions:** Marked POST endpoints as primary targets during security assessments.
- **Outcome:** Recognized that IDOR and Mass Assignment often depend on how request bodies are parsed and bound, reinforcing the need to systematically review POST/PUT/PATCH for authorization, validation, and strict field whitelisting.

---

## Summary
Day 3 solidified how POST requests change server state and why that makes them high‑value attack targets. You practiced sending JSON bodies with Postman and Python, used Burp to intercept and tamper with those bodies, and treated fields like userId, isAdmin, and role as key spots to probe for IDOR and Mass Assignment issues, especially on POST/PUT/PATCH endpoints.

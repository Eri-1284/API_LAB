# Day 13 — Redirect URI Abuse: Stealing Tokens Without Touching the API

## 1. Why Redirects are a Big Deal
- **Actions:** Looked at how OAuth uses Redirect URIs to send back codes and tokens.
- **Outcome:** Realized that if the "validation" is weak, I don't even need to hack the API—I can just trick the server into sending the keys to the wrong place.
- **Pentester Tip:** This isn't just a UI bug; it's a total breakdown of the authentication flow.

---

## 2. Spotting Weak Spots
- **Actions:** Looked for lazy setups like Wildcards (*), Substring matching (checking if a URL just "contains" a string), or allowing HTTP instead of HTTPS.
- **Outcome:** Found that "Parameter Pollution" can also mess with how the redirect behaves, leading to a leaked token.

---

## 3. The Safe Testing Plan
- **Actions:** Intercepted requests in Burp and moved them to Repeater.
- **Actions:** Followed the "one change at a time" rule: changed a path, then a subdomain, then the protocol.
- **Outcome:** I could tell if the server was being too permissive without actually sending data to a real attacker site. Safety first.

---

## 4. What it Means for the API
- **Actions:** Connected the dots: If I get a token through a redirect flaw, I can impersonate users and hit the API just like a real client.
- **Outcome:** Proved that a broken redirect can lead to a full API compromise, even if the API code itself is perfect.

---

## Summary
- **Actions:** Focused on how to find broken redirect logic safely. If you can control where the token goes, you control the account.

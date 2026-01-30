# Day 13 — Testing OAuth: Messing with Tokens in Burp

## 1. The Pentester Mindset: One Change at a Time
- **Actions:** Captured a clean API request with a working Bearer token to use as my "source of truth."
- **Actions:** Followed the rule: "Change one thing, see what breaks." I'm not looking for new endpoints; I'm looking at how the API handles the key I already have.
- **Outcome:** Realized that if you change too much at once, you never know which mistake the API actually caught.

---

## 2. Breaking the Token (Repeater Fun)
- **Actions:** Tried a few classic moves: Removing the token entirely, sending an Empty/Malformed one, and Replaying a valid token on different endpoints.
- **Actions:** Decoded the JWT just to see what's inside—like scopes and roles—without actually touching them yet.
- **Outcome:** Flagged any 200 OK responses where there should have been a 401/403. If the API lets me in without a proper token, the door is wide open.

---

## 3. Watching for Subtle Clues
- **Actions:** Looked for "Silent Failures"—like getting the same response with a fake token as I do with a real one.
- **Actions:** Analyzed error messages to see if the API is being too "chatty" or if it switches between 401 and 403 randomly.
- **Outcome:** Learned that weak security usually doesn't scream at you; it just stays quiet when it should be stopping you.

---

## 4. Conceptual Swaps
- **Actions:** Thought about Token Substitution (swapping my token for someone else's) to see if the API checks who owns the token.
- **Outcome:** Added this to my mental checklist for later exploitation—no brute-forcing or guessing needed, just smart logic.

---

## Summary
- **Actions:** Used Burp to poke at OAuth handling. Found that the best way to spot a broken API is to see how it reacts when you give it a bad key.

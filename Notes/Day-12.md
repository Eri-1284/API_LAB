# Day — 12 Beyond the Token: Hunting for OAuth Logic Flaws

## 1. The Trust Boundary Mess
- **Actions:** Looked at the three-way relationship between the App, the Auth Server, and the API to find where the "Trust" breaks.
- **Actions:** Asked: "Is the API just blindly trusting the token, or is it actually checking what it's for?"
- **Outcome:** Realized that most hacks happen because the API assumes the Auth Server did all the hard work, which is rarely true.

---

## 2. When Scopes and Audiences Fail
- **Actions:** Checked if a "Read-Only" token can secretly "Write" (Scope fail) and if a mobile app token works on the backend (Audience fail).
- **Actions:** Tried to use an "App A" token to open the door for "App B" (Token Reuse).
- **Outcome:** Found out that when these boundaries collapse, you can jump from a low-level user to having access to the whole microservice.

---

## 3. Refresh Tokens & Admin Fails
- **Actions:** Poked at the Refresh Token to see if I can use a long-lived key to stay logged in forever.
- **Actions:** Tested if the API actually checks the Roles inside the token or if it lets a "Basic User" act like an "Admin."
- **Outcome:** Proved that OAuth doesn't replace real authorization; if the API doesn't check the lock, the key type doesn't matter.

---

## 4. The Game Plan: Logic over Fuzzing
- **Actions:** Focused on the "Remove/Replace/Reuse" strategy to see how the server reacts to different token scenarios.
- **Actions:** Reminded myself: Don't fuzz yet—just look at how the business logic handles the token.
- **Outcome:** Learned to spot the logic flaws that make a "mathematically perfect" token a total security disaster.

---

## Summary
- **Actions:** Spent the day mapping out all the ways an API can mess up its side of the OAuth relationship, proving that the best hacks find where the "Trust" is broken.

# 🗓️ WEEK 2 — Broken Authentication, Sessions & Access Control (Days 8–14)

---

# 🛡️ Day 8 — JWT Attacks (None Algorithm, Key Confusion, Claim Tampering)

**Date:** 27/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Exploiting and hardening JSON Web Token implementations
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
By the end of today, I should be able to:
* Explain how a JWT is structured (header.payload.signature) and why the client can read/modify it.
* Exploit the "none" algorithm vulnerability to forge an unsigned token.
* Exploit algorithm/key confusion (RS256→HS256) to forge a validly-signed token without the private key.
* Apply algorithm pinning and secure token storage as defenses.

### Success Criteria
* Explain why JWTs are "signed, not encrypted" without notes.
* Demonstrate a working "none" algorithm bypass in a lab.
* Demonstrate an RS256-to-HS256 key confusion attack in a lab.
* Document both findings with CVSS estimates.

---

# 📚 2. Topics to Study
### Primary Topic
**JWT Structure & Signature Verification Bypass**
### Secondary Topics
* Base64URL encoding (not encryption) of header/payload
* `jwt_tool` workflow
* `httpOnly`/`Secure`/`SameSite` cookie flags for token storage

### Priority
🔴 **Must Know:** JWTs are readable by anyone holding them — security lives entirely in signature verification
🟡 **Should Know:** how a server that trusts the `alg` header lets an attacker choose "none" or swap algorithms
🟢 **Nice to Know:** JWK header injection (`jku`/`x5u` pointing to attacker-controlled keys)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — JWT Structure and the Trust Assumption
* What it is: three Base64URL segments — header (declares algorithm), payload (claims), signature (proof of integrity)
* Why it matters: anyone can decode and read a JWT; the *only* protection is a correctly enforced signature check
* How it works: if the server's verification logic reads the `alg` field from the token itself and honors it, the attacker controls their own trust model
* Real-world example: numerous CVEs in JWT libraries (2015 auth0 write-up on "none" algorithm) reshaped how libraries now default to explicit algorithm allowlists
* Common mistake: verifying "a signature exists" rather than "the signature matches this specific expected algorithm and key"

### Concept 2 — Algorithm/Key Confusion (RS256 → HS256)
* Definition: RS256 uses a public/private keypair; HS256 uses one shared secret. If a server is configured to accept both and uses the *public key* as the HMAC secret when it sees `alg: HS256`, an attacker who knows the public key (often published) can forge a validly "signed" token
* Architecture/process: attacker changes `alg` to `HS256`, signs the token with the server's own public key as the HMAC secret
* Attack scenario: escalate a normal user token to `{"role": "admin"}` and pass signature verification
* Defense/mitigation: pin the expected algorithm server-side (never trust the token's own `alg` claim); use separate, non-interchangeable key material for different algorithms

### Concept 3 — Secure Token Handling
* Key terminology: `httpOnly` (JS can't read the cookie), `Secure` (HTTPS-only transmission), `SameSite` (CSRF mitigation)
* Practical application: store JWTs in `httpOnly` cookies rather than `localStorage` to reduce XSS-driven token theft
* Common vulnerabilities: long-lived tokens with no revocation path; storing JWTs in `localStorage` where any XSS can exfiltrate them
* Best practices: short expiry + refresh-token rotation; algorithm pinning; signature verification library defaults, not custom parsing

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Algorithm confusion | Server trusts the token's own declared algorithm | Root cause of RS256→HS256 forgery |
| `httpOnly` cookie | Cookie inaccessible to JavaScript | Blocks the most common JWT theft vector (XSS) |
| Claim tampering | Modifying payload fields (e.g., `role`) before re-signing/forging | The actual privilege-escalation payload once signature bypass works |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study JWT structure, decode a real token by hand (Base64URL, no tool), and read how "none" algorithm and key-confusion attacks work.
**Output:** Manually decode a sample JWT and write out header/payload/signature meaning in your own words.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** PortSwigger — "JWT authentication bypass via unverified signature" and "JWT authentication bypass via algorithm confusion"
1. Install and configure `jwt_tool`.
2. Solve the "none" algorithm lab (strip signature, set `alg: none`).
3. Solve the algorithm-confusion lab (sign with the server's public key as an HMAC secret).
4. Escalate to admin via claim tampering in both.

**Expected Result:** Two solved labs + saved `jwt_tool` command history.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A JWT header shows `{"alg": "RS256", "typ": "JWT"}`. What single field would you try changing first, and why?
### Problem 2
You find the app's public key published at `/.well-known/jwks.json`. Explain, step by step, how this enables an HS256 confusion forgery.
### Problem 3
Why does storing a JWT in `localStorage` create risk even if the JWT itself is correctly signed and verified?
### Challenge
Design a JWT verification function (pseudocode) that is immune to both "none" algorithm and algorithm-confusion attacks.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Week 1 — Unified Injection Model
Recall without notes: the trust-boundary pattern across SQLi/NoSQLi/command injection/SSTI. Then connect it forward: JWT algorithm confusion is the *same* pattern — trusting attacker-controlled metadata (the `alg` field) to decide how the server processes data.
### Spaced-Repetition Review
* **Yesterday:** Week 1 wrap-up / portfolio publishing
* **1 week ago:** SQL injection fundamentals

---

# 🧪 6. Active Recall Exercises
1. What is a JWT, structurally?
2. How does the "none" algorithm bypass work?
3. Why does trusting the token's own `alg` claim create a vulnerability?
4. What would an attacker need to exploit algorithm confusion (the public key, typically published)?
5. What's the impact of a forged admin-role JWT on an API?
6. How would you detect JWT tampering in logs (unexpected `alg` values, signature verification failures)?
7. How would you prevent both attacks (pinning + secure storage)?
8. Difference between JWT "signed" and "encrypted"?
9. Real-world example of a JWT algorithm-confusion disclosure.
10. Teach JWT algorithm confusion to a junior developer in plain language.

### Feynman Test
Explain JWT algorithm confusion in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** `jwt_tool`
### Today's Tool Goal
Use `jwt_tool` to scan a token for known vulnerabilities and attempt automated exploitation modes.
### Commands / Features to Practice
```text
python3 jwt_tool.py <token> -M at   # scan for known attacks
python3 jwt_tool.py <token> -X n    # none-algorithm attack
python3 jwt_tool.py <token> -X k -pk public.pem   # key-confusion attack
```
### Tool Success Criteria
I can run `jwt_tool`'s automated attack modes and correctly interpret which attack succeeded and why.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: test Juice Shop's JWT (if used) or a self-built Express+JWT auth demo for "none" algorithm and algorithm confusion; log findings.
### Deliverable
`Day 8: Added JWT vulnerability test results (none-algorithm + algorithm confusion) to findings log.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A mobile banking API uses JWTs signed with RS256, and the public key is exposed at a `/jwks.json` endpoint for client-side verification convenience.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact for a banking API specifically.
4. Demonstrate/reproduce it in an authorized lab analog.
5. Recommend a mitigation.
6. Document the result.
### Difficulty
⭐⭐⭐☆☆

---

# 📊 10. End-of-Day Assessment
| Skill | Score |
|---|---:|
| Conceptual understanding | /5 |
| Hands-on ability | /5 |
| Tool proficiency | /5 |
| Security reasoning | /5 |
| Problem solving | /5 |
| Ability to explain the topic | /5 |

**Total: __/30**

---

# 📦 11. Daily Deliverables
* [ ] Study notes  * [ ] Completed labs (2)  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] `jwt_tool` practice  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. JWT structure and why it's readable but should be tamper-proof
2. "None" algorithm bypass mechanics
3. Algorithm/key confusion mechanics
4. Secure token storage principles
5. How this connects to tomorrow's session-based attacks (a different auth mechanism, same "trust the client" root issue)

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "JWT authentication bypass via jwk header injection"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Forge a token by injecting a self-controlled JWK into the token header, tricking the server into verifying against an attacker-supplied key.
### Environment
Target: PortSwigger lab · Tools: Burp Suite, `jwt_tool` · Prerequisite: earlier JWT labs today
### Lab Tasks
1. Generate your own RSA keypair.
2. Craft a token header including a `jwk` field with your public key embedded.
3. Sign the token with your private key.
4. Submit and confirm the server verifies against your embedded key.
5. Escalate via claim tampering (`role: admin`).
### What I Need to Discover
Why does embedding a key in the header work if the server doesn't validate that the `jwk` is from a trusted source? What's the structural fix (only ever trust a pre-configured key, never one supplied in the token itself)? What's the real-world impact if this reached a production identity provider?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 9 — Session Fixation & Session Hijacking

**Date:** 28/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Session management attacks and secure session lifecycle design
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain the difference between session fixation (attacker sets the session) and session hijacking (attacker steals an existing one).
* Exploit a session-fixation flow where the session ID doesn't regenerate on login.
* Exploit session hijacking via a stolen cookie (chained conceptually from XSS, previewed for Week 3).
* Apply secure cookie flags and session-regeneration-on-login as fixes.

### Success Criteria
* Explain fixation vs. hijacking without notes.
* Demonstrate a session-fixation bypass in a lab.
* Explain why session ID regeneration on privilege change (not just login) matters.

---

# 📚 2. Topics to Study
### Primary Topic
**Session Fixation & Session Hijacking**
### Secondary Topics
* Session ID predictability (weak randomness sources)
* Cookie security flags: `Secure`, `HttpOnly`, `SameSite`
* Session invalidation on logout/password change

### Priority
🔴 **Must Know:** why a session ID must change at every privilege-boundary crossing (anonymous→authenticated, user→admin)
🟡 **Should Know:** how predictable session IDs can be brute-forced or guessed
🟢 **Nice to Know:** session-riding via meta-refresh or cross-subdomain cookie scope issues

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Session Fixation
* What it is: an attacker sets a known session ID on the victim (e.g., via a URL parameter or a shared/public terminal) *before* the victim logs in; if the app doesn't issue a new ID at login, the attacker's known ID is now authenticated
* Why it matters: attacker never needs to steal anything — they predetermined the credential
* How it works: `GET /login?sessionid=ATTACKER_KNOWN_VALUE` → victim logs in → app keeps using that same ID → attacker uses the same ID to access the authenticated session
* Real-world example: older PHP apps that accepted `PHPSESSID` from the URL were classically vulnerable to this
* Common mistake: only invalidating sessions on logout, never regenerating on login

### Concept 2 — Session Hijacking
* Definition: stealing a *legitimate* active session ID (via XSS cookie theft, network sniffing on HTTP, or session-ID prediction) and reusing it
* Architecture/process: cookie theft → replay the stolen cookie value → server treats the attacker as the original authenticated user
* Attack scenario: a stored XSS payload (`document.cookie` exfiltration) on an app that stores session tokens without `HttpOnly`
* Defense/mitigation: `HttpOnly` blocks JS-based theft; `Secure` blocks plaintext network capture over HTTP; short session lifetimes limit the exploitation window

### Concept 3 — Secure Session Lifecycle
* Key terminology: session regeneration, session invalidation, idle timeout, absolute timeout
* Practical application: regenerate the session ID at login, at logout, and at any privilege escalation (e.g., entering an admin panel)
* Common vulnerabilities: session tokens that never expire, or that persist server-side after logout
* Best practices: server-side session invalidation on logout (not just clearing the client cookie), short idle timeouts for sensitive apps

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Session fixation | Attacker predetermines the session ID before authentication | Bypasses the need to steal anything |
| Session hijacking | Attacker steals a live, legitimate session ID | Common outcome of XSS or unencrypted transport |
| Session regeneration | Issuing a fresh ID at each trust-boundary crossing | The core structural fix for fixation |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study fixation vs. hijacking, and the cookie-flag defenses.
**Output:** A short written comparison table: Fixation vs. Hijacking — attack vector, prerequisite, and fix.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** PortSwigger — "Authentication bypass via session ID" or equivalent session-fixation-style lab (choose from the Authentication lab category); simulate hijacking by manually replaying a captured session cookie between two browser profiles.
1. Identify whether the app issues a new session ID at login.
2. If not, demonstrate fixation by pre-setting a session ID and having a "victim" (second browser profile) log in with it.
3. Manually copy a valid session cookie into a second browser/incognito profile and confirm hijacking works (simulating what XSS/network capture would achieve).
4. Add `HttpOnly`, `Secure`, `SameSite=Strict` flags to a local Express session config and re-test.

**Expected Result:** A documented fixation/hijacking demonstration + a before/after Express session config diff.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
An app accepts `?sessionid=` as a URL parameter and uses it directly. Explain the exact fixation attack chain against a victim you can send a link to.
### Problem 2
A session cookie lacks `HttpOnly`. Explain, referencing Week 3's upcoming XSS topic, exactly how an attacker would exfiltrate it.
### Problem 3
Why is regenerating the session ID at login alone not sufficient — what privilege-escalation moment (Day 11's IDOR topic) also needs regeneration?
### Challenge
Design a secure session lifecycle (pseudocode/flow) covering login, privilege escalation, idle timeout, and logout.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: JWT Attacks (Day 8)
Recall without notes: the "none" algorithm bypass and algorithm-confusion attack, and why algorithm pinning fixes both.
### Spaced-Repetition Review
* **Yesterday:** JWT attacks
* **1 week + 1 day ago:** SQL injection

---

# 🧪 6. Active Recall Exercises
1. What is session fixation?
2. What is session hijacking?
3. Why does regenerating the session ID at login stop fixation?
4. What would an attacker need to exploit hijacking (a theft vector — XSS, network sniffing, prediction)?
5. What's the impact of a hijacked admin session vs. a hijacked regular user session?
6. How would you detect session anomalies in logs (same session ID from wildly different IPs/user agents)?
7. How would you prevent fixation and hijacking respectively?
8. Difference between fixation and hijacking, one sentence each?
9. Real-world example of a session-fixation-vulnerable framework default.
10. Teach session regeneration to a junior developer in plain language.

### Feynman Test
Explain session fixation vs. hijacking in 3–5 sentences total. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (cookie manipulation) + browser DevTools (Application/Storage tab)
### Today's Tool Goal
Practice inspecting and manually editing cookies across two browser contexts to simulate hijacking, and inspecting `Set-Cookie` headers for flag presence.
### Commands / Features to Practice
```text
DevTools > Application > Cookies > inspect HttpOnly/Secure/SameSite flags
Burp Proxy > HTTP history > inspect Set-Cookie response headers
```
### Tool Success Criteria
I can identify from raw HTTP headers alone whether a session cookie is missing critical security flags.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: inspect Juice Shop's session/cookie handling for fixation risk and missing cookie flags; log findings.
### Deliverable
`Day 9: Documented session cookie flag audit (HttpOnly/Secure/SameSite) and fixation test results for Juice Shop.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An internal HR portal's session cookie has no `Secure` flag and the app is accessible over both HTTP and HTTPS on the corporate network.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact (network-position attacker on shared Wi-Fi/corporate LAN).
4. Demonstrate/reproduce conceptually (network capture is out of scope for hands-on, so describe the attack chain in detail instead).
5. Recommend a mitigation.
6. Document the result.
### Difficulty
⭐⭐☆☆☆

---

# 📊 10. End-of-Day Assessment
| Skill | Score |
|---|---:|
| Conceptual understanding | /5 |
| Hands-on ability | /5 |
| Tool proficiency | /5 |
| Security reasoning | /5 |
| Problem solving | /5 |
| Ability to explain the topic | /5 |

**Total: __/30**

---

# 📦 11. Daily Deliverables
* [ ] Study notes  * [ ] Completed lab demonstration  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice (cookie inspection)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Fixation vs. hijacking, cleanly distinguished
2. Cookie security flags and what each one blocks
3. Session regeneration at every trust-boundary crossing
4. How this connects to tomorrow's OAuth topic (another identity/trust mechanism)
5. A real scenario tying session security to business impact

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy (Authentication category)
**Lab Name:** "2FA broken logic" or a session-management-focused Authentication lab (select based on current lab availability)
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Identify a flaw in session/authentication state management that allows bypassing an intended security control.
### Environment
Target: PortSwigger lab · Tools: Burp Suite · Prerequisite: today's fixation/hijacking concepts
### Lab Tasks
1. Map the full login/session flow (cookies set, when, and under what conditions).
2. Identify where session state is trusted without re-verification.
3. Exploit the gap to bypass the intended control.
4. Document the exact request sequence that achieves the bypass.
5. Propose the structural fix.
### What I Need to Discover
At which exact step does the app trust something it shouldn't? Is this a fixation-style issue, a hijacking-style issue, or a related session-state logic flaw? What's the fix that closes the *class* of bug, not just this instance?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 10 — OAuth 2.0 Misconfigurations

**Date:** 29/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** OAuth 2.0 flow security — redirect URI validation, state parameter, PKCE
**Estimated Total Time:** 3–4 hours
**Difficulty:** Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain the OAuth 2.0 authorization code flow end-to-end and where trust decisions happen.
* Exploit a redirect URI validation flaw to steal an authorization code/token.
* Exploit a missing `state` parameter to perform CSRF on the OAuth login flow.
* Apply PKCE and strict redirect URI validation as fixes.

### Success Criteria
* Explain the authorization code flow without notes.
* Demonstrate a redirect URI bypass in a lab.
* Demonstrate (or clearly explain) a missing-`state` CSRF scenario.
* Explain what PKCE adds for public clients (mobile/SPA).

---

# 📚 2. Topics to Study
### Primary Topic
**OAuth 2.0 Authorization Code Flow & Common Misconfigurations**
### Secondary Topics
* Redirect URI allowlist validation (exact match vs. prefix/substring match pitfalls)
* `state` parameter as CSRF protection
* PKCE (Proof Key for Code Exchange) for public clients

### Priority
🔴 **Must Know:** the authorization code flow's message sequence (client → auth server → redirect with code → client exchanges code for token)
🟡 **Should Know:** why loose redirect URI matching (`*.example.com` or prefix match) is exploitable via open redirects or subdomain takeover
🟢 **Nice to Know:** implicit flow deprecation reasoning (token exposed in URL fragment)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — The Authorization Code Flow
* What it is: the standard OAuth 2.0 flow where a client redirects the user to an authorization server, which redirects back with a short-lived code, later exchanged server-side for a token
* Why it matters: nearly every "Login with Google/GitHub/Microsoft" button uses this flow — misconfiguring it compromises identity across every relying app
* How it works: `client → /authorize?redirect_uri=X&state=Y → user approves → auth server redirects to X with ?code=Z&state=Y → client exchanges code for token`
* Real-world example: numerous bug bounty reports involve redirect URI bypasses against major SSO providers
* Common mistake: validating redirect URI with substring/prefix matching instead of exact match against a registered allowlist

### Concept 2 — Redirect URI Manipulation
* Definition: tricking the authorization server into sending the authorization code to an attacker-controlled (or attacker-influenced) URI
* Architecture/process: if `redirect_uri=https://legit.com.attacker.com` or `redirect_uri=https://legit.com/callback/../../attacker-path` passes a loose validator, the code goes to the attacker
* Attack scenario: attacker sends victim a crafted authorization link; victim approves (trusting the real domain in the address bar); code is redirected to attacker-controlled endpoint; attacker exchanges it for a token
* Defense/mitigation: exact-match redirect URI validation against a pre-registered allowlist — no wildcards, no prefix matching

### Concept 3 — CSRF on the OAuth Flow (Missing `state`)
* Key terminology: `state` parameter — an unpredictable value tying the authorization request to the eventual callback
* Practical application: without `state`, an attacker can initiate their own OAuth flow, capture the resulting code, and trick a victim into completing the callback — linking the *attacker's* third-party account to the *victim's* session (account-linking CSRF)
* Common vulnerabilities: treating `state` as optional or using a predictable/static value
* Best practices: cryptographically random, single-use `state`, validated on callback; PKCE for any public client that can't securely store a client secret

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Redirect URI | Where the auth server sends the authorization code | Loose validation here leaks codes to attackers |
| `state` parameter | Anti-CSRF token binding request to callback | Missing it enables OAuth login CSRF |
| PKCE | Proof Key for Code Exchange | Protects public clients that can't hold a secret from code interception |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study the full authorization code flow diagram and both misconfiguration classes.
**Output:** Draw (or describe in writing) the full OAuth authorization code flow with the `state` parameter and redirect URI validation points labeled.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** PortSwigger — "Authentication bypass via OAuth implicit flow" and "Forced OAuth profile linking" (or nearest available OAuth-category labs)
1. Map the target's OAuth flow in Burp (proxy history).
2. Identify the redirect URI validation logic by testing variations (subdomain, path traversal, open redirect chaining).
3. Successfully redirect the authorization response to an attacker-controlled endpoint.
4. Identify whether `state` is present, and if absent, explain the CSRF chain (safe description if not directly exploitable in the lab).

**Expected Result:** One or two solved labs + a documented redirect URI bypass technique.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A redirect URI validator checks `redirect_uri.startsWith("https://app.example.com")`. Craft a bypass value.
### Problem 2
An OAuth flow has no `state` parameter. Write the step-by-step CSRF attack that links the attacker's account to the victim's session.
### Problem 3
Why does PKCE not help against a redirect URI bypass (they solve different problems) — what does PKCE actually prevent?
### Challenge
Design a secure redirect URI validation function (pseudocode) that is immune to prefix, subdomain, and open-redirect-chaining bypasses.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Session Fixation & Hijacking (Day 9)
Recall without notes: fixation vs. hijacking and the cookie flags that defend against each.
### Spaced-Repetition Review
* **Yesterday:** Session fixation & hijacking
* **2 days ago:** JWT attacks
* **Connect the pattern:** JWTs, sessions, and OAuth are all "identity/trust token" mechanisms — each has its own specific misconfiguration class, but all fail the same way: trusting something the client controls without independent verification.

---

# 🧪 6. Active Recall Exercises
1. What is the OAuth 2.0 authorization code flow, step by step?
2. How does redirect URI manipulation lead to code/token theft?
3. Why does loose (prefix/substring) redirect URI matching fail?
4. What would an attacker need to exploit missing `state` (ability to get a victim to click a crafted link)?
5. What's the impact of an account-linking CSRF via OAuth?
6. How would you detect anomalous OAuth callback patterns in logs?
7. How would you prevent both classes (exact-match allowlist + mandatory random `state`)?
8. Difference between what `state` protects against and what PKCE protects against?
9. Real-world example of an OAuth redirect URI bug bounty finding.
10. Teach the OAuth authorization code flow to a junior developer in plain language.

### Feynman Test
Explain OAuth redirect URI manipulation in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (Proxy history filtering for OAuth flows)
### Today's Tool Goal
Practice tracing a full multi-redirect OAuth flow through Burp's proxy history and identifying each trust decision point.
### Commands / Features to Practice
```text
Burp Proxy > HTTP history > filter by "authorize", "callback", "token"
Manually replay the /authorize request with modified redirect_uri in Repeater
```
### Tool Success Criteria
I can trace an entire OAuth login flow through Burp and point to exactly where redirect URI and `state` are validated (or not).

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: if Juice Shop or a personal project uses OAuth/social login, test it; otherwise, write a standalone "OAuth Misconfiguration" analysis using a deliberately vulnerable local OAuth demo app as your test target.
### Deliverable
`Day 10: Documented OAuth redirect URI and state-parameter analysis (local demo app or Juice Shop, as applicable).`

---

# 📝 9. Practice / Security Challenge
### Scenario
A SaaS product's "Login with Google" integration validates redirect URIs with a regex `^https://.*\.saasapp\.com$`.
### My Task
1. Identify the vulnerability/threat (regex anchoring pitfalls).
2. Explain the root cause.
3. Determine the impact (token/code theft leading to account takeover).
4. Demonstrate/reproduce the bypass logic explicitly (even if only as a written proof-of-concept given regex constraints).
5. Recommend a mitigation.
6. Document the result.
### Difficulty
⭐⭐⭐⭐☆

---

# 📊 10. End-of-Day Assessment
| Skill | Score |
|---|---:|
| Conceptual understanding | /5 |
| Hands-on ability | /5 |
| Tool proficiency | /5 |
| Security reasoning | /5 |
| Problem solving | /5 |
| Ability to explain the topic | /5 |

**Total: __/30**

---

# 📦 11. Daily Deliverables
* [ ] Study notes  * [ ] Completed lab(s)  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice (OAuth flow tracing)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. The full OAuth authorization code flow
2. Redirect URI validation pitfalls and the exact-match fix
3. What `state` protects against and what PKCE protects against
4. How OAuth misconfigurations connect to tomorrow's access control topic (both are "who's allowed to do what" failures)
5. A real bug bounty-style OAuth scenario

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "OAuth account hijacking via redirect_uri"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Hijack another user's OAuth login by manipulating the `redirect_uri` parameter to exfiltrate their authorization code.
### Environment
Target: PortSwigger lab · Tools: Burp Suite (Collaborator for exfiltration confirmation) · Prerequisite: today's OAuth flow mapping
### Lab Tasks
1. Map the full flow and identify the redirect_uri validation logic.
2. Identify a bypass (open redirect chaining, path confusion, or similar).
3. Craft a malicious authorization link that redirects the code to your listener.
4. Capture the code and exchange it for a token/session.
5. Document the full attack chain.
### What I Need to Discover
Exactly which validation step is insufficient? Would exact-match allowlisting have stopped this at any point in the chain? What's the blast radius if this were a real SSO provider used across multiple relying apps?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 11 — Broken Access Control (IDOR & Privilege Escalation)

**Date:** 30/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Detecting and fixing horizontal and vertical access control failures
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain the difference between horizontal privilege escalation (accessing another user's data) and vertical (accessing higher-privilege functionality).
* Exploit an Insecure Direct Object Reference (IDOR) to access another user's resource.
* Exploit a vertical privilege escalation (e.g., reaching an admin-only endpoint as a regular user).
* Apply middleware-level authorization checks and ownership validation as fixes.

### Success Criteria
* Explain horizontal vs. vertical privilege escalation without notes.
* Demonstrate a working IDOR exploitation in a lab.
* Demonstrate a working vertical privilege escalation in a lab.
* Document both with CVSS estimates including the authZ-specific impact factors.

---

# 📚 2. Topics to Study
### Primary Topic
**Broken Access Control — IDOR & Privilege Escalation**
### Secondary Topics
* Object-level vs. function-level authorization
* Parameter tampering to enumerate other users' resources
* Middleware-based centralized authorization patterns

### Priority
🔴 **Must Know:** the authN vs. authZ distinction (from your original baseline diagnostic gap) — authentication proves *who* you are; authorization decides *what you're allowed to do*, and it must be checked on every single request, not just at login
🟡 **Should Know:** why relying on "security through obscurity" (hard-to-guess IDs) is not access control
🟢 **Nice to Know:** mass assignment as a related vertical-escalation vector (client sends `{"role": "admin"}` in a profile-update request)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — AuthN vs. AuthZ (Closing Your Baseline Gap)
* What it is: authentication (authN) establishes identity; authorization (authZ) determines permitted actions for that identity — they are separate checks that must both happen, on every request
* Why it matters: this was flagged as a diagnostic gap at the start of your transition — nearly all of today's vulnerabilities stem from an app doing authN correctly but skipping or under-scoping authZ
* How it works: a request can be perfectly authenticated (valid session/JWT) and still be completely unauthorized (that user should not access this specific resource or function)
* Real-world example: countless breach reports describe "the user was logged in, but shouldn't have been able to see/do that" — this is a pure authZ failure
* Common mistake: checking authZ only in the UI (hiding a button) while the underlying API endpoint performs no server-side check

### Concept 2 — IDOR (Horizontal Escalation)
* Definition: an app exposes an internal object reference (ID, filename, key) directly, and doesn't verify the requester owns/may access that specific object
* Architecture/process: `GET /api/invoices/1042` returns invoice 1042 regardless of whether the requesting user owns it
* Attack scenario: incrementing or enumerating IDs to read/modify other users' orders, invoices, messages, or profile data
* Defense/mitigation: server-side ownership check on every object-level request (`WHERE invoice.user_id = session.user_id`), not just relying on unguessable IDs

### Concept 3 — Vertical Privilege Escalation
* Key terminology: function-level authorization, role-based access control (RBAC)
* Practical application: a regular user directly requesting an admin-only endpoint (`/api/admin/users`) that has no server-side role check
* Common vulnerabilities: admin routes protected only by hiding the link in the UI, or checking role client-side in JavaScript
* Best practices: centralize authorization logic in middleware applied to every route, never trust client-side role claims, re-verify role server-side on every privileged action

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| AuthN vs. AuthZ | Identity verification vs. permission verification | The conceptual foundation for this entire topic and your original diagnostic gap |
| IDOR | Accessing another user's object via a guessable/enumerable reference | One of the most common real-world API vulnerabilities |
| Function-level authZ | Server-side check that a role may invoke a specific action | Prevents vertical escalation regardless of UI hiding |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study the authN/authZ distinction explicitly, then IDOR and vertical escalation mechanics.
**Output:** A written explanation, in your own words, of why "the user is logged in" and "the user is allowed to do this" are two separate questions an app must answer independently.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** PortSwigger — "IDOR" labs (e.g., "Insecure direct object references") and one Access Control lab targeting vertical escalation (e.g., "Unprotected admin functionality")
1. Solve an IDOR lab by enumerating/guessing another user's object ID.
2. Solve a vertical-escalation lab by directly requesting an admin-only route as a regular user.
3. Locally: add an ownership-check middleware to a small Express route and confirm the IDOR no longer works.

**Expected Result:** Two solved labs + a local before/after middleware fix.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
`GET /api/orders/{id}` returns any order by ID with no ownership check. Write the middleware logic (pseudocode) that fixes this generically for all object routes.
### Problem 2
An admin panel is reachable at `/admin/dashboard` and is only hidden via client-side routing (`if (user.role === 'admin') showLink()`). Explain exactly how to bypass this and why it's a vertical escalation, not IDOR.
### Problem 3
Explain, in your own words closing your original diagnostic gap, precisely why "the request had a valid JWT" does not mean "the request was authorized."
### Challenge
Design a centralized authorization middleware pattern (pseudocode) that enforces both object-level (IDOR) and function-level (vertical) checks consistently across an entire Express API.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: OAuth 2.0 Misconfigurations (Day 10)
Recall without notes: redirect URI manipulation and the `state` parameter's role.
### Spaced-Repetition Review
* **Yesterday:** OAuth misconfigurations
* **3 days ago:** JWT attacks
* **Connect the pattern:** JWT, session, OAuth, and now access control are the full "who are you, and what can you do" pipeline — today closes the loop by focusing purely on the authZ half.

---

# 🧪 6. Active Recall Exercises
1. What is the difference between authentication and authorization?
2. How does IDOR work?
3. Why does hiding a UI element not constitute access control?
4. What would an attacker need to exploit IDOR (a predictable/enumerable object reference and no server-side ownership check)?
5. What's the impact of vertical escalation to an admin role on a multi-tenant SaaS app?
6. How would you detect IDOR attempts in logs (sequential ID access patterns from one account)?
7. How would you prevent both IDOR and vertical escalation (centralized middleware, ownership checks)?
8. Difference between horizontal and vertical privilege escalation?
9. Real-world example of an IDOR-driven data breach.
10. Teach the authN/authZ distinction to a junior developer in plain language.

### Feynman Test
Explain the authN vs. authZ distinction, and how IDOR exploits the authZ gap, in 3–5 sentences. If unclear, mark 🟡 **Needs Review** — this closes one of your original baseline diagnostic gaps, so it's worth getting fully solid.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (Intruder for ID enumeration)
### Today's Tool Goal
Use Burp Intruder to enumerate a range of object IDs and identify which return data the current session shouldn't have access to.
### Commands / Features to Practice
```text
Burp Intruder > Sniper attack > payload position on the ID parameter > numeric range payload
Compare response status/length/content across the ID range
```
### Tool Success Criteria
I can set up an Intruder attack to sweep an ID range and quickly spot anomalous (successful) responses indicating IDOR.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: test Juice Shop's order/basket/user-profile endpoints for IDOR and any admin routes for vertical escalation; log findings — this is typically one of the richest vulnerability categories in Juice Shop.
### Deliverable
`Day 11: Documented IDOR and vertical privilege escalation findings across order/profile/admin endpoints.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A telehealth app's `/api/appointments/{id}` endpoint returns full appointment details (including diagnosis notes) for any ID, regardless of which patient is logged in.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact given the sensitivity of health data specifically.
4. Demonstrate/reproduce in an authorized lab analog.
5. Recommend a mitigation.
6. Document the result.
### Difficulty
⭐⭐⭐☆☆

---

# 📊 10. End-of-Day Assessment
| Skill | Score |
|---|---:|
| Conceptual understanding | /5 |
| Hands-on ability | /5 |
| Tool proficiency | /5 |
| Security reasoning | /5 |
| Problem solving | /5 |
| Ability to explain the topic | /5 |

**Total: __/30**

---

# 📦 11. Daily Deliverables
* [ ] Study notes  * [ ] Completed labs (2)  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice (Intruder ID sweep)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. AuthN vs. authZ, cold, closing your baseline diagnostic gap
2. IDOR mechanics and the ownership-check fix
3. Vertical escalation mechanics and the middleware fix
4. Why UI-hiding is never a security control
5. How access control connects forward to tomorrow's credential-attack topic (both are about "who gets to act as whom")

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "User role controlled by request parameter" and "URL-based access control can be circumvented"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Escalate privileges by tampering with a client-controlled role parameter, and separately bypass a URL-pattern-based (not identity-based) access control implementation.
### Environment
Target: PortSwigger labs · Tools: Burp Suite · Prerequisite: today's IDOR lab
### Lab Tasks
1. Identify a request that includes a role/permission field controllable by the client.
2. Modify it to escalate to admin.
3. In the second lab, identify how access control is enforced (URL pattern vs. identity) and find the bypass (e.g., trailing slash, case change, alternate HTTP method).
4. Access the protected admin function in both cases.
5. Document both root causes precisely — they are different failure mechanisms.
### What I Need to Discover
Why is trusting a client-supplied role field fundamentally broken, structurally? Why does URL-pattern-based access control (rather than identity/role-based middleware) fail against alternate paths/methods? What's the single unifying fix that would prevent both?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 12 — Password Storage & Credential Attacks

**Date:** 31/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Credential attack techniques and secure password storage
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain why fast general-purpose hashes (MD5, SHA-256) are unsuitable for password storage, and why slow/adaptive hashes (bcrypt, argon2) are correct.
* Use `hashcat` to crack a set of weakly-hashed passwords in a lab.
* Explain credential stuffing and rate-limiting/breach-detection defenses.
* Implement a secure password storage scheme (bcrypt/argon2 + salting, already handled internally by these libraries).

### Success Criteria
* Explain "slow hash" reasoning without notes.
* Demonstrate a successful `hashcat` crack against sample weak hashes in a lab.
* Explain the difference between credential stuffing and brute-forcing.

---

# 📚 2. Topics to Study
### Primary Topic
**Password Storage Cryptography & Credential Attacks**
### Secondary Topics
* Rainbow tables and why unsalted hashes enable them
* `hashcat` modes and wordlist/rule-based attacks
* Credential stuffing vs. brute-force vs. password spraying
* Rate limiting and breach-detection integration (e.g., Have I Been Pwned API pattern)

### Priority
🔴 **Must Know:** why bcrypt/argon2 (slow, salted, adaptive cost) are correct and MD5/SHA-family (fast, general-purpose) are wrong for passwords
🟡 **Should Know:** the distinction between credential stuffing (reused breached credentials, checked at scale) and traditional brute-force
🟢 **Nice to Know:** pepper (an additional server-side secret beyond the per-user salt) as defense-in-depth

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Why Fast Hashes Fail for Passwords
* What it is: general-purpose hash functions (MD5, SHA-1, SHA-256) are designed to be *fast* — exactly the wrong property for password storage
* Why it matters: a fast hash lets an attacker with a stolen hash database test billions of guesses per second on commodity GPUs
* How it works: a GPU can compute billions of SHA-256 hashes/second but only thousands of bcrypt hashes/second at a reasonable cost factor, because bcrypt is deliberately slow and memory/CPU-tunable
* Real-world example: the 2012 LinkedIn breach (unsalted SHA-1) was cracked at massive scale within days
* Common mistake: adding a hand-rolled "salt" to a fast hash instead of using a purpose-built adaptive algorithm

### Concept 2 — Rainbow Tables and Salting
* Definition: a rainbow table is a precomputed lookup of hash→plaintext for common passwords; salting (a unique random value per password, stored alongside the hash) defeats this by making precomputation infeasible at scale
* Architecture/process: bcrypt/argon2 generate and embed a unique salt automatically as part of their hash output format
* Attack scenario: an unsalted password database lets attackers reuse one massive precomputed table across every user simultaneously
* Defense/mitigation: use a library that handles salting internally (bcrypt, argon2) rather than implementing salting manually

### Concept 3 — Credential Stuffing & Rate Limiting
* Key terminology: credential stuffing (testing known breached username/password pairs across many sites), password spraying (a few common passwords across many accounts), brute-force (exhaustive guessing against one account)
* Practical application: rate limiting per-account and per-IP, CAPTCHA after repeated failures, checking new passwords against known-breached-password lists at signup
* Common vulnerabilities: no rate limiting at all, or rate limiting only per-IP (easily defeated by distributed botnets)
* Best practices: layered defense — rate limiting + anomaly detection + breach-list checking + optional MFA

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Adaptive hash | A hash function with a tunable cost factor (bcrypt/argon2) | Makes brute-forcing computationally expensive by design |
| Rainbow table | Precomputed hash→plaintext lookup table | Defeated by proper per-password salting |
| Credential stuffing | Testing breached credentials across many sites at scale | Different threat model than traditional brute-force; needs different defenses |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study why fast hashes fail, how bcrypt/argon2 work conceptually, and the credential-attack taxonomy.
**Output:** A written explanation of why "just add a random string before hashing with SHA-256" is still an inferior approach to bcrypt/argon2.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** Local `hashcat` exercise against a provided/sample weak-hash wordlist (e.g., a small set of unsalted MD5 password hashes from a public CTF training set), plus implementing bcrypt in a small Node.js script
1. Run `hashcat` in dictionary-attack mode against sample MD5 hashes using a common wordlist (`rockyou.txt` or similar training wordlist).
2. Successfully crack at least 2–3 weak passwords.
3. Write a small Node.js script using `bcrypt` to hash and verify a password, inspecting the output format (cost factor + embedded salt).
4. Attempt (and confirm the impracticality of) a naive dictionary attack against your bcrypt hash at a reasonable cost factor, noting the speed difference.

**Expected Result:** Documented `hashcat` crack results + a working bcrypt hash/verify script with notes on the speed contrast observed.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A breach dump shows unsalted SHA-256 password hashes. Explain exactly why this is crackable at scale and what the fix would have been.
### Problem 2
Explain the difference between credential stuffing and brute-force in terms of what defense actually stops each one.
### Problem 3
Why doesn't per-IP rate limiting alone stop credential stuffing from a botnet with thousands of IPs?
### Challenge
Design a layered login-defense architecture (pseudocode/diagram) combining bcrypt/argon2 storage, rate limiting, and breach-list checking at signup.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Broken Access Control (Day 11)
Recall without notes: authN vs. authZ, IDOR mechanics, vertical escalation mechanics.
### Spaced-Repetition Review
* **Yesterday:** Broken Access Control
* **4 days ago:** OAuth misconfigurations
* **Full-week connection:** JWT (Day 8) → Sessions (Day 9) → OAuth (Day 10) → Access Control (Day 11) → Credentials (Day 12) trace the complete identity lifecycle: proving who you are, maintaining that proof, delegating it to third parties, deciding what it authorizes, and protecting the underlying secret that made it possible in the first place.

---

# 🧪 6. Active Recall Exercises
1. Why are fast hashes (MD5/SHA-256) wrong for password storage?
2. How do bcrypt/argon2 make cracking computationally expensive?
3. Why does salting defeat rainbow tables?
4. What would an attacker need for a `hashcat` dictionary attack (the hash + a wordlist)?
5. What's the impact of an unsalted, fast-hashed password database breach at internet scale?
6. How would you detect credential stuffing in logs (many failed logins across many accounts from overlapping IP ranges, or a burst of logins using known-breached credentials)?
7. How would you prevent credential stuffing specifically (vs. brute-force)?
8. Difference between credential stuffing, password spraying, and brute-force?
9. Real-world example of a breach caused by weak password hashing (LinkedIn 2012).
10. Teach "why bcrypt, not SHA-256" to a junior developer in plain language.

### Feynman Test
Explain why adaptive hashing (bcrypt/argon2) is necessary for password storage in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** `hashcat`
### Today's Tool Goal
Run a basic dictionary attack and interpret cracked output; understand mode/hash-type flags.
### Commands / Features to Practice
```text
hashcat -m 0 -a 0 hashes.txt rockyou.txt          # MD5, dictionary attack
hashcat -m 3200 -a 0 bcrypt_hashes.txt rockyou.txt  # bcrypt, for comparison of speed only
hashcat --show hashes.txt                         # view cracked results
```
### Tool Success Criteria
I can select the correct `-m` hash-type mode for a given hash format and run a dictionary attack to completion, interpreting the cracked-password output.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: if Juice Shop exposes any weakly-hashed credentials (it has known intentional weak-hash challenges), crack them as a documented finding; otherwise document the password-storage mechanism observed and assess it against bcrypt/argon2 best practice.
### Deliverable
`Day 12: Documented password storage assessment and (if applicable) successful hashcat crack of intentionally weak Juice Shop credentials.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A startup stores user passwords as `SHA256(password + static_app_secret)` and believes this is "salted" because of the appended secret.
### My Task
1. Identify the vulnerability/threat (a static, shared secret is not a per-user salt, and SHA-256 remains fast).
2. Explain the root cause.
3. Determine the impact if the database (and, separately, the static secret) leaks.
4. Demonstrate/reproduce the cracking feasibility argument with rough numbers (hashes/second on commodity GPU for SHA-256 vs. bcrypt).
5. Recommend a mitigation.
6. Document the result.
### Difficulty
⭐⭐⭐☆☆

---

# 📊 10. End-of-Day Assessment
| Skill | Score |
|---|---:|
| Conceptual understanding | /5 |
| Hands-on ability | /5 |
| Tool proficiency | /5 |
| Security reasoning | /5 |
| Problem solving | /5 |
| Ability to explain the topic | /5 |

**Total: __/30**

---

# 📦 11. Daily Deliverables
* [ ] Study notes  * [ ] `hashcat` crack completed  * [ ] bcrypt script written  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Why bcrypt/argon2 beat fast general-purpose hashes for password storage
2. How salting defeats rainbow tables
3. Credential stuffing vs. brute-force vs. password spraying
4. Rate limiting and breach-list checking as layered defenses
5. The full Week 2 identity lifecycle (JWT → session → OAuth → access control → credentials), ready for tomorrow's lab/catch-up day

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Lab (hashcat + Node.js) / TryHackMe if a matching room is available
**Lab Name:** "Crack the Weak Hash, Then Prove the Fix"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Crack a set of unsalted fast-hashed passwords, then demonstrate that the same wordlist/time budget fails against a properly bcrypt-hashed equivalent set.
### Environment
Target: local sample hash files · Tools: `hashcat`, Node.js + `bcrypt` · Prerequisite: today's concepts
### Lab Tasks
1. Crack the unsalted MD5/SHA-256 sample set with `hashcat` and record time-to-crack.
2. Hash the same plaintext passwords with bcrypt at a reasonable cost factor.
3. Attempt a comparable dictionary attack against the bcrypt hashes for the same time budget.
4. Record and compare the crack rate between the two.
5. Write a short conclusion connecting the numbers to the "why bcrypt" argument.
### What I Need to Discover
Roughly how many orders of magnitude slower is the bcrypt attack? Why does this gap matter practically for an attacker's cost-benefit calculation? What cost factor would you recommend for a production system, and what's the trade-off (server CPU load vs. security margin)?
### Lab Success Criteria
Document the finding, reproduce both cracks (or lack thereof), explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 13 — Lab Consolidation Day: Authentication + Access Control Catch-Up

**Date:** 01/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Consolidating the full identity/session/access-control lab set
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Complete any remaining PortSwigger Authentication (14 labs), Access Control (13 labs), or JWT (8 labs) labs left unfinished from Days 8–12.
* Complete the TryHackMe "OWASP Broken Access Control" room.
* Re-attempt, unaided, the single hardest lab from this sub-topic block.

### Success Criteria
* Lab backlog from Days 8–12 is at zero (or explicitly logged as carry-over with a plan).
* TryHackMe Broken Access Control room is complete.
* You can re-solve your self-identified hardest lab from memory with minimal hints.

---

# 📚 2. Topics to Study
### Primary Topic
**Consolidation: Authentication, Session, OAuth & Access Control Labs**
### Secondary Topics
* Reviewing your Days 8–12 findings log for consistency and completeness
* Identifying patterns across the PortSwigger Authentication/Access Control lab categories

### Priority
🔴 **Must Know:** be able to complete at least one lab from each of JWT, session, OAuth, and access control categories without hints
🟡 **Should Know:** which lab category took you the longest, and why
🟢 **Nice to Know:** PortSwigger's own solution methodology notes for any labs you needed hints on (review after solving, never before)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Lab Backlog Triage
* What it is: systematically working through any labs not completed during Days 8–12 due to time constraints
* Why it matters: hands-on repetition is what converts "I understood the explanation" into "I can do this under interview/CTF pressure"
* How it works: prioritize by category weight (Authentication has 14 labs — the largest category — so triage here first)
* Common mistake: skipping labs you find "boring" once you've understood the concept — repetition still builds speed and pattern recognition

### Concept 2 — TryHackMe Broken Access Control Room
* Definition: a guided room specifically reinforcing IDOR/privilege-escalation concepts from Day 11 in a different lab environment (different UI, different vulnerable app) than PortSwigger
* Practical application: exposure to a second, differently-structured vulnerable app tests whether your understanding generalizes or was pattern-matched to Juice Shop/PortSwigger's specific UI
* Best practice: attempt each task before reading the room's hints

### Concept 3 — Self-Assessment Through Unaided Retesting
* Key terminology: recall vs. recognition (recap from Day 7)
* Practical application: pick the single lab across Days 8–12 you're least confident about and solve it again from a blank slate
* Best practices: time yourself — a large time improvement from first attempt indicates real learning; similar time indicates you may have just memorized the specific payload

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Lab backlog | Labs assigned but not yet completed | Directly tracks whether you're keeping pace with the 90-day plan |
| Unaided retest | Re-solving a lab with no notes/hints | The real test of whether learning transferred to memory |

---

# ⏱️ 4. Study Schedule

## Session 1 — Backlog Triage (45–60 min)
List every lab from Days 8–12 (JWT ×2, Authentication/session-related, OAuth ×1–2, Access Control ×2) and mark complete/incomplete. Start with the highest-priority incomplete lab.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
Complete remaining backlog labs, then move to the TryHackMe "OWASP Broken Access Control" room and work through its tasks.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Without notes, explain the fix for the "none" algorithm JWT attack.
### Problem 2
Without notes, explain the fix for a redirect URI bypass.
### Problem 3
Without notes, explain the fix for IDOR.
### Challenge
Pick your lowest self-assessed score across Days 8–12 and re-attempt that day's primary lab completely unaided, timed.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full recall sweep across Days 8–12: JWT, sessions, OAuth, access control, credentials — answer what/why/how/attack/defense for each from memory before checking notes.
### Spaced-Repetition Review
* **Yesterday:** Password storage & credential attacks
* **This week:** JWT → Sessions → OAuth → Access Control → Credentials

---

# 🧪 6. Active Recall Exercises
1. Cold-recall the JWT "none" algorithm and key-confusion fixes.
2. Cold-recall session fixation vs. hijacking fixes.
3. Cold-recall the OAuth redirect URI and `state` fixes.
4. Cold-recall the IDOR and vertical-escalation fixes.
5. Cold-recall why bcrypt/argon2 beat fast hashes.
6. How would each of these five vulnerability classes show up differently in a log-monitoring dashboard?
7. Rank these five by which you'd prioritize fixing first in a real audit, and justify the ranking.
8. Which of the five connects most directly to your original baseline diagnostic gap (authN/authZ)?
9. Give one real-world breach example per class if you can recall it.
10. Teach the full "Week 2: Identity & Access" arc to a junior developer in under 3 minutes.

### Feynman Test
Explain the Week 2 identity/access arc (JWT → session → OAuth → access control → credentials) as one connected story in 5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (full workflow review across all Week 2 techniques)
### Today's Tool Goal
Rehearse the full toolkit used this week without hesitation: Repeater for JWT/OAuth tampering, Intruder for IDOR enumeration, cookie inspection for session flags.
### Tool Success Criteria
I can move fluidly between these techniques in Burp without pausing to relearn UI mechanics.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: consolidate all Week 2 findings (JWT, session, OAuth if applicable, IDOR, vertical escalation, credential storage) into the standardized CVSS-vector format alongside Week 1's injection findings.
### Deliverable
`Day 13: Consolidated all Week 2 identity/access findings into standardized findings log.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A technical interviewer says: "Tell me about the relationship between authentication and authorization, and give me one real vulnerability for each that you could exploit if either were done wrong."
### My Task
1. Explain the authN/authZ distinction clearly.
2. Give a concrete authN-adjacent vulnerability (e.g., JWT forgery) and its impact.
3. Give a concrete authZ vulnerability (e.g., IDOR) and its impact.
4. Practice this explanation out loud, under 2 minutes.
5. Anticipate one likely follow-up question.
6. Document the result.
### Difficulty
⭐⭐⭐☆☆

---

# 📊 10. End-of-Day Assessment
| Skill | Score |
|---|---:|
| Conceptual understanding | /5 |
| Hands-on ability | /5 |
| Tool proficiency | /5 |
| Security reasoning | /5 |
| Problem solving | /5 |
| Ability to explain the topic | /5 |

**Total: __/30**

---

# 📦 11. Daily Deliverables
* [ ] All Week 2 labs complete (or logged as carry-over)
* [ ] TryHackMe Broken Access Control room complete
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Findings log consolidated  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Every Week 2 vulnerability class, cold
2. The full identity/access lifecycle story
3. Which class was hardest for you and why
4. How you'd brief a dev team on preventing all five in one session
5. What's genuinely still shaky heading into Day 14's consolidation

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** TryHackMe
**Lab Name:** "OWASP Broken Access Control" room
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Reinforce IDOR and privilege-escalation concepts in a differently-structured vulnerable application, testing whether Day 11's learning generalizes.
### Environment
Target: TryHackMe room VM · Tools: browser, Burp Suite · Prerequisite: Day 11 concepts
### Lab Tasks
1. Complete the room's guided tasks in order.
2. For each task, attempt it before reading any hint.
3. Note anywhere the room's UI/structure differs meaningfully from PortSwigger/Juice Shop.
4. Document each finding in your own words.
5. Compare your time-to-solve against Day 11's PortSwigger labs.
### What I Need to Discover
Did your understanding transfer to a new environment, or did you need the hints more than you did on Day 11? That gap — if it exists — tells you whether you actually generalized the IDOR concept or just pattern-matched one specific app's UI.
### Lab Success Criteria
Complete the room, document each finding, explain root cause and impact for at least two tasks, note transfer-of-learning honestly in your self-assessment.

---
---

# 🛡️ Day 14 — Week 2 Consolidation: AuthN vs. AuthZ Model + Capital One Case Study

**Date:** 02/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Synthesizing the full identity/access-control mental model and applying it to a real breach
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Consolidate the entire Week 2 identity/access lifecycle into one coherent mental model.
* Study the 2019 Capital One breach in depth (SSRF + IAM privilege escalation) and map it onto both Week 1 (SSRF) and Week 2 (access control) concepts.
* Publish the Week 2 portfolio findings section.

### Success Criteria
* You can explain the Capital One breach chain end-to-end from memory.
* A polished "Identity & Access Control Vulnerabilities" section is published to your portfolio repo alongside Week 1's injection section.
* You've previewed Week 3's topics (XSS, CSRF, threat modeling) and noted one connection to this week's material.

---

# 📚 2. Topics to Study
### Primary Topic
**Case Study: The 2019 Capital One Breach**
### Secondary Topics
* How SSRF (Week 1, Day 4) chained into IAM credential theft
* How overly broad IAM permissions turned a network-layer bug into a catastrophic data breach
* Writing a case-study analysis as a portfolio-quality artifact

### Priority
🔴 **Must Know:** the full attack chain — misconfigured WAF/reverse proxy → SSRF → cloud metadata service → temporary IAM credentials → S3 bucket enumeration and exfiltration
🟡 **Should Know:** why least-privilege IAM policies would have limited the blast radius even after the SSRF succeeded
🟢 **Nice to Know:** the specific AWS service (IMDSv1 vs IMDSv2) distinction and how IMDSv2 mitigates this exact SSRF-to-credential-theft pattern

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — The Capital One Attack Chain
* What it is: a former AWS employee exploited a misconfigured web application firewall to perform SSRF against Capital One's cloud infrastructure, reaching the EC2 instance metadata service and retrieving temporary IAM credentials
* Why it matters: this is a textbook example of vulnerability *chaining* — no single bug caused the breach; SSRF (network-layer) plus overly permissive IAM (access-control layer) combined into a catastrophic outcome
* How it works: SSRF request to `169.254.169.254` → metadata service returns temporary IAM role credentials → those credentials had permissions far beyond what the specific service needed → attacker used them to list and download S3 buckets containing over 100 million customer records
* Real-world example: this *is* the real-world example — study it directly rather than as an analogy
* Common mistake: treating this purely as "an SSRF story" — the access-control failure (excessive IAM permissions) is equally central and is why the blast radius was so large

### Concept 2 — Least Privilege as Blast-Radius Control
* Definition: granting only the minimum permissions a service/role needs, so that even a successful compromise has limited reach
* Architecture/process: the compromised role should have been scoped to only the specific S3 operations and buckets it actually required — not broad account-wide access
* Attack scenario: without least privilege, ANY compromise of that role (via SSRF or otherwise) grants far more than necessary
* Defense/mitigation: IAM permission boundaries, scoped policies per service, IMDSv2 (which requires a session token via a PUT request, making it much harder to trigger via a simple SSRF GET request)

### Concept 3 — Writing a Case Study as a Portfolio Artifact
* Key terminology: attack chain diagram, root cause analysis, "what would have stopped this at each layer"
* Practical application: structure your case study as: timeline → technical root cause → why it succeeded → what would have stopped it at each of multiple layers (WAF config, SSRF defense, IAM least privilege, IMDSv2)
* Best practices: this kind of "multi-layer defense analysis" is exactly what AppSec/DevSecOps interviewers want to see — it shows you think in terms of defense-in-depth, not single silver-bullet fixes

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Vulnerability chaining | Combining multiple individually-lower-severity issues into a critical outcome | Real breaches are rarely one clean bug — this is the professional mental model |
| IMDSv2 | AWS's session-token-based metadata service protocol | Directly mitigates the exact SSRF-to-credential-theft pattern from this breach |
| Blast radius | The scope of damage possible after a given compromise | Least privilege is fundamentally about minimizing this, not preventing every possible bug |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Research the Capital One breach timeline and technical root cause from primary/reputable sources (the indictment, Capital One's own disclosure, and reputable security-industry analysis).
**Output:** A written timeline of the attack chain in your own words, from initial WAF misconfiguration through to data exfiltration.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Map It to What You've Learned (60–90 min)
1. Annotate the attack chain with the exact Week 1/Week 2 concepts involved at each step (SSRF from Day 4; IAM/access-control principles from Day 11).
2. Write the "what would have stopped this at each layer" analysis (WAF hardening, SSRF input validation/allowlisting, least-privilege IAM, IMDSv2).
3. Draft this as a polished case-study document for your portfolio.

**Expected Result:** A complete, well-sourced case-study writeup ready to publish.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
If the WAF misconfiguration had never happened but the IAM role still had excessive permissions, would this breach still have been possible via a different initial vector? Explain your reasoning.
### Problem 2
Explain specifically how IMDSv2's session-token requirement breaks the simple SSRF-GET-request exploitation pattern.
### Problem 3
If you were auditing a company's cloud environment today, what two things from this case study would you check first?
### Challenge
Write a 5-sentence executive summary of the Capital One breach suitable for a non-technical audience, focusing on business impact and the multi-layer lesson.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full Week 2 sweep: JWT, sessions, OAuth, access control, credential storage — blank-page recall for each. This is also a good moment to revisit Week 1's SSRF (Day 4) material directly, since today's case study depends on it.
### Spaced-Repetition Review
* **Yesterday:** Lab consolidation
* **This week:** full identity/access arc
* **2 weeks ago:** SSRF fundamentals (Day 4) — today's case study is the "why this matters" payoff for that lab

---

# 🧪 6. Active Recall Exercises
1. What is the full Capital One attack chain, start to finish?
2. How did SSRF specifically enable IAM credential theft?
3. Why did overly broad IAM permissions turn this into a catastrophic breach rather than a contained one?
4. What would an attacker need at each step of this chain?
5. What was the actual business impact (scale, regulatory consequences)?
6. How would you detect this kind of attack in logs (unusual metadata-service requests, unexpected IAM role usage patterns)?
7. How would you prevent it at each of the three layers (WAF, SSRF defense, IAM/IMDSv2)?
8. Difference between "a bug was found" and "a bug was exploitable into a catastrophe" — what made the difference here?
9. This IS the real-world example — be ready to cite specific details (approximate record count, year, root cause) accurately.
10. Teach the full Capital One case study to a junior developer in under 3 minutes.

### Feynman Test
Explain the Capital One breach as a connected, multi-layer story in 5 sentences. If unclear, mark 🟡 **Needs Review** before moving to Week 3.

---

# 🛠️ 7. Tool Practice
**Tool:** Web research methodology (primary sources) + Markdown/GitHub for case-study publishing
### Today's Tool Goal
Practice sourcing a security case study from primary/reputable sources rather than secondary blog summaries, and citing appropriately.
### Tool Success Criteria
Your case study cites specific, verifiable facts (year, scale, root cause) rather than vague generalities.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment (Week 2 section) + standalone case study
Today's contribution: publish `week2-identity-access-findings.md` (JWT/session/OAuth/IDOR/credential findings, actionable format) AND `case-study-capital-one.md` as a supplementary portfolio artifact demonstrating multi-layer security reasoning.
### Deliverable
`Day 14: Published Week 2 findings report and standalone Capital One breach case-study analysis to portfolio repo.`

---

# 📝 9. Practice / Security Challenge
### Scenario
You're asked in an interview: "Walk me through a real-world breach that combined more than one vulnerability class, and tell me what you'd have done differently at each layer."
### My Task
1. Present the Capital One case study concisely.
2. Explicitly name each layer (WAF, SSRF, IAM) and its individual fix.
3. Emphasize the "defense in depth" lesson — no single fix would have been sufficient alone if the others were missing.
4. Practice this out loud, under 2 minutes.
5. Prepare one likely follow-up answer (e.g., "What's IMDSv2?").
6. Document the result.
### Difficulty
⭐⭐⭐☆☆

---

# 📊 10. End-of-Day Assessment
| Skill | Score |
|---|---:|
| Conceptual understanding | /5 |
| Hands-on ability | /5 |
| Tool proficiency | /5 |
| Security reasoning | /5 |
| Problem solving | /5 |
| Ability to explain the topic | /5 |

**Total: __/30**

---

# 📦 11. Daily Deliverables
* [ ] Case study researched and written  * [ ] Week 2 findings report published  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Portfolio repo updated  * [ ] Interview-answer rehearsal  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow (Start of Week 3)
1. The Capital One attack chain, cold, with accurate specifics
2. The full Week 2 identity/access lifecycle
3. How defense-in-depth reasoning works across multiple vulnerability layers
4. Week 3's upcoming topics (XSS, CSRF, threat modeling) and how STRIDE-style threat modeling would have flagged the Capital One chain in advance
5. What's still shaky from Weeks 1–2 that needs review before Week 4's capstone

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link to published case study and findings report]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** N/A — today is a research/consolidation day
**Lab Name:** "Case Study Deep Dive: Capital One (2019)"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes (folded into Sessions 1–2 above)
### Lab Objective
Produce an accurate, well-sourced, multi-layer breach analysis as a standalone portfolio artifact — the "lab" here is analytical writing, not exploitation.
### Environment
Target: primary sources (Capital One's disclosure, court documents, reputable security-industry writeups) · Tools: web research, Markdown
### Lab Tasks
1. Research the timeline from at least two independent, reputable sources.
2. Draft the technical root-cause chain.
3. Draft the multi-layer "what would have stopped this" analysis.
4. Write the executive summary.
5. Publish to your portfolio repo.
### What I Need to Discover
Where exactly does responsibility split across WAF configuration, application-layer SSRF defense, and cloud IAM policy? Why is this a genuinely fair question with no single "the one thing that caused it" answer — and why is being able to articulate that nuance itself a signal of security maturity to an interviewer?
### Lab Success Criteria
Document the finding (the full chain), explain root cause and impact accurately, and produce a clear, cited, multi-layer mitigation analysis.

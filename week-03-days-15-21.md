# 🗓️ WEEK 3 — XSS, CSRF, Client-Side Attacks & Threat Modeling (Days 15–21)

---

# 🛡️ Day 15 — Reflected Cross-Site Scripting (XSS)

**Date:** 03/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Reflected XSS detection, payload crafting, and filter bypass
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain how reflected XSS differs from server-side injection (Week 1) — the payload executes in the *victim's browser*, not on the server.
* Craft a working reflected XSS payload against an unfiltered input.
* Bypass a basic input filter (blocklist-style) to achieve execution.
* Apply output encoding as the primary structural defense.

### Success Criteria
* Explain reflected XSS's execution context (browser, not server) without notes.
* Demonstrate a working reflected XSS payload in a lab.
* Demonstrate a filter bypass technique.
* Document the finding with CVSS estimates.

---

# 📚 2. Topics to Study
### Primary Topic
**Reflected Cross-Site Scripting (XSS)**
### Secondary Topics
* HTML/JS contexts (attribute, tag body, JS string, URL) and how each changes payload syntax
* Blocklist filter bypass techniques (case variation, encoding, event handlers)
* Output encoding as a structural fix

### Priority
🔴 **Must Know:** why HTML output encoding (converting `<`, `>`, `"`, `'` to entities) prevents the browser from interpreting reflected input as markup
🟡 **Should Know:** the difference between injecting into an HTML tag body vs. an HTML attribute vs. a `<script>` block — each needs different escaping and different payloads
🟢 **Nice to Know:** polyglot XSS payloads designed to fire across multiple contexts simultaneously

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Reflected XSS Mechanics
* What it is: user input is reflected back into the page's HTML response without proper encoding, and the browser executes it as markup/script
* Why it matters: unlike Week 1's server-side injection classes, the *victim's* browser executes the payload — this changes the attack model to require victim interaction (a crafted link) rather than direct server compromise
* How it works: `/search?q=<script>alert(1)</script>` reflected directly into `<div>Results for: {q}</div>` with no encoding executes the script
* Real-world example: countless bug bounty reports on search/error-message reflection points
* Common mistake: filtering only `<script>` tags while ignoring event handlers (`onerror`, `onload`) and other injection vectors

### Concept 2 — Context-Aware Encoding
* Definition: the correct encoding scheme depends entirely on *where* in the page structure the input lands (HTML body, HTML attribute, JS string, URL)
* Architecture/process: HTML-body context needs HTML-entity encoding; JS-string context needs JS-string escaping; failing to match encoding to context leaves gaps
* Attack scenario: a template engine auto-escapes HTML-body output but a developer manually concatenates a value into an inline `onclick="..."` attribute, bypassing the auto-escaping entirely
* Defense/mitigation: use your framework's built-in context-aware escaping consistently; never manually build markup with string concatenation

### Concept 3 — Filter Bypass Techniques
* Key terminology: blocklist vs. allowlist filtering, event-handler-based payloads (`<img src=x onerror=alert(1)>`), case/encoding variation
* Practical application: if a filter blocks `<script>` case-sensitively, `<ScRiPt>` may bypass it; if it blocks the string `script`, an `<img onerror>` payload avoids the word entirely
* Common vulnerabilities: regex-based blocklists are inherently incomplete — there is always another vector
* Best practices: output encoding (allowlist-style, encode everything by default) beats input blocklisting every time

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Reflected XSS | Input reflected back into the immediate response, executed in victim's browser | Requires a crafted link + victim interaction, unlike server-side injection |
| Context-aware encoding | Escaping matched to where output lands (HTML/attribute/JS/URL) | The actual structural fix; blocklists are not |
| Event-handler payload | `onerror`, `onload`, etc. as script-execution vectors that avoid `<script>` tags | Common blocklist-bypass technique |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study the HTML/JS context model and why encoding must match context.
**Output:** Write, in your own words, why an XSS filter that only blocks `<script>` tags is insufficient, with two alternative payload types as evidence.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** PortSwigger — "Reflected XSS into HTML context with nothing encoded" and "Reflected XSS into attribute with angle brackets HTML-encoded"
1. Solve the unencoded-context lab with a basic `<script>` payload.
2. Solve the attribute-context lab using an event-handler payload since angle brackets are blocked.
3. Document which context each lab represented and why the payload had to differ.

**Expected Result:** Two solved labs with payloads mapped explicitly to their HTML context.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Input lands inside `<input value="USER_INPUT">`. Craft a payload assuming angle brackets are HTML-encoded but quotes are not.
### Problem 2
A filter blocks the literal string `<script`. Craft two different payloads that avoid this string entirely.
### Problem 3
Explain why context-aware auto-escaping in a modern framework (React, Vue) still fails if a developer uses a "raw HTML" / `dangerouslySetInnerHTML`-style escape hatch.
### Challenge
Design a context-detection checklist you'd run through for any new reflection point (which of the four contexts applies, and what payload family fits).

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Week 2 Consolidation — Capital One Case Study (Day 14)
Recall without notes: the full SSRF-to-IAM-credential-theft chain and the multi-layer defense lesson.
### Spaced-Repetition Review
* **Yesterday:** Capital One case study
* **1 week ago:** JWT attacks
* **2 weeks ago:** SQL injection

---

# 🧪 6. Active Recall Exercises
1. What is reflected XSS?
2. How does the browser's interpretation of unencoded HTML enable it?
3. Why does context matter for choosing the right encoding?
4. What would an attacker need to exploit reflected XSS (a reflection point + a victim clicking a crafted link)?
5. What's the impact of reflected XSS (session theft, action-on-behalf-of-victim, defacement)?
6. How would you detect reflected XSS attempts in logs (script-like patterns in query parameters)?
7. How would you prevent it (context-aware output encoding)?
8. Difference between reflected XSS and the injection classes from Week 1 (execution location: browser vs. server)?
9. Real-world example of a reflected XSS bug bounty finding.
10. Teach reflected XSS to a junior developer in plain language.

### Feynman Test
Explain reflected XSS in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (Repeater for payload iteration) + browser DevTools console
### Today's Tool Goal
Practice rapidly iterating payload variations in Repeater and confirming execution via DevTools/browser rendering.
### Commands / Features to Practice
```text
Repeater: rapid payload substitution and resend
Browser: view-source and inspect element to confirm unencoded reflection
```
### Tool Success Criteria
I can quickly iterate through 3–4 payload variants for a blocked context and identify which one executes.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: test Juice Shop's search bar and any other reflection points for XSS; log findings.
### Deliverable
`Day 15: Documented reflected XSS findings on Juice Shop search functionality.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A support-ticket system reflects the ticket subject line directly into the page title and a status banner with no encoding.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact (what a support agent's session could allow an attacker to do).
4. Demonstrate/reproduce in an authorized lab analog.
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
* [ ] Study notes  * [ ] Completed labs (2)  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Reflected XSS mechanics and context-aware encoding
2. Filter bypass reasoning (why blocklists fail)
3. The difference between reflected XSS's browser-execution model and Week 1's server-side injections
4. Event-handler payload technique
5. How this connects to tomorrow's stored/DOM-based XSS (same root cause, different persistence/location)

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "Reflected XSS with event handlers and href attributes blocked"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Bypass a filter blocking common event handlers and `href` attributes to still achieve script execution.
### Environment
Target: PortSwigger lab · Tools: Burp Suite · Prerequisite: earlier labs today
### Lab Tasks
1. Identify the reflection point and its context.
2. Test standard event-handler payloads and confirm they're blocked.
3. Research and apply an alternative execution vector (e.g., an uncommon event handler, SVG-based vector).
4. Confirm execution.
5. Document the exact bypass and why it worked.
### What I Need to Discover
Why is there always another vector when defense relies on blocking specific known payloads? What would a genuinely structural fix (context-aware encoding + CSP) have done differently here?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 16 — Stored & DOM-Based XSS

**Date:** 04/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Stored and DOM-based XSS exploitation and defense (CSP, DOMPurify)
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain why stored XSS is more severe than reflected (no crafted link needed — every visitor is a victim).
* Explain how DOM-based XSS differs from both (the vulnerability lives entirely in client-side JavaScript, with no server involvement in the unsafe sink).
* Exploit a stored XSS in a lab (e.g., a comment/review field).
* Exploit a DOM-based XSS via an unsafe sink (`innerHTML`, `document.write`).
* Apply CSP headers and DOMPurify sanitization as defenses.

### Success Criteria
* Explain stored vs. reflected vs. DOM-based XSS distinctly, without notes.
* Demonstrate a stored XSS in a lab.
* Demonstrate a DOM-based XSS in a lab.
* Explain what CSP does and does not protect against.

---

# 📚 2. Topics to Study
### Primary Topic
**Stored XSS & DOM-Based XSS**
### Secondary Topics
* Persistent storage of malicious payloads (comments, profile fields, product reviews)
* Client-side "sources" (`location.hash`, `document.URL`) and "sinks" (`innerHTML`, `eval`, `document.write`)
* Content Security Policy (CSP) as defense-in-depth
* DOMPurify for safe HTML sanitization when rich text input is genuinely required

### Priority
🔴 **Must Know:** the source→sink model for DOM-based XSS — untrusted data flows from a source into a dangerous sink entirely within client-side JS, with no server round-trip involved in the vulnerable step
🟡 **Should Know:** why stored XSS has a larger blast radius than reflected (every viewer of the stored content is a victim, no link-clicking required)
🟢 **Nice to Know:** CSP nonce/hash-based script allowlisting for stricter policies than `unsafe-inline`

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Stored XSS
* What it is: a malicious payload is saved to persistent storage (database) and served to every user who views that content, with no victim interaction beyond normal browsing
* Why it matters: severity and blast radius are dramatically higher than reflected XSS — this is why Magecart-style attacks (British Airways 2018, previewed here, detailed further below) target stored injection points like checkout pages
* How it works: `POST /comments {"text": "<script>...</script>"}` stored unencoded, then rendered unencoded to every subsequent visitor
* Real-world example: the 2018 British Airways breach involved a Magecart-style script injection into the payment page, skimming customer card data from every checkout — the mechanism (unauthorized script execution in a trusted page context) is the same family as stored XSS
* Common mistake: encoding output on *display* pages but forgetting an admin/internal view that renders the same stored data unencoded

### Concept 2 — DOM-Based XSS
* Definition: the entire vulnerable data flow (source to sink) happens client-side in JavaScript — the server may never see the malicious payload at all
* Architecture/process: `location.hash` (source, attacker-controlled via URL fragment) flows into `element.innerHTML = ...` (sink) with no encoding in between
* Attack scenario: a client-side router or single-page app reads a URL fragment and renders it directly into the DOM
* Defense/mitigation: use `textContent` instead of `innerHTML` where possible; sanitize with DOMPurify when rich HTML rendering is genuinely required; recognize that server-side output encoding does nothing for this class since the server is never involved

### Concept 3 — CSP as Defense-in-Depth
* Key terminology: Content-Security-Policy header, `script-src`, nonces, `unsafe-inline`
* Practical application: a strict CSP (no `unsafe-inline`, script-src restricted to trusted origins/nonces) prevents an injected `<script>` tag from executing even if the injection itself wasn't caught
* Common vulnerabilities: CSPs configured with `unsafe-inline` or overly broad `script-src` values that defeat the purpose
* Best practices: CSP is a *secondary* layer — output encoding/sanitization remains the primary fix; CSP limits the damage if something slips through

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Stored XSS | Payload persists in storage, served to every viewer | Largest blast radius of the three XSS types |
| DOM-based XSS | Entire vulnerable flow happens client-side, server never sees it | Server-side encoding is irrelevant; needs client-side fixes |
| CSP | Browser-enforced policy restricting script execution sources | Defense-in-depth, not a replacement for proper encoding |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study stored XSS mechanics, the DOM source→sink model, and CSP's role. Read a summary of the British Airways Magecart incident.
**Output:** Write a paragraph distinguishing all three XSS types (reflected, stored, DOM-based) by where the payload lives and who's affected.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** PortSwigger — "Stored XSS into HTML context with nothing encoded" and "DOM XSS in `document.write` sink using source `location.search`"
1. Solve the stored XSS lab via a comment/blog-post field.
2. Solve the DOM-based lab by tracing the source→sink flow in the page's JavaScript.
3. Apply DOMPurify to a local sample "rich text comment" renderer and confirm a payload is neutralized.

**Expected Result:** Two solved labs + a working DOMPurify sanitization demo.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A blog comment section renders stored comments via `element.innerHTML = comment.text`. Identify the vulnerability class and the minimal fix.
### Problem 2
A single-page app reads `location.hash` and inserts it into the DOM via `innerHTML` for client-side routing. Trace the exact source→sink flow and craft a payload.
### Problem 3
Why would a strict CSP (`script-src 'self'`) have prevented the injected payload in Problem 1 from executing, even if the stored-XSS bug itself wasn't fixed yet?
### Challenge
Design a rich-text comment system (pseudocode) that allows safe HTML formatting (bold, links) while blocking script execution, using DOMPurify's allowlist approach.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Reflected XSS (Day 15)
Recall without notes: context-aware encoding and filter-bypass reasoning.
### Spaced-Repetition Review
* **Yesterday:** Reflected XSS
* **1 week + 2 days ago:** Session fixation & hijacking (both are client-side-trust topics)

---

# 🧪 6. Active Recall Exercises
1. What is stored XSS, and why is its blast radius larger than reflected?
2. What is DOM-based XSS, and why doesn't server-side encoding fix it?
3. What are "sources" and "sinks" in the DOM XSS model?
4. What would an attacker need to exploit stored XSS (any persistent field rendered unencoded to other users)?
5. What's the impact of a Magecart-style stored XSS on an e-commerce checkout page?
6. How would you detect stored XSS in logs/monitoring (unusual script-like content in stored fields, CSP violation reports)?
7. How would you prevent DOM-based XSS specifically (avoid `innerHTML`, use `textContent`, sanitize with DOMPurify)?
8. Difference between stored, reflected, and DOM-based XSS?
9. Real-world example: the British Airways Magecart breach.
10. Teach the source→sink DOM XSS model to a junior developer in plain language.

### Feynman Test
Explain stored and DOM-based XSS, distinctly, in 3–5 sentences total. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Browser DevTools (Sources panel, breakpoints) + CSP Evaluator concept
### Today's Tool Goal
Practice tracing a DOM XSS source→sink flow using DevTools' Sources panel and setting a breakpoint on `innerHTML` writes (via a DOM breakpoint).
### Commands / Features to Practice
```text
DevTools > Elements > right-click element > Break on > Attribute modifications
DevTools > Sources > set breakpoint, step through source-to-sink flow
```
### Tool Success Criteria
I can use DevTools to trace exactly where untrusted data enters a dangerous sink in client-side JavaScript.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: test Juice Shop's product review/comment features for stored XSS, and any client-side routing/search features for DOM-based XSS; log findings.
### Deliverable
`Day 16: Documented stored XSS (product reviews) and DOM-based XSS findings for Juice Shop.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An e-commerce checkout page includes a third-party analytics script loaded dynamically, and the page also has a stored-XSS-vulnerable product review field on the same domain.
### My Task
1. Identify the vulnerability/threat (connect this explicitly to the Magecart/British Airways pattern).
2. Explain the root cause.
3. Determine the impact (payment card skimming at scale).
4. Demonstrate/reproduce the stored XSS mechanism in an authorized lab analog.
5. Recommend a mitigation (including CSP as defense-in-depth).
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
* [ ] Study notes  * [ ] Completed labs (2)  * [ ] DOMPurify demo  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Stored vs. DOM-based vs. reflected XSS, cleanly distinguished
2. The source→sink model
3. CSP's role as defense-in-depth
4. The Magecart/British Airways pattern
5. How this connects to tomorrow's CSRF topic (a different client-side trust failure — the browser trusting an origin rather than trusting rendered content)

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "DOM XSS in `jQuery` anchor href attribute sink using location.search source"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Trace and exploit a DOM-based XSS flowing through a jQuery-based sink, reinforcing the source→sink model with a real-world library pattern.
### Environment
Target: PortSwigger lab · Tools: Burp Suite, DevTools · Prerequisite: today's stored/DOM XSS concepts
### Lab Tasks
1. Identify the client-side JavaScript handling the vulnerable feature.
2. Trace the source (`location.search`) through to the sink.
3. Craft a payload that survives the specific sink's handling (jQuery-specific quirks).
4. Confirm execution.
5. Document the full source→sink trace as your finding writeup.
### What I Need to Discover
Why does this vulnerability exist entirely without any server-side involvement in the unsafe step? What library-specific behavior (jQuery's handling of certain attribute values) made this sink dangerous? What's the fix at the code level?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 17 — CSRF & Clickjacking

**Date:** 05/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Cross-Site Request Forgery and Clickjacking exploitation and defense
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain why CSRF exploits the browser's automatic inclusion of credentials (cookies) on cross-origin requests.
* Generate a working CSRF proof-of-concept against a state-changing endpoint lacking CSRF protection.
* Exploit a clickjacking scenario using an invisible iframe overlay.
* Apply CSRF tokens, `SameSite` cookies, and `X-Frame-Options`/`frame-ancestors` as fixes.

### Success Criteria
* Explain CSRF's core mechanism (ambient authority via cookies) without notes.
* Demonstrate a working CSRF PoC in a lab.
* Demonstrate a clickjacking PoC in a lab.
* Explain why CSRF tokens and `SameSite` cookies address the same problem from two different angles.

---

# 📚 2. Topics to Study
### Primary Topic
**Cross-Site Request Forgery (CSRF) & Clickjacking**
### Secondary Topics
* Why cookies are sent automatically on cross-origin requests (ambient authority)
* `SameSite=Strict`/`Lax`/`None` cookie attribute behavior
* Anti-CSRF tokens (synchronizer token pattern)
* Clickjacking via invisible/transparent iframe overlays
* `X-Frame-Options` and the modern `Content-Security-Policy: frame-ancestors` directive

### Priority
🔴 **Must Know:** why a browser automatically attaches cookies to a request regardless of which site initiated it, and why that's the entire root cause of CSRF
🟡 **Should Know:** the difference between `SameSite=Lax` (blocks most cross-site POST but allows top-level navigation) and `Strict` (blocks essentially all cross-site cookie sending)
🟢 **Nice to Know:** double-submit cookie pattern as a stateless alternative to server-side token storage

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — CSRF Core Mechanism
* What it is: a malicious page on a different origin triggers a request to a target site the victim is already authenticated to; the browser automatically attaches the victim's session cookie, so the target site sees a fully "authenticated" request it never intended
* Why it matters: the target application did nothing wrong from an authentication standpoint — the request genuinely comes from an authenticated session — but the *user* never intended to make it
* How it works: `<img src="https://bank.com/transfer?to=attacker&amount=1000">` on an attacker's page silently fires a GET request with the victim's bank cookies attached, if the transfer endpoint uses GET and has no CSRF protection
* Real-world example: numerous historical CSRF vulnerabilities in banking and social media "state-changing GET request" endpoints before CSRF tokens became standard practice
* Common mistake: assuming "the user is logged in, so the request is legitimate" — CSRF specifically breaks that assumption

### Concept 2 — CSRF Defenses (Tokens & SameSite)
* Definition: CSRF tokens are unique, unpredictable values the server issues and requires back on state-changing requests, which a cross-origin attacker page cannot read or predict; `SameSite` cookies tell the browser not to send the cookie at all on qualifying cross-site requests
* Architecture/process: synchronizer token pattern — server embeds a token in the form, checks it matches session state on submission
* Attack scenario prevented: an attacker's cross-origin page cannot read the victim's session-specific CSRF token (same-origin policy blocks reading the response), so it cannot include a valid token in its forged request
* Defense/mitigation: use both layers — CSRF tokens as the primary structural fix, `SameSite=Lax` or `Strict` cookies as strong defense-in-depth

### Concept 3 — Clickjacking
* Key terminology: UI redress attack, transparent iframe overlay, `X-Frame-Options`, `frame-ancestors`
* Practical application: an attacker overlays an invisible iframe of a legitimate site (e.g., a "delete account" or "authorize payment" button) beneath a decoy UI, tricking the victim into clicking the real button while believing they're clicking something else
* Common vulnerabilities: sites with no framing protection at all can be embedded in any attacker-controlled iframe
* Best practices: `Content-Security-Policy: frame-ancestors 'none'` (or a specific allowlist) is the modern, more flexible replacement for the older `X-Frame-Options` header

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Ambient authority | Cookies sent automatically regardless of request origin | The entire root cause of CSRF |
| Synchronizer token pattern | Server-issued, unpredictable token required on state-changing requests | Primary structural CSRF fix |
| Clickjacking | Tricking a user into clicking a hidden legitimate UI element via iframe overlay | A UI-layer trust failure, distinct from but often discussed alongside CSRF |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study ambient authority, CSRF token mechanics, `SameSite` behavior, and clickjacking's iframe-overlay technique.
**Output:** Write, in your own words, why "the request had valid session cookies" does not mean "the user intended to make this request."

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** PortSwigger — "CSRF vulnerability with no defenses" and "Basic clickjacking with CSRF token protected form"
1. Solve the no-defense CSRF lab by generating a PoC HTML page and getting the "victim" browser to submit it.
2. Solve the clickjacking lab by crafting an iframe overlay that tricks the CSRF-protected form's UI interaction (since clickjacking operates at the UI layer, not the request-forgery layer, it can bypass CSRF tokens entirely).
3. Note explicitly why the CSRF token didn't stop the clickjacking attack — this is an important distinction.

**Expected Result:** Two solved labs + a written note on why CSRF tokens don't stop clickjacking (different attack surface).

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A password-change endpoint accepts a GET request with no CSRF token. Craft the minimal HTML PoC that would trigger it from an attacker's page.
### Problem 2
Explain precisely why `SameSite=Strict` would have prevented the CSRF in Problem 1, and why `SameSite=Lax` might not fully protect a GET-based state change reached via top-level navigation.
### Problem 3
Why does clickjacking succeed even against an application with perfect CSRF token implementation?
### Challenge
Design a layered defense (pseudocode/policy) combining CSRF tokens, `SameSite=Strict` cookies, and `frame-ancestors 'none'` for a sensitive banking action.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Stored & DOM-Based XSS (Day 16)
Recall without notes: the source→sink model and why CSP is defense-in-depth, not a primary fix.
### Spaced-Repetition Review
* **Yesterday:** Stored & DOM-based XSS
* **2 days ago:** Reflected XSS
* **Connect the pattern:** XSS exploits the browser trusting *content* as safe to execute; CSRF exploits the browser trusting *the request's origin-independent inclusion of credentials*. Both are browser-trust-model failures, just at different layers.

---

# 🧪 6. Active Recall Exercises
1. What is CSRF, mechanistically?
2. Why does the browser attach cookies regardless of request origin?
3. How do CSRF tokens prevent an attacker's page from forging a valid request?
4. What would an attacker need to exploit CSRF (a victim authenticated to the target + a state-changing endpoint with no protection)?
5. What's the impact of CSRF on a banking transfer endpoint vs. a "change email" endpoint?
6. How would you detect CSRF attempts in logs (requests missing expected tokens, unusual referer/origin headers)?
7. How would you prevent clickjacking specifically (frame-ancestors, X-Frame-Options)?
8. Difference between CSRF and clickjacking as attack mechanisms?
9. Real-world example of a historical CSRF vulnerability class (GET-based state changes).
10. Teach CSRF's ambient-authority root cause to a junior developer in plain language.

### Feynman Test
Explain CSRF and clickjacking, distinctly, in 3–5 sentences total. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (built-in "Generate CSRF PoC" feature)
### Today's Tool Goal
Use Burp's engagement tools to auto-generate a CSRF proof-of-concept HTML page from a captured request, and manually craft an iframe-overlay clickjacking PoC.
### Commands / Features to Practice
```text
Burp: right-click request > Engagement tools > Generate CSRF PoC
Manual HTML: <iframe src="https://target.com/action" style="opacity:0.01; position:absolute;"></iframe>
```
### Tool Success Criteria
I can generate a working CSRF PoC via Burp and a working clickjacking overlay manually, and explain why each succeeds.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: test Juice Shop's state-changing endpoints (profile update, password change) for CSRF protection, and test whether the app can be framed for clickjacking; log findings.
### Deliverable
`Day 17: Documented CSRF and clickjacking findings for Juice Shop state-changing endpoints.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A social media platform's "delete post" action is a simple GET request with no CSRF token, and the platform sets no framing-protection headers.
### My Task
1. Identify both vulnerabilities present.
2. Explain the root cause of each.
3. Determine the combined impact (an attacker page could both forge deletions directly via CSRF AND clickjack the delete-confirmation UI as a backup vector).
4. Demonstrate/reproduce both in an authorized lab analog.
5. Recommend mitigations for both.
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
* [ ] Tool practice (CSRF PoC generation)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. CSRF's ambient-authority root cause and its two-layer defense (tokens + SameSite)
2. Clickjacking's UI-layer mechanism and why CSRF tokens don't stop it
3. `X-Frame-Options`/`frame-ancestors` framing protection
4. How CSRF and XSS are both browser-trust failures at different layers
5. How this connects to tomorrow's insecure deserialization topic (a server-side trust failure once again)

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "CSRF where token validation depends on request method"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Bypass a CSRF defense that only validates the token for certain HTTP methods, exploiting the inconsistency.
### Environment
Target: PortSwigger lab · Tools: Burp Suite · Prerequisite: today's CSRF fundamentals
### Lab Tasks
1. Identify that the target validates CSRF tokens for POST but not GET (or a similar method-based inconsistency).
2. Determine whether the state-changing action can be triggered via the unprotected method.
3. Craft a PoC using the unprotected method.
4. Confirm the forged action succeeds.
5. Document the inconsistency as the precise root cause.
### What I Need to Discover
Why does defending "some" methods but not others fail as a security control? What does this tell you about partial/inconsistent security controls in general — is a control that only covers some paths actually a control at all?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation (consistent protection across all state-changing methods).

---
---

# 🛡️ Day 18 — Insecure Deserialization

**Date:** 06/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Insecure deserialization exploitation and safe data-format defenses
**Estimated Total Time:** 3–4 hours
**Difficulty:** Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain why deserializing untrusted data can lead to object injection and, in some languages, RCE.
* Understand gadget chains conceptually across Java/Python/Node ecosystems.
* Identify insecure deserialization in a lab and reason through exploitation methodology.
* Apply JSON-only/type-validated data exchange as the primary structural fix.

### Success Criteria
* Explain why deserialization is dangerous, at a conceptual level, without notes.
* Identify an insecure deserialization vulnerability in a lab and explain the exploitation path (even if using an existing tool/gadget chain rather than building one from scratch — full gadget-chain construction is graduate-level and outside today's scope).
* Explain why switching to JSON-only interchange (no native object serialization) eliminates the vulnerability class structurally.

---

# 📚 2. Topics to Study
### Primary Topic
**Insecure Deserialization**
### Secondary Topics
* Native serialization formats (Java `ObjectInputStream`, Python `pickle`, Node `node-serialize`) vs. safe formats (JSON)
* Gadget chains (conceptual understanding, not from-scratch construction)
* Tools: `ysoserial` (Java) as a reference for how gadget chain exploitation tooling works

### Priority
🔴 **Must Know:** why deserializing untrusted data using a language's *native* object-serialization mechanism is fundamentally different (and far more dangerous) than parsing JSON — native deserialization can reconstruct arbitrary objects, including ones with dangerous side effects (constructors, magic methods) triggered just by being instantiated
🟡 **Should Know:** what a "gadget chain" conceptually is (a sequence of existing, otherwise-harmless classes/methods that, when chained together during deserialization, produce code execution)
🟢 **Nice to Know:** specific historical CVEs (Apache Commons Collections gadget chains that powered many Java deserialization RCEs)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Why Native Deserialization Is Dangerous
* What it is: converting a serialized byte stream back into a live object, where the *type* of object to create is often determined by data embedded in the stream itself, controlled by the sender
* Why it matters: unlike JSON (which produces only plain data — strings, numbers, arrays, dictionaries), native deserialization can instantiate arbitrary classes, and some classes have constructors or special methods (`__reduce__` in Python, `readObject()` in Java) that execute code as a side effect of simply being created
* How it works: an attacker crafts a serialized payload representing an instance of a dangerous class already present in the application's dependencies; deserializing it triggers that class's side-effecting method
* Real-world example: the 2015 Apache Commons Collections gadget chain enabled RCE in a huge number of Java applications that deserialized untrusted data, because that one common library was present nearly everywhere
* Common mistake: believing a custom `readObject()` override or basic type-checking after deserialization is sufficient — by the time you can check the type, the dangerous side effect may have already executed during the deserialization process itself

### Concept 2 — Gadget Chains (Conceptual)
* Definition: a chain of method calls across otherwise-unrelated, legitimate classes that, when triggered in sequence by the deserialization process, ultimately achieves attacker-controlled code execution
* Architecture/process: tools like `ysoserial` maintain pre-built gadget chains for known-vulnerable common libraries, since discovering new chains is highly specialized research
* Attack scenario: an attacker doesn't need to write custom exploit code — they select a pre-built gadget chain matching a library present in the target, generate the payload, and submit it wherever deserialization occurs
* Defense/mitigation: this is precisely why avoiding native deserialization of untrusted data entirely is the only reliable fix — patching individual gadget chains as they're discovered is a losing, reactive battle

### Concept 3 — The Structural Fix: JSON-Only Interchange
* Key terminology: type validation, safe data interchange formats
* Practical application: replace native object serialization for any untrusted data path with JSON (or Protocol Buffers with a strict schema), which cannot instantiate arbitrary classes with side-effecting constructors
* Common vulnerabilities: legacy systems or "convenient" internal APIs that still use native serialization for performance or historical reasons, without realizing the data crossing that boundary is no longer fully trusted (e.g., after a network topology change)
* Best practices: never deserialize data from an untrusted source using a native mechanism; if you must interoperate with a legacy format, deserialize into a strict allowlisted schema and validate before any object construction with side effects

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Native deserialization | Reconstructing live objects (not just plain data) from a serialized stream | Can trigger arbitrary code execution as a side effect |
| Gadget chain | A sequence of legitimate classes/methods chained to achieve RCE | Explains why one vulnerable dependency can compromise an entire app |
| JSON-only interchange | Restricting untrusted data exchange to plain-data formats | The structural fix — nothing to construct maliciously |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study why native deserialization differs fundamentally from JSON parsing, and the gadget-chain concept at a high level.
**Output:** Write, in your own words, why "we validate the object's type after deserializing it" is not a sufficient defense.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** PortSwigger — "Modifying serialized objects" and "Arbitrary object injection in PHP" (or nearest available insecure-deserialization-category labs)
1. Identify a serialized object in a cookie or hidden field.
2. Decode/modify a serialized value to change application behavior (e.g., flipping an `isAdmin` flag encoded within a serialized object).
3. If a gadget-chain-based RCE lab is available and appropriate to your current tooling, walk through the conceptual exploitation flow using the lab's guided methodology rather than building a chain from scratch.

**Expected Result:** At least one solved lab demonstrating object-property tampering via deserialization, with a written explanation of how a fuller gadget-chain attack would escalate this to RCE in a Java/Python-native-serialization context.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A Node.js app uses `node-serialize` to store user preferences in a cookie. Explain, at a conceptual level, why this is dangerous even without constructing a full exploit.
### Problem 2
Why does simply "encrypting" the serialized data (without changing the deserialization mechanism) fail to fix the vulnerability if the encryption key is ever exposed or predictable?
### Problem 3
Contrast the fix for insecure deserialization (avoid native deserialization entirely) with the fix for SQLi (parameterize) — why is deserialization's fix more "avoid the mechanism" rather than "use it safely"?
### Challenge
Design a migration plan (high-level steps) for moving a legacy Java app from native object serialization to JSON-based interchange for any endpoint that accepts untrusted input.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: CSRF & Clickjacking (Day 17)
Recall without notes: ambient authority, CSRF tokens, `SameSite`, and clickjacking's UI-layer mechanism.
### Spaced-Repetition Review
* **Yesterday:** CSRF & Clickjacking
* **3 days ago:** Stored & DOM-based XSS
* **Connect the pattern:** insecure deserialization is, once again, the Week 1 "trust boundary" pattern — untrusted data crossing into a context (object construction) where it's treated as more trustworthy than it should be.

---

# 🧪 6. Active Recall Exercises
1. What is insecure deserialization?
2. Why is native deserialization more dangerous than JSON parsing?
3. What is a gadget chain, conceptually?
4. What would an attacker need to exploit it (a native deserialization sink accepting untrusted input + a known-vulnerable class present in dependencies)?
5. What's the impact of a successful deserialization RCE?
6. How would you detect deserialization attacks in logs (unusual serialized-object patterns, unexpected class instantiation errors)?
7. How would you prevent it structurally (JSON-only interchange for untrusted data)?
8. Difference between "validate after deserializing" and "avoid native deserialization entirely" as defenses?
9. Real-world example: Apache Commons Collections gadget chain.
10. Teach why deserialization RCE is possible to a junior developer in plain language.

### Feynman Test
Explain insecure deserialization in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (serialized-data detection) + conceptual familiarity with `ysoserial`
### Today's Tool Goal
Practice recognizing serialized-data patterns (Java serialized object magic bytes, PHP serialization syntax) in captured traffic, and understand at a high level how `ysoserial` generates gadget-chain payloads (without necessarily running it against a live target beyond an authorized lab).
### Commands / Features to Practice
```text
Burp: identify base64-encoded values that decode to Java serialized-object magic bytes (0xACED)
Recognize PHP serialization syntax: O:4:"User":2:{...}
```
### Tool Success Criteria
I can visually identify serialized-object data in a captured request/cookie and explain what format it's in.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: since Juice Shop is a Node/JSON-based app, deserialization findings may be limited — document this as a negative finding with reasoning (why the JSON-based architecture structurally avoids this class), which is itself a valuable, accurate assessment statement.
### Deliverable
`Day 18: Documented insecure deserialization risk assessment for Juice Shop (negative finding with structural reasoning).`

---

# 📝 9. Practice / Security Challenge
### Scenario
A legacy Java enterprise application accepts a serialized session object from a client-side cookie to avoid server-side session storage overhead.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact (potential RCE via gadget chain if any vulnerable library is present in the classpath).
4. Demonstrate/reproduce the conceptual exploitation path (tampering with the serialized object, and describing how a gadget-chain tool would escalate further).
5. Recommend a mitigation (migrate to JSON-based session tokens/JWTs with proper validation).
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
* [ ] Tool practice (serialized-data recognition)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Why native deserialization of untrusted data is dangerous
2. The gadget-chain concept at a high level
3. Why JSON-only interchange is the structural fix
4. How to visually recognize serialized-object data in traffic
5. How this connects to tomorrow's threat-modeling topic (proactively identifying trust-boundary risks like this before they're built, rather than finding them after)

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "Using application functionality to exploit insecure deserialization"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Combine a deserialization vulnerability with existing application functionality (e.g., a file-deletion feature) to escalate a simple object-tampering bug into a more significant impact.
### Environment
Target: PortSwigger lab · Tools: Burp Suite · Prerequisite: today's deserialization concepts
### Lab Tasks
1. Locate the serialized object in the request.
2. Understand what legitimate application functionality exists that could be repurposed (e.g., a class with a file-path property used elsewhere in the app).
3. Modify the serialized object to redirect that functionality toward a malicious outcome.
4. Confirm the escalated impact.
5. Document the full chain: initial bug → combined with app functionality → final impact.
### What I Need to Discover
Why does combining a "minor" deserialization bug with unrelated application functionality often produce a much more severe outcome than either alone? What does this teach you about assessing severity — should you always consider what else is reachable, not just the immediate bug?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 19 — Threat Modeling Part 1: The STRIDE Framework

**Date:** 07/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Systematic threat modeling using Microsoft's STRIDE framework
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain each of the six STRIDE categories and what question each one asks about a system.
* Apply STRIDE to a simple system diagram (e.g., a login flow) to systematically enumerate threats.
* Understand how STRIDE shifts security work from "reactive bug-hunting" to "proactive design analysis."

### Success Criteria
* Explain all six STRIDE letters and their meaning without notes.
* Produce a STRIDE analysis of a simple data-flow diagram (e.g., your own past project's login flow).
* Map at least one Week 1–3 vulnerability class to each relevant STRIDE category.

---

# 📚 2. Topics to Study
### Primary Topic
**STRIDE Threat Modeling Framework**
### Secondary Topics
* Data Flow Diagrams (DFDs) as the input artifact for STRIDE analysis
* Trust boundaries within a DFD
* Mapping STRIDE categories to concrete vulnerability classes you've already studied

### Priority
🔴 **Must Know:** the six STRIDE categories — Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege — and one question each answers
🟡 **Should Know:** how to draw a basic Data Flow Diagram with trust boundaries marked
🟢 **Nice to Know:** STRIDE-per-element vs. STRIDE-per-interaction analysis approaches

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — The Six STRIDE Categories
* What it is: a mnemonic framework for systematically asking "what could go wrong here" across six threat categories, applied to each component/data flow in a system diagram
* Why it matters: rather than randomly guessing at vulnerabilities, STRIDE gives you a repeatable checklist to apply to *any* system, including ones you haven't seen exploit techniques for yet
* How it works, category by category:
  - **Spoofing** — can an attacker pretend to be someone/something else? (maps to authentication weaknesses, JWT forgery from Day 8)
  - **Tampering** — can data be modified without authorization? (maps to IDOR from Day 11, insecure deserialization from Day 18)
  - **Repudiation** — can a user deny having performed an action, due to insufficient logging? (maps to Week 4's logging topic)
  - **Information Disclosure** — can data be exposed to unauthorized parties? (maps to IDOR, SSRF, error-message leakage)
  - **Denial of Service** — can the system be made unavailable? (a category not yet deeply covered — flagged as a gap to research further)
  - **Elevation of Privilege** — can an attacker gain permissions beyond what they should have? (maps to vertical escalation from Day 11)
* Real-world example: revisit the Capital One case study (Day 14) and identify which STRIDE categories apply (Tampering via SSRF-enabled metadata access, Information Disclosure via S3 exfiltration, Elevation of Privilege via the overly broad IAM role)
* Common mistake: treating STRIDE as a one-time exercise rather than something applied whenever the system's design changes

### Concept 2 — Data Flow Diagrams and Trust Boundaries
* Definition: a DFD represents a system as processes, data stores, external entities, and data flows between them; a trust boundary marks where the level of trust changes (e.g., between the public internet and your internal network, or between a regular user and an admin)
* Architecture/process: draw the system first, mark every trust boundary crossing, then apply STRIDE at each crossing
* Attack scenario: most real vulnerabilities occur exactly at trust boundary crossings — this is the same "trust boundary" concept from Week 1, now applied proactively at the design stage instead of reactively during testing
* Mitigation: STRIDE analysis performed during design catches classes of issues before a single line of code is written

### Concept 3 — Mapping STRIDE to Known Vulnerability Classes
* Key terminology: threat category, mitigation control, residual risk
* Practical application: for each STRIDE category identified at a trust boundary, list the specific mitigation control that addresses it (e.g., Spoofing → strong authentication + JWT algorithm pinning)
* Common vulnerabilities in threat modeling itself: doing it once at project kickoff and never revisiting it as the system evolves
* Best practices: treat threat modeling as a living document, revisited whenever the system's trust boundaries change (new integrations, new user roles, new external dependencies)

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| STRIDE | Spoofing, Tampering, Repudiation, Info Disclosure, DoS, Elevation of Privilege | A systematic, repeatable threat-enumeration checklist |
| Data Flow Diagram | Visual representation of processes, data stores, and flows in a system | The input artifact STRIDE is applied to |
| Trust boundary | A point where the level of trust in data/requests changes | Where most real vulnerabilities occur — the same concept as Week 1, applied proactively |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study all six STRIDE categories in depth, with one concrete example per category drawn from your Weeks 1–3 material.
**Output:** A table mapping each STRIDE category to at least one specific vulnerability class you've already studied.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Exercise:** Draw a Data Flow Diagram for a simple login + profile-update flow (use one of your own past projects, or a simplified version of Juice Shop's login flow).
1. Identify all processes, data stores, external entities, and data flows.
2. Mark every trust boundary crossing explicitly.
3. Apply STRIDE at each crossing — for each of the six categories, ask "could this happen here?" and note yes/no with reasoning.
4. For every "yes," note the mitigating control (existing or needed).

**Expected Result:** A completed STRIDE-annotated DFD for your chosen system, with mitigations noted.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
For a simple "upload profile picture" feature, walk through all six STRIDE categories and identify which apply.
### Problem 2
Revisit the Capital One case study (Day 14) — map its attack chain onto specific STRIDE categories.
### Problem 3
Why is "Repudiation" (insufficient logging to prove what happened) a security concern distinct from the other five categories?
### Challenge
Choose a system you haven't deeply analyzed yet (e.g., a simple chat application) and produce a full STRIDE pass from a one-paragraph description alone, reasoning through likely trust boundaries without a diagram.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Insecure Deserialization (Day 18)
Recall without notes: why native deserialization is dangerous and why JSON-only interchange is the fix.
### Spaced-Repetition Review
* **Yesterday:** Insecure deserialization
* **4 days ago:** CSRF & Clickjacking
* **This is also a strong moment to revisit Week 1's "unified trust boundary model" (Day 6)** — STRIDE is that same concept, formalized into a repeatable framework you can apply to systems you've never seen before.

---

# 🧪 6. Active Recall Exercises
1. What does each STRIDE letter stand for?
2. What question does each STRIDE category ask about a system?
3. Why is a Data Flow Diagram the right input artifact for STRIDE analysis?
4. What would you need to perform a STRIDE analysis on a system (a diagram with trust boundaries marked)?
5. What's the value of performing STRIDE at design time vs. finding these issues via later penetration testing?
6. How would STRIDE analysis show up as a work artifact in a real AppSec role (design review documents, security requirements in tickets)?
7. How would you map STRIDE findings to concrete mitigating controls?
8. Difference between STRIDE-per-element and STRIDE-per-interaction approaches?
9. Real-world example: map the Capital One breach to specific STRIDE categories.
10. Teach STRIDE to a junior developer in plain language, using their own project as the example.

### Feynman Test
Explain STRIDE (all six categories) in 5–6 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Diagramming (draw.io, Excalidraw, or hand-drawn/photographed) for DFD creation
### Today's Tool Goal
Produce a clean, readable Data Flow Diagram with clearly marked trust boundaries.
### Tool Success Criteria
My DFD would be understandable to another security professional without additional verbal explanation.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment (this STRIDE exercise feeds directly into the capstone threat model due in Week 11, and strengthens Week 4's Month 1 report)
Today's contribution: produce a STRIDE-annotated DFD for Juice Shop's login/authentication flow specifically, to include as a threat-modeling artifact in your Month 1 capstone report.
### Deliverable
`Day 19: Produced STRIDE-annotated Data Flow Diagram for Juice Shop's authentication flow.`

---

# 📝 9. Practice / Security Challenge
### Scenario
You're asked in an interview to threat-model, live, a simple "password reset via email link" feature you've never seen the code for.
### My Task
1. Sketch (mentally or on paper) the data flow: user requests reset → system generates token → email sent → user clicks link → system validates token → password updated.
2. Mark the trust boundaries (public internet ↔ your backend; email delivery ↔ your system).
3. Walk through STRIDE at each boundary out loud.
4. Identify at least 2 concrete risks (e.g., token predictability — Spoofing; token reuse after use — Tampering).
5. Propose mitigations for each.
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
* [ ] Study notes  * [ ] STRIDE-annotated DFD produced  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice (diagramming)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. All six STRIDE categories and their questions
2. How to produce a DFD with trust boundaries marked
3. How STRIDE maps to concrete vulnerability classes from Weeks 1–3
4. Why threat modeling is proactive vs. penetration testing's reactive nature
5. How tomorrow's attack-tree exercise builds on today's STRIDE output

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** TryHackMe
**Lab Name:** "Intro to Threat Modeling" room
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Reinforce STRIDE methodology through a guided room with structured practice scenarios distinct from your own project analysis.
### Environment
Target: TryHackMe room · Tools: browser, notes · Prerequisite: today's STRIDE fundamentals
### Lab Tasks
1. Complete the room's guided STRIDE exercises.
2. Compare the room's example DFDs to the one you built for Juice Shop.
3. Note any STRIDE category you find yourself consistently under-applying.
4. Document your completion and key takeaways.
5. Update your Juice Shop DFD if the room reveals a gap in your earlier analysis.
### What I Need to Discover
Which STRIDE category do you find hardest to apply consistently, and why? Is it a knowledge gap (don't understand the category) or an application gap (understand it but forget to check for it)?
### Lab Success Criteria
Complete the room, document key takeaways, identify and note your weakest STRIDE category honestly.

---
---

# 🛡️ Day 20 — Threat Modeling Part 2: Attack Trees on Your Own Past Projects

**Date:** 08/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Building and reasoning with attack trees
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain what an attack tree is and how it differs from STRIDE (goal-oriented decomposition vs. category-based enumeration).
* Build a complete attack tree for a specific attacker goal against one of your own past full-stack projects.
* Identify the cheapest/most likely attack path in your tree and propose the highest-leverage mitigation.

### Success Criteria
* Explain attack trees vs. STRIDE as complementary techniques without notes.
* Produce a complete attack tree (root goal + at least 2 levels of sub-goals + leaf-level concrete attacks) for a real system.
* Identify and justify the single highest-priority mitigation from your tree.

---

# 📚 2. Topics to Study
### Primary Topic
**Attack Trees**
### Secondary Topics
* Root goal, AND/OR nodes, leaf nodes (concrete attack techniques)
* Estimating cost/likelihood/impact per leaf to prioritize defenses
* Combining attack trees with STRIDE output from Day 19 for a fuller threat model

### Priority
🔴 **Must Know:** the difference between an AND node (all child conditions must be true) and an OR node (any one child condition suffices) in an attack tree
🟡 **Should Know:** how to decompose a high-level attacker goal ("compromise user accounts") into concrete, testable sub-goals and leaf attacks
🟢 **Nice to Know:** formal attack-tree scoring systems (assigning cost/skill/detection-likelihood values per leaf)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Attack Tree Structure
* What it is: a hierarchical diagram starting from a single root node (the attacker's ultimate goal, e.g., "steal customer payment data"), decomposed into sub-goals, down to leaf nodes representing concrete, executable attack techniques
* Why it matters: while STRIDE enumerates threat *categories* systematically across a system, an attack tree reasons *goal-first* — starting from what an attacker actually wants and working backward to every path that achieves it, which surfaces different insights (especially about which paths are cheapest/easiest for an attacker)
* How it works: root = "Compromise admin account" → OR → [Phish admin credentials] OR [Exploit JWT algorithm confusion (Day 8)] OR [IDOR to admin's password reset token (Day 11)] — each OR branch is a *sufficient* independent path; an AND branch would require all listed conditions together
* Real-world example: security teams use attack trees during red-team planning to prioritize which paths to actually test, given limited time
* Common mistake: building a tree only one level deep (goal → a few attacks) without decomposing further into concrete, testable leaf techniques

### Concept 2 — AND/OR Node Logic
* Definition: OR nodes represent alternative paths (any one suffices for the attacker); AND nodes represent a path requiring multiple conditions to hold simultaneously
* Architecture/process: most real attack trees are OR-heavy at the top (many independent ways to reach a goal) with occasional AND nodes for chained/combined attacks (e.g., "SSRF AND overly-permissive IAM role" — directly mirroring the Capital One case study from Day 14)
* Attack scenario: recognizing an AND relationship (like Capital One's SSRF+IAM combination) tells you that fixing *either* condition alone breaks that specific path — valuable prioritization information
* Mitigation: for OR-heavy goals, you must close *every* branch to fully prevent the goal; for AND branches, closing just one condition suffices for that specific path

### Concept 3 — Prioritizing Mitigations from a Tree
* Key terminology: leaf-level cost/likelihood/impact estimation, "cheapest attacker path"
* Practical application: for each leaf, rate the attacker's required skill/cost and the defender's mitigation cost; prioritize fixing the leaves that are simultaneously cheap-for-attacker and cheap-for-defender to close
* Common vulnerabilities in threat-modeling practice: treating every branch as equally worth fixing immediately, rather than prioritizing by realistic attacker cost-benefit
* Best practices: build the tree collaboratively with people who understand both the system and realistic attacker economics; revisit and prune/extend as the system evolves

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Attack tree | Goal-first hierarchical decomposition of attacker paths | Complements STRIDE's category-first approach with prioritization insight |
| OR node | Any one child condition suffices for the attacker | Means every branch must be closed to fully prevent the goal |
| AND node | All child conditions must jointly hold | Means closing any single condition breaks that specific path |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study attack tree structure, AND/OR logic, and how attack trees complement (rather than replace) STRIDE.
**Output:** Write a short explanation of when you'd reach for an attack tree vs. STRIDE in a real engagement.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Choose one of your own past full-stack projects (or Juice Shop, if you don't have a suitable past project readily documented).
2. Define a clear root goal (e.g., "Access another user's private data").
3. Decompose it into 3–5 sub-goals, then into concrete leaf-level attacks, drawing on specific techniques from Weeks 1–3 (SQLi, IDOR, XSS-driven session theft, JWT forgery, CSRF, etc.).
4. Mark AND/OR relationships explicitly.
5. Rate each leaf's rough attacker cost and defender mitigation cost.

**Expected Result:** A complete, multi-level attack tree diagram with prioritized mitigations noted.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Build a 2-level attack tree for the goal "Perform an unauthorized bank transfer," using at least one technique each from Week 1 and Week 3.
### Problem 2
Identify an AND-node relationship in your tree, similar to the Capital One SSRF+IAM pattern, and explain why breaking either condition suffices to close that path.
### Problem 3
Given limited time before a real engagement, which 2 leaves from your Session 2 tree would you prioritize testing first, and why?
### Challenge
Combine today's attack tree with yesterday's STRIDE-annotated DFD for the same system — identify any threat your STRIDE pass missed that the attack tree surfaced, or vice versa.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: STRIDE Framework (Day 19)
Recall without notes: all six STRIDE categories and how they map to concrete vulnerability classes.
### Spaced-Repetition Review
* **Yesterday:** STRIDE framework
* **5 days ago:** CSRF & Clickjacking
* **1 week ago:** week 2 wrap / Capital One case study — revisit this specifically, since today's AND-node concept directly mirrors that breach's structure

---

# 🧪 6. Active Recall Exercises
1. What is an attack tree?
2. How does it differ from STRIDE in approach (goal-first vs. category-first)?
3. What's the difference between an AND node and an OR node?
4. What would you need to build a realistic attack tree (knowledge of the system + knowledge of concrete attack techniques)?
5. What's the value of estimating attacker cost/likelihood per leaf?
6. How would an attack tree inform a red team's engagement planning?
7. How would you prioritize mitigations from a completed tree?
8. Difference between an OR-heavy tree and an AND-heavy tree in terms of defensive strategy?
9. Real-world example: map Capital One's SSRF+IAM combination as an AND node.
10. Teach attack-tree construction to a junior developer using their own project as an example.

### Feynman Test
Explain attack trees and how they complement STRIDE in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Diagramming tool (same as Day 19) for attack tree visualization
### Today's Tool Goal
Produce a clean attack tree diagram with clearly marked AND/OR nodes and leaf-level ratings.
### Tool Success Criteria
My attack tree is readable and clearly distinguishes AND from OR relationships visually.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment / capstone threat-model prep for Week 11
Today's contribution: produce a complete attack tree for "Access another user's account/data" against Juice Shop, complementing yesterday's STRIDE-annotated DFD, to include in your Month 1 capstone report.
### Deliverable
`Day 20: Produced attack tree for unauthorized data access against Juice Shop, with prioritized mitigations.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer hands you a one-paragraph description of a ride-sharing app and asks you to sketch an attack tree, live, for the goal "Take a ride without paying."
### My Task
1. Identify at least 3 distinct sub-goals/paths (e.g., manipulate the fare-calculation API, exploit a coupon/promo logic flaw, tamper with payment confirmation callback).
2. Mark which are OR (independent paths) vs. any AND relationships.
3. Identify the leaf-level technique for each (e.g., IDOR on fare object, race condition on promo redemption).
4. Rank them by realistic attacker cost.
5. Propose the highest-leverage single fix.
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
* [ ] Study notes  * [ ] Attack tree produced  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice (diagramming)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Attack tree structure and AND/OR logic
2. How attack trees complement STRIDE
3. How to prioritize mitigations using leaf-level cost estimation
4. The Capital One breach as an AND-node example
5. How tomorrow's lab/Burp-configuration day rounds out Week 3's client-side and design-analysis skills

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Self-directed (no PortSwigger/TryHackMe lab maps directly to attack-tree construction)
**Lab Name:** "Red Team Planning Simulation: Prioritize the Attack Tree"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60 minutes (folded into Session 2/3 above)
### Lab Objective
Simulate the real-world task of a red team lead who has limited engagement time and must choose which 2–3 attack-tree branches to actually test, using your Session 2 tree.
### Environment
Target: your own attack tree from today · Tools: none beyond your diagram and notes
### Lab Tasks
1. Assume you have time to test only 3 of your tree's leaf branches in an upcoming engagement.
2. Justify your selection using cost/likelihood/impact reasoning.
3. Write a one-paragraph justification as if presenting this prioritization to a client.
4. Note which branches you're consciously choosing NOT to test, and why that's an acceptable risk decision given constraints.
5. Document the result.
### What I Need to Discover
How do real security teams make defensible prioritization decisions under time constraints? Is "test everything" ever actually the right answer, or is prioritization itself a core professional skill?
### Lab Success Criteria
Produce a clearly justified prioritization with documented reasoning, distinguishing tested vs. accepted-risk branches.

---
---

# 🛡️ Day 21 — Week 3 Lab Day: Burp Suite Deep Configuration & Consolidation

**Date:** 09/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Advanced Burp Suite configuration and full Week 3 consolidation
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Complete any remaining PortSwigger XSS (30 labs total), CSRF (12 labs), or Clickjacking (5 labs) labs left unfinished from Days 15–18.
* Configure Burp Suite's advanced features against your own past projects: scope restriction, match/replace rules, and a basic custom scan configuration.
* Consolidate Week 3's client-side/design-analysis material into your portfolio.

### Success Criteria
* Lab backlog from Days 15–20 is at zero (or explicitly logged as carry-over).
* Burp Suite is configured with a proper scope against one of your own personal projects, ready for future use.
* Week 3 findings (XSS, CSRF, clickjacking) plus threat-modeling artifacts (STRIDE DFD, attack tree) are consolidated into your portfolio repo.

---

# 📚 2. Topics to Study
### Primary Topic
**Advanced Burp Suite Configuration & Week 3 Consolidation**
### Secondary Topics
* Target scope configuration (avoiding accidentally testing out-of-scope hosts)
* Match and Replace rules (automatically modifying requests, e.g., adding a header on every request)
* Reviewing and finalizing Week 3's threat-modeling artifacts alongside its exploitation findings

### Priority
🔴 **Must Know:** how to set Burp's target scope correctly before testing anything — critical professional/ethical hygiene to avoid ever touching out-of-scope systems
🟡 **Should Know:** Match and Replace for streamlining repetitive testing tasks (e.g., always injecting a specific test header)
🟢 **Nice to Know:** Burp extensions from the BApp Store relevant to XSS/CSRF testing (e.g., a CSP-evaluator extension)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Burp Scope Configuration
* What it is: explicitly defining which hosts/URLs are in-scope for a given project, so Burp's tools (and your own manual testing) never inadvertently target something outside authorized boundaries
* Why it matters: this is a core professional and ethical practice — accidentally testing out-of-scope systems (even unintentionally, via a redirect or shared infrastructure) can have serious legal and professional consequences
* How it works: Project Options → Scope → define included/excluded URL patterns; combine with "Suite-wide" scope enforcement so Proxy, Repeater, and Intruder all respect it consistently
* Best practices: always set scope explicitly at the start of any engagement, even personal lab work, to build the habit before it matters in a paid/authorized context

### Concept 2 — Match and Replace Rules
* Definition: rules that automatically modify specified parts of requests/responses matching a pattern, applied consistently across all traffic
* Practical application: automatically adding a custom test header, or automatically decoding a specific parameter for easier inspection
* Best practices: use sparingly and document any active rules, since they can cause confusing results if forgotten mid-session

### Concept 3 — Week 3 Consolidation
* Key terminology: same actionable-finding structure established in Week 1 (Day 7) and Week 2 (Day 14)
* Practical application: finalize the XSS/CSRF/clickjacking findings log, and package the STRIDE DFD + attack tree as a combined "Threat Modeling: Juice Shop Authentication & Data Access" artifact
* Best practices: cross-reference the threat-modeling artifacts against the actual exploitation findings — did your proactive STRIDE/attack-tree analysis predict the vulnerabilities you actually found through hands-on testing? This comparison is itself a valuable, interview-worthy insight

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Scope enforcement | Restricting tools to explicitly authorized targets | Core professional/ethical hygiene, non-negotiable in real engagements |
| Match and Replace | Automatic request/response modification rules | Streamlines repetitive testing tasks |
| Predictive validation | Comparing threat-modeling predictions against actual findings | Demonstrates the practical value of proactive threat modeling |

---

# ⏱️ 4. Study Schedule

## Session 1 — Backlog Triage & Burp Configuration (45–60 min)
List remaining labs from Days 15–20 and prioritize. Configure Burp's scope and one Match and Replace rule against a personal project.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Lab Backlog + Consolidation (60–90 min)
Complete remaining backlog labs. Then consolidate Week 3 findings into the standardized actionable format, and package the STRIDE DFD + attack tree together as a combined threat-modeling artifact.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Without notes, explain the fix for stored XSS vs. DOM-based XSS.
### Problem 2
Without notes, explain why CSRF tokens and SameSite cookies are complementary, not redundant, defenses.
### Problem 3
Compare your STRIDE/attack-tree predictions (Days 19–20) against your actual Week 1–3 hands-on findings — did the proactive analysis predict what you found? Write a short reflection.
### Challenge
Re-attempt, fully unaided, the single hardest XSS or CSRF lab from this week.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full Week 3 recall sweep: Reflected XSS → Stored/DOM XSS → CSRF/Clickjacking → Insecure Deserialization → STRIDE → Attack Trees. Answer what/why/how/attack/defense for each from memory.
### Spaced-Repetition Review
* **Yesterday:** Attack trees
* **This week:** the full client-side/design-analysis arc
* **3 weeks ago:** SQL injection (a good long-interval spaced-repetition check)

---

# 🧪 6. Active Recall Exercises
1. Cold-recall reflected XSS's context-aware encoding fix.
2. Cold-recall stored vs. DOM-based XSS distinctions.
3. Cold-recall CSRF's ambient-authority root cause and its two-layer defense.
4. Cold-recall why insecure deserialization's fix is "avoid the mechanism" rather than "use it safely."
5. Cold-recall all six STRIDE categories.
6. Cold-recall AND vs. OR logic in attack trees.
7. Rank this week's five technical vulnerability classes by real-world prevalence, to the best of your knowledge, and justify your ranking.
8. Which of this week's topics connects most directly to the Capital One case study from Week 2?
9. Give one real-world breach example for at least two of this week's classes.
10. Teach the full Week 3 arc (client-side attacks + threat modeling) to a junior developer in under 4 minutes.

### Feynman Test
Explain how Week 3's exploitation topics (XSS, CSRF, deserialization) and its design topics (STRIDE, attack trees) fit together as one coherent skill set. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (full advanced configuration review)
### Today's Tool Goal
Confirm scope enforcement works correctly (test that an out-of-scope request is flagged/blocked as expected) and that your Match and Replace rule fires correctly.
### Tool Success Criteria
Burp is configured professionally enough that you'd be comfortable using this exact setup on a real, authorized paid engagement.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment (Week 3 section) + threat-modeling artifact
Today's contribution: publish `week3-client-side-findings.md` (XSS/CSRF/clickjacking, actionable format) AND `threat-model-juice-shop-auth.md` (combined STRIDE DFD + attack tree) to your portfolio repo.
### Deliverable
`Day 21: Published Week 3 findings report and combined threat-modeling artifact (STRIDE + attack tree) to portfolio repo.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "How do proactive threat modeling and reactive penetration testing complement each other in a mature AppSec program?"
### My Task
1. Explain STRIDE and attack trees as proactive, design-time techniques.
2. Explain penetration testing/manual exploitation as reactive, validation-focused techniques.
3. Use your own Day 19–21 comparison (did your threat model predict your actual findings?) as a concrete personal example.
4. Practice this explanation out loud, under 2 minutes.
5. Document the result.
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
* [ ] All Week 3 labs complete (or logged as carry-over)  * [ ] Burp scope + Match/Replace configured
* [ ] Week 3 findings report published  * [ ] Threat-modeling artifact published
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow (Start of Week 4)
1. Every Week 3 vulnerability class and threat-modeling technique, cold
2. How proactive threat modeling and reactive testing complement each other
3. Professional Burp Suite hygiene (scope enforcement)
4. What's still shaky heading into Week 4's logging/SOC/capstone material
5. How Week 4's logging topic connects to STRIDE's "Repudiation" category from Day 19

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy (catch-up) / Personal project (Burp configuration)
**Lab Name:** Learner's choice — hardest remaining Week 3 lab, plus Burp scope configuration exercise
**Difficulty:** ⭐⭐⭐☆☆ (self-selected)
**Estimated Time:** 60–90 minutes
### Lab Objective
Close out any remaining Week 3 lab backlog and establish a properly scoped Burp Suite project against a personal codebase for ongoing future use.
### Environment
Target: remaining Week 3 labs + your own personal project · Tools: Burp Suite · Prerequisite: Days 15–20
### Lab Tasks
1. Complete remaining backlog labs.
2. Set up a new Burp project with explicit scope for your personal project's local dev server.
3. Configure one Match and Replace rule.
4. Run a brief Proxy-intercepted browsing session against your own project to confirm scope enforcement works.
5. Document the setup as a reusable configuration note for future weeks.
### What I Need to Discover
Does your scope configuration correctly include only intended targets? What would happen (and how would you notice) if a request accidentally went out of scope?
### Lab Success Criteria
Complete remaining backlog labs, confirm scope enforcement works correctly, document the configuration for reuse.

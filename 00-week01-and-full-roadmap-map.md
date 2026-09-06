# 🛡️ 90-Day Cybersecurity Transition — Day-by-Day Study Plan

**Start Date:** Thursday, 20 August 2026 (shifted from the original 16 Aug start)
**End Date:** ~Tuesday, 17 November 2026
**Format:** Week 1 is fully expanded using your 14-section template below. Weeks 2–12 are laid out as a complete day-by-day topic map so you can see the entire 90 days at once — tell me which week to expand next and I'll build it out in the same full format.

---
---

# 🗓️ WEEK 1 — Injection Attacks & Input Trust Boundaries (Fully Expanded)

---

# 🛡️ Day 1 — SQL Injection Part 1: Classic & In-Band SQLi

**Date:** 20/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Manual SQL Injection exploitation and parameterized query defense
**Estimated Total Time:** 3–4 hours
**Difficulty:** Beginner

---

## 🎯 1. Daily Goal / Expected Outcome

By the end of today, I should be able to:

* Explain why string-concatenated SQL queries create an injectable trust boundary.
* Manually detect and exploit a classic (in-band) SQL injection using Burp Suite.
* Extract data from a database using `UNION`-based injection.
* Rewrite a vulnerable query using parameterized statements.

### Success Criteria

I can consider today successful if I can:

* Explain the difference between query logic and query data without notes.
* Demonstrate a working `UNION`-based SQLi payload against a lab target.
* Complete at least 3 PortSwigger SQLi labs.
* Document one finding in CVSS-style format.

---

# 📚 2. Topics to Study

### Primary Topic
**SQL Injection — Classic / In-Band (Error-based & Union-based)**

### Secondary Topics
* How web apps build SQL queries from user input
* The `UNION` operator and column-count matching
* Database fingerprinting (MySQL vs. PostgreSQL vs. MSSQL error messages)
* Parameterized queries vs. string concatenation

### Priority
🔴 **Must Know**
* Why untrusted input concatenated into a query becomes executable code
* How to find the injectable column count with `ORDER BY` / `UNION SELECT NULL`

🟡 **Should Know**
* Database-specific syntax differences (MySQL `--` vs `#`, MSSQL `;--`)
* Error-based data extraction (`extractvalue()`, `CAST()`)

🟢 **Nice to Know**
* Out-of-band SQLi (DNS exfiltration) — not required this week

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — The Trust Boundary Problem
* What it is: the point where user input crosses from "data" into "code"
* Why it matters: almost every injection class (SQL, NoSQL, command, template) is a variant of this one failure
* How it works: string concatenation lets attacker-supplied characters (`'`, `;`, `--`) change query structure
* Real-world example: 2008 Heartland Payment Systems breach began with SQLi
* Common security mistakes: building queries with `+` / template literals instead of placeholders

### Concept 2 — UNION-Based Extraction
* Definition: appending a second `SELECT` to pull data from other tables
* Architecture/process: match column count → match data types → replace columns with target data
* Attack scenario: extracting `username`/`password` from an unrelated `users` table via a product search field
* Defense/mitigation: parameterized queries make `UNION` injection structurally impossible

### Concept 3 — Parameterized Queries
* Key terminology: prepared statement, bind variable, placeholder (`?` / `$1` / `:name`)
* Practical application: using a driver's parameter API instead of building SQL strings
* Common vulnerabilities: mixing parameterized and concatenated fields in the same query (partial fix = still vulnerable)
* Best practices: never build identifiers (table/column names) from user input even with parameters — use allowlists there

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| In-band SQLi | Attacker sees results directly in the response | Easiest class to detect and exploit |
| Prepared statement | Query compiled with placeholders before data is bound | Data can never alter query structure |
| CVSS | Common Vulnerability Scoring System | Standard way to communicate severity to engineering teams |

---

# ⏱️ 4. Study Schedule

## Session 1 — Learn the Fundamentals
**Time: 45–60 minutes**

Study:
* The trust boundary problem
* How SQL queries are built in a typical Node.js/Express + raw SQL driver app
* `UNION`-based extraction mechanics

### Tasks
1. Read PortSwigger's "SQL injection" topic primer.
2. Take concise notes in your own words.
3. Write a one-paragraph explanation of *why* concatenation is dangerous.
4. Identify the Heartland Payment Systems breach as your real-world example.

**Output:** A short explanation of SQL injection written without copying the resource.

---

## ☕ Break — 10–15 minutes

---

## Session 2 — Hands-On Practice
**Time: 60–90 minutes**

### Lab / Exercise
**PortSwigger: "SQL injection vulnerability in WHERE clause allowing retrieval of hidden data"** and **"SQL injection UNION attack, determining the number of columns returned by the query"**

Complete:
1. Set up Burp Suite Community, configure browser proxy.
2. Solve the WHERE-clause bypass lab (`' OR 1=1--`).
3. Solve the UNION column-count lab.
4. Extract a full row of data using UNION SELECT.

### What I Should Practice
* Intercepting and modifying requests in Burp Repeater
* Reading SQL error messages to fingerprint the DB engine
* Iteratively building a UNION payload

### Expected Result
By the end of this session, I should have: two solved PortSwigger labs and a saved Burp Repeater history showing the working payloads.

---

## ☕ Break — 10–15 minutes

---

## Session 3 — Practice Problems
**Time: 30–45 minutes**

### Problem 1
A login form query is: `SELECT * FROM users WHERE username='$user' AND password='$pass'`. What single-character payload bypasses authentication?

### Problem 2
A product search returns 3 columns of data. Write the `UNION SELECT` skeleton to test for injectability.

### Problem 3
An error message reveals `you have an error in your SQL syntax; check the manual that corresponds to your MySQL server`. What does this tell you about the target and your next step?

### Challenge
Given a search field vulnerable to UNION SQLi with an unknown column count, describe your full methodology from detection to full data extraction, step by step.

For each problem: identify the security issue, explain why it's a problem, determine the potential impact, explain how you would mitigate it.

---

# 🔄 5. Revision of Previously Studied Material

**Time: 20–30 minutes**

Since this is Day 1, there is no prior curriculum day to review. Instead, spend this time reviewing your own diagnostic gaps from the baseline assessment: authN vs. authZ distinction, server-side RCE via `eval()`, and log triage methodology — skim one short primer on each so you recognize the terms when they appear later this week and in Week 4.

---

# 🧪 6. Active Recall Exercises

**Time: 15–20 minutes**

Answer these without looking at your notes:

1. What is SQL injection?
2. How does UNION-based extraction work?
3. Why does string concatenation cause this vulnerability?
4. What would an attacker need to exploit it (in terms of app behavior)?
5. What would the impact be for an e-commerce app's user table?
6. How would you detect this in a WAF/log?
7. How would you prevent it at the code level?
8. What is the difference between in-band and blind SQLi?
9. Give a real-world example.
10. Explain SQLi as if teaching it to a junior developer.

### Feynman Test
Explain SQL injection in 3–5 sentences using simple language. If you cannot explain it clearly, mark it 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice

**Tool:** Burp Suite Community Edition

### Today's Tool Goal
Learn how to:
* Configure FoxyProxy/browser proxy settings to route traffic through Burp
* Use Proxy → Intercept to capture a request
* Send a request to Repeater and modify parameters

### Commands / Features to Practice
```text
Proxy > Intercept > Forward
Repeater > Send (Ctrl+Enter)
Decoder (for URL-encoding payloads)
```

### Tool Success Criteria
I should be able to use Burp Repeater to send a modified SQLi payload and read the response diff without reloading the browser.

---

# 🏗️ 8. Project Connection

**Current Project:** Portfolio Piece #1 — OWASP Juice Shop Vulnerability Assessment (due end of Week 4)

Today's contribution:
* Identify and note any SQLi entry points in Juice Shop's login/search functionality
* Begin a findings log spreadsheet/markdown file: columns for vulnerability, location, severity, status

### Deliverable
`Day 1: Initialized Juice Shop findings log; identified candidate SQLi injection point in login form.`

---

# 📝 9. Practice / Security Challenge

### Scenario
An internal e-commerce platform's product search endpoint (`/api/search?q=`) passes the query parameter directly into a raw SQL string. A penetration test is scheduled next week.

### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact.
4. Demonstrate or reproduce it in an authorized lab (Juice Shop or PortSwigger).
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

🔴 1–12: Needs significant review · 🟡 13–21: Developing · 🟢 22–26: Good progress · 🏆 27–30: Strong mastery

---

# 📦 11. Daily Deliverables

* [ ] Study notes
* [ ] Completed hands-on lab (2 PortSwigger labs)
* [ ] Practice problems
* [ ] Active-recall answers
* [ ] Tool practice (Burp Repeater)
* [ ] Project contribution (findings log started)
* [ ] GitHub/documentation update
* [ ] End-of-day self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow

1. Why string concatenation in SQL is dangerous
2. How UNION-based extraction works, step by step
3. How to determine column count for a UNION payload
4. The difference between error-based and union-based SQLi
5. The Heartland Payment Systems breach as a real-world case

---

# 🚀 13. Daily Completion Summary

**Today I learned:** [fill in after session]
**Today I practiced:** [fill in]
**Today I built:** [fill in]
**Today I struggled with:** [fill in]
**Tomorrow I need to review:** [fill in]
**GitHub Evidence:** [link/commit]

**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB

**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "SQL injection UNION attack, retrieving data from other tables"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes

### Lab Objective
Determine the number of columns and data types available for a UNION-based attack, then extract usernames and passwords from an unrelated `users` table.

### Environment
* Target: PortSwigger lab instance
* Tools: Burp Suite Community, browser
* Starting knowledge: basic SQL SELECT syntax

### Lab Tasks
1. Identify the injectable parameter in the product category filter.
2. Determine the number of columns using `ORDER BY` or `UNION SELECT NULL,NULL...`.
3. Determine which columns accept string data.
4. Query `information_schema` (or guess table name if given) to find the `users` table.
5. Extract username and password columns via UNION.

### What I Need to Discover
* What is happening when the app returns an error vs. a blank page as you add columns?
* Why does column *count* matter for UNION to work?
* What security weakness allows you to query tables the app never intended to expose?
* How could this be abused beyond just reading data (e.g., in write-enabled DBs)?
* What is the impact if this were a production payments database?
* How should it be fixed?

### Lab Success Criteria
* Document the finding
* Reproduce the issue
* Explain the root cause
* Explain the impact
* Demonstrate the issue safely in the lab
* Recommend a mitigation (parameterized queries + least-privilege DB account)

---
---

# 🛡️ Day 2 — SQL Injection Part 2: Blind & Second-Order SQLi

**Date:** 21/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Blind (boolean/time-based) and second-order SQL injection
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
By the end of today, I should be able to:
* Explain why blind SQLi requires inference rather than direct output.
* Exploit a boolean-based blind SQLi to extract data one bit at a time.
* Exploit a time-based blind SQLi using conditional sleep functions.
* Explain what makes second-order SQLi harder to detect in code review.

### Success Criteria
* Explain boolean vs. time-based blind SQLi without notes.
* Demonstrate a working blind SQLi extraction (manual, then with `sqlmap`).
* Complete at least 2 PortSwigger blind SQLi labs.
* Document a second-order SQLi scenario from a past project of your own.

---

# 📚 2. Topics to Study

### Primary Topic
**Blind SQL Injection (Boolean-based & Time-based) and Second-Order SQLi**

### Secondary Topics
* `sqlmap` basics and when automation is appropriate vs. risky
* Conditional response differences as an oracle
* Second-order injection (payload stored now, executed later in a different query)

### Priority
🔴 **Must Know:** boolean-based inference logic, why response timing reveals data
🟡 **Should Know:** `sqlmap` flags (`--technique`, `--dbms`, `--risk`)
🟢 **Nice to Know:** out-of-band exfiltration via DNS

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Boolean-Based Blind SQLi
* What it is: injecting conditions that change the page's *behavior* (not visible output) based on true/false
* Why it matters: most real-world SQLi is blind — no errors, no visible data
* How it works: `AND 1=1` (page loads normally) vs `AND 1=2` (page differs) as an oracle
* Real-world example: many login-bypass CTF scenarios rely on this
* Common mistake: assuming "no error = not vulnerable"

### Concept 2 — Time-Based Blind SQLi
* Definition: using `SLEEP()`/`WAITFOR DELAY` conditionally to infer data via response time
* Process: `IF(condition, SLEEP(5), 0)` — a 5-second delay confirms the condition was true
* Attack scenario: extracting an admin password character-by-character when there's zero visible difference in output
* Mitigation: same as all SQLi — parameterized queries; also rate-limiting and DB timeout caps reduce practicality

### Concept 3 — Second-Order SQLi
* Key terminology: "stored" injection point vs. "sink" point
* Practical application: a username is safely stored via parameterized insert, but later concatenated unsafely into an admin report query
* Common vulnerabilities: teams parameterize the *input* form but forget every downstream query that reads the same stored data
* Best practices: parameterize at every query site, not just the first point of entry

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Blind SQLi | No visible output difference from injection | Most production SQLi is blind — you must know inference techniques |
| Oracle | Any observable signal (content, timing, error) that reveals true/false | The whole basis of blind exploitation |
| Second-order injection | Payload executes in a different query than where it was submitted | Easy to miss in code review; breaks "we sanitize on input" assumptions |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study boolean-based and time-based blind SQLi mechanics, then second-order SQLi.
**Output:** Written explanation of the difference between first-order and second-order SQLi, in your own words.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** PortSwigger — "Blind SQL injection with conditional responses" and "Blind SQL injection with time delays"
1. Detect the blind injection point.
2. Build a boolean oracle payload.
3. Manually extract one character of the admin password.
4. Switch to `sqlmap --batch --technique=B` to confirm/automate, and compare against your manual result.

**Expected Result:** Two solved labs + a `sqlmap` command history showing successful extraction.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A search page shows "No results" for both `' AND 1=1--` and `' AND 1=2--`. What does this suggest, and what should you try next?
### Problem 2
Write the logic (pseudocode) for extracting a password one character at a time using boolean blind SQLi.
### Problem 3
A username is stored via a parameterized `INSERT`, but an admin dashboard later builds `"SELECT * FROM logs WHERE user='" + username + "'"`. Identify the vulnerability class and why input-time sanitization didn't help.
### Challenge
Design (on paper) a full blind-SQLi extraction script's logic without using `sqlmap` — binary search over ASCII values via time-based delay.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: SQL Injection — Classic/In-Band (Day 1)
Without notes, answer: What is UNION-based SQLi? Why does column count matter? What's the fix? Then check your Day 1 notes and correct any gaps.
### Spaced-Repetition Review
* **Yesterday:** In-band SQLi
* **3–7 days ago:** N/A (Day 2 of curriculum)

---

# 🧪 6. Active Recall Exercises
1. What is blind SQL injection?
2. How does boolean-based inference work?
3. Why does time-based SQLi work even with zero visible output difference?
4. What would an attacker need to exploit second-order SQLi?
5. What's the impact of a blind SQLi in an authentication system?
6. How would you detect blind SQLi attempts in logs (repeated similar queries with slight variations, abnormal response-time patterns)?
7. How would you prevent second-order SQLi specifically?
8. Difference between first-order and second-order SQLi?
9. Real-world example of blind SQLi exploitation.
10. Teach time-based blind SQLi to a junior developer in plain language.

### Feynman Test
Explain blind SQLi in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** `sqlmap`
### Today's Tool Goal
Learn how to point `sqlmap` at a request, choose a technique, and interpret its output responsibly (never against unauthorized targets).
### Commands to Practice
```text
sqlmap -u "https://lab-target/filter?category=1" --batch --level=2 --risk=1
sqlmap -u "..." --technique=B --current-db
sqlmap -u "..." --technique=T --time-sec=3
```
### Tool Success Criteria
I can run `sqlmap` against an authorized lab target and correctly interpret whether it confirms boolean- or time-based injection.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: test Juice Shop's other input fields (feedback form, product review) for blind/second-order SQLi and add findings to your log with CVSS estimates.
### Deliverable
`Day 2: Added blind SQLi test results for feedback form; logged second-order injection hypothesis for review path.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A healthcare portal's patient-notes field appears completely unaffected by SQLi payloads — no errors, identical page content regardless of input.
### My Task
1. Identify the vulnerability/threat (consider blind classes).
2. Explain the root cause.
3. Determine the impact given PHI is involved.
4. Demonstrate/reproduce safely in a lab analog.
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
* [ ] `sqlmap` practice  * [ ] Project contribution  * [ ] Documentation update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Boolean vs. time-based blind SQLi
2. Why second-order SQLi bypasses input-time sanitization
3. How `sqlmap` techniques map to manual methods
4. Why "no visible error" doesn't mean "not vulnerable"
5. A real second-order SQLi scenario

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "Blind SQL injection with time delays and information retrieval"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Use time-based blind SQLi to extract the administrator's password one character at a time.
### Environment
Target: PortSwigger lab · Tools: Burp Suite, `sqlmap` · Prerequisite: Day 1 SQLi fundamentals
### Lab Tasks
1. Confirm the injection point causes a delay with `SLEEP(5)`.
2. Build a conditional payload testing password length.
3. Build a conditional payload testing character-by-character value.
4. Automate the last mile with `sqlmap` and compare timing to your manual result.
5. Log in as admin using the extracted credential.
### What I Need to Discover
What signal tells you the condition was true? Why does this technique still work with a fully generic error page? What's the real-world detection signature for this attack (repeated slow queries from one IP)? What's the impact and the fix?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 3 — NoSQL Injection (MongoDB Operator Abuse)

**Date:** 22/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** NoSQL injection via MongoDB query operators
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain how NoSQL query languages can still be "injected" despite not using SQL syntax.
* Exploit MongoDB operator injection (`$gt`, `$ne`, `$regex`) to bypass authentication.
* Implement schema validation with `zod`/`joi` and Mongo sanitization middleware.

### Success Criteria
* Explain why JSON-body injection differs from SQLi without notes.
* Demonstrate an authentication bypass using `$ne`/`$gt` in a lab.
* Fix a vulnerable Express/Mongoose route with schema validation.

---

# 📚 2. Topics to Study
### Primary Topic
**NoSQL Injection — MongoDB Operator Injection**
### Secondary Topics
* JSON body parsing and how operators sneak into query objects
* `$where` JavaScript execution risk
* Input schema validation (`joi`, `zod`) and `express-mongo-sanitize`

### Priority
🔴 **Must Know:** how `{"password": {"$ne": null}}` bypasses a naive login check
🟡 **Should Know:** `$regex` for blind data extraction
🟢 **Nice to Know:** `$where` operator RCE-adjacent risk in older MongoDB versions

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Operator Injection via JSON Body
* What it is: when a client can submit `{"$gt": ""}` instead of a string, Mongo interprets it as a query operator, not a literal
* Why it matters: developers assume "no SQL syntax = no injection," which is false
* How it works: `db.users.findOne({username: req.body.username, password: req.body.password})` — if `req.body.password` is `{"$ne": null}`, the check always matches
* Real-world example: numerous CTF and bug-bounty writeups on MEAN-stack login bypass
* Common mistake: trusting `Content-Type: application/json` bodies as "already safe" because there's no string concatenation

### Concept 2 — Schema Validation as Defense
* Definition: enforcing expected types (string, not object) before the query layer ever sees the input
* Architecture: middleware validates `req.body` against a schema (`zod.object({username: z.string(), password: z.string()})`) before the handler runs
* Attack scenario without it: any field can silently become an operator object
* Mitigation: reject non-primitive types for fields that should be strings; use `express-mongo-sanitize` to strip `$`/`.` keys as defense-in-depth

### Concept 3 — Blind NoSQLi Extraction
* Key terminology: `$regex` operator, character-by-character inference (same *logic* as blind SQLi, different syntax)
* Practical application: brute-forcing a field value via `{"field": {"$regex": "^a"}}` and observing true/false
* Common vulnerabilities: search/filter endpoints accepting raw JSON operators from the client
* Best practices: never pass `req.query`/`req.body` directly into a Mongo filter object — always project through a strict allowlist

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Operator injection | Attacker-supplied JSON becomes a Mongo query operator instead of a literal | Root cause of most NoSQLi auth bypasses |
| `$where` | Runs raw JS server-side in old MongoDB versions | Near-RCE risk; should be disabled |
| Schema validation | Enforcing type/shape of input before it reaches the query | Primary defense against operator injection |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study the operator-injection mechanism and read the MongoDB security checklist section on query operators.
**Output:** Explain in your own words why `$ne`/`$gt` cause an auth bypass, with a code snippet.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** TryHackMe "NoSQL Injection" room (or build a 20-line local Express + MongoDB login route to self-test if the room is unavailable).
1. Spin up a minimal vulnerable login endpoint locally (Docker MongoDB + Express).
2. Send `{"username": "admin", "password": {"$ne": null}}` via Burp/Postman and confirm bypass.
3. Patch it with `zod` schema validation and `express-mongo-sanitize`; confirm the bypass now fails.
4. Try `$regex` against a "search notes" endpoint to blind-extract a hidden field value.

**Expected Result:** A before/after code diff showing the vulnerable and fixed route, both tested.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Given `db.users.findOne({user: req.body.user, pass: req.body.pass})`, craft a JSON body that bypasses authentication for any username.
### Problem 2
A search endpoint accepts `{"note": {"$regex": "^secret"}}`. Explain how you'd use this to extract a hidden note's full content character by character.
### Problem 3
Why does adding `JSON.parse()` alone *not* fix operator injection?
### Challenge
Design a schema (in `zod` or `joi` syntax) that would make today's vulnerable login route safe, and explain each constraint's purpose.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Blind SQL Injection (Day 2)
Recall without notes: boolean vs time-based blind SQLi, then check against your notes.
### Spaced-Repetition Review
* **Yesterday:** Blind & second-order SQLi
* **2 days ago:** Classic/UNION SQLi

---

# 🧪 6. Active Recall Exercises
1. What is NoSQL injection?
2. How does `$ne`/`$gt` operator injection bypass authentication?
3. Why does JSON-body input still create an injection risk despite no SQL syntax?
4. What would an attacker need (in terms of request control) to exploit it?
5. What's the impact on an app relying on Mongo for auth?
6. How would you detect this in logs (JSON bodies containing `$` keys where strings are expected)?
7. How would you prevent it (schema validation + sanitize middleware)?
8. Difference between SQLi and NoSQLi at the mechanism level?
9. Real-world example / CTF pattern.
10. Teach NoSQLi to a junior dev in plain language.

### Feynman Test
Explain NoSQL operator injection in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (Repeater with JSON bodies) + Postman
### Today's Tool Goal
Modify JSON request bodies to inject Mongo operators and observe server behavior differences.
### Commands / Features to Practice
```text
Burp Repeater: edit Content-Type: application/json body directly
Postman: raw JSON body with {"$ne": null} style payloads
```
### Tool Success Criteria
I can craft and send a JSON-operator-injection payload and correctly read whether it succeeded.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment (Juice Shop uses SQL, not Mongo, so today's contribution is a standalone mini-writeup)
Today's contribution: a short standalone markdown writeup — "NoSQL Injection: Mechanism, Exploitation, and Defense" — with your own vulnerable/fixed code sample, to include as supplementary material in your portfolio.

### Deliverable
`Day 3: Added standalone NoSQL injection writeup with vulnerable/fixed Express+Mongo code sample.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A fintech startup's mobile app login API accepts a JSON body and queries MongoDB directly with the parsed object. A bug bounty researcher reports being able to log in as any user.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact for a fintech app specifically.
4. Demonstrate/reproduce it in your local lab.
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
* [ ] Study notes  * [ ] Local vulnerable/fixed lab built and tested  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Tool practice  * [ ] Project writeup  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. How Mongo operator injection works
2. Why schema validation is the primary fix
3. How blind NoSQLi extraction via `$regex` works
4. Difference between SQLi and NoSQLi mechanisms
5. A real bypass scenario end-to-end

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Lab (Docker) — TryHackMe NoSQL room as backup
**Lab Name:** "MongoDB Login Bypass via Operator Injection"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Bypass authentication on a self-built Express+MongoDB login route using operator injection, then patch it.
### Environment
Target: local Docker MongoDB + Express app · Tools: Burp/Postman, Node.js, `zod`/`joi` · Prerequisite: Day 3 concepts
### Lab Tasks
1. Build the minimal vulnerable route.
2. Confirm normal login works with valid credentials.
3. Bypass it using `$ne`/`$gt` payloads.
4. Add schema validation + `express-mongo-sanitize`.
5. Re-test the bypass and confirm it now fails.
### What I Need to Discover
Why does the bypass work at the query-object level? What's different about how Express parses JSON vs. form-urlencoded bodies here? What's the blast radius if this were a real auth system? How does schema validation close the gap structurally rather than just patching one payload?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, show the fix working.

---
---

# 🛡️ Day 4 — Command Injection & SSRF

**Date:** 23/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** OS command injection and Server-Side Request Forgery
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain how `child_process.exec()` creates a shell-injection risk vs. `execFile()`.
* Exploit a command injection to achieve arbitrary command execution in a lab.
* Exploit SSRF to reach an internal-only endpoint (e.g., cloud metadata service pattern).
* Apply allowlisting and safe API alternatives as fixes for both.

### Success Criteria
* Explain the `exec()` vs `execFile()` distinction without notes.
* Demonstrate command injection via a lab (chained command execution).
* Demonstrate SSRF reaching an internal resource in a lab.
* Document both findings with CVSS estimates.

---

# 📚 2. Topics to Study
### Primary Topic
**Command Injection (Node.js `child_process`) and Server-Side Request Forgery**
### Secondary Topics
* Shell metacharacters (`;`, `&&`, `|`, backticks)
* SSRF against cloud metadata endpoints (e.g., `169.254.169.254`)
* Blind SSRF (no response body, only side effects/timing)

### Priority
🔴 **Must Know:** why passing user input to a shell is dangerous; what SSRF is and why internal endpoints are "trusted by network position"
🟡 **Should Know:** DNS rebinding as an SSRF-filter bypass technique
🟢 **Nice to Know:** SSRF via redirect chains

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — OS Command Injection
* What it is: user input reaching a shell interpreter, where metacharacters change what command runs
* Why it matters: this is RCE — the most severe class of vulnerability
* How it works: `exec("ping " + userInput)` — input `"; cat /etc/passwd"` chains a second command
* Real-world example: 2017 Equifax breach exploited a related class (Apache Struts OGNL/command execution) at massive scale
* Common mistake: trying to "sanitize" by blocklisting characters instead of avoiding the shell entirely

### Concept 2 — SSRF (Server-Side Request Forgery)
* Definition: tricking a server into making a request to a destination the attacker chose, often an internal-only resource
* Architecture/process: an app fetches a URL on the user's behalf (e.g., "import from URL" feature) without restricting the target
* Attack scenario: pivoting through the server to reach cloud metadata services and extract IAM credentials — directly relevant to the Capital One breach case you'll study in Week 2
* Defense/mitigation: allowlist destination hosts, block internal IP ranges, disable redirects, use a network-level egress proxy

### Concept 3 — Safe API Alternatives
* Key terminology: `execFile()`/`spawn()` (array-based args, no shell interpretation) vs `exec()` (string passed to shell)
* Practical application: refactor `exec("ping " + host)` to `execFile("ping", [host])`
* Common vulnerabilities: developers "fixing" command injection by escaping quotes, which is bypassable
* Best practices: never invoke a shell with untrusted input; validate/allowlist inputs even when using safe APIs

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Shell metacharacter | `;`, `&&`, `|`, backtick — chain or substitute commands | The mechanism of command injection |
| SSRF | Server tricked into requesting attacker-chosen URL | Common pivot into internal/cloud infrastructure |
| Metadata service | Internal-only endpoint exposing instance credentials (e.g. `169.254.169.254`) | Classic high-value SSRF target |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study command injection mechanics and SSRF mechanics side by side — both are "trust boundary" failures like SQLi, just in different contexts (shell, network).
**Output:** One paragraph connecting command injection, SQLi, and NoSQLi as the same root pattern in different syntaxes.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Labs:** PortSwigger — "OS command injection, simple case" and "Basic SSRF against the local server"
1. Solve the command injection lab; chain a second command to read a file.
2. Solve the SSRF lab; reach an internal-only admin endpoint.
3. Locally: refactor a vulnerable `exec()` call to `execFile()` and confirm injection no longer works.

**Expected Result:** Two solved labs + a local before/after code fix for command injection.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
`exec("nslookup " + userDomain)` — craft a payload that also runs `whoami`.
### Problem 2
An "avatar from URL" feature fetches whatever URL the user submits. Explain how you'd use this to probe for an internal admin panel on `10.0.0.0/8`.
### Problem 3
Why doesn't blocklisting `;` and `&&` fully fix command injection (consider `|`, newlines, `$()`, backticks)?
### Challenge
Design an allowlist-based SSRF defense for a "fetch image from URL" feature — what exactly would you check, and where (DNS resolution time vs. request time)?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: NoSQL Injection (Day 3)
Recall without notes: how does `$ne`/`$gt` bypass work, and what's the fix?
### Spaced-Repetition Review
* **Yesterday:** NoSQL injection
* **2 days ago:** Blind/second-order SQLi
* **3 days ago:** Classic SQLi

---

# 🧪 6. Active Recall Exercises
1. What is OS command injection?
2. How does SSRF work?
3. Why does `exec()` create risk that `execFile()` avoids?
4. What would an attacker need to exploit SSRF against a cloud metadata endpoint?
5. What's the impact of command injection vs. SSRF (RCE vs. credential/data exposure)?
6. How would you detect command injection attempts in logs (shell metacharacters in parameters)?
7. How would you prevent SSRF (allowlisting, blocking internal ranges, disabling redirects)?
8. Difference between command injection and SSRF as trust-boundary failures?
9. Real-world example connecting to the Capital One breach (previewed for Week 2).
10. Teach SSRF to a junior developer in plain language.

### Feynman Test
Explain command injection AND SSRF, each in 2–3 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (Repeater + Collaborator concept for blind SSRF)
### Today's Tool Goal
Learn to test for blind SSRF using an out-of-band interaction concept (Burp Collaborator or a public request-bin equivalent).
### Commands / Features to Practice
```text
Burp Repeater: modify "url" parameter to internal IP ranges (127.0.0.1, 169.254.169.254)
Burp Collaborator: generate a unique payload URL, submit it, check for a callback
```
### Tool Success Criteria
I can distinguish visible SSRF (response reflected) from blind SSRF (only an out-of-band callback confirms it).

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: test any file/URL-import features in Juice Shop for SSRF; test any OS-level integration points for command injection; log findings.
### Deliverable
`Day 4: Logged SSRF test results for image/URL import feature; documented command injection test methodology used.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An internal "generate PDF report from URL" microservice fetches whatever URL is submitted and renders it. It runs inside the company's AWS VPC.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact (consider what's reachable inside a VPC — metadata service, internal admin tools).
4. Demonstrate/reproduce in an authorized lab analog.
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
* [ ] Study notes  * [ ] Completed labs (2)  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice (Collaborator/blind SSRF)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. How command injection works and how to prevent it structurally
2. How SSRF works and why internal network position matters
3. Blind vs. visible SSRF
4. Why the Capital One breach is a canonical SSRF example (full detail comes Week 2)
5. The `exec()` vs `execFile()` fix pattern

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "SSRF with blacklist-based input filter" (stretch lab after the basic one)
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Bypass a naive blacklist-based SSRF filter to reach an internal admin endpoint.
### Environment
Target: PortSwigger lab · Tools: Burp Suite · Prerequisite: basic SSRF lab completed earlier in the day
### Lab Tasks
1. Identify the URL-fetching feature and its filter.
2. Test obvious blocked values (`localhost`, `127.0.0.1`) and confirm they're blocked.
3. Try bypass techniques: alternate IP encodings (decimal, octal), `[::1]`, redirect chains.
4. Reach the internal admin endpoint.
5. Document which bypass worked and why.
### What I Need to Discover
Why do blacklists fail structurally against a determined attacker? What would an allowlist-based approach have prevented here? What's the impact if the internal endpoint were the cloud metadata service instead? How should this be fixed at both the app and network level?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation (allowlist + network segmentation).

---
---

# 🛡️ Day 5 — Server-Side Template Injection (SSTI)

**Date:** 24/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Template injection detection and exploitation (Jinja2/Twig-style)
**Estimated Total Time:** 3–4 hours
**Difficulty:** Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain how template engines evaluating user input as template syntax leads to RCE.
* Detect SSTI using the classic polyglot payload technique.
* Escalate a confirmed SSTI to remote code execution in a lab.
* Apply sandboxing/logic-less-template defenses.

### Success Criteria
* Explain SSTI detection methodology without notes.
* Demonstrate detection → confirmation → RCE escalation in a lab.
* Explain Content Security Policy's role as defense-in-depth (not a primary fix).

---

# 📚 2. Topics to Study
### Primary Topic
**Server-Side Template Injection (SSTI)**
### Secondary Topics
* Template engine basics (Jinja2, Twig, Freemarker patterns)
* SSTI detection polyglot: `${{<%[%'"}}%\`
* Sandboxed vs. logic-less template engines (Mustache/Handlebars-style)

### Priority
🔴 **Must Know:** the difference between XSS (client-side HTML injection) and SSTI (server-side code evaluation)
🟡 **Should Know:** engine-specific payload syntax (Jinja2 `{{7*7}}` vs Twig `{{7*'7'}}`)
🟢 **Nice to Know:** SSTI-to-RCE gadget chains in Python's Jinja2 (`__class__.__mro__` traversal)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — What Makes SSTI Different from XSS
* What it is: user input is embedded into a template *before* rendering, so the templating engine evaluates it as code, not just as data displayed in HTML
* Why it matters: SSTI often escalates directly to RCE, whereas XSS is client-side and limited to the browser context
* How it works: `render_template_string("Hello " + user_input)` — if `user_input` is `{{7*7}}`, output is `Hello 49`, confirming code execution
* Real-world example: numerous CVEs in CMS/wiki platforms using Jinja2/Twig with unsanitized user content in templates
* Common mistake: confusing SSTI with XSS because the initial symptom (reflected input) looks similar

### Concept 2 — Detection Methodology
* Definition: using math-expression polyglots that are valid syntax across multiple template engines to fingerprint the engine
* Architecture/process: submit `${{<%[%'"}}%\`, observe which characters get "eaten" or error out, narrowing down the engine
* Attack scenario: a "customize your email template" feature that renders user-supplied text server-side
* Defense/mitigation: never pass user input directly into a template *string* — only ever into template *variables*/context data

### Concept 3 — Escalation to RCE
* Key terminology: gadget chain, sandbox escape, `__class__`/`__globals__` traversal (Python-specific)
* Practical application: from confirming `{{7*7}}` → 49, escalate to `{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}`-style chains
* Common vulnerabilities: sandboxed engines that still expose enough introspection to escape
* Best practices: use logic-less template engines (Mustache) for any template rendering that touches user input; if a full engine is required, never let user input become part of the template source itself

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| SSTI | Template engine evaluates user input as code | Frequently escalates to full RCE |
| Polyglot payload | Single payload that probes multiple engines at once | Speeds up engine fingerprinting |
| Logic-less template | Engine with no code-execution capability (e.g. Mustache) | Structural fix — nothing to inject into |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study the SSTI-vs-XSS distinction and the detection polyglot technique.
**Output:** A written explanation of why SSTI is more severe than reflected XSS, with the math-expression detection logic explained in your own words.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** PortSwigger — "Basic server-side template injection" and "Server-side template injection using documentation"
1. Submit the detection polyglot and identify the engine from behavior.
2. Confirm code execution with a math-expression payload.
3. Escalate to reading a file or executing a command using engine-specific documentation lookup (this mirrors real-world methodology — you're expected to research the exact gadget syntax, not memorize it).

**Expected Result:** Two solved labs with documented payloads and reasoning for each escalation step.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A "preview your custom greeting" feature renders `render_template_string(f"Hi {name}!")`. Craft a detection payload for the `name` field.
### Problem 2
You confirm `{{7*7}}` renders as 49. What's your next step toward RCE, and why do you research engine-specific gadget chains rather than guessing?
### Problem 3
Why does switching from Jinja2 to Mustache (logic-less) eliminate this vulnerability class structurally, rather than just closing one payload?
### Challenge
Explain, at a high level, how CSP headers reduce the *impact* of some template-injection-adjacent client-side risks but do NOT stop server-side RCE from SSTI.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Command Injection & SSRF (Day 4)
Recall without notes: the `exec()` vs `execFile()` fix, and the SSRF allowlist approach.
### Spaced-Repetition Review
* **Yesterday:** Command Injection & SSRF
* **3 days ago:** Classic/blind SQL injection
* **This is also a good moment to connect the pattern:** SQLi, NoSQLi, command injection, and SSTI are all the same root cause — untrusted input crossing into a code/command context — expressed in four different syntaxes.

---

# 🧪 6. Active Recall Exercises
1. What is SSTI?
2. How does the detection polyglot work?
3. Why does SSTI often escalate to full RCE while XSS usually doesn't?
4. What would an attacker need to exploit SSTI (a feature that renders user input server-side)?
5. What's the impact of confirmed SSTI on a production app?
6. How would you detect SSTI attempts in logs (template syntax characters in form fields)?
7. How would you prevent it (logic-less templates, never embedding input into template source)?
8. Difference between SSTI and XSS?
9. Real-world example of a template-injection CVE pattern.
10. Teach SSTI to a junior developer in plain language.

### Feynman Test
Explain SSTI in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite Repeater + engine documentation research (official Jinja2/Twig docs)
### Today's Tool Goal
Practice the real-world SSTI workflow: detect → fingerprint → research gadget syntax → escalate, using documentation as a resource rather than memorized payloads.
### Commands / Features to Practice
```text
Payload: ${{<%[%'"}}%\
Payload: {{7*7}}
Payload: {{7*'7'}}   (Twig renders 7777777, Jinja2 renders 49 — engine fingerprint)
```
### Tool Success Criteria
I can fingerprint which template engine is in use purely from how it handles the polyglot payload.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: test any Juice Shop features that render user-supplied text (feedback, product descriptions in admin panel) for SSTI; document findings even if negative (absence of vulnerability is still a documented test).
### Deliverable
`Day 5: Completed SSTI test pass on Juice Shop feedback/admin rendering paths; documented negative and positive findings.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A SaaS platform lets users customize their outgoing notification emails with a "template" text box, rendered server-side before sending.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact (server compromise via RCE, not just email content tampering).
4. Demonstrate/reproduce in an authorized lab analog.
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
* [ ] Study notes  * [ ] Completed labs (2)  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice (fingerprinting)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. SSTI detection methodology end-to-end
2. Why SSTI escalates to RCE and XSS usually doesn't
3. Logic-less templates as a structural fix
4. How this week's four injection classes share one root cause
5. A real scenario where SSTI would be catastrophic

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy
**Lab Name:** "Server-side template injection with information disclosure via user-supplied objects"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Escalate SSTI to disclose sensitive server-side configuration information via object introspection.
### Environment
Target: PortSwigger lab · Tools: Burp Suite, official Jinja2 docs · Prerequisite: earlier SSTI labs today
### Lab Tasks
1. Detect and confirm the injection point.
2. Fingerprint the engine.
3. Use object introspection payloads to explore available Python objects in scope.
4. Locate and disclose a sensitive config value.
5. Document the full chain from detection to disclosure.
### What I Need to Discover
Why does object introspection work even in a "sandboxed" context? What does this tell you about the limits of blocklist-based sandboxing? What's the realistic impact if this were a production secrets-management system? What's the structural fix vs. the tactical patch?
### Lab Success Criteria
Document the finding, reproduce it, explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 6 — Lab Consolidation Day: Full Injection Review

**Date:** 25/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Consolidating SQLi, NoSQLi, command injection, SSRF, and SSTI into one mental model
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Complete any PortSwigger/TryHackMe labs from Days 1–5 left unfinished.
* Consolidate all five injection classes into a single "trust boundary" framework.
* Write a one-page comparison document: injection class → syntax → detection → fix.

### Success Criteria
* All required Week 1 injection labs are complete (or explicitly logged as carried over).
* You can draw/describe, from memory, how SQLi/NoSQLi/command injection/SSTI/SSRF relate to one root cause.
* One-page comparison document exists in your portfolio repo.

---

# 📚 2. Topics to Study
### Primary Topic
**Consolidation: The Unified Injection Model**
### Secondary Topics
* Catch-up on any incomplete labs (16 SQLi, 7 SSTI, 5 command injection labs targeted this week)
* Building your first vulnerability-log entries into a consistent CVSS-style format

### Priority
🔴 **Must Know:** the shared "input crosses a trust boundary into a code/command/query context" pattern across all five classes covered this week
🟡 **Should Know:** which defense pattern maps to which class (parameterization / schema validation / safe APIs / allowlisting / logic-less templates)
🟢 **Nice to Know:** how these classes appear differently in code review vs. black-box testing

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — The Unified Trust-Boundary Model
* What it is: every vulnerability this week is the same failure — user data was allowed to become code/instructions in a downstream interpreter (SQL engine, Mongo query planner, shell, template engine)
* Why it matters: once you see this pattern, you can predict new injection classes you haven't studied yet (LDAP injection, XPath injection, XML injection follow the same shape)
* How it works: draw the pattern as `[User Input] → [Concatenated/Embedded Untrusted] → [Interpreter Executes It]`
* Real-world example: revisit Equifax (command injection via Struts) and consider how the same diagram applies
* Common mistake: treating each class as an unrelated fact to memorize instead of a pattern to recognize

### Concept 2 — Defense Pattern Mapping
* Definition: each injection class has a *structural* fix (not a blocklist patch)
* Mapping: SQLi → parameterized queries; NoSQLi → schema validation; Command injection → `execFile()`/`spawn()` with array args; SSRF → allowlisting + network segmentation; SSTI → logic-less templates
* Attack scenario: a code reviewer sees `exec(` or string-built queries and should immediately flag it regardless of which specific payload would work
* Mitigation: build a personal checklist of "red flag" code patterns you now recognize on sight

### Concept 3 — Vulnerability Documentation Standard
* Key terminology: CVSS base score components (Attack Vector, Attack Complexity, Privileges Required, User Interaction, Impact)
* Practical application: standardize your findings log format now so Weeks 2–4 additions are consistent
* Common vulnerabilities in *reporting* (not code): vague descriptions, missing reproduction steps, no remediation guidance
* Best practices: every finding should have: title, severity, CVSS vector, reproduction steps, root cause, business impact, fix

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Trust boundary | The line where data becomes untrusted (or where it's wrongly treated as trusted) | The single concept underlying all injection classes |
| CVSS vector | Standardized string encoding severity factors | How you'll communicate findings professionally in interviews and reports |
| Structural fix | A fix that eliminates the vulnerability class, not just one payload | Distinguishes junior from senior security thinking |

---

# ⏱️ 4. Study Schedule

## Session 1 — Catch-Up & Consolidation (45–60 min)
Finish any incomplete PortSwigger labs from Days 1–5. If all complete, use this time to re-attempt one lab from memory with zero hints, timed.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Build the Comparison Document (60–90 min)
Create a one-page markdown table: Injection Class | Trust Boundary Crossed | Detection Technique | Structural Fix | Real-World Example. Fill it in for all five classes from this week using your own words (no copy-paste from PortSwigger).

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Given only a code snippet (no vulnerability class labeled), identify which of this week's five classes it belongs to and why.
### Problem 2
Explain why "sanitize the input" is a weaker answer than "eliminate the trust-boundary crossing" in an interview setting.
### Problem 3
Pick one finding from your Juice Shop log and rewrite it in full CVSS-vector format.
### Challenge
Without notes, whiteboard (or draw digitally) the unified trust-boundary diagram and label all five classes on it.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full-week active recall: SQLi (Days 1–2), NoSQLi (Day 3), Command Injection & SSRF (Day 4), SSTI (Day 5) — answer the "what/why/how/attack/defense" for each from memory, then check against notes.
### Spaced-Repetition Review
* **Yesterday:** SSTI
* **This week overall:** all five injection classes

---

# 🧪 6. Active Recall Exercises
1. What is the trust boundary, and why is it the unifying concept for this week?
2. How does SQLi map onto that model?
3. Why does NoSQLi require a different fix (schema validation) than SQLi (parameterization)?
4. What would an attacker need across each of the five classes (a rendering/execution/query sink reachable with untrusted input)?
5. What's the business impact difference between SSTI/command injection (RCE) vs. SSRF (lateral movement/data exposure)?
6. How would a log-monitoring system flag each of the five classes differently?
7. How would you brief a development team on preventing all five in a 10-minute talk?
8. Difference between a tactical patch and a structural fix — give one example of each per class.
9. Which real-world breach maps to which class from this week?
10. Teach the unified trust-boundary model to a junior developer in plain language.

### Feynman Test
Explain the unified injection model in 3–5 sentences. If unclear, mark 🟡 **Needs Review** and revisit Day 1–5 notes before Day 7.

---

# 🛠️ 7. Tool Practice
**Tool:** Burp Suite (full workflow review)
### Today's Tool Goal
Practice a smooth end-to-end workflow: Proxy intercept → Repeater test → Decoder for payload encoding → documenting a finding, without hesitating on tool mechanics.
### Commands / Features to Practice
```text
Full workflow rehearsal across 2-3 saved requests from this week's labs
```
### Tool Success Criteria
I can move through detection → exploitation → documentation in Burp without pausing to look up basic UI mechanics.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: reformat all Week 1 findings into the standardized CVSS-vector format; this becomes the backbone of your Week 4 capstone report.
### Deliverable
`Day 6: Standardized all Week 1 findings (SQLi, NoSQLi if applicable, SSRF, SSTI) into consistent CVSS-vector format.`

---

# 📝 9. Practice / Security Challenge
### Scenario
You're in a technical screening interview and asked: "Walk me through how you'd approach testing a new web app for injection vulnerabilities, without knowing the tech stack in advance."
### My Task
1. Identify which vulnerability classes you'd test for, in what order, and why.
2. Explain your detection methodology per class.
3. Explain how you'd prioritize findings by impact.
4. Practice saying this out loud, timed to under 3 minutes.
5. Write it down as an interview-prep note.
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
* [ ] All Week 1 labs complete (or logged as carry-over)
* [ ] Comparison document built
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Tool workflow rehearsal
* [ ] Findings log standardized  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. The unified trust-boundary model across all five classes
2. CVSS-vector documentation format
3. Every Week 1 injection class's detection + fix, cold
4. How to structure an interview answer about injection testing methodology
5. What's still shaky and needs review before Week 2

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** PortSwigger Web Academy (catch-up) / OWASP Juice Shop
**Lab Name:** Learner's choice — whichever Week 1 lab is least confidently understood
**Difficulty:** ⭐⭐⭐☆☆ (self-selected)
**Estimated Time:** 60–90 minutes
### Lab Objective
Re-attempt, unaided, the single lab from this week you found hardest, to convert passive familiarity into active recall.
### Environment
Target: your choice of this week's labs · Tools: Burp Suite · Prerequisite: Days 1–5
### Lab Tasks
1. Pick the lab you're least confident about.
2. Attempt it from scratch with no notes.
3. If stuck for more than 15 minutes, check notes for a hint only (not the full solution).
4. Complete it and re-document the finding in your own words.
5. Rate your confidence before and after.
### What I Need to Discover
Where exactly did my memory fail — concept, tool mechanics, or payload syntax? Is that a knowledge gap or a practice gap? What would close it?
### Lab Success Criteria
Document the finding, reproduce it unaided (or with minimal hints), explain root cause and impact, demonstrate safely, recommend mitigation.

---
---

# 🛡️ Day 7 — Week 1 Portfolio Consolidation & Buffer Day

**Date:** 26/08/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Professional security writeup and self-assessment
**Estimated Total Time:** 3–4 hours
**Difficulty:** Beginner

---

## 🎯 1. Daily Goal / Expected Outcome
* Finalize Week 1's contribution to Portfolio Piece #1 (partial Juice Shop report — injection section).
* Complete a genuine self-diagnostic: which of this week's five classes are solid vs. shaky.
* Preview Week 2 topics so Day 8 starts with context.

### Success Criteria
* A polished, interview-ready "Injection Vulnerabilities" section of your Juice Shop report exists.
* You have an honest, written list of gaps to revisit during Week 4's review buffer.
* You've skimmed the Week 2 topic list (auth, sessions, access control) and identified one thing you already know and one you don't.

---

# 📚 2. Topics to Study
### Primary Topic
**Professional Security Report Writing**
### Secondary Topics
* Executive-summary writing (translating technical findings for non-technical stakeholders)
* Buffer/catch-up for any labs still outstanding
* Light preview reading for Week 2 (Broken Authentication & Access Control)

### Priority
🔴 **Must Know:** how to write a finding that a developer can act on without back-and-forth clarification
🟡 **Should Know:** how to write a 3-sentence executive summary of a technical finding
🟢 **Nice to Know:** report formatting conventions used by real bug bounty platforms (HackerOne report structure) as a style reference

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Actionable Finding Write-Ups
* What it is: a report entry that gives a developer everything needed to fix the issue without asking follow-up questions
* Why it matters: this is what actually gets you hired — technical skill alone doesn't show in an interview as clearly as a polished writeup sample does
* How it works: Title → Severity/CVSS → Affected Component → Reproduction Steps → Root Cause → Impact → Remediation → (optional) Code-level fix suggestion
* Real-world example: study one public HackerOne disclosed report as a formatting reference (read only the structure, not to copy content)
* Common mistake: writing reproduction steps assuming the reader has the same context you do

### Concept 2 — Executive Summaries
* Definition: a 2–3 sentence, jargon-free summary of business risk, placed at the top of a report for non-technical stakeholders
* Architecture/process: state what was found, why it matters to the business, and the overall risk level — before any technical detail
* Attack scenario n/a — this is a communication skill, not a technical one
* Mitigation n/a — the "fix" here is clarity of writing

### Concept 3 — Honest Self-Diagnosis
* Key terminology: "known unknowns" vs. "unknown unknowns"
* Practical application: for each of the five Week 1 classes, rate yourself 1–5 and write one sentence on what would raise the score
* Common vulnerabilities in self-assessment: overconfidence right after finishing a lab (recognition ≠ recall)
* Best practices: test yourself with a blank-page recall exercise, not a "does this look familiar" check

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Actionable finding | A report entry a developer can fix without follow-up questions | The actual deliverable employers care about |
| Executive summary | Non-technical, business-risk framing at the top of a report | Shows you can communicate beyond just "hacking" |
| Recall vs. recognition | Producing an answer unaided vs. just recognizing it as familiar | The real test of whether you've learned something |

---

# ⏱️ 4. Study Schedule

## Session 1 — Report Writing (45–60 min)
Study the actionable-finding structure and executive-summary technique.
**Output:** A written executive summary (3 sentences) for your Juice Shop injection findings overall.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Finalize the Report Section (60–90 min)
1. Take your Days 1–6 findings log and format each into the full actionable-finding structure.
2. Add the executive summary at the top.
3. Add a short "Methodology" paragraph describing your testing approach this week.
4. Push this to your GitHub portfolio repo as `week1-injection-findings.md`.

**Expected Result:** A polished, standalone document that could be shown to an interviewer today.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Rewrite one of your rougher Day 1–5 findings into the full actionable structure.
### Problem 2
Write a 3-sentence executive summary as if explaining the week's findings to a non-technical founder.
### Problem 3
Identify the single weakest of the five injection classes for you personally, and write a 20-minute review plan for it.
### Challenge
Skim the Week 2 topic list (JWT attacks, session fixation, OAuth misconfig, IDOR, password storage) and write one sentence per topic: "I already know roughly how this works" or "This is new to me."

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full spaced-repetition sweep of the whole week: SQLi (Days 1–2), NoSQLi (Day 3), Command Injection & SSRF (Day 4), SSTI (Day 5), unified model (Day 6) — blank-page recall for each, then check notes.
### Spaced-Repetition Review
* **Yesterday:** Unified injection model
* **This week:** all five classes, cold recall

---

# 🧪 6. Active Recall Exercises
1. What is an actionable security finding, structurally?
2. How do you write an executive summary for a technical audience gap?
3. Why does recognition-based confidence mislead self-assessment?
4. What would an interviewer want to see in your Week 1 portfolio piece?
5. What's the impact of a poorly written finding on developer trust in a security team?
6. How would you know (as a hiring manager) that a candidate actually understands injection vs. just ran a scanner?
7. How would you structure a 10-minute walkthrough of this week's report for an interview?
8. Difference between a technical finding and a business-risk summary?
9. Give a real-world example of a well-structured public vulnerability report.
10. Teach "how to write an actionable finding" to a junior developer moving into security.

### Feynman Test
Explain what makes a security finding "actionable" in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Markdown/GitHub (portfolio repo hygiene)
### Today's Tool Goal
Get your GitHub portfolio repo structured cleanly: a `README.md`, a `findings/` folder, consistent file naming.
### Commands / Features to Practice
```text
git add . && git commit -m "Week 1: injection findings report" && git push
```
### Tool Success Criteria
Your portfolio repo has a clean, browsable structure that an interviewer could navigate in under a minute.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment (Week 1 section complete)
Today's contribution: finalize and publish `week1-injection-findings.md` with executive summary, methodology, and all five class findings in actionable format.
### Deliverable
`Day 7: Published Week 1 injection findings report (executive summary + 5 documented findings) to portfolio repo.`

---

# 📝 9. Practice / Security Challenge
### Scenario
You're asked in an interview: "Show me one thing from your portfolio and walk me through it."
### My Task
1. Select your strongest Week 1 finding.
2. Practice explaining it out loud in under 90 seconds: what it was, how you found it, why it mattered, how you'd fix it.
3. Identify the one follow-up question an interviewer is most likely to ask.
4. Prepare that answer too.
5. Record yourself (audio is fine) once and listen back.
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
* [ ] Week 1 findings report finalized and published to GitHub
* [ ] Executive summary written
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Portfolio repo hygiene
* [ ] Week 2 preview skim  * [ ] Interview-answer rehearsal  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow (Start of Week 2)
1. All five Week 1 injection classes, cold, in interview format
2. How to present a finding in under 90 seconds
3. The unified trust-boundary model
4. What authN vs. authZ means at a basic level (Week 2 starts here — this closes your original diagnostic gap)
5. Why Week 2 (auth/session/access control) is a natural next step after injection

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link to published report]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** N/A — today is a portfolio/consolidation day, not a new-lab day
**Lab Name:** "Self-Review: Present Week 1 Findings"
**Difficulty:** ⭐☆☆☆☆
**Estimated Time:** 45–60 minutes (folded into Session 3/Practice Challenge above)
### Lab Objective
Confirm you can present your own work clearly and handle a likely follow-up question — the actual "lab" this week is communication, not exploitation.
### Environment
Target: your own Week 1 report · Tools: none, just your voice/notes · Prerequisite: Day 1–6 findings
### Lab Tasks
1. Choose your strongest finding.
2. Present it out loud, timed.
3. Anticipate and answer one likely follow-up question.
4. Get feedback if possible (mentor, peer, or self-review via recording).
5. Note what you'd tighten next time.
### What I Need to Discover
Where does your explanation get vague or hand-wavy? That's usually where the underlying understanding — not just the communication — is weakest, and it tells you exactly what to review before Week 2 begins.
### Lab Success Criteria
You can explain the finding clearly, unaided, in under 90 seconds, and answer one reasonable follow-up question without notes.

---
---

# 🗺️ WEEKS 2–12 — Day-by-Day Topic Map (Full 90-Day Roadmap)

*Each entry below follows the same 14-section template as Week 1. Say "expand Week X" or "expand Day Y" and I'll generate that day in full detail.*

## Week 2 (Days 8–14) — Broken Authentication, Sessions & Access Control
| Day | Date | Topic |
|---|---|---|
| 8 | 27/08/2026 | JWT attacks: none-algorithm, key confusion, claim tampering |
| 9 | 28/08/2026 | Session fixation & session hijacking |
| 10 | 29/08/2026 | OAuth 2.0 misconfigurations: redirect URI, CSRF on auth flows |
| 11 | 30/08/2026 | Broken access control: IDOR, horizontal/vertical privilege escalation |
| 12 | 31/08/2026 | Password storage & credential attacks: hashcat, bcrypt/argon2, rate limiting |
| 13 | 01/09/2026 | Lab day: catch-up on Authentication + Access Control PortSwigger labs |
| 14 | 02/09/2026 | Consolidation: authN vs. authZ mental model + Capital One SSRF/IAM case study |

## Week 3 (Days 15–21) — XSS, CSRF, Client-Side Attacks & Threat Modeling
| Day | Date | Topic |
|---|---|---|
| 15 | 03/09/2026 | Reflected XSS: payload crafting, filter bypass |
| 16 | 04/09/2026 | Stored & DOM-based XSS: output encoding, CSP, DOMPurify |
| 17 | 05/09/2026 | CSRF & Clickjacking: token defenses, SameSite cookies, X-Frame-Options |
| 18 | 06/09/2026 | Insecure deserialization: gadget chains, JSON-only defense |
| 19 | 07/09/2026 | Threat modeling part 1: STRIDE framework |
| 20 | 08/09/2026 | Threat modeling part 2: attack trees on your own past projects |
| 21 | 09/09/2026 | Lab day + Burp Suite deep configuration on your own apps |

## Week 4 (Days 22–28) — Security Logging, Log Triage, SOC Fundamentals & Month 1 Capstone
| Day | Date | Topic |
|---|---|---|
| 22 | 10/09/2026 | Security logging fundamentals: what to log, what attackers hide |
| 23 | 11/09/2026 | Log analysis with ELK/Splunk free tier: SPL/KQL queries |
| 24 | 12/09/2026 | SIEM & alert engineering: alert fatigue, detection rule writing |
| 25 | 13/09/2026 | Incident response basics: Cyber Kill Chain, MITRE ATT&CK, IR playbooks |
| 26 | 14/09/2026 | **Month 1 Capstone Day 1:** Full OWASP Top 10 assessment on Juice Shop |
| 27 | 15/09/2026 | **Month 1 Capstone Day 2:** Complete assessment, 15+ findings documented |
| 28 | 16/09/2026 | **Portfolio Piece #1 finalized:** full Juice Shop report published |

## Week 5 (Days 29–35) — Secure SDLC & SAST/DAST Tooling
| Day | Date | Topic |
|---|---|---|
| 29 | 17/09/2026 | Secure SDLC models: MS SDL, OWASP SAMM |
| 30 | 18/09/2026 | SAST part 1: Semgrep fundamentals + scanning a past project |
| 31 | 19/09/2026 | SAST part 2: writing custom Semgrep rules, triaging findings |
| 32 | 20/09/2026 | DAST part 1: OWASP ZAP fundamentals, authenticated scanning |
| 33 | 21/09/2026 | DAST part 2: Nuclei templates, CI/CD integration prep |
| 34 | 22/09/2026 | SCA: npm audit, Snyk, Dependabot, SBOM generation with `cdxgen`/`syft` |
| 35 | 23/09/2026 | Lab day: full SAST+DAST+SCA pass on a past personal project |

## Week 6 (Days 36–42) — CI/CD Pipeline Security
| Day | Date | Topic |
|---|---|---|
| 36 | 24/09/2026 | CI/CD attack surface: secret extraction, poisoned pipelines |
| 37 | 25/09/2026 | Dependency confusion attacks & least-privilege CI tokens |
| 38 | 26/09/2026 | Secret management: `trufflehog`, `git-secrets`, HashiCorp Vault basics |
| 39 | 27/09/2026 | Build the secure pipeline part 1: GitHub Actions skeleton + Semgrep gate |
| 40 | 28/09/2026 | Build the secure pipeline part 2: add ZAP + npm audit + secret scanning gates |
| 41 | 29/09/2026 | Lab day: `trufflehog` against a public repo, document findings |
| 42 | 30/09/2026 | **Portfolio Piece #2 milestone:** pipeline repo structure finalized |

## Week 7 (Days 43–49) — Container & Kubernetes Security
| Day | Date | Topic |
|---|---|---|
| 43 | 01/10/2026 | Docker security: container escape, privilege escalation |
| 44 | 02/10/2026 | Hardened Dockerfiles: minimal base images, non-root users, multi-stage builds |
| 45 | 03/10/2026 | Image scanning & supply chain: Trivy/Grype, image signing with Cosign |
| 46 | 04/10/2026 | Kubernetes security part 1: pod escape, exposed dashboards |
| 47 | 05/10/2026 | Kubernetes security part 2: RBAC hardening, network policies |
| 48 | 06/10/2026 | Lab day: KillerCoda Kubernetes security scenarios |
| 49 | 07/10/2026 | Consolidation: Tesla Kubernetes dashboard breach case study |

## Week 8 (Days 50–56) — Cloud Security Fundamentals (AWS/GCP) & Month 2 Capstone
| Day | Date | Topic |
|---|---|---|
| 50 | 08/10/2026 | IAM & least privilege: privilege escalation paths, policy enumeration |
| 51 | 09/10/2026 | S3/storage misconfigurations: public bucket enumeration, Block Public Access |
| 52 | 10/10/2026 | Network security: VPC design, security groups, flow logs |
| 53 | 11/10/2026 | flaws.cloud CTF part 1 |
| 54 | 12/10/2026 | flaws.cloud CTF part 2 / flaws2.cloud |
| 55 | 13/10/2026 | **Month 2 Capstone:** integrate pipeline + hardened container + cloud config |
| 56 | 14/10/2026 | **Portfolio Piece #2 finalized:** secure CI/CD pipeline repo published |

## Week 9 (Days 57–63) — OWASP LLM Top 10 & Prompt Injection
| Day | Date | Topic |
|---|---|---|
| 57 | 15/10/2026 | OWASP Top 10 for LLM Applications: overview + risk categories 1–5 |
| 58 | 16/10/2026 | OWASP Top 10 for LLM Applications: risk categories 6–10 |
| 59 | 17/10/2026 | Direct prompt injection: jailbreaking, instruction override |
| 60 | 18/10/2026 | Indirect prompt injection: payload embedding in external content |
| 61 | 19/10/2026 | Data poisoning & training data extraction: model inversion basics |
| 62 | 20/10/2026 | Lab day: Lakera Gandalf levels 1–5+ |
| 63 | 21/10/2026 | Lab day: Lakera Gandalf remaining levels + HackAPrompt intro |

## Week 10 (Days 64–70) — AI Red Teaming & MITRE ATLAS
| Day | Date | Topic |
|---|---|---|
| 64 | 22/10/2026 | MITRE ATLAS framework: adversarial ML taxonomy |
| 65 | 23/10/2026 | MITRE ATLAS Navigator: map attack paths for a fictional AI product |
| 66 | 24/10/2026 | AI red teaming methodology: systematic guardrail testing |
| 67 | 25/10/2026 | AI red teaming: evaluation frameworks, red team playbooks |
| 68 | 26/10/2026 | RAG attacks part 1: data exfiltration through retrieval |
| 69 | 27/10/2026 | RAG attacks part 2: build a simple RAG app and attempt to exploit it |
| 70 | 28/10/2026 | Lab day: fix and re-test the RAG app; document findings |

## Week 11 (Days 71–77) — Capstone Portfolio Project
| Day | Date | Topic |
|---|---|---|
| 71 | 29/10/2026 | Scope the capstone: "Full Security Assessment of an AI-Integrated Web App" |
| 72 | 30/10/2026 | Execute: OWASP Top 10 (web) assessment on the target app |
| 73 | 31/10/2026 | Execute: OWASP LLM Top 10 assessment on the AI components |
| 74 | 01/11/2026 | Execute: threat model (STRIDE + attack trees) + CI/CD pipeline review |
| 75 | 02/11/2026 | Write the report part 1: executive summary + web findings |
| 76 | 03/11/2026 | Write the report part 2: AI/LLM findings + remediation roadmap |
| 77 | 04/11/2026 | **Portfolio Piece #3 finalized:** capstone report published |

## Week 12 (Days 78–84) — Job Readiness & Interview Prep
| Day | Date | Topic |
|---|---|---|
| 78 | 05/11/2026 | Resume optimization: reframe dev experience as security-relevant |
| 79 | 06/11/2026 | LinkedIn & GitHub profile polish, portfolio site update |
| 80 | 07/11/2026 | Mock interview 1: AppSec scenario questions |
| 81 | 08/11/2026 | Mock interview 2: CTF-style whiteboard challenges |
| 82 | 09/11/2026 | Certification roadmap: schedule CompTIA Security+ / eJPT / AWS Security Specialty |
| 83 | 10/11/2026 | Job search sprint part 1: apply to 10+ targeted UAE roles |
| 84 | 11/11/2026 | Job search sprint part 2: apply to remaining 10+ roles, follow up on Week 12 Day 1 applications |

## Buffer / Review Days (Days 85–90)
Your original template allocates ~780 hours over 90 days at 5 hrs/day, 6 days/week — the week breakdown above naturally leaves the remaining ~6 days as buffer. Recommended use:
| Day | Suggested Use |
|---|---|
| 85–86 | Catch-up on any carried-over labs from Weeks 1–11 |
| 87 | Second full mock interview, incorporating feedback from Days 80–81 |
| 88 | Continue application sprint (aim for 20+ total applications by end of Week 12) |
| 89 | Review weakest self-assessed topic from the whole 90 days (pick from your Day-7-style logs) |
| 90 | Final portfolio review: confirm all 3 pieces are polished, links work, GitHub is clean |

---

## How to Use This From Here

1. **This week:** work Days 1–7 exactly as expanded above.
2. **Before Day 8:** tell me to expand Week 2, and I'll generate Days 8–14 in the same full 14-section format — I'll also fold in real answers to the curriculum's open questions if anything's changed (employment status, AWS account status, certification target).
3. **Ongoing:** I can expand one week at a time (keeps each day's scenarios and labs specific rather than generic), or a single day at a time if you want to move slower than the schedule.

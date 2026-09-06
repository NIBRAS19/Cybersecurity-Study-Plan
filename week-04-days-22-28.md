# 🗓️ WEEK 4 — Security Logging, Log Triage, SOC Fundamentals & Month 1 Capstone (Days 22–28)

---

# 🛡️ Day 22 — Security Logging Fundamentals

**Date:** 10/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** What to log, what attackers try to hide, and log design principles
**Estimated Total Time:** 3–4 hours
**Difficulty:** Beginner

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain why security logging exists as its own discipline distinct from general application/debug logging.
* Identify what a well-designed security log entry must contain (who, what, when, where, outcome).
* Explain what attackers try to hide or manipulate in logs, and why log integrity matters.
* Directly close your original baseline diagnostic gap: log triage methodology.

### Success Criteria
* Explain the difference between debug logging and security/audit logging without notes.
* Design a security-relevant log schema for a login endpoint.
* Explain at least two log-evasion techniques attackers use.

---

# 📚 2. Topics to Study
### Primary Topic
**Security Logging Fundamentals**
### Secondary Topics
* What to log: authentication events, authorization failures, input validation anomalies, admin actions
* Log integrity: why logs must be tamper-evident/append-only for security purposes
* Log evasion techniques: encoding tricks, slow/low-and-slow attacks designed to avoid threshold-based alerting

### Priority
🔴 **Must Know:** the minimum fields every security-relevant log entry needs (timestamp, actor identity, action, target resource, outcome/result, source IP)
🟡 **Should Know:** why STRIDE's "Repudiation" category (Day 19) exists specifically because of insufficient logging
🟢 **Nice to Know:** structured logging (JSON logs) vs. unstructured text logs for downstream SIEM parsing

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — What Makes a Log "Security-Relevant"
* What it is: not every log line matters for security — a security log entry specifically supports answering "who did what, when, and did it succeed" for actions with security consequences
* Why it matters: this is your original baseline diagnostic gap — closing it means you can now reason about detection, not just exploitation
* How it works: every authentication attempt (success AND failure), every authorization decision (especially denials), every admin/privileged action, and every input-validation rejection should produce a structured log entry
* Real-world example: post-breach investigations (including Capital One, Day 14) depend entirely on whether sufficient logging existed to reconstruct the attacker's actions
* Common mistake: logging only failures, or only successes — both directions matter (a string of failed logins followed by one success is a critical pattern that's invisible if failures aren't logged)

### Concept 2 — Log Integrity and Tamper Resistance
* Definition: security logs must be protected from modification or deletion by the very actors they might implicate, including a successful attacker who gains system access
* Architecture/process: write logs to an append-only, centrally-aggregated destination (not just local disk) so an attacker who compromises one host can't erase evidence of their own actions
* Attack scenario: an attacker who gains shell access clears local log files to cover their tracks — this fails if logs were already shipped to a separate, access-restricted log aggregation system
* Defense/mitigation: centralized log shipping (to ELK/Splunk, previewed for Day 23), restricted write/delete permissions on log storage

### Concept 3 — What Attackers Try to Hide
* Key terminology: log evasion, low-and-slow attacks, encoding obfuscation
* Practical application: an attacker attempting SQLi might deliberately space out requests to stay under a rate-limiting/alerting threshold, or use encoding variations specifically to avoid string-matching detection rules
* Common vulnerabilities in logging design: threshold-based alerts that are easily evaded by simply going slower, or detection rules that match one specific payload encoding
* Best practices: log the *decoded/normalized* form of suspicious input where safely possible, and consider behavioral (not just threshold) anomaly detection

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Security-relevant log entry | A log line answering who/what/when/outcome for a security-consequential action | The foundation of detection and incident response |
| Log integrity | Protection against tampering, especially by a successful attacker | Without it, breach investigation becomes guesswork |
| Low-and-slow attack | Deliberately paced to evade threshold-based detection | Shows why naive detection rules are insufficient |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study what makes a log security-relevant, log integrity principles, and log evasion techniques. Read the OWASP Logging Cheat Sheet.
**Output:** Design a JSON log schema for a login endpoint including every field discussed above.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Instrument a small local Express login route with structured (JSON) security logging covering successful login, failed login, and account lockout events.
2. Simulate a credential-stuffing-style burst of failed logins against it and review your own log output.
3. Identify, from your log output alone, whether you could reconstruct the attack — if not, refine your schema.

**Expected Result:** A working, structured security-logging implementation with a demonstrated (simulated) attack visible in the logs.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Design the log fields needed to detect a credential-stuffing attack from log data alone (what would distinguish it from normal failed-login noise?).
### Problem 2
An attacker clears the local `/var/log/auth.log` after gaining shell access. What architectural decision would have prevented this from destroying the evidence?
### Problem 3
Why is logging only successful logins (not failures) insufficient for detecting a brute-force attempt?
### Challenge
Design a complete security-logging policy (which events, which fields, where shipped, retention period) for a mid-sized SaaS application.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Week 3 Consolidation (Day 21)
Recall without notes: the full Week 3 arc (XSS/CSRF/deserialization + STRIDE/attack trees).
### Spaced-Repetition Review
* **Yesterday:** Week 3 consolidation
* **2 weeks ago:** JWT attacks
* **This is the direct payoff of STRIDE's "Repudiation" category (Day 19)** — today's topic is literally what closes that threat category.

---

# 🧪 6. Active Recall Exercises
1. What makes a log entry "security-relevant" vs. general debug output?
2. What fields must a security log entry contain at minimum?
3. Why must logs be shipped off the originating host for integrity?
4. What would an attacker need to evade threshold-based log alerting (patience — spacing out requests)?
5. What's the impact of insufficient logging during a breach investigation?
6. How would you detect a low-and-slow attack given only rate-limited alerting?
7. How would you design logging to prevent an attacker from erasing evidence of their own actions?
8. Difference between logging failures only vs. logging both successes and failures?
9. Real-world example connecting insufficient logging to a breach investigation difficulty.
10. Teach "why security logging is its own discipline" to a junior developer in plain language.

### Feynman Test
Explain what makes a log security-relevant, and why log integrity matters, in 3–5 sentences. If unclear, mark 🟡 **Needs Review** — this closes one of your original baseline gaps.

---

# 🛠️ 7. Tool Practice
**Tool:** Node.js structured logging (e.g., `winston` or `pino`)
### Today's Tool Goal
Implement structured JSON logging for security events in a small Express app.
### Commands / Features to Practice
```text
npm install winston
logger.info({event: "login_failure", user: username, ip: req.ip, timestamp: Date.now()})
```
### Tool Success Criteria
I can produce structured, machine-parseable security log entries for both success and failure paths.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment (Month 1 capstone begins in 4 days)
Today's contribution: review what logging (if any) Juice Shop exposes or documents, and note this as an assessment observation; also finalize your own local logging-instrumented demo as a standalone portfolio artifact demonstrating security-logging design skill.
### Deliverable
`Day 22: Built structured security-logging demo (Express + winston) and documented a security-logging design schema.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A post-incident review reveals that an attacker had access to a company's customer database for three weeks before detection, and investigators struggle to reconstruct the attacker's actions due to sparse, unstructured logs.
### My Task
1. Identify the underlying weakness (not a specific exploit, but a logging/detection design failure).
2. Explain the root cause.
3. Determine the impact of the three-week detection gap.
4. Propose a logging redesign (fields, centralization, retention) that would have shortened detection time.
5. Document the result.
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
* [ ] Study notes  * [ ] Logging demo built  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice (winston/pino)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. What makes a log security-relevant
2. Log integrity principles and why centralized shipping matters
3. Log evasion techniques attackers use
4. A complete security-logging schema design
5. How tomorrow's ELK/Splunk topic builds on today's "what to log" foundation with "how to analyze it at scale"

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Lab (Node.js + winston)
**Lab Name:** "Instrument and Detect: Build a Security-Logged Login System"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Build a login endpoint with proper structured security logging, then simulate an attack and confirm you can reconstruct it from logs alone.
### Environment
Target: local Node.js/Express app · Tools: `winston` or `pino` · Prerequisite: today's concepts
### Lab Tasks
1. Build a minimal login endpoint.
2. Add structured logging for success, failure, and lockout events.
3. Simulate a burst of failed login attempts (a simple script looping requests).
4. Review the resulting log file/output.
5. Confirm you can answer "what happened, when, and from where" using only the logs.
### What I Need to Discover
Does your log schema actually let you reconstruct the attack timeline without needing to check anything else? What field, if missing, would have made reconstruction impossible?
### Lab Success Criteria
Document the finding, reproduce the simulated attack, explain what the logs reveal, and confirm the schema is sufficient for reconstruction.

---
---

# 🛡️ Day 23 — Log Analysis with ELK/Splunk (Free Tiers)

**Date:** 11/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Querying and analyzing security logs at scale using SIEM-style tools
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Set up a local ELK stack (or Splunk free tier) and ingest sample security log data.
* Write basic search queries (KQL for Kibana/ELK, SPL for Splunk) to find specific security events.
* Build one simple visualization/dashboard panel from log data.
* Directly extend your baseline diagnostic gap: log triage methodology, now with a real tool.

### Success Criteria
* Explain what ELK/Splunk do conceptually (ingest, index, search, visualize) without notes.
* Write a working query finding all failed login attempts within a time window.
* Produce one basic visualization panel.

---

# 📚 2. Topics to Study
### Primary Topic
**Log Analysis with ELK Stack (Elasticsearch, Logstash, Kibana) or Splunk Free**
### Secondary Topics
* Log ingestion pipeline basics (how raw logs become searchable, indexed data)
* KQL (Kibana Query Language) / SPL (Splunk Processing Language) basics
* Building a simple dashboard panel

### Priority
🔴 **Must Know:** the conceptual pipeline — logs are shipped → parsed/indexed → made searchable → visualized; this is the same pipeline regardless of which specific SIEM tool a future employer uses
🟡 **Should Know:** basic query syntax for filtering by field, time range, and simple boolean logic
🟢 **Nice to Know:** field extraction/parsing rules for unstructured log formats

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — The Log Analysis Pipeline
* What it is: raw logs (from Day 22's structured logging output) are shipped to a central system, parsed into structured fields, indexed for fast search, and made available for querying and visualization
* Why it matters: at any real scale, manually reading log files is infeasible — this pipeline is what turns Day 22's "what to log" design into something a human (or automated rule) can actually act on
* How it works: Logstash/Filebeat (ingestion) → Elasticsearch (indexing/storage) → Kibana (search/visualization) is the ELK equivalent of Splunk's all-in-one ingestion/indexing/search platform
* Real-world example: virtually every SOC analyst role you're targeting will involve daily use of a SIEM matching this general pattern

### Concept 2 — Query Basics (KQL/SPL)
* Definition: a query language for filtering, searching, and aggregating indexed log data
* Practical application: `event:"login_failure" AND @timestamp >= "now-1h"` (KQL-style) or `index=main sourcetype=auth event=login_failure earliest=-1h` (SPL-style) to find recent failed logins
* Common vulnerabilities in query design: overly broad queries that return too much noise, or overly narrow queries that miss variant log formats
* Best practices: start broad, narrow iteratively, and always sanity-check your query against a known event you deliberately generated

### Concept 3 — Building a Basic Dashboard Panel
* Key terminology: visualization, dashboard, aggregation (count over time)
* Practical application: a simple "failed logins over time" line chart or "top source IPs by failed login count" table
* Best practices: dashboards should answer a specific operational question at a glance, not just display raw data attractively

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Ingestion pipeline | Logs → parsed/indexed → searchable | The foundation every SIEM tool shares |
| KQL/SPL | Query languages for searching indexed log data | The practical skill SOC analyst roles test directly |
| Dashboard panel | A visualization answering a specific operational question | Turns raw log data into actionable insight |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study the ELK/Splunk pipeline conceptually and basic query syntax.
**Output:** Write out, in plain language, what happens to a single log line from the moment it's generated to the moment a SOC analyst sees it on a dashboard.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
**Lab:** TryHackMe — "Splunk" free rooms and/or "ELK" room; alternatively set up a local ELK stack via Docker Compose and feed it Day 22's log output.
1. Ingest your Day 22 log data (or the room's sample dataset).
2. Write a query finding all failed login attempts in the last hour.
3. Write a query finding the top 5 source IPs by failed-login count.
4. Build one simple visualization (line chart of failed logins over time).

**Expected Result:** A working query set + one dashboard panel, documented with screenshots or exported query text.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Write a KQL/SPL query to find all admin-privileged actions performed outside business hours.
### Problem 2
Given a log dataset with inconsistent field naming (`user` vs. `username` vs. `user_id` across different services), what would you do before writing detection queries?
### Problem 3
Design a dashboard layout (panel list) for a SOC analyst's daily triage view.
### Challenge
Write a query that would specifically catch a low-and-slow credential-stuffing attempt (referencing Day 22's evasion discussion) rather than just an obvious burst.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Security Logging Fundamentals (Day 22)
Recall without notes: what makes a log security-relevant, and log integrity principles.
### Spaced-Repetition Review
* **Yesterday:** Security logging fundamentals
* **1 week ago:** STRIDE framework

---

# 🧪 6. Active Recall Exercises
1. What is the ELK stack's ingestion pipeline, conceptually?
2. How does KQL/SPL let you filter and aggregate log data?
3. Why is a dashboard more useful than raw log file viewing at scale?
4. What would a SOC analyst need to triage effectively (structured logs + working queries + relevant dashboards)?
5. What's the impact of inconsistent field naming across log sources on detection query accuracy?
6. How would you detect a low-and-slow attack using query-based aggregation over a longer time window?
7. How would you design a query to minimize false positives while still catching real threats?
8. Difference between Logstash/Filebeat's role and Elasticsearch's role in the ELK pipeline?
9. Real-world example: how a SOC analyst's daily workflow depends on this tooling.
10. Teach basic SIEM querying to a junior developer moving into security, in plain language.

### Feynman Test
Explain the log ingestion-to-dashboard pipeline in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** ELK Stack (Kibana) or Splunk Free
### Today's Tool Goal
Write and refine at least 3 distinct queries against real (or your own simulated) log data.
### Commands / Features to Practice
```text
KQL: event:"login_failure" and user:"admin"
SPL: index=main event=login_failure user=admin earliest=-24h
```
### Tool Success Criteria
I can write a query from scratch to answer a specific security question without consulting documentation for basic syntax.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: feed your Day 22 structured logging demo's output into your ELK/Splunk setup and produce one dashboard as a combined "detection engineering" artifact — this pairs your logging design skill with actual analysis tooling.
### Deliverable
`Day 23: Built ELK/Splunk dashboard analyzing simulated login attack data from Day 22's logging demo.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A SOC is drowning in 50,000 log events per day and needs to build a triage dashboard to surface the handful that actually matter.
### My Task
1. Identify which event categories deserve dedicated panels (failed logins, privilege escalations, anomalous access times).
2. Explain the reasoning behind your panel choices.
3. Determine what "noise reduction" techniques would help (aggregation, thresholding, allowlisting known-benign patterns).
4. Sketch the dashboard layout.
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
* [ ] Study notes  * [ ] ELK/Splunk lab completed  * [ ] Dashboard panel built  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. The full log ingestion-to-dashboard pipeline
2. Basic KQL/SPL query construction
3. Dashboard design principles for SOC triage
4. How to catch low-and-slow attacks via aggregation queries
5. How tomorrow's SIEM/alert-engineering topic builds directly on today's querying foundation

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** TryHackMe
**Lab Name:** "Splunk" (free rooms) or "Investigating with ELK"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Practice real SIEM querying against a guided dataset distinct from your own simulated data, reinforcing transferable query skills.
### Environment
Target: TryHackMe room · Tools: browser-based Splunk/ELK instance · Prerequisite: today's fundamentals
### Lab Tasks
1. Complete the room's guided ingestion/query tasks.
2. Answer the room's investigation questions using queries you write yourself (not by guessing).
3. Note any query syntax that differs from what you practiced in Session 2.
4. Document your completion and key findings.
5. Compare this room's dataset structure to your own Day 22 log schema — what would you add to your schema based on this exposure?
### What I Need to Discover
Does your query-writing skill transfer to a differently-structured dataset, or did you only learn one specific dataset's quirks?
### Lab Success Criteria
Complete the room's tasks, answer investigation questions via your own queries, document transferable takeaways for your own logging schema.

---
---

# 🛡️ Day 24 — SIEM & Alert Engineering

**Date:** 12/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Writing detection rules and managing alert quality
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain "alert fatigue" and why false-positive-heavy detection rules actively harm security outcomes.
* Write a basic detection rule (pseudocode or actual SIEM syntax) for a specific attack pattern from Weeks 1–3.
* Classify alerts by severity using a defensible, consistent methodology.

### Success Criteria
* Explain alert fatigue's mechanism and consequences without notes.
* Write a working detection rule for at least one Week 1–3 vulnerability class's exploitation pattern.
* Propose a severity classification scheme and apply it to 3 example alerts.

---

# 📚 2. Topics to Study
### Primary Topic
**SIEM Alert Engineering**
### Secondary Topics
* False positive vs. false negative trade-offs in detection rule design
* Severity classification frameworks (e.g., informational/low/medium/high/critical)
* Writing detection rules that map directly to specific attack techniques you already understand technically

### Priority
🔴 **Must Know:** why a detection rule generating too many false positives leads analysts to ignore or disable it entirely — alert fatigue is a genuine, well-documented cause of missed real incidents
🟡 **Should Know:** how to write a detection rule that maps a known exploitation pattern (e.g., Day 1's UNION SQLi payload structure) to a searchable log signature
🟢 **Nice to Know:** MITRE ATT&CK technique IDs as a standard way to tag detection rules for coverage tracking (previewed further in Week 4's incident-response topic tomorrow)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Alert Fatigue
* What it is: when analysts are flooded with low-value or false-positive alerts, they become desensitized and start ignoring or quickly dismissing alerts — including real ones
* Why it matters: numerous major breaches (documented in post-incident public reports across the industry) involved a genuine alert firing correctly, but being lost in noise or dismissed due to fatigue
* How it works: a detection rule with a high false-positive rate trains analysts (consciously or not) to treat that alert category as noise
* Defense/mitigation: tune detection rules iteratively based on real triage outcomes, and track false-positive rate as a first-class metric for every rule, not just "did it fire"

### Concept 2 — Writing Detection Rules from Known Attack Patterns
* Definition: translating a technical understanding of *how* an attack works (which you now have from Weeks 1–3) into a specific, searchable log signature
* Architecture/process: for example, a UNION-based SQLi attempt often produces a distinctive pattern — repeated requests to the same parameter with increasing `UNION SELECT NULL` column counts — which is a far more specific and lower-false-positive signature than simply alerting on the string `UNION`
* Attack scenario this catches: an attacker methodically probing for column count, exactly as you practiced manually on Day 1
* Best practices: base detection rules on the *methodology* of an attack (as you now understand it from hands-on practice), not just naive string matching, which produces both false positives (legitimate uses of the word) and false negatives (encoded/obfuscated variants)

### Concept 3 — Severity Classification
* Key terminology: informational, low, medium, high, critical (or an organization's specific equivalent scale)
* Practical application: severity should reflect *both* the technical impact (what could the attacker achieve) and the confidence level (how certain are we this is malicious, not a false positive)
* Common vulnerabilities in classification: over-classifying everything as "critical" (which itself contributes to alert fatigue) or under-classifying genuinely severe patterns due to unfamiliarity with the technique
* Best practices: use your own hands-on knowledge from Weeks 1–3 to calibrate severity accurately — you now know, from direct experience, how severe a JWT algorithm-confusion attempt actually is versus a routine scanning probe

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Alert fatigue | Analyst desensitization from excessive low-value alerts | A documented root cause of missed real incidents |
| Detection rule | A searchable log signature mapped to a specific attack pattern | Effective only when based on genuine attack methodology, not naive matching |
| Severity classification | Rating alerts by impact and confidence | Directly informs triage prioritization and reduces fatigue |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study alert fatigue's causes and consequences, and the principles of writing methodology-based detection rules.
**Output:** Write, in your own words, why a detection rule alerting on the literal string "UNION" in any request would generate excessive false positives, and what a better rule would check instead.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Using your Day 23 ELK/Splunk setup, write a detection rule (saved search/alert) for one specific Week 1–3 attack pattern of your choice (e.g., repeated requests with incrementing `UNION SELECT NULL` counts, or repeated JWT `alg` header modifications).
2. Test it by simulating the exact attack pattern (replaying your own Day 1 or Day 8 payloads) and confirming the alert fires.
3. Test it against benign traffic and confirm it does NOT fire (a basic false-positive check).
4. Assign a severity classification to your rule with documented justification.

**Expected Result:** A working, tested detection rule with documented true-positive and false-positive test results.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A detection rule fires on any request containing a single quote character (`'`). Explain why this generates excessive noise and design a better rule for detecting SQLi probing specifically.
### Problem 2
Design a severity classification scheme (5 levels) and assign each of the following to a level: a single failed login, 50 failed logins from one IP in 60 seconds, a successful JWT "none" algorithm bypass, a port scan from an external IP.
### Problem 3
Explain how you would measure and track a detection rule's false-positive rate over time in a real SOC environment.
### Challenge
Design 3 detection rules (pseudocode) mapping directly to specific techniques from Weeks 1–3 (one injection-class, one identity/access-class, one client-side-class), each with a documented rationale for why it minimizes false positives.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Log Analysis with ELK/Splunk (Day 23)
Recall without notes: the ingestion pipeline and basic KQL/SPL query construction.
### Spaced-Repetition Review
* **Yesterday:** Log analysis fundamentals
* **1 week + 1 day ago:** Insecure deserialization

---

# 🧪 6. Active Recall Exercises
1. What is alert fatigue, and why is it dangerous?
2. How does a methodology-based detection rule differ from a naive string-match rule?
3. Why does testing a rule against both malicious AND benign traffic matter?
4. What would you need to write an effective detection rule (specific technical understanding of the attack methodology)?
5. What's the impact of an under-classified critical alert being buried among low-severity noise?
6. How would you measure a detection rule's real-world effectiveness over time?
7. How would you prevent alert fatigue at a program level (tuning, severity discipline, regular rule review)?
8. Difference between a false positive and a false negative in detection engineering, and which is worse in which contexts?
9. Real-world example (general, industry-documented pattern) of alert fatigue contributing to a missed incident.
10. Teach detection rule design to a junior developer moving into a SOC role, in plain language.

### Feynman Test
Explain alert fatigue and methodology-based detection rule design in 3–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** ELK/Splunk alerting features
### Today's Tool Goal
Configure a saved search/alert that fires based on a query threshold, and test both true-positive and false-positive scenarios.
### Tool Success Criteria
I can configure, test, and tune an actual alert rule, not just a one-off search query.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment
Today's contribution: write and document 2–3 detection rules mapped directly to Week 1–3 vulnerability classes you personally exploited, as a "detection engineering" supplementary artifact showing you can think defensively, not just offensively.
### Deliverable
`Day 24: Documented and tested 2-3 detection rules mapped to specific Week 1-3 attack techniques.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A SOC's detection rule for "possible XSS attempt" fires over 500 times per day, and analysts have started auto-dismissing it without review.
### My Task
1. Identify the underlying problem (alert fatigue from a poorly tuned rule).
2. Explain the likely root cause (probably naive string matching on common characters like `<` or `script`).
3. Determine the risk this creates (a genuine XSS attempt could be hiding among the noise).
4. Propose a redesigned rule using methodology-based detection (e.g., specific payload structures from Days 15–16).
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
* [ ] Study notes  * [ ] Detection rule(s) built and tested  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice (alerting)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Alert fatigue's mechanism and consequences
2. Methodology-based detection rule design
3. Severity classification principles
4. At least 2 detection rules you personally built and tested
5. How tomorrow's incident-response topic uses these detection rules as its starting trigger

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** TryHackMe
**Lab Name:** "Intro to SIEM" room
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Reinforce SIEM alert-engineering concepts through a guided room, comparing its approach to your own Session 2 rule-building exercise.
### Environment
Target: TryHackMe room · Tools: browser-based SIEM instance · Prerequisite: today's fundamentals
### Lab Tasks
1. Complete the room's guided alert-configuration tasks.
2. Compare its severity classification approach to the scheme you designed in Session 3.
3. Note any alert-tuning technique the room demonstrates that you hadn't considered.
4. Document your completion and key takeaways.
### What I Need to Discover
Does the room's approach to alert tuning match or differ from your own reasoning? What would you adopt from it into your own severity classification scheme?
### Lab Success Criteria
Complete the room, document key takeaways, and refine your own severity classification scheme if warranted.

---
---

# 🛡️ Day 25 — Incident Response Basics

**Date:** 13/09/2026
**Phase:** Month 1 — Application Security Foundations
**Primary Skill:** Incident response fundamentals — Cyber Kill Chain, MITRE ATT&CK, IR playbooks
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain the Cyber Kill Chain's stages and how they map to real attacker behavior.
* Explain how MITRE ATT&CK differs from the Kill Chain (a detailed technique taxonomy vs. a high-level phase model) and how they complement each other.
* Understand the core structure of an incident response playbook.
* Explain evidence preservation principles for incident response.

### Success Criteria
* Explain the Cyber Kill Chain's stages without notes.
* Map at least 3 Week 1–3 vulnerability classes onto specific Kill Chain stages and MITRE ATT&CK tactics.
* Outline a basic IR playbook structure for one incident type.

---

# 📚 2. Topics to Study
### Primary Topic
**Incident Response Fundamentals**
### Secondary Topics
* Cyber Kill Chain (Reconnaissance → Weaponization → Delivery → Exploitation → Installation → Command & Control → Actions on Objectives)
* MITRE ATT&CK framework: Tactics (the "why") and Techniques (the "how"), focusing on Initial Access and Persistence tactics
* IR playbook structure: identification → containment → eradication → recovery → lessons learned
* Evidence preservation: chain of custody, avoiding contamination of compromised systems during investigation

### Priority
🔴 **Must Know:** the Cyber Kill Chain's stages and being able to map a real attack (e.g., the Capital One breach) onto them
🟡 **Should Know:** the distinction between MITRE ATT&CK's Tactics (goals) and Techniques (specific methods) — this maps naturally onto your existing hands-on knowledge of specific techniques
🟢 **Nice to Know:** ATT&CK Navigator as a visualization tool for mapping detection coverage against known techniques

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — The Cyber Kill Chain
* What it is: a 7-stage model (originally developed by Lockheed Martin) describing the typical progression of a targeted cyberattack, from initial reconnaissance through to final objectives
* Why it matters: it gives you a shared vocabulary for describing *where* in an attack's lifecycle a given detection or defense operates — a WAF blocks at Delivery, patching blocks at Exploitation, and IAM least-privilege limits damage at Actions on Objectives
* How it works, applied to Capital One (Day 14): Reconnaissance (identifying the misconfigured WAF) → Exploitation (SSRF against the metadata service) → Actions on Objectives (S3 data exfiltration) — note that not every stage is always distinctly observable, but the model helps structure your analysis
* Common mistake: treating the Kill Chain as a strict, always-sequential checklist rather than a flexible analytical lens — real attacks don't always cleanly separate into these 7 boxes

### Concept 2 — MITRE ATT&CK (Tactics vs. Techniques)
* Definition: ATT&CK is a much more granular, continuously updated knowledge base of adversary Tactics (the attacker's tactical goal, e.g., "Initial Access") and Techniques (the specific method used to achieve that goal, e.g., "Exploit Public-Facing Application")
* Architecture/process: each technique has a unique ID (e.g., T1190 for exploiting public-facing applications) and documented sub-techniques, real-world usage examples, and suggested detections/mitigations
* Attack scenario: nearly every Week 1–3 vulnerability class you've studied maps to a specific ATT&CK technique — SQLi/command injection/SSTI map to T1190 (Initial Access); JWT/session attacks map to Credential Access and Defense Evasion tactics
* Best practices: using ATT&CK's standard vocabulary in your reports and interviews demonstrates industry-standard professional communication

### Concept 3 — IR Playbooks and Evidence Preservation
* Key terminology: identification, containment, eradication, recovery, lessons learned (the standard IR lifecycle phases)
* Practical application: a playbook for a specific incident type (e.g., "suspected credential stuffing") should specify exactly which logs to check, who to notify, and what containment actions are authorized without further approval
* Common vulnerabilities in IR practice: investigators inadvertently destroying forensic evidence by rebooting a compromised system or modifying files before imaging/capturing the original state
* Best practices: preserve original evidence (disk images, log copies) before any remediation action that could alter system state; maintain a clear chain of custody for anything that might support later legal/HR action

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Cyber Kill Chain | 7-stage model of a targeted attack's lifecycle | Shared vocabulary for describing where defenses/detections operate |
| MITRE ATT&CK | Granular Tactics/Techniques knowledge base with unique IDs | Industry-standard vocabulary; maps directly to techniques you've studied |
| IR lifecycle | Identification → Containment → Eradication → Recovery → Lessons Learned | The structural backbone of any incident response playbook |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study the Cyber Kill Chain stages, MITRE ATT&CK's Tactics/Techniques structure (focusing on Initial Access and Persistence), and the IR lifecycle.
**Output:** Map the Capital One breach (Day 14) onto both the Kill Chain stages and at least 2 specific MITRE ATT&CK technique IDs.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Choose 3 vulnerability classes from Weeks 1–3 (e.g., SQLi, JWT algorithm confusion, stored XSS) and map each to: its Kill Chain stage(s) and its corresponding MITRE ATT&CK technique ID(s), using the ATT&CK website/Navigator to look up accurate IDs.
2. Draft a basic IR playbook (identification → containment → eradication → recovery → lessons learned) for one specific incident type: "Suspected SQL injection attack detected via Day 24's detection rule."

**Expected Result:** A completed technique-mapping table + a drafted, usable IR playbook document.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Map the JWT "none" algorithm attack (Day 8) onto a specific MITRE ATT&CK tactic and technique.
### Problem 2
Why is "containment" (isolating the affected system) generally prioritized before "eradication" (removing the threat) in the IR lifecycle — what could go wrong if you skip straight to eradication?
### Problem 3
An investigator reboots a compromised server to "fix" the issue before capturing a disk image. What evidence is likely lost, and why does this matter for potential legal proceedings?
### Challenge
Draft a complete IR playbook for "suspected credential-stuffing attack" (from Day 24's detection rule context), covering all 5 lifecycle phases with specific, actionable steps.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: SIEM & Alert Engineering (Day 24)
Recall without notes: alert fatigue's mechanism and methodology-based detection rule design.
### Spaced-Repetition Review
* **Yesterday:** SIEM & alert engineering
* **1 week + 2 days ago:** Threat modeling / STRIDE

---

# 🧪 6. Active Recall Exercises
1. What are the Cyber Kill Chain's 7 stages?
2. How does MITRE ATT&CK's Tactic/Technique structure differ from the Kill Chain's stage model?
3. Why is evidence preservation critical before remediation actions?
4. What would an incident responder need to properly contain an active incident (clear authority, a tested playbook, isolated network segments)?
5. What's the impact of skipping containment and going straight to eradication?
6. How would a detection rule (Day 24) trigger the start of an IR playbook?
7. How would you structure an IR playbook for a new incident type you haven't seen before?
8. Difference between containment and eradication in the IR lifecycle?
9. Real-world example: map Capital One's breach onto Kill Chain stages.
10. Teach the IR lifecycle to a junior developer moving into a SOC role, in plain language.

### Feynman Test
Explain the Cyber Kill Chain, MITRE ATT&CK's role, and the IR lifecycle together as one connected story in 5–6 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** MITRE ATT&CK website / ATT&CK Navigator
### Today's Tool Goal
Practice looking up specific technique IDs for known attack patterns and understanding how Navigator visualizes technique coverage.
### Tool Success Criteria
I can accurately look up and cite the correct ATT&CK technique ID for at least 3 vulnerability classes from Weeks 1–3.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment (Month 1 capstone begins tomorrow)
Today's contribution: add a MITRE ATT&CK technique mapping column to your consolidated findings log (Weeks 1–3) — every finding should now reference its corresponding technique ID, a professional touch that will strengthen your Month 1 capstone report.
### Deliverable
`Day 25: Added MITRE ATT&CK technique ID mapping to all Weeks 1-3 findings; drafted IR playbook for SQL injection incident type.`

---

# 📝 9. Practice / Security Challenge
### Scenario
Your Day 24 detection rule fires: a burst of `UNION SELECT` pattern requests against the login endpoint from a single IP.
### My Task
1. Identify which IR lifecycle phase you're now in (identification).
2. Explain your next containment step and why.
3. Determine what evidence you'd preserve before taking remediation action.
4. Walk through eradication and recovery steps.
5. Note what "lessons learned" documentation you'd produce.
6. Document the result as a mini incident-response walkthrough.
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
* [ ] Study notes  * [ ] Technique mapping table completed  * [ ] IR playbook drafted  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. The Cyber Kill Chain's 7 stages
2. MITRE ATT&CK's Tactic/Technique structure and how to look up technique IDs
3. The IR lifecycle (identification → containment → eradication → recovery → lessons learned)
4. Evidence preservation principles
5. How all of Month 1's material (injection, identity/access, client-side, logging/IR) comes together for tomorrow's capstone assessment

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** TryHackMe / Blue Team Labs Online (free challenge tier)
**Lab Name:** "Intro to Incident Response" or nearest equivalent free room
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Practice walking through a guided incident scenario end-to-end, applying the IR lifecycle and Kill Chain/ATT&CK mapping to a case you haven't designed yourself.
### Environment
Target: TryHackMe/BTLO room · Tools: browser, provided scenario materials · Prerequisite: today's fundamentals
### Lab Tasks
1. Read the incident scenario provided by the room.
2. Identify the Kill Chain stage(s) represented.
3. Identify the relevant MITRE ATT&CK technique(s), looking them up if not given directly.
4. Walk through your own containment/eradication/recovery recommendations before checking the room's expected answers.
5. Document your reasoning and compare against the room's guidance.
### What I Need to Discover
Did your independently-reasoned response match professional best practice, or did the room reveal a gap in your IR reasoning? Where specifically?
### Lab Success Criteria
Complete the scenario, document your own reasoning before checking guidance, and note any gap honestly in your self-assessment.

---
---

# 🛡️ Day 26 — Month 1 Capstone, Part 1: Full OWASP Top 10 Assessment on Juice Shop

**Date:** 14/09/2026
**Phase:** Month 1 — Application Security Foundations (Capstone)
**Primary Skill:** Executing a complete, professional-grade vulnerability assessment
**Estimated Total Time:** 4 hours (capstone days may run slightly longer than the standard 3–4 hour block — plan for the full 4)
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Execute a systematic, complete pass across OWASP Juice Shop covering all 10 OWASP Top 10 categories, building directly on 4 weeks of hands-on testing.
* Identify and document at least 10–15 distinct vulnerabilities (of your target 15+ for the full capstone) with accurate severity ratings.
* Apply your STRIDE/attack-tree threat-modeling artifacts (Days 19–20) to guide where you focus testing time.

### Success Criteria
* You have a working methodology checklist covering all 10 OWASP Top 10 categories.
* At least 10 findings are documented today with reproduction steps (the remainder completes tomorrow, Day 27).
* Each finding references its MITRE ATT&CK technique ID where applicable (from Day 25's mapping work).

---

# 📚 2. Topics to Study
### Primary Topic
**Systematic OWASP Top 10 Assessment Methodology**
### Secondary Topics
* Structuring a full assessment as a checklist rather than ad-hoc exploration
* Time-boxing technique: allocating limited hours across 10 categories without over-indexing on any one
* Using your Week 1–3 threat-modeling artifacts to prioritize testing effort

### Priority
🔴 **Must Know:** the current OWASP Top 10 (2021 edition) category list, and which of your Weeks 1–3 topics map to each category
🟡 **Should Know:** how to time-box testing so you don't spend 3 hours on injection (already deeply practiced) and zero time on categories like "Security Misconfiguration" or "Vulnerable and Outdated Components" that weren't explicitly covered week-by-week but are still part of the Top 10
🟢 **Nice to Know:** how professional penetration testers structure engagement time across recon, testing, and reporting phases

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — The OWASP Top 10 (2021) as a Complete Checklist
* What it is: A01 Broken Access Control, A02 Cryptographic Failures, A03 Injection, A04 Insecure Design, A05 Security Misconfiguration, A06 Vulnerable and Outdated Components, A07 Identification and Authentication Failures, A08 Software and Data Integrity Failures, A09 Security Logging and Monitoring Failures, A10 Server-Side Request Forgery
* Why it matters: Weeks 1–3 covered most of these deeply (A03 Injection = Week 1; A07 Auth Failures = Week 2 JWT/session/OAuth; A01 Access Control = Week 2 IDOR; A10 SSRF = Week 1 Day 4) — today's job is to apply that depth systematically AND cover the categories that received less direct week-by-week focus (A02, A05, A06, A08, A09)
* How it works: for each of the 10 categories, ask "what would I test for this in Juice Shop specifically, given what I know," referencing your Week 1–3 findings logs directly for A01/A03/A07/A10
* Common mistake: capstones that are really just a re-listing of Week 1–3 findings without genuinely covering the categories that weren't a dedicated week's topic

### Concept 2 — Time-Boxing Across Categories
* Definition: allocating a fixed, disciplined time budget per category so the full assessment completes within the 2-day capstone window
* Practical application: since A03/A07/A01/A10 already have deep findings logs from Weeks 1–3, today's time should be weighted toward A02 (Cryptographic Failures — review password storage from Day 12 and any transport-layer issues), A05 (Security Misconfiguration — default credentials, verbose error messages, exposed admin panels), A06 (Vulnerable Components — run `npm audit` against Juice Shop's dependencies, previewing Week 5's SCA topic), A08 (Software/Data Integrity — revisit Day 18's deserialization findings), and A09 (Logging/Monitoring Failures — apply Day 22–24's logging assessment lens)
* Best practices: use a simple tracking sheet (category × time spent × findings count) to stay disciplined

### Concept 3 — Applying Threat-Modeling Artifacts to Guide Testing
* Key terminology: risk-prioritized testing
* Practical application: your Day 19 STRIDE DFD and Day 20 attack tree already identified likely high-value targets in Juice Shop's authentication/data-access flows — use these to decide where to look first today rather than testing randomly
* Best practices: this demonstrates to a future employer that you don't just "run tools and see what happens" — you reason about where vulnerabilities are likely to be before testing

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| OWASP Top 10 (2021) | The 10 standard web-application risk categories | The organizing framework for your entire capstone assessment |
| Time-boxing | Disciplined time allocation per category | Prevents over-indexing on familiar topics at the expense of complete coverage |
| Risk-prioritized testing | Using threat-modeling output to guide where you test first | Demonstrates professional methodology, not just tool-running |

---

# ⏱️ 4. Study Schedule

## Session 1 — Methodology Setup (45–60 min)
Build your full OWASP Top 10 checklist, noting which categories already have deep Week 1–3 findings and which need fresh testing today. Allocate rough time budgets per category for today and tomorrow combined.
**Output:** A completed methodology checklist/tracking sheet.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Execute Testing, Part 1 (90–120 min, extended for capstone)
1. Consolidate and re-verify your existing A01/A03/A07/A10 findings from Weeks 1–3 against Juice Shop specifically (confirm they still reproduce).
2. Begin fresh testing for A02 (Cryptographic Failures) and A05 (Security Misconfiguration).
3. Run `npm audit` against Juice Shop's dependency tree for a preliminary A06 (Vulnerable Components) pass.
4. Document every finding using your established actionable-finding format, with CVSS and ATT&CK technique ID.

**Expected Result:** At least 10 documented findings, spanning multiple OWASP Top 10 categories, with reproduction steps.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems / Review (30–45 min)
### Problem 1
Compare your actual findings so far against your Day 20 attack tree's predictions — did the tree correctly anticipate where vulnerabilities would be found?
### Problem 2
For any OWASP category where you found zero issues, write a brief justified statement of why (either genuinely well-defended, or an area needing more testing time tomorrow).
### Problem 3
Identify your single most severe finding so far and justify its CVSS score component by component.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full Month 1 recall sweep: Injection (Week 1) → Identity/Access (Week 2) → Client-Side/Threat Modeling (Week 3) → Logging/IR (Week 4, Days 22–25). This is your last spaced-repetition check before finalizing the capstone.
### Spaced-Repetition Review
* **Yesterday:** Incident response basics
* **This month:** the complete AppSec foundations arc

---

# 🧪 6. Active Recall Exercises
1. List all 10 OWASP Top 10 (2021) categories from memory.
2. Which of your Week 1–3 topics map to which categories?
3. Why is time-boxing important for a capstone assessment of this scope?
4. What would you need to test A06 (Vulnerable Components) effectively (dependency-scanning tooling — a preview of Week 5)?
5. What's the impact of skipping A09 (Logging/Monitoring Failures) in a real assessment, given everything you learned in Days 22–24?
6. How would you use your threat-modeling artifacts to prioritize testing time?
7. How would you justify a CVSS score to a skeptical engineering team?
8. Difference between a finding you verified yourself today vs. one carried over from earlier weeks' testing?
9. Real-world parallel: how does this capstone mirror an actual paid penetration-testing engagement's structure?
10. Teach your overall capstone methodology to a junior developer in under 3 minutes.

### Feynman Test
Explain your full capstone methodology (checklist, time-boxing, risk-prioritization) in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Full toolkit review — Burp Suite, `jwt_tool`, `hashcat`, ELK/Splunk, `npm audit`
### Today's Tool Goal
Use whichever tools are relevant to each category being tested today, moving fluidly between them as a real assessment would require.
### Tool Success Criteria
I'm not pausing to relearn any tool's basic mechanics — Month 1's repeated practice should make this feel fluent.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment (THIS IS THE CAPSTONE — today's work IS the project)
Today's contribution: at least 10 fresh, verified findings spanning multiple OWASP Top 10 categories, building toward the 15+ total findings target.
### Deliverable
`Day 26: Completed capstone testing Part 1 - 10+ verified OWASP Top 10 findings documented across 6+ categories.`

---

# 📝 9. Practice / Security Challenge
### Scenario
This IS today's practice challenge — the entire day's testing work against Juice Shop.
### My Task
1. Identify vulnerabilities across as many OWASP Top 10 categories as time allows.
2. Explain each finding's root cause.
3. Determine each finding's impact and CVSS score.
4. Reproduce each finding with clear, replicable steps.
5. Recommend a specific, actionable mitigation for each.
6. Document every result in your standardized format.
### Difficulty
⭐⭐⭐⭐☆ (capstone-level, cumulative)

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
* [ ] Methodology checklist built  * [ ] 10+ findings documented across multiple categories
* [ ] `npm audit` preliminary results  * [ ] Practice problems  * [ ] Active-recall answers  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Your complete findings list so far, by category
2. Which categories still need coverage tomorrow (Day 27)
3. Your most severe finding and its full justification
4. How your threat-modeling predictions compared to actual results
5. What "report writing" tasks remain for tomorrow's completion

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** OWASP Juice Shop (this IS the lab — the entire capstone)
**Lab Name:** "Month 1 Capstone: Full OWASP Top 10 Assessment (Day 1 of 2)"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 3+ hours (the bulk of today's schedule)
### Lab Objective
Systematically test Juice Shop across all 10 OWASP Top 10 categories, building on 4 weeks of accumulated skill, and document at least 10 findings today.
### Environment
Target: OWASP Juice Shop · Tools: full toolkit (Burp Suite, `jwt_tool`, `hashcat`, ELK/Splunk, `npm audit`) · Prerequisite: Weeks 1–4
### Lab Tasks
1. Re-verify and consolidate Weeks 1–3 findings against current Juice Shop behavior.
2. Test fresh categories: A02, A05, A06 (preliminary), A08 (revisit A08-adjacent deserialization).
3. Document each finding in full actionable format with CVSS and ATT&CK ID.
4. Track category coverage against your checklist.
5. Identify remaining work for Day 27.
### What I Need to Discover
Does systematic, checklist-driven testing surface anything your earlier, more exploratory Week 1–3 testing missed? What does that tell you about the value of methodology over ad-hoc exploration?
### Lab Success Criteria
At least 10 findings documented today with full reproduction steps, root cause, impact, and mitigation; clear tracking of remaining category coverage for tomorrow.

---
---

# 🛡️ Day 27 — Month 1 Capstone, Part 2: Complete the Assessment (15+ Findings)

**Date:** 15/09/2026
**Phase:** Month 1 — Application Security Foundations (Capstone)
**Primary Skill:** Completing a full-coverage vulnerability assessment and finalizing severity ratings
**Estimated Total Time:** 4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Complete testing across any remaining OWASP Top 10 categories from Day 26.
* Reach your target of 15+ documented, severity-rated findings.
* Finalize all CVSS scores and MITRE ATT&CK mappings for consistency.

### Success Criteria
* All 10 OWASP Top 10 categories have been explicitly considered (tested, or documented as "not applicable/well-defended with reasoning").
* 15+ total findings are documented in the standardized actionable format.
* Every finding has a consistent, defensible CVSS score.

---

# 📚 2. Topics to Study
### Primary Topic
**Completing and Finalizing a Vulnerability Assessment**
### Secondary Topics
* Consistency review: ensuring similar vulnerability types received similar severity treatment across the whole findings set
* Identifying any category gaps and closing them today
* Preparing raw findings for tomorrow's report-writing pass

### Priority
🔴 **Must Know:** how to do a final consistency pass across a large findings set — inconsistent severity scoring is a common amateur mistake that undermines report credibility
🟡 **Should Know:** how to honestly document "not applicable" or "not found" for a category, with reasoning, rather than forcing a weak finding just to check a box
🟢 **Nice to Know:** how professional pentest reports typically order findings (by severity, or by category) for reader usability

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Consistency Review Across Findings
* What it is: reviewing your full findings set side-by-side to ensure comparable vulnerabilities received comparable severity scores
* Why it matters: a report where similar-impact bugs receive wildly different severity ratings signals to a reviewer (or interviewer) that the scoring wasn't rigorous
* How it works: group findings by rough impact category (e.g., all "unauthorized data access" findings) and confirm their CVSS scores are internally consistent
* Common mistake: scoring severity based on how "interesting" or "technically impressive" a finding was to discover, rather than its actual real-world impact

### Concept 2 — Honest "Not Applicable" Documentation
* Definition: for any OWASP category where genuine testing found no exploitable issue, documenting this explicitly with the testing performed and reasoning, rather than silently omitting the category or forcing a weak/invalid finding
* Practical application: e.g., "A08 Software and Data Integrity Failures: tested for insecure deserialization patterns (Day 18 methodology); Juice Shop's JSON-only API architecture structurally avoids this class; no exploitable finding"
* Best practices: this kind of honest, reasoned negative finding is itself a valuable professional signal — it shows genuine testing occurred rather than assumption

### Concept 3 — Preparing for Report Writing
* Key terminology: raw findings vs. polished report
* Practical application: today's job is to finalize the *raw, complete* findings set; tomorrow (Day 28) is dedicated purely to polishing this into the final Portfolio Piece #1 report with an executive summary
* Best practices: don't try to perfect prose today — focus on completeness and accuracy of the underlying technical content first

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Consistency review | Checking that similar findings received similar severity treatment | Undermines or reinforces the credibility of your entire report |
| Honest negative finding | Documenting genuinely tested-and-not-found categories with reasoning | A signal of rigor, not a gap to hide |
| Raw vs. polished findings | Complete technical accuracy first, prose polish second | Keeps today's focus on substance, tomorrow's on presentation |

---

# ⏱️ 4. Study Schedule

## Session 1 — Gap Analysis (45–60 min)
Review your Day 26 checklist and identify exactly which categories/areas still need testing today to reach full coverage and your 15+ finding target.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Complete Testing (90–120 min)
1. Test any remaining categories (likely A06 Vulnerable Components in full, A09 Logging/Monitoring Failures applying your Days 22–24 lens directly to Juice Shop, and any remaining A02/A05/A08 gaps).
2. Push your total findings count to 15+.
3. Perform the full consistency review across all findings, adjusting any CVSS scores that don't hold up to scrutiny.

**Expected Result:** A complete, internally consistent findings set of 15+ vulnerabilities spanning all relevant OWASP Top 10 categories, with honest documentation for any category with no findings.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems / Final Review (30–45 min)
### Problem 1
Pick two findings with similar real-world impact and verify their CVSS scores are consistent; adjust if not.
### Problem 2
For any category with zero findings, write the honest "tested, not found" documentation with your specific testing methodology noted.
### Problem 3
Rank your full findings list by severity — does the ranking match your intuitive sense of "what would actually hurt this business most if exploited"?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Final full Month 1 recall sweep before report writing tomorrow: every major concept from Weeks 1–4, cold, no notes.
### Spaced-Repetition Review
* **Yesterday:** Capstone testing, part 1
* **This month:** the complete arc

---

# 🧪 6. Active Recall Exercises
1. How many total findings do you have, and across how many OWASP Top 10 categories?
2. Which finding is your highest severity, and why?
3. Which category (if any) had no exploitable findings, and what was your testing methodology there?
4. What would make your findings set more credible to a hiring manager reviewing it (consistency, honest negatives, clear reproduction steps)?
5. What's the overall risk picture this assessment paints for Juice Shop as a hypothetical real business?
6. How would you brief a CTO on the top 3 findings in under 5 minutes?
7. How would you prioritize remediation order if the engineering team could only fix 5 things this sprint?
8. Difference between your Week 1–3 exploratory findings and today's systematic capstone findings, in terms of process rigor?
9. Real-world parallel: how does a 15+ finding report compare in scope to a typical professional web-app pentest deliverable?
10. Teach your complete capstone results to a junior developer as if handing off the report, in under 4 minutes.

### Feynman Test
Summarize your complete Month 1 capstone findings and methodology in 5–6 sentences. If unclear, mark 🟡 **Needs Review** before tomorrow's report-writing day.

---

# 🛠️ 7. Tool Practice
**Tool:** Full toolkit + spreadsheet/markdown for findings tracking
### Today's Tool Goal
Finalize a clean, sortable findings tracker (spreadsheet or structured markdown table) covering all 15+ findings with category, severity, CVSS, and ATT&CK ID columns.
### Tool Success Criteria
My findings tracker could be handed to someone else and immediately understood without additional explanation.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — Juice Shop assessment (capstone completion)
Today's contribution: finalize the complete, 15+ finding, internally-consistent raw findings set — this is the technical core of tomorrow's polished report.
### Deliverable
`Day 27: Completed Month 1 capstone testing - 15+ findings documented, consistency-reviewed, ready for report writing.`

---

# 📝 9. Practice / Security Challenge
### Scenario
This IS today's practice challenge — completing and quality-checking the full capstone findings set.
### My Task
1. Ensure all 10 OWASP categories were genuinely considered.
2. Ensure severity scoring is internally consistent across all 15+ findings.
3. Ensure every finding has complete reproduction steps.
4. Ensure honest documentation exists for any category with no findings.
5. Document the final, complete result.
### Difficulty
⭐⭐⭐⭐☆ (capstone-level, cumulative)

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
* [ ] All 10 OWASP categories tested or honestly documented as N/A
* [ ] 15+ findings finalized  * [ ] Consistency review completed  * [ ] Findings tracker finalized
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Your complete, finalized findings set (15+ findings) cold
2. Your top 3 highest-severity findings in detail
3. Your overall assessment methodology end-to-end
4. What "report polish" work remains (executive summary, formatting, narrative)
5. How this capstone demonstrates Month 1's full skill arc to an employer

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** OWASP Juice Shop (capstone completion)
**Lab Name:** "Month 1 Capstone: Full OWASP Top 10 Assessment (Day 2 of 2)"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 3+ hours
### Lab Objective
Complete all remaining category testing, reach the 15+ finding target, and perform a full consistency review.
### Environment
Target: OWASP Juice Shop · Tools: full toolkit · Prerequisite: Day 26
### Lab Tasks
1. Complete remaining category testing (A06 in full, A09 applied directly, remaining gaps).
2. Reach 15+ total findings.
3. Perform the consistency review across the complete set.
4. Finalize the findings tracker.
5. Note explicitly what remains for tomorrow's report-writing day.
### What I Need to Discover
Now that you have complete coverage, does the overall picture change your sense of Juice Shop's biggest real risk compared to what Weeks 1–3's more exploratory testing suggested?
### Lab Success Criteria
15+ findings finalized, all categories addressed (tested or honestly documented), consistency review complete, findings tracker ready for tomorrow's report.

---
---

# 🛡️ Day 28 — Portfolio Piece #1 Finalized: Publish the Full Juice Shop Report

**Date:** 16/09/2026
**Phase:** Month 1 — Application Security Foundations (Capstone Completion)
**Primary Skill:** Professional security report writing and portfolio publication
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Transform the raw, complete findings set (Days 26–27) into a polished, professional vulnerability assessment report.
* Write an executive summary suitable for a non-technical stakeholder.
* Publish Portfolio Piece #1 to your GitHub portfolio repo, ready to show in interviews.

### Success Criteria
* A complete, polished report exists covering: executive summary, methodology, all 15+ findings in actionable format, and an overall risk summary/remediation roadmap.
* The report is published to your GitHub portfolio repo with clean formatting.
* You can present this report's highlights, live, in under 3 minutes.

---

# 📚 2. Topics to Study
### Primary Topic
**Professional Vulnerability Assessment Report Structure**
### Secondary Topics
* Executive summary writing for a full assessment (not just one finding, as practiced in Week 1 Day 7)
* Remediation roadmap prioritization (which fixes first, and why)
* Portfolio presentation: making this document genuinely interview-ready

### Priority
🔴 **Must Know:** the standard structure of a professional pentest/vulnerability assessment report: title/scope → executive summary → methodology → findings (by severity) → remediation roadmap → appendices
🟡 **Should Know:** how to write a remediation roadmap that accounts for both severity and implementation effort, not severity alone
🟢 **Nice to Know:** how real bug bounty/pentest firms format public disclosure reports as a style reference

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Full Report Structure
* What it is: title page/scope statement → executive summary → methodology → detailed findings (typically ordered by severity, highest first) → overall risk summary and remediation roadmap → appendices (tool output, technique references)
* Why it matters: this is the exact structure a hiring manager or technical interviewer expects when reviewing a portfolio piece — following it precisely signals professional readiness
* How it works: each section serves a distinct reader — the executive summary is for a non-technical stakeholder or hiring manager skimming your portfolio, the methodology section demonstrates your process rigor to a technical reviewer, and the findings themselves demonstrate technical depth
* Best practices: keep the executive summary genuinely non-technical — no CVE numbers or payload syntax, just business risk in plain language

### Concept 2 — Remediation Roadmap Prioritization
* Definition: an ordered plan for fixing findings that balances severity against realistic implementation effort, rather than a flat severity-only list
* Practical application: group findings into "quick wins" (high severity, low effort — fix immediately), "planned work" (high severity, high effort — schedule properly), and "backlog" (lower severity — address as capacity allows)
* Best practices: this demonstrates you understand that real organizations have limited engineering capacity — a purely severity-ranked list without effort consideration reads as less practically minded

### Concept 3 — Making the Portfolio Piece Interview-Ready
* Key terminology: the "elevator pitch" version vs. the full document
* Practical application: prepare a 2–3 sentence summary of the entire capstone that you can say confidently in an interview, with the full document available as backup detail
* Best practices: ensure your GitHub repo's README clearly signposts this report as your flagship Month 1 deliverable, with a working table of contents/links

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Executive summary | Non-technical, business-risk framing at the top of the full report | The first (and sometimes only) section a reviewer reads |
| Remediation roadmap | Severity + effort-balanced prioritized fix plan | Shows practical, business-aware security thinking |
| Portfolio-readiness | The document's suitability for direct interview presentation | The actual measure of whether this deliverable achieves its purpose |

---

# ⏱️ 4. Study Schedule

## Session 1 — Report Structure & Executive Summary (45–60 min)
Draft the full report skeleton (all section headers) and write the executive summary first, since it forces you to distill the entire assessment's business meaning before diving into details.
**Output:** A complete executive summary (4–6 sentences) covering overall risk level, most critical findings, and top-line recommendation.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Assemble the Full Report (90–120 min)
1. Insert your methodology section (referencing your Day 26 checklist approach and threat-modeling-guided testing).
2. Insert all 15+ findings, ordered by severity, in your standardized actionable format (title, CVSS, ATT&CK ID, reproduction steps, root cause, impact, remediation).
3. Write the remediation roadmap (quick wins / planned work / backlog).
4. Format the complete document cleanly in Markdown, with a table of contents.
5. Publish to your GitHub portfolio repo.

**Expected Result:** A complete, polished, published Portfolio Piece #1.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems / Interview Prep (30–45 min)
### Problem 1
Write your 2–3 sentence "elevator pitch" summary of this entire capstone project.
### Problem 2
Prepare an answer for the likely interview follow-up: "Which finding are you most proud of, and why?"
### Problem 3
Prepare an answer for: "If you had to pick just one finding to fix first at a real company, which would it be and why?"
### Challenge
Practice presenting the full report's highlights out loud, unaided, in under 3 minutes, as if walking an interviewer through your portfolio live.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
This IS the revision — the entire report-writing process requires recalling and correctly representing every Month 1 concept. Use this time specifically to proofread for technical accuracy against your actual notes from Weeks 1–4.
### Spaced-Repetition Review
* **Yesterday:** Capstone completion
* **This entire month:** the full AppSec foundations arc, now crystallized into one deliverable

---

# 🧪 6. Active Recall Exercises
1. What is the standard structure of a professional vulnerability assessment report?
2. How does an executive summary differ from the technical findings section in audience and content?
3. Why does a remediation roadmap need to consider effort, not just severity?
4. What would a hiring manager look for first when reviewing this portfolio piece?
5. What's the overall business risk narrative your report tells about Juice Shop?
6. How would you defend your CVSS scoring if challenged in an interview?
7. How would you explain your testing methodology's rigor (checklist-driven, threat-model-guided) to a skeptical reviewer?
8. Difference between "a list of bugs" and "a professional risk assessment" — what elevates one to the other?
9. Real-world parallel: how does this deliverable compare to an actual paid consulting engagement's final report?
10. Teach your complete Month 1 capstone story to a junior developer, in under 4 minutes, as if mentoring them through their own future first capstone.

### Feynman Test
Present your Portfolio Piece #1's executive summary and top 3 findings, live, in under 3 minutes. If you stumble or need notes, mark 🟡 **Needs Review** and rehearse again before considering this complete.

---

# 🛠️ 7. Tool Practice
**Tool:** Markdown/GitHub (final portfolio publishing)
### Today's Tool Goal
Ensure your portfolio repo's structure, README, and this report are clean, professional, and easily navigable.
### Commands / Features to Practice
```text
git add . && git commit -m "Month 1 Capstone: Full Juice Shop OWASP Top 10 Assessment (Portfolio Piece #1)" && git push
```
### Tool Success Criteria
An interviewer could open your GitHub repo cold and find, understand, and be impressed by this report within 2 minutes, unaided.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #1 — COMPLETE
Today's contribution: the final, polished, published report — Month 1's flagship deliverable.
### Deliverable
`Day 28: PORTFOLIO PIECE #1 COMPLETE - Published full Juice Shop OWASP Top 10 Assessment Report (executive summary, methodology, 15+ findings, remediation roadmap).`

---

# 📝 9. Practice / Security Challenge
### Scenario
You're now in a real interview, and the interviewer says: "I see you have a Juice Shop assessment in your portfolio — walk me through it."
### My Task
1. Deliver your elevator pitch (2–3 sentences).
2. Highlight your top 2–3 findings with brief technical detail.
3. Explain your methodology (checklist + threat-modeling-guided).
4. Explain your remediation roadmap's prioritization logic.
5. Anticipate and prepare for one likely deep-dive follow-up question on your highest-severity finding.
6. Document the result.
### Difficulty
⭐⭐⭐☆☆ (interview-simulation level)

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
* [ ] Full report assembled and formatted  * [ ] Executive summary written  * [ ] Remediation roadmap written
* [ ] Report published to GitHub  * [ ] Elevator pitch rehearsed  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow (Start of Month 2)
1. Your complete Portfolio Piece #1, cold, interview-ready
2. Every major Month 1 concept (injection, identity/access, client-side, threat modeling, logging/IR) at a working professional level
3. How Month 1's AppSec foundation sets up Month 2's DevSecOps focus (shifting from "finding bugs in a running app" to "preventing them via pipeline/infrastructure automation")
4. What you'd do differently if repeating this capstone knowing what you know now
5. Your honest self-assessment of remaining Month 1 gaps to keep an eye on during Month 2

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link to complete Portfolio Piece #1]
**Final Status:** 🟢 / 🟡 / 🔴

**🏆 MONTH 1 MILESTONE ACHIEVED: Portfolio Piece #1 (OWASP Juice Shop Vulnerability Assessment) is complete and published.**

---

# 14. Hands-On LAB
**Lab Platform:** N/A — today is a report-writing and portfolio-publishing day
**Lab Name:** "Present the Capstone: Full Report Walkthrough"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 45–60 minutes (folded into Session 3 above)
### Lab Objective
Confirm you can present the entire month's culminating work clearly and confidently — the real "lab" today is professional communication of technical work, the actual skill that converts a completed project into a job offer.
### Environment
Target: your own completed report · Tools: none, just your voice/notes · Prerequisite: Days 22–27
### Lab Tasks
1. Present the full report's executive summary and top findings out loud, timed.
2. Answer one self-generated likely interview follow-up question without notes.
3. Get feedback if possible (mentor, peer, or self-review via recording).
4. Note anything you'd tighten in either the document or your live delivery.
5. Confirm the GitHub repo link works and displays cleanly.
### What I Need to Discover
Can you talk about this work with genuine confidence, or does part of it still feel shaky under the pressure of explaining it live? That gap — if it exists — is exactly what's worth another pass before you're in a real interview room.
### Lab Success Criteria
You can present Portfolio Piece #1 clearly and confidently, unaided, in under 3 minutes, and answer at least one reasonable follow-up question without notes.

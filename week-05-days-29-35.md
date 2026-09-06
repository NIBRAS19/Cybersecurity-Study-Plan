# 🗓️ WEEK 5 — Secure SDLC & SAST/DAST Tooling (Days 29–35)

*Month 2 begins: shifting from finding vulnerabilities in a running app to preventing them via process and automation.*

---

# 🛡️ Day 29 — Secure SDLC Models (MS SDL, OWASP SAMM)

**Date:** 17/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Understanding where security fits into the software development lifecycle
**Estimated Total Time:** 3–4 hours
**Difficulty:** Beginner

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain what a Secure SDLC is and why "shifting left" (catching issues earlier in development) reduces cost and risk.
* Compare Microsoft's Security Development Lifecycle (MS SDL) and OWASP SAMM as two influential models.
* Identify where, in a typical Agile sprint cycle, each Month 1 vulnerability class should ideally have been caught.

### Success Criteria
* Explain "shift left" without notes.
* Map MS SDL's phases and OWASP SAMM's business functions from memory.
* For 3 Month 1 vulnerability classes, identify the earliest SDLC phase where each could have been caught.

---

# 📚 2. Topics to Study
### Primary Topic
**Secure Software Development Lifecycle Models**
### Secondary Topics
* Microsoft SDL's phases (Training, Requirements, Design, Implementation, Verification, Release, Response)
* OWASP SAMM's business functions (Governance, Design, Implementation, Verification, Operations)
* The cost-of-fixing-late-vs-early argument

### Priority
🔴 **Must Know:** the "shift left" principle — the same bug costs exponentially more to fix the later it's caught (design vs. code review vs. production incident)
🟡 **Should Know:** MS SDL's 7 phases at a high level
🟢 **Nice to Know:** OWASP SAMM's maturity-model scoring approach (each business function scored 0–3)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — "Shift Left" and Cost-of-Fixing
* What it is: the principle that finding and fixing a security issue earlier in development (design/requirements) is dramatically cheaper than finding it in production
* Why it matters: every Month 1 vulnerability class you exploited was ultimately found via *testing* a finished application — this week reframes the question as "how do we catch this during design/coding instead?"
* How it works: a threat-modeling session (Week 3) during design catches an IDOR-prone architecture before a line of code exists; a SAST scan (this week) catches a SQLi-prone code pattern before it merges; a DAST scan catches a live vulnerability before release — each stage is a progressively later, more expensive safety net
* Real-world example: industry cost studies consistently show fixing a design-stage flaw costs a small fraction of fixing the same class of flaw once it's in production
* Common mistake: treating security testing as something that only happens right before release, rather than integrated throughout

### Concept 2 — Microsoft SDL
* Definition: a structured 7-phase process (Training → Requirements → Design → Implementation → Verification → Release → Response) embedding security activities at each phase
* Architecture/process: Design phase includes threat modeling (directly = your Week 3 work); Implementation includes secure coding standards and SAST; Verification includes DAST and fuzzing; Response includes incident response planning (directly = your Week 4 work)
* Practical application: recognizing that Weeks 1–4 of your own curriculum already mirror several MS SDL phases gives you a ready-made narrative for interviews about how your training maps to real process

### Concept 3 — OWASP SAMM
* Key terminology: business functions (Governance, Design, Implementation, Verification, Operations), maturity levels (0–3 per practice)
* Practical application: SAMM is often used as a self-assessment framework — an organization scores itself on each practice and builds a roadmap to improve
* Best practices: SAMM's operations function specifically includes incident management and environment hardening, tying directly forward into this month's container/cloud topics

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Shift left | Catching security issues earlier in the SDLC | The core cost-benefit argument for all of Month 2's automation focus |
| MS SDL | Microsoft's 7-phase secure development process | A widely-referenced industry model, useful interview vocabulary |
| OWASP SAMM | A maturity-model self-assessment framework for security practices | Useful for describing organizational security posture, not just single vulnerabilities |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study "shift left," MS SDL's 7 phases, and OWASP SAMM's 5 business functions.
**Output:** A table mapping each of your Month 1 weeks (injection, identity/access, client-side, logging/IR) to the MS SDL phase where that class of issue is ideally caught.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Take one of your own past full-stack projects (or a hypothetical feature you'll build for Week 6's pipeline project).
2. Walk through MS SDL's 7 phases explicitly for that project: what security activity would happen at each phase?
3. Score that same project informally against OWASP SAMM's 5 business functions (0–3 each), with a one-sentence justification per score.

**Expected Result:** A completed MS SDL phase walkthrough + a rough SAMM scorecard for a real project.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
An organization only runs a penetration test right before each quarterly release. Where does this fall short of a full Secure SDLC, and what's missing?
### Problem 2
Explain, with a concrete cost argument, why catching Day 11's IDOR vulnerability at design/threat-modeling time (Week 3) would have been cheaper than catching it via Week 1–3's hands-on testing.
### Problem 3
Score a hypothetical startup with "no formal security process, occasional ad-hoc code review" against OWASP SAMM's 5 functions.
### Challenge
Design a lightweight Secure SDLC (informed by both MS SDL and SAMM) appropriate for a small startup team of 5 developers — what's the minimum viable version that still catches most Month 1-style issues early?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Portfolio Piece #1 Finalized (Day 28)
Recall without notes: your top 3 capstone findings and overall methodology.
### Spaced-Repetition Review
* **Yesterday:** Portfolio Piece #1 completion
* **1 week ago:** Incident response basics

---

# 🧪 6. Active Recall Exercises
1. What does "shift left" mean, and why does it matter economically?
2. What are MS SDL's 7 phases?
3. What are OWASP SAMM's 5 business functions?
4. What would an organization need to genuinely adopt a Secure SDLC (executive buy-in, tooling, developer training)?
5. What's the impact of only testing security right before release vs. throughout development?
6. How would you use SAMM as a self-assessment tool for an organization you're interviewing with?
7. How would you pitch "shift left" to a skeptical engineering manager focused only on shipping speed?
8. Difference between MS SDL (a process model) and SAMM (a maturity self-assessment model)?
9. Real-world example: the general cost-of-late-fixing argument from industry studies.
10. Teach "shift left" to a junior developer in plain language.

### Feynman Test
Explain shift left, MS SDL, and SAMM together in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** None specific today — this is a conceptual/process day; Semgrep tooling begins tomorrow
### Today's Tool Goal
N/A
### Tool Success Criteria
N/A

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (begins this month; today lays conceptual groundwork)
Today's contribution: write a short "Secure SDLC Approach" document for your upcoming Portfolio Piece #2, explaining which SDLC phases your pipeline will embody (this becomes part of that project's README).
### Deliverable
`Day 29: Documented Secure SDLC approach (MS SDL + SAMM informed) to guide Portfolio Piece #2's design.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A CTO asks you: "We ship fast and don't have time for a heavy security process — convince me shift-left is worth it without slowing us down."
### My Task
1. Identify the core cost-benefit argument.
2. Explain how lightweight practices (threat modeling in design reviews, SAST in CI) add minimal friction while catching issues early.
3. Determine the risk of not adopting this.
4. Propose a minimal-friction starting point.
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
* [ ] Study notes  * [ ] MS SDL phase mapping completed  * [ ] SAMM scorecard completed  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Shift left and its cost argument
2. MS SDL's 7 phases
3. OWASP SAMM's 5 business functions
4. How Month 1's weeks map onto SDLC phases
5. How tomorrow's SAST topic is the concrete tooling implementation of "Implementation phase" security

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Self-directed (conceptual application to your own project)
**Lab Name:** "Secure SDLC Gap Analysis on a Past Project"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60 minutes (folded into Session 2 above)
### Lab Objective
Apply MS SDL and SAMM frameworks to a real past project to identify concrete process gaps.
### Environment
Target: one of your own past full-stack projects · Tools: none, just analysis
### Lab Tasks
1. Walk through each MS SDL phase for the chosen project.
2. Identify which phases had zero security activity historically.
3. Score the project against SAMM's 5 functions.
4. Identify the single highest-leverage process improvement you'd make first.
5. Document the result.
### What I Need to Discover
Looking back honestly, which SDLC phase did your own past development process skip most completely? Is that consistent with the kinds of bugs you'd expect to find if you tested that project today?
### Lab Success Criteria
Complete phase walkthrough and SAMM scorecard, with one clearly justified highest-priority improvement identified.

---
---

# 🛡️ Day 30 — SAST Part 1: Semgrep Fundamentals

**Date:** 18/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Static Application Security Testing with Semgrep
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain how SAST tools analyze source code without executing it, and what classes of bugs they catch well vs. poorly.
* Install and run Semgrep against a real codebase.
* Interpret and triage Semgrep's findings, distinguishing true positives from false positives.

### Success Criteria
* Explain SAST's strengths/limitations without notes.
* Run Semgrep with a standard ruleset against one of your own past projects.
* Triage at least 5 findings, correctly labeling true/false positives with reasoning.

---

# 📚 2. Topics to Study
### Primary Topic
**Static Application Security Testing (SAST) with Semgrep**
### Secondary Topics
* How SAST differs fundamentally from DAST (analyzing source code vs. testing a running application)
* Semgrep's rule-matching approach (pattern matching against AST-like structures)
* False positive triage methodology

### Priority
🔴 **Must Know:** SAST analyzes source code directly and can catch issues before the app ever runs, but tends to produce more false positives and can miss vulnerabilities that only manifest at runtime (e.g., certain business-logic flaws)
🟡 **Should Know:** how to run Semgrep with a pre-built ruleset (e.g., `p/owasp-top-ten`) against a real project
🟢 **Nice to Know:** Semgrep's difference from traditional SAST tools (lightweight, pattern-based, fast) vs. heavier tools like SonarQube (deeper data-flow analysis)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — SAST Strengths and Limitations
* What it is: SAST tools parse source code into an abstract representation and search for known-dangerous patterns (e.g., a `child_process.exec()` call with a variable argument — directly recalling Day 4's command injection topic)
* Why it matters: it catches issues extremely early (before the code even runs) and scales well across huge codebases, but it can't see runtime behavior, so it often produces false positives (flagging code that's actually safe due to context the tool can't see) and false negatives (missing logic flaws that only manifest in execution, like Day 11's IDOR, which usually requires runtime/business-logic understanding SAST tools don't have)
* How it works: Semgrep matches source code against a rule written in a simplified pattern language resembling the target language itself, making custom rules easy to write and read
* Real-world example: a Semgrep rule for `child_process.exec($CMD)` where `$CMD` isn't a string literal will flag exactly the kind of vulnerable pattern you manually exploited on Day 4
* Common mistake: treating a clean SAST scan as proof of security — SAST is one layer, and it structurally cannot catch several vulnerability classes you've already learned to exploit manually (most access-control and business-logic issues)

### Concept 2 — Running Semgrep
* Definition: an open-source, fast, pattern-based static analysis tool with pre-built rulesets for common languages and vulnerability categories
* Practical application: `semgrep --config=p/owasp-top-ten .` runs a curated OWASP-focused ruleset against the current directory
* Best practices: start with a well-maintained community ruleset before writing custom rules (Day 31)

### Concept 3 — False Positive Triage
* Key terminology: true positive, false positive, suppression/ignore comments
* Practical application: for each finding, determine whether the flagged pattern is genuinely exploitable in context (e.g., is the "unsanitized" input actually attacker-controlled, or is it a hardcoded internal value the tool couldn't distinguish?)
* Common vulnerabilities in triage: reflexively dismissing findings as false positives without genuinely verifying, which can hide real issues (directly recalling Day 24's alert-fatigue lesson — the same dynamic applies to SAST findings, not just SIEM alerts)
* Best practices: document your triage reasoning for every finding, not just the ones you act on

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| SAST | Analyzing source code without executing it | Catches issues early but misses runtime/logic flaws |
| Semgrep | A fast, pattern-based, easy-to-customize SAST tool | Industry-relevant, low barrier to entry for custom rule-writing |
| False positive triage | Verifying whether a flagged pattern is genuinely exploitable | Prevents both wasted effort and dismissed real issues (echoes Day 24's alert fatigue) |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study SAST's strengths/limitations relative to what you now know from manual exploitation (Weeks 1–3), and Semgrep's rule-matching model.
**Output:** A written list of which Week 1–3 vulnerability classes SAST would likely catch well vs. poorly, with reasoning.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Install Semgrep (`pip install semgrep --break-system-packages` or via the recommended install method).
2. Run `semgrep --config=p/owasp-top-ten` against one of your own past full-stack projects.
3. Triage the first 5–10 findings: for each, determine true/false positive with documented reasoning.
4. For at least one true positive, fix the underlying code and re-scan to confirm the finding disappears.

**Expected Result:** A documented Semgrep scan report with triaged findings and at least one confirmed fix.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Semgrep flags a hardcoded string that looks like an API key in a test fixture file. Is this a true or false positive, and what additional context would you need to be certain?
### Problem 2
Explain, using Day 11's IDOR vulnerability as the example, why a SAST tool would likely NOT catch this class of bug even with a perfect ruleset.
### Problem 3
Design a triage workflow (steps) for handling a Semgrep scan with 200 findings on a large legacy codebase, so real issues aren't lost in volume.
### Challenge
Compare and contrast SAST (today) with the SIEM alert-fatigue problem (Day 24) — what's structurally similar about the false-positive management challenge in both domains?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Secure SDLC Models (Day 29)
Recall without notes: shift-left principle, MS SDL phases, OWASP SAMM functions.
### Spaced-Repetition Review
* **Yesterday:** Secure SDLC models
* **1 week + 1 day ago:** Month 1 capstone completion

---

# 🧪 6. Active Recall Exercises
1. What is SAST, and how does it differ from DAST?
2. How does Semgrep's rule-matching approach work?
3. Why does SAST struggle with access-control/business-logic bugs like IDOR?
4. What would you need to triage a SAST finding correctly (context about how the flagged code is actually used)?
5. What's the impact of blindly trusting a clean SAST scan as "proof of security"?
6. How would you integrate Semgrep into a CI pipeline (previewed for Week 6)?
7. How would you prevent SAST-fatigue (analogous to Day 24's alert fatigue) on a large codebase?
8. Difference between a true positive and a false positive in SAST triage?
9. Real-world example: a `child_process.exec()` pattern SAST would correctly flag.
10. Teach SAST's strengths and limitations to a junior developer in plain language.

### Feynman Test
Explain what SAST catches well and poorly, using specific Week 1–3 examples, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Semgrep
### Today's Tool Goal
Run a standard ruleset scan and interpret output confidently.
### Commands / Features to Practice
```text
pip install semgrep --break-system-packages
semgrep --config=p/owasp-top-ten .
semgrep --config=p/owasp-top-ten . --json > results.json
```
### Tool Success Criteria
I can run Semgrep, interpret its output format, and triage findings without consulting documentation for basic usage.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (Semgrep will become a pipeline gate in Week 6)
Today's contribution: document your Semgrep scan results and triage against a past project as a standalone "SAST Assessment" artifact, and note the specific ruleset/configuration you'll carry forward into the Week 6 pipeline build.
### Deliverable
`Day 30: Ran and documented Semgrep SAST scan against a past project; triaged findings; identified pipeline configuration for Week 6.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A development team adopts Semgrep, runs it once, finds 300 findings, and gives up on the tool entirely as "too noisy."
### My Task
1. Identify the underlying problem (this mirrors Day 24's alert-fatigue pattern).
2. Explain the root cause (likely an overly broad initial ruleset with no triage/suppression workflow).
3. Determine the impact of abandoning the tool entirely.
4. Propose a phased adoption approach (start narrow, expand ruleset as triage capacity allows, suppress known-accepted patterns).
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
* [ ] Study notes  * [ ] Semgrep installed and scan run  * [ ] Findings triaged  * [ ] One fix confirmed
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. SAST's strengths and limitations
2. How to run and interpret a Semgrep scan
3. False-positive triage methodology
4. Which Month 1 vulnerability classes SAST would/wouldn't catch
5. How tomorrow's custom-rule-writing topic extends today's foundation

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Lab (Semgrep against a personal project)
**Lab Name:** "First SAST Pass: Scan, Triage, Fix"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Complete a full scan-triage-fix-rescan cycle against real code, establishing the workflow you'll automate in Week 6.
### Environment
Target: your own past project · Tools: Semgrep · Prerequisite: today's fundamentals
### Lab Tasks
1. Run the initial scan.
2. Triage all findings into true/false positive categories.
3. Fix at least one true positive.
4. Re-scan and confirm the finding is resolved.
5. Document the before/after diff and reasoning.
### What I Need to Discover
How much of your own past code — written before this transition began — contains patterns you'd now flag yourself, even before running the tool? What does that tell you about how much your security intuition has already developed?
### Lab Success Criteria
Complete scan-triage-fix-rescan cycle documented, with at least one confirmed resolved finding.

---
---

# 🛡️ Day 31 — SAST Part 2: Writing Custom Semgrep Rules

**Date:** 19/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Writing and triaging custom Semgrep detection rules
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Write a custom Semgrep rule from scratch targeting a specific vulnerable pattern you know from Weeks 1–3.
* Test the rule against both a vulnerable and a fixed code sample to confirm correct true/false positive behavior.
* Understand rule metadata (severity, message, references) for producing actionable output.

### Success Criteria
* Explain Semgrep's rule YAML structure without notes.
* Write a working custom rule catching a specific Week 1 vulnerability pattern (e.g., unparameterized SQL queries).
* Confirm the rule correctly flags vulnerable code and does NOT flag the fixed equivalent.

---

# 📚 2. Topics to Study
### Primary Topic
**Writing Custom Semgrep Rules**
### Secondary Topics
* Semgrep rule YAML structure: `pattern`, `pattern-not`, `message`, `severity`, `languages`
* Metavariables (`$VAR`) for matching flexible code patterns
* Testing rules against both positive (vulnerable) and negative (safe) code samples

### Priority
🔴 **Must Know:** how to write a basic Semgrep rule using `pattern` and metavariables to match a specific dangerous code shape
🟡 **Should Know:** `pattern-not` for excluding known-safe variations and reducing false positives
🟢 **Nice to Know:** Semgrep's taint-tracking mode for more advanced source-to-sink pattern matching

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Semgrep Rule Structure
* What it is: a YAML file defining `rules`, each with an `id`, a `pattern` (or `patterns` combining multiple conditions), a `message`, a `severity`, and target `languages`
* Why it matters: writing your own rules lets you encode your organization's (or your own portfolio's) specific known-dangerous patterns — directly turning your Week 1–3 exploitation knowledge into automated, repeatable detection
* How it works: `pattern: $DB.query("..." + $INPUT)` with metavariables `$DB` and `$INPUT` matches string-concatenated SQL queries regardless of variable names — this directly encodes Day 1's SQLi root cause as a detectable pattern
* Real-world example: security teams regularly write custom rules for organization-specific frameworks/patterns that generic community rulesets don't cover

### Concept 2 — Metavariables and Pattern Flexibility
* Definition: `$VAR`-style placeholders that match any code fragment in that position, enabling one rule to catch many syntactic variations of the same dangerous pattern
* Practical application: `pattern: exec($CMD)` where `$CMD` is not a string literal catches Day 4's command injection pattern across any variable name
* Common vulnerabilities in rule-writing: overly broad patterns that match too much (high false positive rate) or overly narrow patterns that only match one exact code shape (easily evaded, high false negative rate)

### Concept 3 — Testing Rules Rigorously
* Key terminology: positive test case (should match), negative test case (should NOT match)
* Practical application: for every custom rule, write both a deliberately vulnerable snippet and its properly fixed equivalent, and confirm the rule behaves correctly on both
* Best practices: Semgrep supports inline test annotations (`# ruleid: rule-name` and `# ok: rule-name`) for exactly this kind of automated rule testing

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Metavariable | A `$VAR`-style flexible pattern placeholder | Enables one rule to match many syntactic variations |
| `pattern-not` | Excludes specified safe variations from matching | Reduces false positives without weakening true detection |
| Positive/negative test case | Vulnerable/safe code samples used to validate a rule | Prevents shipping a rule that doesn't actually work as intended |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study Semgrep's rule YAML structure and metavariable syntax using the official rule-writing documentation.
**Output:** Draft (on paper/in notes) the pattern structure for a rule targeting Day 1's SQLi concatenation pattern, before writing actual YAML.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Write a custom Semgrep rule catching string-concatenated SQL queries (Day 1's pattern).
2. Write a positive test case (vulnerable code) and a negative test case (parameterized query) for this rule.
3. Run the rule against both and confirm correct behavior.
4. Repeat for a second rule of your choice (e.g., catching `exec()` with non-literal arguments from Day 4, or JWT verification without algorithm pinning from Day 8).

**Expected Result:** 2 working, tested custom Semgrep rules with documented positive/negative test cases.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Write the Semgrep pattern (conceptually, in your own notation if not exact YAML) for detecting a missing `HttpOnly` flag on a cookie-setting call.
### Problem 2
Your rule for detecting SQL concatenation also flags a legitimate use where `$INPUT` is a hardcoded constant, not user input. How would you refine the rule to reduce this false positive?
### Problem 3
Why is writing both a positive AND negative test case for every custom rule non-negotiable practice, not just a nice-to-have?
### Challenge
Design (write the rule) for detecting Day 18's insecure deserialization pattern (e.g., use of `node-serialize`'s `unserialize` function) in a Node.js codebase.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: SAST Fundamentals with Semgrep (Day 30)
Recall without notes: SAST's strengths/limitations and basic Semgrep usage.
### Spaced-Repetition Review
* **Yesterday:** SAST fundamentals
* **2 weeks + 3 days ago:** Insecure deserialization (Day 18) — directly useful for today's custom-rule challenge

---

# 🧪 6. Active Recall Exercises
1. What is the structure of a Semgrep rule (id, pattern, message, severity, languages)?
2. How do metavariables enable flexible pattern matching?
3. Why does `pattern-not` help reduce false positives without weakening detection?
4. What would you need to write an effective custom rule (a clear understanding of the vulnerable code shape, from hands-on exploitation experience)?
5. What's the value of encoding your own Week 1–3 exploitation knowledge as automated Semgrep rules?
6. How would you test a new custom rule before trusting it in a CI pipeline?
7. How would you avoid writing an overly narrow rule that's trivially evaded?
8. Difference between a positive and negative test case in rule validation?
9. Real-world example: a custom rule an organization might write for a framework-specific dangerous pattern.
10. Teach custom Semgrep rule-writing to a junior developer in plain language.

### Feynman Test
Explain how to write and validate a custom Semgrep rule in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Semgrep (rule authoring and testing)
### Today's Tool Goal
Write, test, and validate 2 custom rules with positive/negative test cases.
### Commands / Features to Practice
```text
semgrep --config=my-rule.yaml test-file.js
semgrep --test --config=my-rule.yaml tests/
```
### Tool Success Criteria
I can write a working custom rule from scratch, without copying an existing rule verbatim, and validate it with proper test cases.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline
Today's contribution: your 2 custom Semgrep rules become part of the custom ruleset your Week 6 pipeline will run — save them in a `custom-rules/` directory in your emerging pipeline repo.
### Deliverable
`Day 31: Wrote and validated 2 custom Semgrep rules (SQLi concatenation pattern + one additional pattern), saved to pipeline repo's custom-rules directory.`

---

# 📝 9. Practice / Security Challenge
### Scenario
Your company uses a custom internal ORM wrapper, and the community Semgrep OWASP ruleset doesn't recognize its query methods as SQL sinks, so real SQLi-prone code slips through undetected.
### My Task
1. Identify the gap (community rules can't know about internal/proprietary code patterns).
2. Explain why custom rule-writing is essential for any organization with non-standard frameworks.
3. Determine the risk of relying solely on community rulesets.
4. Propose (write) a custom rule pattern for the hypothetical internal ORM's query method.
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
* [ ] Study notes  * [ ] 2 custom rules written and tested  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Tool practice (rule authoring)  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Semgrep rule YAML structure and metavariable syntax
2. How to reduce false positives with `pattern-not`
3. Positive/negative test case validation
4. Your 2 custom rules and what they catch
5. How tomorrow's DAST topic complements SAST by testing the running application instead of source code

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Lab (Semgrep custom rule authoring)
**Lab Name:** "Encode Your Own Exploitation Knowledge as a Detection Rule"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Write a custom Semgrep rule for a vulnerability class you personally exploited by hand in Weeks 1–3, proving you can translate manual exploitation knowledge into automated detection.
### Environment
Target: your own test code samples · Tools: Semgrep · Prerequisite: today's rule-writing fundamentals
### Lab Tasks
1. Choose a Week 1–3 vulnerability class not yet covered by today's Session 2 rules (e.g., missing CSRF token check, JWT `alg: none` acceptance).
2. Write the rule pattern.
3. Write positive and negative test cases.
4. Validate the rule catches the vulnerable case and passes the safe case.
5. Document the rule's purpose and validation results.
### What I Need to Discover
Can you translate something you understand only as a hands-on exploitation technique into a formal, generalized pattern a tool can check automatically? Where does that translation get difficult, and why?
### Lab Success Criteria
A working, validated custom rule with documented positive/negative test results and a clear explanation of the vulnerability class it targets.

---
---

# 🛡️ Day 32 — DAST Part 1: OWASP ZAP Fundamentals

**Date:** 20/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Dynamic Application Security Testing with OWASP ZAP
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain how DAST differs from SAST (testing a running application vs. analyzing source code) and what it catches that SAST can't.
* Run an automated ZAP scan against a running application (Juice Shop).
* Configure and run an authenticated scan (testing behind a login).

### Success Criteria
* Explain DAST's strengths/limitations relative to SAST without notes.
* Run a baseline ZAP scan and interpret its findings.
* Configure ZAP for authenticated scanning against Juice Shop.

---

# 📚 2. Topics to Study
### Primary Topic
**Dynamic Application Security Testing (DAST) with OWASP ZAP**
### Secondary Topics
* Passive scanning (observing traffic) vs. active scanning (sending test payloads)
* Authenticated scanning (scanning behind a login wall)
* DAST's complementary relationship with SAST (Day 30–31)

### Priority
🔴 **Must Know:** DAST tests the actual running application from the outside, like an attacker would, so it naturally catches runtime/business-logic issues (like Day 11's IDOR, which SAST typically misses) but can't see source code directly, so it may miss issues that never manifest in reachable, testable behavior
🟡 **Should Know:** the difference between ZAP's passive scan (safe, just observes traffic) and active scan (sends actual attack payloads, which requires authorization, exactly like your Week 1–3 manual testing)
🟢 **Nice to Know:** ZAP's automation framework (YAML-based) for scripting repeatable scan configurations

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — DAST vs. SAST: Complementary Coverage
* What it is: DAST interacts with a fully running application over HTTP, exactly as you did manually throughout Weeks 1–3, but automated and systematic
* Why it matters: this is precisely why DAST can catch some of what SAST structurally can't — since DAST observes actual runtime behavior, it's better positioned to find issues like broken access control that depend on application logic and state, not just code patterns
* How it works: ZAP proxies traffic (like Burp Suite, which you've used extensively), passively analyzing what it sees, then actively injecting test payloads (SQLi probes, XSS payloads, etc. — the same payload families you've been crafting by hand all month) against discovered endpoints
* Real-world example: your own Week 1–3 manual testing WAS essentially a very targeted, human-driven DAST process — today automates and scales that same fundamental approach
* Common mistake: relying on DAST alone and assuming full coverage — automated scanners typically miss complex, multi-step business-logic flaws that require human creativity (many of your Week 2 access-control and Week 3 CSRF findings required exactly this kind of manual reasoning)

### Concept 2 — Passive vs. Active Scanning
* Definition: passive scanning observes traffic you generate by browsing normally and flags issues visible just from that traffic (missing security headers, exposed sensitive data in responses); active scanning deliberately sends attack payloads to test endpoints
* Architecture/process: always start with a passive scan (safe, no active payloads sent) before running an active scan, and always confirm you're authorized to run active scans against the target
* Attack scenario: active scanning is functionally similar to the manual injection testing from Week 1 — ZAP is automating that same process across every discovered parameter

### Concept 3 — Authenticated Scanning
* Key terminology: authentication script/context, session management configuration
* Practical application: configuring ZAP with valid login credentials so it can scan pages/functionality only reachable after authentication (much of Juice Shop's interesting attack surface, like Week 2's IDOR-prone endpoints, requires being logged in)
* Best practices: use a dedicated test account, never scan authenticated as a real production user

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| DAST | Testing a running application from the outside, like an attacker | Catches runtime/business-logic issues SAST structurally misses |
| Passive scan | Analyzing observed traffic without sending attack payloads | Safe first step, no authorization complications |
| Authenticated scan | Scanning behind a login using configured test credentials | Essential for reaching most of a real application's attack surface |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study DAST's relationship to SAST and ZAP's passive/active scanning model.
**Output:** A written comparison: for each Week 1–3 vulnerability class, would DAST likely catch it well, poorly, or only with manual guidance? Justify each.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Install OWASP ZAP.
2. Run a baseline passive scan against Juice Shop (browse normally through the ZAP proxy).
3. Review passive findings (missing headers, etc.).
4. Configure ZAP with an authentication context using a Juice Shop test account.
5. Run an authenticated active scan against a limited, defined scope.

**Expected Result:** A documented passive scan report + a configured, working authenticated active scan.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A passive scan flags "Missing Content-Security-Policy Header." How does this connect back to Day 16's CSP-as-defense-in-depth discussion?
### Problem 2
Why would an unauthenticated active scan against Juice Shop likely miss most of the IDOR vulnerabilities you found manually on Day 11?
### Problem 3
Compare your Day 30 Semgrep findings and today's ZAP findings for the same codebase/application — do they overlap, or are they largely complementary?
### Challenge
Design a scan configuration strategy (passive-only vs. active, scope restrictions, authentication context) appropriate for scanning a production application safely without causing disruption.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Custom Semgrep Rules (Day 31)
Recall without notes: rule YAML structure and positive/negative test case validation.
### Spaced-Repetition Review
* **Yesterday:** Custom Semgrep rules
* **1 week ago:** Month 1 capstone, part 2

---

# 🧪 6. Active Recall Exercises
1. What is DAST, and how does it differ from SAST?
2. How does passive scanning differ from active scanning?
3. Why does authenticated scanning matter for reaching a realistic attack surface?
4. What would you need to configure authenticated scanning correctly (a dedicated test account + session/auth context configuration)?
5. What's the impact of running an active scan against a production system without authorization?
6. How would DAST findings complement your existing SAST findings from Days 30–31?
7. How would you scope a DAST scan safely against a live production environment?
8. Difference between what SAST catches well and what DAST catches well?
9. Real-world example: how your own manual Week 1–3 testing mirrors what DAST automates.
10. Teach the SAST/DAST complementary relationship to a junior developer in plain language.

### Feynman Test
Explain DAST, and how it complements SAST, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** OWASP ZAP
### Today's Tool Goal
Configure and run both a passive scan and an authenticated active scan.
### Commands / Features to Practice
```text
ZAP: Manual Explore / Automated Scan
ZAP: Context > Authentication > Form-based auth configuration
ZAP: Active Scan > select scope > Start Scan
```
### Tool Success Criteria
I can configure ZAP's authentication context correctly and confirm (via the scan log) that it's genuinely testing authenticated pages.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (ZAP becomes a pipeline gate in Week 6)
Today's contribution: document your ZAP scan configuration (authentication context, scope) as the baseline configuration you'll automate into the Week 6 pipeline.
### Deliverable
`Day 32: Configured and ran authenticated OWASP ZAP scan against Juice Shop; documented scan configuration for Week 6 pipeline integration.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A team runs only unauthenticated DAST scans against their application and reports "no critical findings," but most of the application's functionality requires login.
### My Task
1. Identify the gap (unauthenticated scanning misses the vast majority of real attack surface).
2. Explain the root cause.
3. Determine the false confidence this creates.
4. Propose the fix (proper authenticated scan configuration).
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
* [ ] Study notes  * [ ] Passive scan completed  * [ ] Authenticated active scan completed  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. DAST vs. SAST's complementary coverage
2. Passive vs. active scanning
3. Authenticated scan configuration
4. How your ZAP findings compare to your Semgrep findings for overlap/gaps
5. How tomorrow's Nuclei/CI-integration topic extends today's manual ZAP usage into automation

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** OWASP ZAP against OWASP Juice Shop
**Lab Name:** "Authenticated Active Scan: Full ZAP Configuration"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Configure and execute a complete authenticated active scan and compare its findings against your manual Weeks 1–3 findings.
### Environment
Target: OWASP Juice Shop · Tools: OWASP ZAP · Prerequisite: today's fundamentals
### Lab Tasks
1. Configure a Juice Shop test account authentication context.
2. Set an explicit, safe scan scope.
3. Run the authenticated active scan.
4. Compare ZAP's findings against your own manual findings log from Weeks 1–3.
5. Document overlaps, ZAP-only findings, and manual-only findings.
### What I Need to Discover
What did ZAP find automatically that you also found manually? What did ZAP miss that required your manual creativity? What does this comparison teach you about the right balance between automated and manual testing in a real assessment?
### Lab Success Criteria
Completed authenticated scan with documented comparison against manual findings, and clear reasoning about automated vs. manual testing's respective strengths.

---
---

# 🛡️ Day 33 — DAST Part 2: Nuclei Templates & CI/CD Integration Prep

**Date:** 21/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Nuclei-based scanning and preparing DAST for pipeline automation
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain Nuclei's template-based scanning model and how it differs from ZAP's broader crawl-and-attack approach.
* Run Nuclei against a target using community templates.
* Design a CI/CD-ready DAST configuration (ZAP baseline scan) suitable for automation in Week 6.

### Success Criteria
* Explain Nuclei's YAML template model without notes.
* Run a Nuclei scan and interpret its findings.
* Produce a documented ZAP CI/CD automation configuration (command-line invocation, expected exit codes/thresholds) ready for Week 6's pipeline.

---

# 📚 2. Topics to Study
### Primary Topic
**Nuclei Templates & DAST Automation for CI/CD**
### Secondary Topics
* Nuclei's YAML-based template structure for defining specific, targeted checks
* ZAP's CLI/Docker modes for headless, automatable scanning (as opposed to yesterday's GUI-driven manual scan)
* Setting pass/fail thresholds for a pipeline gate (e.g., "fail the build on any High severity finding")

### Priority
🔴 **Must Know:** Nuclei is template-driven and fast, ideal for checking known, specific vulnerability signatures (CVEs, misconfigurations) at scale, while ZAP is a more general-purpose crawler/scanner better suited for exploring an application's full attack surface
🟡 **Should Know:** how to run ZAP headlessly via Docker/CLI (`zap-baseline.py` or the ZAP Docker automation framework) — required for CI/CD integration, since yesterday's GUI-driven scan can't run inside a pipeline
🟢 **Nice to Know:** Nuclei's massive community template library covering thousands of known CVEs and misconfigurations

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Nuclei's Template Model
* What it is: Nuclei runs lightweight YAML templates, each defining a specific request/response check for a known vulnerability or misconfiguration signature
* Why it matters: this makes Nuclei extremely fast for checking "is this specific known issue present" at scale across many hosts, complementing ZAP's slower, more thorough exploratory crawling
* How it works: a template specifies the request to send and the response pattern that indicates a match (e.g., a specific exposed `.git` directory, a known default-credential login page)
* Real-world example: security teams often run Nuclei broadly across many assets for known-issue sweeps, reserving ZAP's deeper crawling for specific high-value applications

### Concept 2 — Headless DAST for CI/CD
* Definition: running a DAST tool without a GUI, via command line or Docker, producing machine-readable output suitable for pipeline consumption
* Architecture/process: `zap-baseline.py -t https://target -r report.html` runs a quick passive-focused baseline scan headlessly, appropriate for a CI gate where a full active scan would be too slow/risky for every build
* Attack scenario this addresses: manually running yesterday's GUI-driven scan every time code changes doesn't scale — a pipeline needs a fast, automatable, headless equivalent
* Best practices: use a lighter baseline scan for every CI run, and reserve deeper authenticated active scans for scheduled (e.g., nightly/weekly) runs against a staging environment

### Concept 3 — Setting Pipeline Pass/Fail Thresholds
* Key terminology: severity threshold, build gate, exit code
* Practical application: configure the DAST tool to exit with a non-zero code (failing the CI build) if any finding at or above a chosen severity threshold is present
* Common vulnerabilities in threshold design: setting the bar too strict (blocking builds on low-severity noise, recalling Day 24/Day 30's fatigue lessons) or too loose (missing real issues)
* Best practices: start with a threshold that only fails builds on High/Critical findings, and tighten over time as the team's triage capacity matures

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Nuclei template | A lightweight YAML-defined check for a specific known issue | Fast, scalable scanning for known signatures |
| Headless scan | Running a DAST tool via CLI/Docker without a GUI | Required for any CI/CD pipeline integration |
| Severity threshold | The bar at which a finding fails the build | Balances catching real issues against alert/build fatigue |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study Nuclei's template model and ZAP's headless/Docker automation modes.
**Output:** A written comparison of when you'd reach for Nuclei vs. ZAP vs. both together in a real security program.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Install Nuclei and run it with community templates against Juice Shop.
2. Review and document findings.
3. Run ZAP's baseline scan headlessly via Docker/CLI against Juice Shop, producing a report file.
4. Design and document the exact command-line invocation and severity threshold you'll use in Week 6's pipeline.

**Expected Result:** Documented Nuclei scan results + a working headless ZAP baseline scan command + a documented CI threshold policy.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Explain why running a full active ZAP scan on every single CI build (potentially dozens of times per day) is impractical, and what alternative cadence you'd propose.
### Problem 2
Design the exact severity threshold policy you'd use for a pipeline gate — what fails the build, what only warns?
### Problem 3
Compare today's Nuclei findings against yesterday's ZAP findings for the same target — where do they overlap, and where does each tool add unique value?
### Challenge
Draft the complete headless DAST configuration (command, target, threshold, report format) ready to drop directly into Week 6's GitHub Actions pipeline.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: OWASP ZAP Fundamentals (Day 32)
Recall without notes: DAST vs. SAST, passive vs. active scanning, authenticated scan configuration.
### Spaced-Repetition Review
* **Yesterday:** ZAP fundamentals
* **1 week + 2 days ago:** Secure SDLC models — revisit "shift left," since today's CI/CD-ready configuration is the concrete tooling implementation of that principle

---

# 🧪 6. Active Recall Exercises
1. What is Nuclei's template-based scanning model?
2. How does headless/Docker-mode ZAP differ from yesterday's GUI-driven scan?
3. Why is a full active scan on every CI build impractical?
4. What would you need to set an effective severity threshold for a pipeline gate (understanding of your team's triage capacity)?
5. What's the impact of a threshold set too strictly (build fatigue) vs. too loosely (missed real issues)?
6. How would Nuclei and ZAP be used together in a mature security program?
7. How would you decide what cadence (every build vs. nightly vs. weekly) different scan depths should run at?
8. Difference between Nuclei's focus (known signatures) and ZAP's focus (exploratory crawling)?
9. Real-world example: a scenario where Nuclei's speed matters more than ZAP's depth, and vice versa.
10. Teach headless DAST automation to a junior developer in plain language.

### Feynman Test
Explain Nuclei, headless ZAP, and pipeline threshold design together in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Nuclei + ZAP (Docker/headless mode)
### Today's Tool Goal
Run both tools in their fast, automatable modes and produce machine-readable reports.
### Commands / Features to Practice
```text
nuclei -u https://target -t nuclei-templates/
docker run -t zaproxy/zap-stable zap-baseline.py -t https://target -r report.html
```
### Tool Success Criteria
I can run both tools headlessly, without a GUI, and produce a report file suitable for CI consumption.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline
Today's contribution: finalize and document the exact headless ZAP command and severity threshold policy that will become a job step in Week 6's GitHub Actions workflow.
### Deliverable
`Day 33: Documented headless Nuclei and ZAP scan configurations with severity threshold policy, ready for Week 6 pipeline integration.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A team wants DAST in their pipeline but is worried a full active scan will slow every deployment down by 30+ minutes.
### My Task
1. Identify the tension (thoroughness vs. pipeline speed).
2. Explain the tiered-scanning solution (fast baseline on every build, deeper scheduled scan separately).
3. Determine the risk trade-off of this approach.
4. Propose the specific configuration (which scan type, what cadence).
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
* [ ] Study notes  * [ ] Nuclei scan completed  * [ ] Headless ZAP scan completed  * [ ] Threshold policy documented
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Nuclei's template model and when to use it vs. ZAP
2. Headless DAST configuration for CI/CD
3. Severity threshold design principles
4. Your documented pipeline-ready DAST configuration
5. How tomorrow's SCA topic (dependency scanning) is the third pillar alongside SAST and DAST for Week 6's pipeline

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Nuclei + Docker (headless ZAP) against OWASP Juice Shop
**Lab Name:** "Build the Pipeline-Ready DAST Configuration"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Produce a fully working, documented, headless DAST configuration (both Nuclei and ZAP) ready to be dropped directly into a CI/CD pipeline next week.
### Environment
Target: OWASP Juice Shop · Tools: Nuclei, Docker, ZAP · Prerequisite: today's fundamentals
### Lab Tasks
1. Run Nuclei with community templates and save results.
2. Run headless ZAP baseline scan via Docker and save the report.
3. Define and document your severity threshold policy.
4. Write the exact CLI commands (with all flags) you'll paste into Week 6's GitHub Actions YAML.
5. Test that both commands run successfully end-to-end without manual intervention.
### What I Need to Discover
Does your configuration actually run unattended, start to finish, producing a clear pass/fail signal? What would break if this ran inside an ephemeral CI container rather than your local machine — and how would you account for that in Week 6?
### Lab Success Criteria
Both tools run successfully headlessly with documented, reusable commands and a clear, justified severity threshold policy.

---
---

# 🛡️ Day 34 — Software Composition Analysis (SCA): npm audit, Snyk, SBOM Generation

**Date:** 22/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Dependency vulnerability scanning and Software Bill of Materials generation
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain why third-party dependency vulnerabilities are a distinct risk category from the code you write yourself.
* Run `npm audit`, Snyk, and Dependabot-equivalent checks against a real project.
* Generate a Software Bill of Materials (SBOM) using `cdxgen` or `syft`.

### Success Criteria
* Explain SCA's purpose and why it's distinct from SAST/DAST without notes.
* Run and interpret `npm audit` and Snyk scans against a real project.
* Generate a valid SBOM file and explain its structure.

---

# 📚 2. Topics to Study
### Primary Topic
**Software Composition Analysis (SCA) & SBOM Generation**
### Secondary Topics
* Known-CVE dependency scanning (`npm audit`, Snyk)
* Automated dependency update tooling (Dependabot)
* Software Bill of Materials (SBOM) — CycloneDX/SPDX formats

### Priority
🔴 **Must Know:** SCA specifically addresses risk in *third-party* code you didn't write yourself but that runs in your application — a category entirely distinct from SAST (your own code) and DAST (runtime behavior)
🟡 **Should Know:** how `npm audit` matches installed package versions against known-CVE databases
🟢 **Nice to Know:** SBOM's growing regulatory/compliance importance (supply-chain transparency requirements)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Why Dependency Risk Is Distinct
* What it is: modern applications typically include far more third-party dependency code than code the development team wrote itself — every one of those dependencies is a potential vulnerability vector outside your direct control
* Why it matters: Day 18's insecure deserialization discussion already touched this (Apache Commons Collections gadget chains existed in a hugely popular, widely-trusted library) — SCA is the systematic practice of tracking exactly which known-vulnerable dependency versions you're running
* How it works: `npm audit` checks your `package-lock.json` against a vulnerability database and reports known CVEs affecting your exact dependency versions, often with a suggested fix version
* Real-world example: numerous major incidents (Log4Shell being perhaps the most widely recognized example industry-wide) stemmed from a single vulnerable dependency present across enormous numbers of downstream applications
* Common mistake: treating "we didn't write this code" as meaning "it's not our responsibility" — the vulnerability runs in your application regardless of who authored it

### Concept 2 — Automated Dependency Update Tooling
* Definition: Dependabot (and similar tools) automatically opens pull requests updating vulnerable dependencies to patched versions, continuously
* Architecture/process: configured via a repository config file, it periodically checks for known vulnerabilities and available updates, then creates PRs automatically
* Best practices: combine automated PR creation with CI checks (from Days 30–33) so that even automated dependency updates get tested before merging

### Concept 3 — Software Bill of Materials (SBOM)
* Key terminology: SBOM, CycloneDX format, SPDX format
* Practical application: an SBOM is a complete, structured inventory of every component (direct and transitive dependency) in your application, generated with tools like `cdxgen` or `syft`
* Best practices: SBOMs are increasingly required for regulatory/compliance/vendor-assessment purposes — being able to generate one is a directly marketable DevSecOps skill

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| SCA | Scanning third-party dependencies for known vulnerabilities | A distinct risk category from your own code (SAST) or runtime behavior (DAST) |
| Dependabot | Automated dependency-update PR bot | Turns SCA findings into continuous, low-friction remediation |
| SBOM | A complete structured inventory of an application's components | Increasingly required for compliance/supply-chain transparency |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study why dependency risk is distinct, how `npm audit`/Snyk work, and SBOM's purpose and format.
**Output:** A written explanation connecting Day 18's Apache Commons Collections example to why SCA exists as its own discipline.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Run `npm audit` against one of your own past Node.js projects (or Juice Shop's dependency tree).
2. Review findings and identify at least one genuinely fixable vulnerable dependency.
3. Set up a free Snyk account and run a Snyk scan against the same project for comparison.
4. Generate an SBOM using `cdxgen` or `syft` and inspect its structure.

**Expected Result:** Documented `npm audit`/Snyk findings + a valid generated SBOM file.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
`npm audit` reports a "High" severity vulnerability in a transitive dependency (a dependency of a dependency, not one you installed directly). Explain the remediation challenge this creates.
### Problem 2
Explain, using the general Log4Shell-type incident pattern, why SBOMs became critical for organizations trying to determine their exposure quickly during a widespread dependency vulnerability disclosure.
### Problem 3
Compare `npm audit`'s findings against Snyk's findings for the same project — do they differ, and if so, why might that be?
### Challenge
Design a dependency-update policy (cadence, auto-merge criteria, exception process) for a team using Dependabot in production.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Nuclei & CI/CD DAST Prep (Day 33)
Recall without notes: Nuclei's template model and headless ZAP configuration.
### Spaced-Repetition Review
* **Yesterday:** Nuclei & CI/CD DAST prep
* **2 weeks + 2 days ago:** Insecure deserialization (Day 18) — directly relevant to today's dependency-risk discussion

---

# 🧪 6. Active Recall Exercises
1. What is SCA, and how does it differ from SAST/DAST?
2. How does `npm audit` identify known-vulnerable dependencies?
3. Why is dependency risk still "your" responsibility even though you didn't write that code?
4. What would you need to remediate a vulnerable transitive dependency (understanding the dependency tree, sometimes requiring an update to a direct dependency to pull in the fixed transitive version)?
5. What's the impact of a widely-used, critically vulnerable dependency (referencing Log4Shell-type incidents generally)?
6. How would Dependabot's automated PRs fit into your Week 6 pipeline's CI checks?
7. How would you use an SBOM during a rapid incident-response scenario when a new widespread dependency CVE is disclosed?
8. Difference between CycloneDX and SPDX SBOM formats (at a conceptual level — both serve the same core purpose via different schemas)?
9. Real-world example: the general pattern of Log4Shell-type dependency incidents.
10. Teach why SCA matters to a junior developer in plain language.

### Feynman Test
Explain SCA, dependency risk, and SBOMs together in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** `npm audit`, Snyk, `cdxgen`/`syft`
### Today's Tool Goal
Run all three tool categories against a real project and generate a valid SBOM.
### Commands / Features to Practice
```text
npm audit
npm audit fix
snyk test
cdxgen -o sbom.json
```
### Tool Success Criteria
I can run each tool, interpret its output, and generate a valid, well-formed SBOM file.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (SCA becomes the third pipeline gate alongside SAST and DAST)
Today's contribution: document your `npm audit`/Snyk findings and generated SBOM as the SCA component of your emerging pipeline design.
### Deliverable
`Day 34: Ran npm audit and Snyk scans; generated SBOM; documented SCA configuration for Week 6 pipeline integration.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A critical CVE is disclosed in a widely-used logging library used across your company's 40 microservices. Leadership asks: "Are we affected, and where?"
### My Task
1. Identify why this question is hard to answer quickly without SBOMs already in place.
2. Explain how a maintained SBOM inventory would let you answer within minutes rather than days.
3. Determine the business risk of the "days" scenario (extended exposure window).
4. Propose an SBOM-maintenance practice that would prevent this scramble in the future.
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
* [ ] Study notes  * [ ] npm audit / Snyk scans completed  * [ ] SBOM generated  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Project contribution  * [ ] Docs update  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Why dependency/SCA risk is distinct from SAST/DAST
2. How `npm audit`/Snyk identify known-vulnerable dependencies
3. SBOM's purpose and generation process
4. How SCA becomes the third pillar of Week 6's pipeline (alongside SAST and DAST)
5. Tomorrow's lab day, consolidating all three security-testing pillars into one full pass

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Lab (npm audit, Snyk, cdxgen/syft)
**Lab Name:** "Full Dependency Risk Assessment and SBOM Generation"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Produce a complete dependency risk assessment and a valid SBOM for a real project, ready to reference in Week 6's pipeline build.
### Environment
Target: your own past project or Juice Shop · Tools: npm audit, Snyk, cdxgen/syft · Prerequisite: today's fundamentals
### Lab Tasks
1. Run `npm audit` and document all findings by severity.
2. Attempt `npm audit fix` and note what it resolves automatically vs. what requires manual intervention.
3. Run Snyk for comparison.
4. Generate a full SBOM.
5. Document the complete dependency risk picture for this project.
### What I Need to Discover
How many of this project's vulnerabilities came from direct dependencies you chose vs. transitive dependencies you never directly selected? What does that tell you about the real scope of dependency risk in modern development?
### Lab Success Criteria
Complete, documented dependency risk assessment across both tools, plus a valid generated SBOM file.

---
---

# 🛡️ Day 35 — Week 5 Lab Day: Full SAST + DAST + SCA Pass on a Personal Project

**Date:** 23/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Consolidating all three security-testing pillars into one integrated assessment
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Run a complete SAST + DAST + SCA pass against one of your own personal projects, combining everything from Days 29–34.
* Consolidate findings across all three tool categories into one unified report.
* Identify overlaps and unique contributions from each testing category.

### Success Criteria
* All three testing categories (SAST, DAST, SCA) have been run against the same target project.
* A unified findings report exists, clearly attributing each finding to its detection method.
* You can explain which categories found which types of issues, and why.

---

# 📚 2. Topics to Study
### Primary Topic
**Integrated SAST + DAST + SCA Assessment**
### Secondary Topics
* Consolidating findings from heterogeneous tools into one coherent report
* Recognizing detection-method overlap and gaps
* Preparing this integrated approach as the technical foundation for Week 6's automated pipeline

### Priority
🔴 **Must Know:** how to run all three tool categories (Semgrep, ZAP/Nuclei, npm audit/Snyk) against the same target and produce one consolidated view
🟡 **Should Know:** which vulnerability classes each tool category is naturally suited to find, based on this week's concept work
🟢 **Nice to Know:** how real DevSecOps teams typically present multi-tool findings to engineering teams without overwhelming them with tool-specific jargon

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Integrated Assessment Methodology
* What it is: running SAST (source code), DAST (running application), and SCA (dependencies) together against the same target, then consolidating results into a unified view
* Why it matters: this mirrors exactly how a mature security program actually operates — no single tool category provides complete coverage, and this week has systematically built your understanding of why each is necessary
* How it works: for each finding, tag it with its detection method (SAST/DAST/SCA) and its OWASP Top 10 category (recalling Days 26–27's capstone structure) for cross-referencing

### Concept 2 — Recognizing Overlap and Gaps
* Definition: some vulnerabilities may be detectable by more than one method (increasing confidence), while others are uniquely visible to just one
* Practical application: a hardcoded credential might show up in both a SAST scan (pattern match in source) and a DAST scan (if it's actually exploitable at runtime) — while an IDOR is likely DAST/manual-only, and a vulnerable dependency is SCA-only
* Best practices: use this overlap analysis to build intuition about tool selection for future, real engagements with limited time/tooling budgets

### Concept 3 — Preparing for Week 6's Pipeline Build
* Key terminology: integration readiness
* Practical application: today's manual, consolidated run-through is exactly what Week 6 will automate into a single CI/CD pipeline — treat today as a "dry run" of the pipeline's eventual output
* Best practices: keep careful notes on exact commands/configurations used today, since they'll be directly translated into GitHub Actions steps next week

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Integrated assessment | Running SAST + DAST + SCA together against one target | Mirrors real-world mature security program practice |
| Detection-method tagging | Labeling findings by which tool category found them | Builds intuition for tool selection and coverage gaps |
| Pipeline dry run | Today's manual process as a preview of Week 6's automation | Directly informs the pipeline you'll build next week |

---

# ⏱️ 4. Study Schedule

## Session 1 — Setup & Planning (45–60 min)
Choose your target project (ideally a personal project you haven't yet fully tested, for a fresh assessment). Plan the order of operations: SAST first (fastest, no running app needed), then SCA, then DAST (requires the app running).

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Run the Full Integrated Pass (90–120 min)
1. Run Semgrep (Days 30–31) against the project's source code.
2. Run `npm audit`/Snyk (Day 34) against its dependencies.
3. Run the project locally and execute both a Nuclei scan and a ZAP baseline scan (Days 32–33) against it.
4. Consolidate all findings into one unified markdown report, tagged by detection method and OWASP category.

**Expected Result:** A complete, consolidated, multi-tool security assessment report for a real personal project.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems / Reflection (30–45 min)
### Problem 1
Which findings, if any, were caught by more than one tool? What does that overlap tell you about confidence in that specific finding?
### Problem 2
Which finding type (if any) did none of today's automated tools catch that you'd expect a manual reviewer (using your Week 1–3 skills) to find?
### Problem 3
Estimate how long today's manual, sequential process took vs. how you'd expect an automated CI pipeline running the same three tools to take — what's the speed argument for Week 6's automation work?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full Week 5 recall sweep: Secure SDLC models → Semgrep fundamentals → custom rules → ZAP fundamentals → Nuclei/CI-DAST → SCA/SBOM. Answer what/why/how for each from memory.
### Spaced-Repetition Review
* **Yesterday:** SCA & SBOM generation
* **This week:** the full SAST/DAST/SCA arc
* **1 month ago:** SQL injection (a good long-interval check)

---

# 🧪 6. Active Recall Exercises
1. Cold-recall SAST's strengths/limitations and Semgrep's rule model.
2. Cold-recall DAST's strengths/limitations and ZAP/Nuclei's respective roles.
3. Cold-recall SCA's purpose and SBOM's role.
4. Which vulnerability classes from Weeks 1–3 map most naturally to which tool category (SAST/DAST/SCA)?
5. What's the business case for running all three together rather than picking just one?
6. How would you explain this week's full toolkit to an engineering team unfamiliar with security tooling?
7. How would you prioritize which tool to adopt first if a team could only start with one?
8. Difference between what each tool category structurally can and cannot see?
9. Real-world example connecting to this week's Log4Shell-type SCA discussion.
10. Teach the complete SAST+DAST+SCA integrated methodology to a junior developer in under 4 minutes.

### Feynman Test
Explain how SAST, DAST, and SCA complement each other as one integrated assessment strategy, in 5–6 sentences. If unclear, mark 🟡 **Needs Review** before Week 6's pipeline build.

---

# 🛠️ 7. Tool Practice
**Tool:** Full Week 5 toolkit — Semgrep, ZAP, Nuclei, npm audit/Snyk, cdxgen/syft
### Today's Tool Goal
Run all tools in sequence against one target without hesitation, moving fluidly between them.
### Tool Success Criteria
I can execute the complete integrated pass without needing to look up basic command syntax for any of the five tools.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (begins construction next week)
Today's contribution: the consolidated, unified findings report from today's manual dry-run becomes both a standalone portfolio artifact AND the direct blueprint for Week 6's automated pipeline configuration.
### Deliverable
`Day 35: Completed integrated SAST+DAST+SCA assessment on personal project; published consolidated findings report; documented exact tool configurations for Week 6 pipeline automation.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "We're considering adopting SAST, DAST, and SCA tooling but can only start with one this quarter due to budget. Which would you recommend first, and why?"
### My Task
1. Consider the organization's likely risk profile (this requires asking clarifying questions in a real scenario, or reasoning through common cases).
2. Weigh each tool category's setup cost, false-positive burden, and coverage.
3. Make and justify a recommendation.
4. Acknowledge what risk remains uncovered by your recommended starting point.
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
* [ ] All three tool categories run against one target  * [ ] Consolidated unified report published
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow (Start of Week 6)
1. The complete SAST+DAST+SCA integrated methodology
2. Which tools you'll translate directly into Week 6's GitHub Actions pipeline
3. Overlap and gap patterns across the three tool categories
4. Your documented, ready-to-use tool configurations from this week
5. How Week 6's CI/CD security topic will take everything from this week and make it run automatically on every code push

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Personal project (integrated multi-tool assessment)
**Lab Name:** "Week 5 Capstone Dry Run: Full Integrated Security Assessment"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 2+ hours (the bulk of today's schedule)
### Lab Objective
Execute and document a complete SAST+DAST+SCA pass against a real project, producing both a portfolio-quality report and a direct blueprint for Week 6's pipeline automation.
### Environment
Target: your own personal project · Tools: Semgrep, ZAP, Nuclei, npm audit/Snyk, cdxgen/syft · Prerequisite: Days 29–34
### Lab Tasks
1. Run all three tool categories in sequence.
2. Consolidate findings into one unified, tagged report.
3. Identify overlap and gap patterns.
4. Document exact commands/configurations for direct reuse next week.
5. Write a short reflection on manual-process time vs. expected automation speed.
### What I Need to Discover
Having now run this full integrated methodology by hand, do you feel you genuinely understand *why* each tool exists and what unique value it adds — enough to explain and defend the approach in an interview, not just execute it?
### Lab Success Criteria
Complete integrated assessment executed and documented, unified report published, and configurations ready for direct translation into Week 6's automated pipeline.

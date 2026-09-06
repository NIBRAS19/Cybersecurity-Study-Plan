# 🗓️ WEEK 10 — AI Red Teaming & MITRE ATLAS (Days 64–70)

---

# 🛡️ Day 64 — MITRE ATLAS Framework: Adversarial ML Taxonomy

**Date:** 22/10/2026
**Phase:** Month 3 — AI Security & Capstone Portfolio
**Primary Skill:** Understanding MITRE ATLAS as a structured adversarial-ML knowledge base
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand MITRE ATLAS's structure (Tactics and Techniques, directly parallel to MITRE ATT&CK from Day 25) as applied specifically to AI/ML systems.
* Map Week 9's hands-on prompt-injection work onto specific ATLAS technique IDs.
* Understand how ATLAS extends beyond just LLMs to broader adversarial machine learning (e.g., traditional ML model evasion).

### Success Criteria
* Explain ATLAS's Tactic/Technique structure without notes, connecting explicitly to Day 25's ATT&CK foundation.
* Correctly map at least 3 Week 9 techniques (prompt injection, data poisoning) to specific ATLAS technique IDs.
* Explain how ATLAS covers a broader scope than just LLM-specific risks (e.g., traditional ML evasion attacks).

---

# 📚 2. Topics to Study
### Primary Topic
**MITRE ATLAS Framework**
### Secondary Topics
* ATLAS's relationship to MITRE ATT&CK (same organizing philosophy, applied to adversarial ML)
* ATLAS Tactics (the attacker's goal categories) and Techniques (specific methods)
* ATLAS's broader scope: traditional ML evasion/poisoning attacks alongside LLM-specific risks

### Priority
🔴 **Must Know:** ATLAS uses the exact same Tactic/Technique structure and philosophy as MITRE ATT&CK (Day 25) — if you understood ATT&CK's Tactics-are-goals, Techniques-are-methods structure, you already understand ATLAS's structure; the content is what's new, not the framework's shape
🟡 **Should Know:** ATLAS covers a broader scope than just LLM prompt injection — it includes traditional adversarial ML techniques like evasion attacks (crafting inputs that fool a classifier, e.g., an image classifier) and model extraction, which predate the current LLM-focused wave of AI security concern
🟢 **Nice to Know:** ATLAS Navigator (the visualization tool, mirroring ATT&CK Navigator from Day 25) lets you map and visualize technique coverage for a specific AI system

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — ATLAS as ATT&CK's AI-Specific Counterpart
* What it is: MITRE ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems) applies the same Tactic/Technique knowledge-base philosophy from ATT&CK (Day 25) specifically to attacks against AI/ML systems
* Why it matters: recognizing this parallel means you're not learning a brand-new framework structure — you're applying a structure you already know (from Day 25) to new content
* How it works: ATLAS Tactics include things like "Reconnaissance," "ML Model Access," "Initial Access," "ML Attack Staging," and "Exfiltration" — several directly named the same as ATT&CK tactics, others AI-specific
* Real-world example: just as Day 25 had you map Week 1–3 vulnerabilities to ATT&CK technique IDs, today you'll map Week 9's AI-specific work to ATLAS technique IDs

### Concept 2 — Mapping Week 9's Work to ATLAS
* Definition: identifying the specific ATLAS technique ID(s) corresponding to prompt injection, data poisoning, and other Week 9 concepts
* Practical application: prompt injection maps to an ATLAS technique under something like "LLM Prompt Injection"; data poisoning maps to a technique under "Poison Training Data"; this exercise directly parallels Day 25's ATT&CK mapping work
* Best practices: using standardized ATLAS technique IDs in your capstone report (Week 11) demonstrates the same professional, industry-standard communication as your Day 25 ATT&CK mapping did for traditional web vulnerabilities

### Concept 3 — ATLAS's Broader Scope Beyond LLMs
* Key terminology: evasion attack, adversarial example, model extraction
* Practical application: an evasion attack crafts an input (e.g., a subtly perturbed image) specifically designed to cause a traditional ML classifier to misclassify it — a different attack surface than LLM prompt injection, but still within ATLAS's scope
* Best practices: understand that "AI security" is broader than "LLM security" — your curriculum has focused heavily on LLMs (reflecting current industry focus and job market demand), but ATLAS's existence and scope is a reminder that adversarial ML predates and extends beyond the current LLM wave

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| MITRE ATLAS | ATT&CK's structure applied to adversarial ML/AI systems | Directly reuses the Day 25 framework you already understand |
| ATLAS Tactic/Technique | Goal categories and specific methods for attacking AI systems | The standard vocabulary for professional AI red-teaming communication |
| Evasion attack | Crafting input to fool a traditional ML classifier | Reminds you AI security scope extends beyond LLM-specific risks |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study ATLAS's structure using the official MITRE ATLAS website, explicitly comparing it side-by-side with your Day 25 ATT&CK notes.
**Output:** A written comparison table: ATT&CK Tactic examples vs. ATLAS Tactic examples, noting overlaps and AI-specific additions.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Using the ATLAS website/Navigator, look up the specific technique ID(s) for prompt injection (Days 59–60) and data poisoning (Day 61).
2. Map at least 3 more Week 9 concepts to their corresponding ATLAS technique IDs.
3. Research one traditional (non-LLM) ATLAS technique (e.g., an evasion attack against an image classifier) to understand ATLAS's broader scope.

**Expected Result:** A documented mapping table of Week 9 concepts to ATLAS technique IDs + notes on one traditional ML attack technique.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Look up and cite the specific ATLAS technique ID for "LLM Prompt Injection" (or its closest equivalent in the current ATLAS version).
### Problem 2
Explain, using an evasion attack example, how ATLAS's scope extends beyond LLM-specific risks to traditional ML systems.
### Problem 3
Compare the Tactic categories in ATT&CK (Day 25) and ATLAS — which are shared conceptually, and which are AI-specific additions?
### Challenge
Design a simple ATLAS-mapped attack narrative (3–4 techniques in sequence) for a hypothetical attack against an LLM-powered application with tool access, mirroring how you mapped Capital One's attack chain to ATT&CK tactics on Day 25.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Week 9 Consolidation (Day 63)
Recall without notes: the complete OWASP LLM Top 10 and your Gandalf/HackAPrompt experience.
### Spaced-Repetition Review
* **Yesterday:** Week 9 consolidation
* **6 weeks + 2 days ago:** Incident response basics & MITRE ATT&CK (Day 25) — directly reused today

---

# 🧪 6. Active Recall Exercises
1. What is MITRE ATLAS, and how does it relate to MITRE ATT&CK?
2. What are ATLAS Tactics and Techniques?
3. Why does ATLAS's scope extend beyond just LLM-specific risks?
4. What would you need to map a real AI security finding to a specific ATLAS technique ID (familiarity with the ATLAS taxonomy)?
5. What's the value of using standardized ATLAS technique IDs in a professional report?
6. How would you use ATLAS Navigator to visualize technique coverage for a specific AI system assessment?
7. How would you explain the difference between LLM-specific risks (Week 9) and broader adversarial ML risks (evasion attacks) to a colleague?
8. Difference between an ATT&CK technique and an ATLAS technique, structurally?
9. Real-world example: an evasion attack against a traditional ML classifier.
10. Teach MITRE ATLAS to a junior developer, explicitly building on their existing ATT&CK knowledge from Day 25.

### Feynman Test
Explain MITRE ATLAS's structure and scope, and how it relates to ATT&CK, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** MITRE ATLAS website / ATLAS Navigator
### Today's Tool Goal
Look up and correctly cite specific ATLAS technique IDs for concepts you already understand deeply from Week 9.
### Tool Success Criteria
I can accurately look up and cite the correct ATLAS technique ID for at least 3 AI security concepts from Week 9.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #3 — AI Security Capstone
Today's contribution: add ATLAS technique ID mapping to your Week 9 prompt-injection technique log, giving it the same professional, standardized-taxonomy treatment your Week 1–4 findings received with ATT&CK on Day 25.
### Deliverable
`Day 64: Mapped Week 9 prompt-injection and data-poisoning findings to specific MITRE ATLAS technique IDs; researched traditional ML evasion attack example.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "How would you use MITRE ATLAS in a real AI red-teaming engagement?"
### My Task
1. Explain ATLAS's role as a structured technique taxonomy, directly parallel to ATT&CK's role in traditional security engagements.
2. Explain how you'd use it to plan an assessment's scope (which technique categories to test) and to report findings using standardized IDs.
3. Reference your own Week 9 → ATLAS mapping exercise as concrete evidence of this skill.
4. Practice this answer out loud, under 2 minutes.
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
* [ ] Study notes  * [ ] Week 9 concepts mapped to ATLAS technique IDs  * [ ] Traditional ML attack researched
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. MITRE ATLAS's structure and its relationship to ATT&CK
2. Your mapped Week 9 findings with ATLAS technique IDs
3. ATLAS's broader scope beyond LLM-specific risks
4. How to use ATLAS Navigator for coverage visualization
5. How tomorrow's ATLAS Navigator exercise will let you map a complete attack path for a fictional AI product

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** MITRE ATLAS website / Navigator
**Lab Name:** "Map Week 9's Findings to MITRE ATLAS"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Produce a complete, accurate mapping of your Week 9 prompt-injection and data-poisoning work to specific ATLAS technique IDs.
### Environment
Target: MITRE ATLAS website · Tools: browser, your Week 9 technique log · Prerequisite: today's concepts
### Lab Tasks
1. Look up ATLAS technique IDs for prompt injection (direct and indirect).
2. Look up the ATLAS technique ID for data poisoning.
3. Look up at least one additional relevant technique from your Week 9 work.
4. Update your consolidated technique log with the correct ATLAS IDs.
5. Research one traditional (non-LLM) ATLAS technique for scope awareness.
### What I Need to Discover
Does having already done the equivalent exercise with ATT&CK on Day 25 make this ATLAS-mapping exercise feel notably faster and more intuitive? That's a genuine sign of your growing professional fluency with structured threat taxonomies in general, not just one specific framework.
### Lab Success Criteria
A complete, accurate mapping of Week 9 findings to specific ATLAS technique IDs, integrated into your technique log.

---
---

# 🛡️ Day 65 — MITRE ATLAS Navigator: Mapping Attack Paths for a Fictional AI Product

**Date:** 23/10/2026
**Phase:** Month 3 — AI Security & Capstone Portfolio
**Primary Skill:** Using ATLAS Navigator to visualize and reason about a complete attack path
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Design a fictional AI-powered product with realistic architecture (LLM + tools/plugins + data sources).
* Use ATLAS Navigator to map a complete, multi-technique attack path against this fictional product.
* Connect this exercise explicitly to Week 3's STRIDE/attack-tree threat-modeling skills, applied to an AI-specific context.

### Success Criteria
* A designed fictional AI product with clearly documented architecture exists.
* A complete, multi-step ATLAS-mapped attack path against it is documented, using Navigator or equivalent visualization.
* You can explain how this exercise combines Week 3's threat-modeling skill with Week 9–10's AI-specific technique knowledge.

---

# 📚 2. Topics to Study
### Primary Topic
**Applied ATLAS Navigator: Full Attack Path Mapping**
### Secondary Topics
* Designing a realistic (if fictional) AI product architecture as a threat-modeling target
* Chaining multiple ATLAS techniques into a coherent, multi-step attack narrative
* Connecting this exercise to Week 3's STRIDE and attack-tree skills

### Priority
🔴 **Must Know:** a real attack rarely involves just one technique in isolation — chaining techniques (e.g., indirect prompt injection to manipulate an agent, combined with excessive agency to cause real damage via an over-permissioned tool) is how genuine AI security assessments should be reasoned about, directly mirroring how Day 20's attack trees chained traditional web techniques
🟡 **Should Know:** how to use ATLAS Navigator's visualization to lay out a multi-technique path clearly for a report/presentation
🟢 **Nice to Know:** this kind of "fictional product design + attack path mapping" exercise is a common format for both AI red-teaming interview questions and actual pre-engagement scoping documents

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Designing a Realistic Fictional AI Product
* What it is: before you can map an attack path, you need a target — design a plausible AI-powered product (e.g., "an AI-powered internal IT helpdesk assistant with access to a ticketing system and the ability to reset user passwords")
* Why it matters: a realistic architecture (what data it accesses, what tools/actions it can take, who its users are) makes the subsequent attack-path exercise genuinely instructive rather than abstract
* How it works: document the product's purpose, its system prompt's intended behavior, its connected tools/plugins, and its user base — directly recalling Week 3's Day 19 STRIDE exercise's need for a clear system description before analysis

### Concept 2 — Chaining ATLAS Techniques into an Attack Path
* Definition: designing a realistic, multi-step attack narrative where each step uses a specific ATLAS technique, building toward a meaningful final impact
* Practical application: for the IT helpdesk assistant example — Step 1: indirect prompt injection via a malicious "urgent IT request" ticket (ATLAS technique for prompt injection) → Step 2: the injected instructions exploit excessive agency (the assistant has broad password-reset capability) → Step 3: the attacker gains account takeover of a privileged user
* Best practices: this mirrors Day 20's attack-tree exercise exactly, just using ATLAS's AI-specific technique vocabulary instead of traditional web vulnerability classes

### Concept 3 — Connecting to Week 3's Threat-Modeling Foundation
* Key terminology: AI-specific threat modeling
* Practical application: recognize that you're not learning a brand-new skill today — you're applying your existing Week 3 threat-modeling methodology (system description → trust boundaries → systematic threat enumeration → attack path construction) to AI-specific technique content
* Best practices: this recognition itself is valuable to articulate in interviews — "I apply the same threat-modeling discipline to AI systems as I do to traditional web applications, using ATLAS instead of just STRIDE/ATT&CK as my technique vocabulary"

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Fictional product design | A documented, realistic AI system architecture as an analysis target | The necessary foundation for any meaningful attack-path exercise |
| Technique chaining | Combining multiple ATLAS techniques into one attack narrative | Mirrors Day 20's attack-tree methodology with AI-specific content |
| AI-specific threat modeling | Applying Week 3's threat-modeling discipline to AI system architecture | Demonstrates transferable methodology, not just memorized AI facts |

---

# ⏱️ 4. Study Schedule

## Session 1 — Design the Fictional Product (45–60 min)
Design and document a realistic fictional AI-powered product, including its purpose, system prompt intent, connected tools/data sources, and user base.
**Output:** A documented product architecture description.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Map the Attack Path (60–90 min)
1. Identify a realistic attacker goal against your fictional product (e.g., "gain unauthorized administrative access").
2. Design a multi-step attack chain using at least 2–3 distinct ATLAS techniques.
3. Use ATLAS Navigator (or a manual diagram if Navigator access is limited) to visualize this chain.
4. Document the complete attack narrative with technique IDs at each step.

**Expected Result:** A complete, documented, multi-technique ATLAS-mapped attack path against your fictional product.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
For your designed attack path, identify which single mitigation (at which step) would break the entire chain — directly recalling Day 20's "cheapest attacker path" prioritization exercise.
### Problem 2
Redesign your fictional product's architecture with the mitigation from Problem 1 already in place, and confirm your original attack path no longer works.
### Problem 3
Explain, explicitly, how today's exercise combines Week 3's STRIDE/attack-tree methodology with Week 9–10's AI-specific technique knowledge.
### Challenge
Design a second, independent attack path against the same fictional product, using a different combination of ATLAS techniques than your first path.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: MITRE ATLAS Framework (Day 64)
Recall without notes: ATLAS's structure and its relationship to ATT&CK.
### Spaced-Repetition Review
* **Yesterday:** MITRE ATLAS framework
* **7 weeks ago:** Threat modeling — attack trees (Day 20) — directly reused today

---

# 🧪 6. Active Recall Exercises
1. What are the key architectural elements you need to document before mapping an attack path against an AI product?
2. How does chaining multiple ATLAS techniques mirror Day 20's attack-tree methodology?
3. Why does designing a realistic (even if fictional) product matter for this exercise's value?
4. What would you need to identify the "cheapest" or highest-priority mitigation in a multi-step attack chain?
5. What's the impact of the complete attack chain you designed today, in business terms?
6. How would you present this attack-path analysis to a non-technical stakeholder?
7. How would you use this same methodology in a real AI red-teaming engagement?
8. Difference between analyzing a single ATLAS technique in isolation vs. chaining multiple techniques into a realistic attack narrative?
9. Real-world parallel: how does this exercise mirror real pre-engagement scoping for an AI security assessment?
10. Teach your designed attack path to a junior developer, connecting it explicitly to Day 20's attack-tree skill.

### Feynman Test
Explain your complete fictional-product attack path, and how it combines Week 3 and Week 9–10 skills, in 5–6 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** MITRE ATLAS Navigator
### Today's Tool Goal
Use Navigator to visualize a multi-technique attack path clearly.
### Tool Success Criteria
I can produce a clear, presentable visualization of a chained attack path using ATLAS Navigator or an equivalent diagram.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #3 — AI Security Capstone
Today's contribution: your designed fictional product and mapped attack path become a substantial, portfolio-quality exercise demonstrating AI-specific threat-modeling skill, directly building toward your Week 11 capstone's own threat-modeling section.
### Deliverable
`Day 65: Designed fictional AI product architecture; mapped complete multi-technique ATLAS attack path with identified highest-leverage mitigation.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer hands you a one-paragraph description of a real (hypothetical, for interview purposes) AI-powered product and asks you to sketch a likely attack path, live.
### My Task
1. Identify the product's key architectural elements (tools, data access, user base).
2. Identify a realistic attacker goal.
3. Sketch a plausible multi-step attack chain using ATLAS-style technique categories.
4. Identify the highest-leverage mitigation.
5. Practice this live-reasoning exercise, timed.
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
* [ ] Fictional product architecture documented  * [ ] Multi-technique attack path mapped and visualized
* [ ] Highest-leverage mitigation identified  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Your designed fictional product and its complete attack path
2. How this exercise combines Week 3 and Week 9–10 methodologies
3. ATLAS Navigator's use for attack-path visualization
4. Your identified highest-leverage mitigation
5. How tomorrow's AI red-teaming methodology topic formalizes this kind of analysis into a repeatable professional process

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** MITRE ATLAS Navigator
**Lab Name:** "Design and Map: Complete Fictional AI Product Attack Path"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 90–120 minutes (the bulk of today's schedule)
### Lab Objective
Produce a complete, portfolio-quality fictional AI product design and multi-technique ATLAS-mapped attack path.
### Environment
Target: your own designed fictional product · Tools: MITRE ATLAS Navigator, diagramming tools · Prerequisite: Day 64 + Week 3's threat-modeling skills
### Lab Tasks
1. Document the fictional product's architecture.
2. Identify a realistic attacker goal.
3. Design and map a 2–3 step attack chain using specific ATLAS technique IDs.
4. Visualize the chain using Navigator or an equivalent diagram.
5. Identify and document the highest-leverage mitigation.
### What I Need to Discover
Does this exercise feel like a natural extension of Day 20's attack-tree work, just with a new vocabulary — or does something about AI systems' architecture (tool access, natural-language attack surface) require genuinely different threat-modeling instincts than traditional web applications?
### Lab Success Criteria
A complete, documented fictional product design with a mapped, visualized, multi-technique attack path and identified mitigation.

---
---

# 🛡️ Day 66 — AI Red Teaming Methodology: Systematic Guardrail Testing

**Date:** 24/10/2026
**Phase:** Month 3 — AI Security & Capstone Portfolio
**Primary Skill:** Formal AI red-teaming methodology and systematic guardrail testing
**Estimated Total Time:** 3–4 hours
**Difficulty:** Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand AI red-teaming as a formal, systematic discipline (not just ad-hoc prompt-injection attempts, as practiced more casually in Week 9).
* Design a structured red-teaming test plan covering multiple ATLAS/OWASP LLM Top 10 categories systematically.
* Understand the concept of "guardrails" and how to systematically test their coverage and gaps.

### Success Criteria
* Explain the difference between casual prompt-injection experimentation (Week 9) and formal AI red-teaming methodology (today) without notes.
* Produce a structured test plan covering multiple risk categories with defined test cases per category.
* Explain what a "guardrail" is and how to systematically probe for coverage gaps.

---

# 📚 2. Topics to Study
### Primary Topic
**Formal AI Red-Teaming Methodology**
### Secondary Topics
* The distinction between ad-hoc technique testing (Week 9) and systematic, planned red-teaming (today)
* Guardrails: input filters, output filters, system-prompt constraints, and their respective coverage gaps
* Structuring a red-teaming test plan: scope, categories, test cases, success criteria, documentation format

### Priority
🔴 **Must Know:** professional AI red-teaming, like professional penetration testing (recall Day 26–27's Month 1 capstone methodology), requires a systematic test plan covering defined categories — not just "trying things until something works," which is valuable for learning (Week 9) but insufficient for a professional engagement
🟡 **Should Know:** a "guardrail" can exist at multiple layers (input filtering before the LLM sees a request, the system prompt itself, output filtering after the LLM generates a response) — a thorough red-team test plan should probe each layer distinctly, since a bypass at one layer doesn't mean the others are also compromised
🟢 **Nice to Know:** real AI red-teaming engagements often combine automated testing (tools that systematically try known technique variations) with manual, creative human testing — similar to the SAST+DAST+manual-testing balance from Month 1–2

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — From Ad-Hoc Testing to Formal Methodology
* What it is: Week 9's Gandalf/HackAPrompt practice was valuable, hands-on technique-building, but a professional red-teaming engagement requires a defined scope, systematic category coverage, and reproducible documentation — directly mirroring the shift from Week 1's exploratory testing to Days 26–27's systematic Month 1 capstone methodology
* Why it matters: this is the same "checklist-driven, risk-prioritized testing" lesson from Day 26, now applied to AI systems specifically
* How it works: define the categories to test (from OWASP LLM Top 10 and/or ATLAS tactics), design specific test cases per category, execute systematically, and document results consistently — exactly the same structure as your Month 1 capstone

### Concept 2 — Guardrails and Layered Testing
* Definition: a guardrail is any control designed to prevent an AI system from producing undesired behavior — this can be an input filter (blocking known-malicious phrasings before they reach the model), the system prompt's own instructions, or an output filter (checking generated content before it's shown to the user or acted upon)
* Architecture/process: a thorough red-team plan tests each layer distinctly — does an input filter catch obvious injection attempts? If bypassed, does the system prompt's own resistance hold? If that fails too, does an output filter catch the resulting bad content before it causes harm?
* Attack scenario: a red-teamer who only tests "can I get the final output to be bad" without understanding which specific layer failed provides much less actionable feedback than one who identifies exactly where in the defense stack the failure occurred

### Concept 3 — Structuring a Red-Teaming Test Plan
* Key terminology: scope, test case, success criteria, documentation format
* Practical application: for each OWASP LLM Top 10 / ATLAS category in scope, define specific test cases (e.g., for Prompt Injection: 3 instruction-override variants, 2 role-play variants, 1 indirect-injection scenario), expected safe behavior, and how you'll document actual results
* Best practices: this test plan structure directly mirrors your Month 1 capstone's OWASP Top 10 checklist approach (Day 26) — recognize this as the same professional discipline applied to a new domain

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Formal red-teaming methodology | Systematic, planned testing vs. ad-hoc technique attempts | The professional-grade version of Week 9's more exploratory practice |
| Guardrail | Any control preventing undesired AI behavior (input/prompt/output layer) | Testing each layer distinctly provides more actionable findings |
| Red-teaming test plan | A structured document defining scope, test cases, and documentation format | Directly mirrors Day 26's Month 1 capstone checklist methodology |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study formal AI red-teaming methodology principles and the guardrail-layer concept.
**Output:** Write, explicitly, how today's methodology directly parallels Day 26's Month 1 capstone approach — checklist-driven, category-based, systematically documented.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Design a complete red-teaming test plan covering at least 4 OWASP LLM Top 10 / ATLAS categories (e.g., Prompt Injection, Insecure Output Handling, Excessive Agency, Sensitive Information Disclosure), with 2–3 specific test cases per category.
2. If you have LLM API access, execute several of these test cases against a system prompt you define, specifically noting which guardrail layer (input/prompt/output) each test probes.
3. Document results in a structured, consistent format.

**Expected Result:** A complete, documented red-teaming test plan with executed test cases and layer-specific findings.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Design 2 test cases specifically targeting Excessive Agency (LLM08) for a hypothetical AI agent with tool access.
### Problem 2
Explain why testing "does the final output look bad" alone is less valuable than identifying exactly which guardrail layer failed.
### Problem 3
Compare today's test-plan structure to Day 26's Month 1 capstone checklist — what's the same, what's different given AI systems' unique architecture?
### Challenge
Design a complete, presentable red-teaming test plan document (scope, categories, test cases, documentation format) ready to apply to your eventual Week 11 capstone target.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: ATLAS Navigator Attack Path Mapping (Day 65)
Recall without notes: your fictional product design and mapped attack chain.
### Spaced-Repetition Review
* **Yesterday:** ATLAS Navigator attack path mapping
* **9 weeks + 3 days ago:** Month 1 Capstone, Part 1 (Day 26) — directly reused today

---

# 🧪 6. Active Recall Exercises
1. What's the difference between ad-hoc prompt-injection testing (Week 9) and formal AI red-teaming methodology (today)?
2. What is a guardrail, and what are its typical layers?
3. Why does layer-specific testing provide more actionable findings than "does the output look bad" testing alone?
4. What would you need to design a complete red-teaming test plan (defined scope, categories, specific test cases)?
5. What's the impact of only testing the final output without understanding which layer failed?
6. How would you structure documentation for a red-teaming engagement's findings?
7. How would you present layer-specific findings to a development team for remediation?
8. Difference between input-layer, prompt-layer, and output-layer guardrails?
9. Real-world parallel: how does this test-plan structure mirror Day 26's Month 1 capstone checklist?
10. Teach formal AI red-teaming methodology to a junior developer, explicitly connecting to Day 26's capstone methodology.

### Feynman Test
Explain formal AI red-teaming methodology, guardrail layers, and their connection to Day 26's Month 1 capstone approach, in 5–6 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** LLM API (systematic, layer-aware testing)
### Today's Tool Goal
Execute test cases with explicit attention to which guardrail layer is being probed.
### Tool Success Criteria
I can articulate, for each test case executed, specifically which guardrail layer (input/prompt/output) it targets and what the result reveals about that layer's effectiveness.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #3 — AI Security Capstone
Today's contribution: your complete red-teaming test plan template is now ready for direct application to your Week 11 capstone target — this is a major building block toward your final AI security assessment methodology.
### Deliverable
`Day 66: Designed complete AI red-teaming test plan (4+ categories, layer-specific test cases); executed and documented initial test results.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A company asks you to red-team their new AI-powered internal tool before launch, giving you two weeks and access to a staging environment.
### My Task
1. Define your engagement scope (which OWASP LLM Top 10 / ATLAS categories to prioritize given the tool's specific architecture).
2. Design your test plan structure.
3. Explain how you'd allocate your two weeks across categories (recalling Day 26's time-boxing lesson).
4. Explain how you'd structure your final report for both technical and non-technical stakeholders.
5. Document the result.
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
* [ ] Study notes  * [ ] Complete red-teaming test plan designed  * [ ] Test cases executed and documented
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Formal AI red-teaming methodology vs. ad-hoc testing
2. Guardrail layers and layer-specific testing
3. Your complete, reusable red-teaming test plan template
4. How this connects to Day 26's Month 1 capstone methodology
5. How tomorrow's evaluation-frameworks topic extends today's test-plan design with formal scoring/rubric approaches

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** LLM API + self-designed test plan
**Lab Name:** "Execute a Formal, Layer-Aware Red-Teaming Test Plan"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 90 minutes
### Lab Objective
Execute your designed test plan against a real (self-defined) system prompt, documenting layer-specific findings systematically.
### Environment
Target: an LLM API + your own system prompt · Tools: your test plan document · Prerequisite: today's concepts
### Lab Tasks
1. Execute at least 8 test cases across your 4+ categories.
2. For each, document which layer it probed and the result.
3. Identify any category where your system prompt's defenses were weakest.
4. Propose a specific improvement.
5. Re-test to confirm the improvement's effect.
### What I Need to Discover
Does systematic, layer-aware testing reveal different or more actionable insights than your more casual Week 9 experimentation did? This is the practical payoff of "methodology" over "ad-hoc exploration" made concrete.
### Lab Success Criteria
A complete, executed, documented test plan with layer-specific findings and at least one demonstrated improvement based on results.

---
---

# 🛡️ Day 67 — AI Red Teaming: Evaluation Frameworks & Red Team Playbooks

**Date:** 25/10/2026
**Phase:** Month 3 — AI Security & Capstone Portfolio
**Primary Skill:** Formal evaluation scoring and red-team playbook design
**Estimated Total Time:** 3–4 hours
**Difficulty:** Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Design a formal evaluation/scoring rubric for AI red-teaming test results (beyond simple pass/fail).
* Design a reusable "red-team playbook" — a repeatable procedure for testing a new AI system, directly analogous to Day 25's IR playbook concept.
* Understand how evaluation frameworks like Microsoft's Counterfit or PyRIT approach automated/semi-automated AI red-teaming.

### Success Criteria
* Produce a scoring rubric with defined severity/confidence levels for AI red-teaming findings.
* Produce a complete, reusable red-team playbook document.
* Explain, conceptually, how a tool like PyRIT or Counterfit automates parts of the red-teaming process you've been doing manually.

---

# 📚 2. Topics to Study
### Primary Topic
**AI Red-Teaming Evaluation Frameworks & Playbooks**
### Secondary Topics
* Scoring AI red-teaming findings: severity, confidence, reproducibility
* Red-team playbooks as reusable, repeatable testing procedures
* Automated/semi-automated red-teaming tools (Microsoft Counterfit, PyRIT) at a conceptual level

### Priority
🔴 **Must Know:** a red-teaming finding needs the same rigor as any other security finding you've documented throughout this curriculum — severity, reproducibility, and confidence level all matter, and "the model said something weird once" is very different from "this technique reliably succeeds across N attempts"
🟡 **Should Know:** a red-team playbook (directly analogous to Day 25's IR playbook) captures a repeatable procedure so that testing a new AI system doesn't require reinventing your methodology from scratch each time
🟢 **Nice to Know:** tools like PyRIT (Microsoft's Python Risk Identification Toolkit) and Counterfit provide semi-automated frameworks for running large batches of known technique variations against a target system, complementing manual creative testing much like DAST tools complement manual web-app testing

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Scoring AI Red-Teaming Findings
* What it is: a rubric assigning severity (how bad is the potential impact if this succeeds), confidence (how reliably does this technique succeed — one lucky attempt vs. consistent reproduction across many attempts), and category (which OWASP LLM Top 10 / ATLAS technique) to each finding
* Why it matters: without reproducibility tracking specifically, a red-teaming report risks either overstating a fluke as a systemic issue or understating a genuinely reliable bypass — LLMs' non-deterministic outputs make this dimension particularly important compared to traditional deterministic vulnerability testing
* How it works: run each test case multiple times (e.g., 5–10 attempts) and report the success rate, not just a single pass/fail — this is a genuinely distinct methodological consideration compared to traditional appsec testing, where a SQLi payload either works or doesn't, consistently

### Concept 2 — Red-Team Playbooks (Recalling Day 25's IR Playbook)
* Definition: a documented, reusable procedure specifying exactly what categories to test, what test cases to run, how to score results, and how to report findings for any new AI system you're asked to assess
* Architecture/process: directly parallel to Day 25's IR playbook structure (identification → containment → eradication → recovery → lessons learned), a red-team playbook might structure as: scope definition → category selection → test case execution (with repetition for reproducibility) → scoring → reporting
* Best practices: building this playbook now, based on Days 64–66's work, means you'll have a genuinely reusable asset for your Week 11 capstone and future professional work, not a one-off exercise

### Concept 3 — Automated/Semi-Automated Tools (Counterfit, PyRIT)
* Key terminology: automated technique libraries, batch testing
* Practical application: tools like PyRIT maintain libraries of known injection techniques and can run them systematically against a target, generating a large volume of test results far faster than manual testing alone — conceptually similar to how Nuclei (Day 33) automates known-signature checking at scale, complementing your manual ZAP-style exploratory testing
* Best practices: understand these tools conceptually for now; hands-on tool-specific practice can follow as you continue developing in this space beyond the current curriculum

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Reproducibility scoring | Tracking success rate across repeated attempts, not just one instance | Critical given LLMs' non-deterministic behavior, unlike traditional deterministic vulnerabilities |
| Red-team playbook | A reusable, repeatable testing procedure for AI systems | Directly analogous to Day 25's IR playbook; prevents reinventing methodology each time |
| PyRIT / Counterfit | Semi-automated AI red-teaming tools | Conceptually parallel to Nuclei's automated-signature role alongside manual DAST testing |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study evaluation/scoring principles for AI red-teaming and the playbook concept, explicitly recalling Day 25's IR playbook structure.
**Output:** Write, explicitly, why reproducibility scoring matters uniquely for AI red-teaming in a way it didn't for your Month 1 traditional web vulnerability testing.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Design a complete scoring rubric (severity × confidence/reproducibility × category).
2. Take 2–3 test cases from Day 66's test plan and re-run each multiple times (if you have API access), recording the success rate to demonstrate the reproducibility-scoring concept concretely.
3. Draft a complete, reusable red-team playbook document, structured in phases like Day 25's IR playbook.

**Expected Result:** A documented scoring rubric + reproducibility data for at least 2 test cases + a complete red-team playbook document.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A prompt-injection technique succeeded once in 10 attempts. How should this be scored/reported compared to a technique that succeeded 9 times in 10?
### Problem 2
Draft the "scope definition" phase of your red-team playbook — what questions would you ask before beginning any new AI system assessment?
### Problem 3
Explain, conceptually, how a tool like PyRIT could speed up the reproducibility-testing exercise from Session 2.
### Challenge
Finalize your complete red-team playbook document, ready to apply directly to your Week 11 capstone target system.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: AI Red-Teaming Methodology (Day 66)
Recall without notes: formal methodology vs. ad-hoc testing, and guardrail layers.
### Spaced-Repetition Review
* **Yesterday:** AI red-teaming methodology
* **6 weeks ago:** Incident response basics & IR playbooks (Day 25) — directly reused today

---

# 🧪 6. Active Recall Exercises
1. What dimensions should a red-teaming finding's scoring rubric include (severity, confidence/reproducibility, category)?
2. Why does reproducibility matter uniquely for AI red-teaming compared to traditional vulnerability testing?
3. What is a red-team playbook, and how does it parallel Day 25's IR playbook?
4. What would you need to build a genuinely reusable red-team playbook (a defined, phase-based structure)?
5. What's the impact of reporting a one-off fluke as if it were a reliably reproducible finding?
6. How would automated tools like PyRIT complement your manual red-teaming approach?
7. How would you structure a red-teaming report so both technical and non-technical stakeholders understand severity/confidence?
8. Difference between severity and confidence/reproducibility as separate scoring dimensions?
9. Real-world parallel: how does this playbook structure mirror Day 25's IR playbook phases?
10. Teach AI red-teaming evaluation and playbook design to a junior developer, explicitly connecting to Day 25's IR playbook concept.

### Feynman Test
Explain your scoring rubric and red-team playbook, and their connection to Day 25's IR playbook, in 5–6 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** LLM API (repeated testing for reproducibility data)
### Today's Tool Goal
Run repeated trials of the same test case and calculate a success rate.
### Tool Success Criteria
I have concrete reproducibility data (e.g., "6/10 attempts succeeded") for at least 2 test cases, not just single pass/fail results.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #3 — AI Security Capstone
Today's contribution: your complete scoring rubric and red-team playbook are now finalized, reusable assets ready for direct application to your Week 11 capstone target.
### Deliverable
`Day 67: Designed AI red-teaming scoring rubric (severity x confidence/reproducibility); documented reproducibility data for test cases; finalized reusable red-team playbook.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "How would you determine whether a prompt-injection finding is actually a serious risk worth fixing immediately, versus a minor edge case?"
### My Task
1. Explain your severity × confidence/reproducibility scoring approach.
2. Explain why running multiple trials matters, given LLM non-determinism.
3. Give a concrete example (from your Session 2 work) of how reproducibility data changed your assessment of a finding's priority.
4. Practice this answer out loud, under 2 minutes.
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
* [ ] Study notes  * [ ] Scoring rubric designed  * [ ] Reproducibility data collected for 2+ test cases
* [ ] Complete red-team playbook finalized  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Your complete AI red-teaming scoring rubric
2. Reproducibility's unique importance for AI systems
3. Your finalized, reusable red-team playbook
4. How PyRIT/Counterfit conceptually automate parts of this process
5. How tomorrow's RAG-attacks topic introduces a new, architecturally distinct AI attack surface (retrieval-augmented generation) to add to your red-teaming scope

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** LLM API + self-designed rubric/playbook
**Lab Name:** "Reproducibility Testing and Playbook Finalization"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Produce concrete, multi-trial reproducibility data and a complete, presentable red-team playbook document.
### Environment
Target: LLM API + your own test cases · Tools: your scoring rubric, playbook draft · Prerequisite: today's concepts
### Lab Tasks
1. Select 3 test cases from your existing technique log.
2. Run each 5–10 times, recording success/failure per attempt.
3. Calculate and document the success rate for each.
4. Score each using your severity × confidence rubric.
5. Finalize your red-team playbook document with all phases complete.
### What I Need to Discover
How much did your assessment of any given technique's "seriousness" change once you had actual reproducibility data, compared to your gut sense from Week 9's more casual, single-attempt testing?
### Lab Success Criteria
Concrete reproducibility data for 3 test cases, correctly scored, and a complete, finalized red-team playbook document.

---
---

# 🛡️ Day 68 — RAG Attacks Part 1: Data Exfiltration Through Retrieval

**Date:** 26/10/2026
**Phase:** Month 3 — AI Security & Capstone Portfolio
**Primary Skill:** Understanding Retrieval-Augmented Generation architecture and its specific attack surface
**Estimated Total Time:** 3–4 hours
**Difficulty:** Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand RAG (Retrieval-Augmented Generation) architecture: how it combines a knowledge base/vector store with an LLM to ground responses in specific data.
* Understand how RAG introduces a new attack surface distinct from a standalone LLM: the retrieval/knowledge-base layer itself.
* Understand data exfiltration risks specific to RAG systems (extracting content from the knowledge base the system wasn't meant to reveal).

### Success Criteria
* Explain RAG architecture (retrieval + generation) without notes.
* Explain why RAG introduces attack surface beyond standard prompt injection (the knowledge base itself becomes a target).
* Design a data-exfiltration attack scenario specific to a RAG system.

---

# 📚 2. Topics to Study
### Primary Topic
**RAG Architecture and Data Exfiltration Risks**
### Secondary Topics
* How RAG works: query → retrieve relevant documents from a vector store → augment the LLM's prompt with retrieved content → generate a grounded response
* Why access-control gaps in the retrieval layer can leak content the LLM's system prompt never intended to expose
* Cross-tenant/cross-document leakage risk in multi-tenant RAG systems

### Priority
🔴 **Must Know:** RAG systems have an attack surface that pure LLM prompt injection (Week 9) doesn't fully cover — the retrieval/vector-store layer itself can have access-control gaps, directly analogous to Day 11's IDOR lesson but applied to a document/vector retrieval system instead of a traditional database
🟡 **Should Know:** if a RAG system's retrieval layer doesn't properly scope which documents a given user/query is allowed to retrieve from, an attacker might craft queries specifically designed to retrieve and have the LLM reveal content from documents outside their authorized scope
🟢 **Nice to Know:** this connects directly to Day 60's indirect prompt injection concept — if the retrieved documents themselves can contain attacker-planted content, RAG systems combine both a data-access risk AND an indirect-injection risk

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — RAG Architecture Fundamentals
* What it is: RAG combines a retrieval system (typically a vector database storing document embeddings) with an LLM — instead of relying purely on the LLM's training data, a RAG system retrieves relevant documents/passages at query time and includes them in the LLM's context, letting the model "ground" its response in specific, current, or proprietary data
* Why it matters: RAG has become an extremely common architecture pattern for enterprise AI applications (e.g., "chat with your company's documents") specifically because it lets organizations use an LLM's language capabilities while grounding responses in their own controlled data — but this architecture introduces new components (the vector store, the retrieval logic) that themselves need security consideration
* How it works: user query → embedding generated for the query → vector similarity search against the document store → top-N most relevant document chunks retrieved → these chunks inserted into the LLM's prompt alongside the user's query → LLM generates a response using both its training and the retrieved context

### Concept 2 — Access-Control Gaps in Retrieval (The IDOR Parallel)
* Definition: if the retrieval layer doesn't properly enforce which documents a specific user/query is authorized to access, an attacker can potentially retrieve (and have the LLM summarize/reveal) content they shouldn't have access to
* Architecture/process: this is directly analogous to Day 11's IDOR — just as an API endpoint must check "does this user own/have access to this specific record" before returning data, a RAG retrieval layer must check "is this specific document/chunk within this user's authorized scope" before including it in the LLM's context
* Attack scenario: in a multi-tenant RAG system (e.g., a SaaS product where each customer's documents should be isolated), a retrieval-layer bug allowing cross-tenant document retrieval would let one customer's queries surface another customer's confidential data

### Concept 3 — Combining RAG Risk with Indirect Injection (Day 60)
* Key terminology: poisoned knowledge base
* Practical application: if an attacker can get malicious content into the document store that RAG retrieves from (e.g., by submitting a support ticket, uploading a document, or any other user-contributed content path that ends up in the vector store), this becomes a combined data-poisoning-meets-indirect-injection risk — the malicious content might not just misinform other users, but could contain embedded instructions manipulating the LLM's behavior when that chunk is retrieved and included in someone else's query context
* Best practices: apply the exact same access-control rigor (Day 11's ownership checks) to the retrieval layer as you would to any traditional data-access API, and treat any user-contributed content that might end up in the retrieval corpus with the same suspicion as any other untrusted input (Day 60's lesson)

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| RAG (Retrieval-Augmented Generation) | Combining a document retrieval system with an LLM for grounded responses | A very common enterprise AI architecture pattern with its own distinct attack surface |
| Retrieval-layer access control | Ensuring queries only retrieve documents the requester is authorized for | Directly analogous to Day 11's IDOR lesson, applied to vector/document retrieval |
| Poisoned knowledge base | Malicious content injected into the retrieval corpus | Combines Day 61's data-poisoning risk with Day 60's indirect-injection risk |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study RAG architecture in depth and its distinct attack surface (retrieval-layer access control, poisoned knowledge base risk).
**Output:** Write, explicitly, the parallel between Day 11's IDOR lesson and RAG's retrieval-layer access-control risk, in your own words.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Design a hypothetical multi-tenant RAG system (e.g., "a SaaS helpdesk product where each customer's support documents are stored and retrieved separately").
2. Design a specific attack scenario exploiting a retrieval-layer access-control gap to achieve cross-tenant data exfiltration.
3. Design a second attack scenario combining a poisoned knowledge base (Day 61) with indirect injection (Day 60) in this same RAG system.

**Expected Result:** Two fully documented RAG-specific attack scenarios, each connecting explicitly to earlier curriculum concepts.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A RAG system's retrieval query doesn't filter by tenant/customer ID before performing vector similarity search. Explain exactly how this creates cross-tenant data exfiltration risk.
### Problem 2
Design the access-control fix for Problem 1, directly modeled on Day 11's ownership-check pattern.
### Problem 3
Explain how a malicious document uploaded to a shared knowledge base could combine data poisoning and indirect injection into one attack.
### Challenge
Design a complete RAG security checklist covering retrieval-layer access control, knowledge-base content vetting, and output handling — ready to apply to your Day 69 hands-on RAG-building exercise.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Evaluation Frameworks & Playbooks (Day 67)
Recall without notes: your scoring rubric and red-team playbook.
### Spaced-Repetition Review
* **Yesterday:** Evaluation frameworks & playbooks
* **9 weeks + 3 days ago:** Broken Access Control / IDOR (Day 11) — directly reused today

---

# 🧪 6. Active Recall Exercises
1. What is RAG, architecturally?
2. How does RAG's retrieval layer create attack surface beyond a standalone LLM?
3. How does the IDOR parallel apply to RAG retrieval-layer access control?
4. What would an attacker need to exploit a retrieval-layer access-control gap (queries that aren't properly scoped by tenant/user)?
5. What's the impact of cross-tenant data exfiltration in a multi-tenant RAG SaaS product?
6. How would you detect a poisoned knowledge base (content auditing, anomaly detection in retrieved-content patterns)?
7. How would you fix a retrieval-layer access-control gap, directly applying Day 11's pattern?
8. Difference between a standard prompt-injection risk and a RAG-specific data-exfiltration risk?
9. Real-world example: a plausible enterprise RAG deployment where this risk would matter significantly.
10. Teach RAG architecture and its attack surface to a junior developer, explicitly connecting to Day 11's IDOR lesson.

### Feynman Test
Explain RAG architecture and its distinct attack surface, connecting explicitly to Day 11's IDOR lesson, in 5–6 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Design/documentation tools (hands-on RAG building begins tomorrow)
### Today's Tool Goal
N/A — conceptual depth day preparing for tomorrow's hands-on build.
### Tool Success Criteria
N/A

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #3 — AI Security Capstone
Today's contribution: your two documented RAG attack scenarios and complete RAG security checklist directly prepare you for tomorrow's hands-on RAG-building-and-exploiting exercise, and add a substantial new risk category to your capstone's eventual scope.
### Deliverable
`Day 68: Documented RAG architecture and two attack scenarios (retrieval access-control gap, poisoned knowledge base); designed complete RAG security checklist.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A legal-tech startup builds a RAG-based product letting law firms "chat with their case documents." Each law firm's documents are stored in a shared vector database, distinguished only by a `firm_id` field that the retrieval query is supposed to filter on.
### My Task
1. Identify the vulnerability/threat (a retrieval-layer access-control gap, directly recalling Day 11).
2. Explain the root cause (likely: the filter is applied inconsistently, or can be bypassed via query manipulation).
3. Determine the impact given the extreme sensitivity of legal case documents specifically.
4. Recommend a mitigation (enforce tenant filtering at the database/vector-store query level, not just in application logic, with defense-in-depth).
5. Document the result.
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
* [ ] Study notes  * [ ] Two RAG attack scenarios documented  * [ ] RAG security checklist designed
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. RAG architecture and its distinct attack surface
2. The IDOR parallel for retrieval-layer access control
3. Poisoned knowledge base risk combining data poisoning and indirect injection
4. Your complete RAG security checklist
5. How tomorrow's hands-on RAG-building exercise will let you test these concepts against a system you build yourself

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Self-directed design exercise
**Lab Name:** "Design RAG Attack Scenarios and Security Checklist"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60 minutes (folded into Session 2/3 above)
### Lab Objective
Produce complete, well-reasoned RAG-specific attack scenarios and a security checklist, directly preparing for tomorrow's hands-on build.
### Environment
Target: a hypothetical multi-tenant RAG system · Tools: none, just design/documentation
### Lab Tasks
1. Design the hypothetical system's architecture.
2. Design the retrieval-layer access-control attack scenario.
3. Design the poisoned-knowledge-base attack scenario.
4. Write the complete RAG security checklist.
5. Prepare specific test ideas to try against tomorrow's hands-on-built RAG app.
### What I Need to Discover
Does recognizing RAG's retrieval-layer risk as "IDOR, but for documents/vectors instead of database rows" make this new architecture pattern feel immediately more approachable, using knowledge you already deeply possess from Day 11?
### Lab Success Criteria
Two complete, well-reasoned attack scenarios and a comprehensive RAG security checklist ready for tomorrow's hands-on application.

---
---

# 🛡️ Day 69 — RAG Attacks Part 2: Build a Simple RAG App, Then Try to Exploit It

**Date:** 27/10/2026
**Phase:** Month 3 — AI Security & Capstone Portfolio
**Primary Skill:** Building a working RAG application and testing it against your own attack scenarios
**Estimated Total Time:** 3–4 hours
**Difficulty:** Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Build a simple, working RAG application (a small document set + vector store + LLM integration).
* Attempt to exploit it using yesterday's designed attack scenarios (retrieval access-control gap, poisoned knowledge base).
* Document what worked, what didn't, and why — building genuine hands-on RAG security experience.

### Success Criteria
* A working, minimal RAG application exists (even if simple — e.g., a handful of test documents with basic Q&A capability).
* At least one of yesterday's attack scenarios is genuinely attempted against your own built system.
* Results (successful or not) are documented with clear reasoning.

---

# 📚 2. Topics to Study
### Primary Topic
**Building and Testing a RAG Application**
### Secondary Topics
* Minimal RAG implementation (a simple vector store, embedding generation, retrieval, LLM integration)
* Deliberately building both a vulnerable version (no proper access control) and testing your Day 68 scenarios against it
* The value of "build it yourself, then break it" for genuine understanding (recalling this pattern from Day 3's NoSQL injection lab)

### Priority
🔴 **Must Know:** building even a minimal, simplified RAG system yourself — rather than only ever reading about RAG architecture — transforms abstract understanding into concrete, testable knowledge, exactly as Day 3's "build the vulnerable app, then exploit it" pattern did for NoSQL injection
🟡 **Should Know:** you don't need a production-grade, fully-featured RAG system for this exercise — a simple local implementation (a handful of text documents, a basic embedding/similarity search, and an LLM call including retrieved content) is sufficient to test the core concepts
🟢 **Nice to Know:** many lightweight libraries/frameworks exist for quickly prototyping RAG systems, though for genuine learning value, understanding what's happening at each step matters more than which specific library you use

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Building a Minimal RAG System
* What it is: for learning purposes, a RAG system can be built quite simply — a small set of text documents, a basic method for finding the most relevant document(s) for a given query (even simple keyword/embedding-based similarity search), and an LLM call that includes the retrieved content in its prompt
* Why it matters: building this yourself, even in simplified form, means you genuinely understand each component (rather than treating "RAG" as an abstract black box), directly recalling the value of Day 3's hands-on build-then-exploit approach
* How it works: (1) prepare a small set of test documents, some marked as belonging to different simulated "tenants"; (2) implement basic retrieval (even simple text search is fine for this learning exercise, though embedding-based similarity is more realistic); (3) build a simple query interface that retrieves relevant documents and includes them in an LLM prompt

### Concept 2 — Testing Your Own Attack Scenarios
* Definition: deliberately building a version with a retrieval-layer access-control gap (no tenant filtering) first, confirming the Day 68 attack scenario works, then adding proper access control and confirming it's fixed — directly mirroring Day 3's vulnerable-then-fixed NoSQL injection pattern
* Practical application: attempt cross-tenant retrieval by crafting queries designed to surface another simulated tenant's documents; if you also have time, attempt the poisoned-knowledge-base scenario by adding a document with embedded instructions and observing whether they influence output when retrieved
* Best practices: document exactly what worked, what didn't, and your best understanding of why — negative results (a scenario that didn't work as expected) are still valuable, honestly-recorded learning

### Concept 3 — Connecting Build-Then-Exploit to Your Broader Curriculum
* Key terminology: hands-on validation
* Practical application: recognize this exercise as the same fundamental learning pattern you've used since Week 1 (Day 3's NoSQL lab, Day 44's Dockerfile before/after, Day 55's pipeline integration) — build something, understand it deeply enough to find its flaws, then fix it and confirm the fix works
* Best practices: this RAG build-and-test exercise becomes a genuinely substantial, demonstrable piece of hands-on AI security work for your capstone

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Minimal RAG implementation | A simplified but functionally real RAG system for learning purposes | Transforms abstract architecture knowledge into concrete, testable understanding |
| Vulnerable-then-fixed testing | Building both a flawed and corrected version to confirm cause and effect | The same pattern used successfully since Day 3's NoSQL injection lab |
| Hands-on validation | Confirming conceptual understanding through direct building and testing | The core learning principle underlying this entire curriculum's lab structure |

---

# ⏱️ 4. Study Schedule

## Session 1 — Build the Minimal RAG System (45–60 min)
Set up a small document set (including simulated multi-tenant content) and implement basic retrieval + LLM integration.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Test Your Attack Scenarios (60–90 min)
1. First, build the version WITHOUT proper tenant filtering and confirm the cross-tenant retrieval attack (Day 68) works.
2. Add proper tenant-scoped filtering and confirm the attack no longer works.
3. If time allows, add a document with embedded instructions and test the poisoned-knowledge-base scenario.

**Expected Result:** A working RAG system with documented before/after (vulnerable/fixed) testing results.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Document the exact query/prompt that achieved cross-tenant retrieval in your vulnerable version.
### Problem 2
Explain precisely what code/logic change fixed the vulnerability, and why it works (directly connecting to Day 11's ownership-check pattern).
### Problem 3
If you tested the poisoned-knowledge-base scenario, document the exact result — did the embedded instructions influence the output? Why or why not?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: RAG Attacks Part 1 (Day 68)
Recall without notes: RAG architecture and the two designed attack scenarios.
### Spaced-Repetition Review
* **Yesterday:** RAG attacks, part 1 (conceptual)
* **9 weeks + 4 days ago:** NoSQL injection lab (Day 3) — the build-then-exploit pattern directly reused today

---

# 🧪 6. Active Recall Exercises
1. What are the core components of even a minimal, simplified RAG system?
2. How did building the vulnerable version yourself confirm your Day 68 conceptual attack scenario?
3. What exact fix resolved the cross-tenant retrieval vulnerability?
4. What would you need to test the poisoned-knowledge-base scenario properly (a way to add content to the retrieval corpus and observe its influence on output)?
5. What's the impact of the vulnerability you demonstrated, in a realistic multi-tenant SaaS context?
6. How would you detect this kind of vulnerability in a code review of a real RAG implementation?
7. How would you generalize today's specific fix into a broader RAG security principle?
8. Difference between reading about RAG's attack surface (Day 68) and directly demonstrating it (today)?
9. Real-world parallel: how does this exercise mirror Day 3's NoSQL injection build-then-exploit lab?
10. Teach your complete RAG build-and-exploit exercise to a junior developer in plain language.

### Feynman Test
Explain your RAG build, the vulnerability you demonstrated, and the fix you applied, in 5–6 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Python/Node.js (whichever you're most comfortable with) + a simple embedding/vector-search approach + LLM API
### Today's Tool Goal
Build a working, minimal RAG implementation from scratch, understanding each component.
### Tool Success Criteria
I can explain every line of my RAG implementation's retrieval and generation logic, not just that it "works."

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #3 — AI Security Capstone
Today's contribution: your working RAG build-and-exploit demonstration, with before/after code and documented results, becomes a substantial, genuinely hands-on portfolio artifact for your capstone's AI-specific technical depth.
### Deliverable
`Day 69: Built minimal RAG application; demonstrated and fixed cross-tenant retrieval vulnerability; documented complete before/after results.`

---

# 📝 9. Practice / Security Challenge
### Scenario
This IS today's practice challenge — the complete build-then-exploit-then-fix exercise itself.
### My Task
1. Identify the vulnerability in your initial build.
2. Explain the root cause.
3. Determine the impact.
4. Demonstrate the exploitation.
5. Recommend and implement the mitigation.
6. Document the complete result.
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
* [ ] Working RAG system built  * [ ] Cross-tenant vulnerability demonstrated  * [ ] Fix implemented and confirmed
* [ ] Poisoned-knowledge-base scenario attempted (if time allows)  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Your complete RAG build, from architecture to exploitation to fix
2. The specific vulnerability and fix, in code-level detail
3. How this exercise mirrors Day 3's build-then-exploit pattern
4. Any poisoned-knowledge-base results, if attempted
5. How tomorrow's lab day will let you continue refining and documenting this RAG exercise as a polished capstone-ready artifact

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local development environment
**Lab Name:** "Build, Exploit, and Fix: A Minimal RAG Application"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 2+ hours (the bulk of today's schedule)
### Lab Objective
Produce a complete, working, documented RAG build-then-exploit-then-fix demonstration.
### Environment
Target: your own local RAG implementation · Tools: Python/Node.js, LLM API, simple vector search · Prerequisite: Day 68
### Lab Tasks
1. Build a minimal RAG system with simulated multi-tenant documents.
2. Build the vulnerable (no tenant filtering) version first and confirm the attack works.
3. Implement proper tenant-scoped access control.
4. Confirm the fix resolves the vulnerability.
5. Document the complete before/after with code snippets and test results.
### What I Need to Discover
Having now built, broken, and fixed a real (if minimal) RAG system yourself, does "RAG security" feel like a concrete, well-understood skill area now, comparable to how concretely you understand traditional web app security after Month 1's hands-on labs?
### Lab Success Criteria
A complete, working RAG application with documented vulnerable and fixed versions, clear code-level explanation of the fix, and honest documentation of any additional scenarios attempted.

---
---

# 🛡️ Day 70 — Lab Day: Fix and Re-Test the RAG App; Document Findings

**Date:** 28/10/2026
**Phase:** Month 3 — AI Security & Capstone Portfolio
**Primary Skill:** Consolidating and polishing the Week 10 RAG/red-teaming work into portfolio-ready form
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Complete any remaining testing on your Day 69 RAG application, including the poisoned-knowledge-base scenario if not yet attempted.
* Consolidate the complete Week 10 arc (ATLAS mapping, red-teaming methodology, playbook, RAG build) into a polished, portfolio-quality write-up.
* Close any remaining backlog from Days 64–69.

### Success Criteria
* Both Day 68 attack scenarios (retrieval access-control gap and poisoned knowledge base) have been genuinely tested against your RAG build.
* A consolidated Week 10 write-up exists, combining the ATLAS mapping, red-team playbook, and RAG findings.
* All Week 10 backlog is closed, ready for Week 11's capstone project to begin.

---

# 📚 2. Topics to Study
### Primary Topic
**Consolidating Week 10: AI Red-Teaming & RAG Security**
### Secondary Topics
* Completing any remaining RAG testing from Day 69
* Structuring a consolidated write-up combining methodology (playbook), taxonomy (ATLAS mapping), and hands-on findings (RAG build)
* Preparing for Week 11's capstone, which will directly apply this week's complete toolkit to a real assessment target

### Priority
🔴 **Must Know:** be able to present the complete Week 10 arc — ATLAS framework understanding, formal red-teaming methodology and playbook, and hands-on RAG build/exploit/fix — as one coherent, professional body of work
🟡 **Should Know:** how to structure a consolidated write-up that leads with methodology (the playbook) before diving into specific findings (the RAG demonstration), mirroring Day 28's "executive summary before details" lesson
🟢 **Nice to Know:** this consolidation exercise directly previews the structure your Week 11 capstone report itself will need

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Completing the RAG Testing
* What it is: finishing any Day 69 testing left incomplete, particularly the poisoned-knowledge-base scenario if not yet attempted
* Why it matters: a complete demonstration (both attack scenarios from Day 68, not just one) provides stronger, more comprehensive portfolio evidence
* Best practices: if genuinely out of time even today, honestly document what was and wasn't completed, exactly as established in earlier "lab consolidation day" patterns throughout this curriculum

### Concept 2 — Structuring the Consolidated Write-Up
* Definition: a document combining your ATLAS technique mapping, your red-team playbook, and your RAG build/exploit/fix demonstration into one coherent narrative
* Practical application: structure as — methodology overview (playbook + ATLAS mapping as your systematic approach) → applied demonstration (the RAG build/exploit/fix as concrete evidence of the methodology in action) → key findings and lessons learned
* Best practices: this mirrors exactly how you'll eventually need to structure your Week 11 capstone report — methodology, then application, then findings

### Concept 3 — Preparing for Week 11
* Key terminology: capstone readiness
* Practical application: Week 11's capstone will require you to apply this exact toolkit (playbook, ATLAS mapping, RAG/agent security awareness) to a real, more complex assessment target — today's consolidation ensures that toolkit is genuinely ready, not scattered across a week's worth of separate documents
* Best practices: treat today's consolidation as seriously as Days 27–28's Month 1 capstone-completion work — this is the direct bridge into your final major deliverable

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Consolidated write-up | Combining methodology, taxonomy mapping, and hands-on findings into one document | Mirrors the structure your Week 11 capstone report will itself need |
| Capstone readiness | Having a genuinely reusable, complete toolkit ready to apply | Prevents Week 11 from starting with scattered, unconsolidated Week 10 work |

---

# ⏱️ 4. Study Schedule

## Session 1 — Complete Remaining RAG Testing (45–60 min)
Finish any Day 69 testing left incomplete, particularly the poisoned-knowledge-base scenario.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Consolidate the Write-Up (60–90 min)
1. Structure and write the consolidated Week 10 document: methodology (playbook + ATLAS mapping) → applied demonstration (RAG build/exploit/fix) → key findings/lessons learned.
2. Clean up and organize all supporting files (technique logs, playbook document, RAG code) into a clear repository structure.
3. Publish the consolidated write-up.

**Expected Result:** A complete, polished, published Week 10 consolidated write-up.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Without notes, explain your complete red-team playbook's phases.
### Problem 2
Without notes, explain the RAG vulnerability you demonstrated and its fix.
### Problem 3
Reflect: how ready do you feel for Week 11's capstone, given this week's complete toolkit? What, if anything, still feels shaky?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full Week 10 recall sweep: ATLAS framework → ATLAS Navigator attack-path mapping → red-teaming methodology → evaluation frameworks/playbooks → RAG attacks (parts 1 and 2).
### Spaced-Repetition Review
* **Yesterday:** RAG build/exploit/fix
* **This week:** the complete AI red-teaming arc
* **10 weeks ago:** Week 1's injection material (a good long-interval check, given AI security's deep conceptual roots there)

---

# 🧪 6. Active Recall Exercises
1. Cold-recall MITRE ATLAS's structure and its relationship to ATT&CK.
2. Cold-recall your fictional-product attack-path mapping exercise.
3. Cold-recall formal red-teaming methodology and guardrail layers.
4. Cold-recall your scoring rubric and red-team playbook.
5. Cold-recall your RAG build, vulnerability, and fix.
6. Which of this week's five topics felt most novel compared to your existing Month 1/2 security knowledge?
7. How would you brief a team on AI red-teaming methodology in a single 15-minute session?
8. Which concept from this week connects most directly to a specific Month 1/2 concept (IDOR, IR playbooks, ATT&CK)?
9. Give a real-world example for at least two of this week's topics.
10. Teach the complete Week 10 arc to a junior developer in under 5 minutes.

### Feynman Test
Explain the complete Week 10 AI red-teaming and RAG security arc as one connected story in 6–7 sentences. If unclear, mark 🟡 **Needs Review** before Week 11's capstone begins.

---

# 🛠️ 7. Tool Practice
**Tool:** Full Week 10 toolkit review — ATLAS Navigator, LLM API, RAG implementation
### Today's Tool Goal
Confirm fluency across the complete week's toolkit in preparation for Week 11.
### Tool Success Criteria
I'm not pausing to relearn any tool or concept from this week when reasoning about my capstone approach.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #3 — AI Security Capstone (Week 11 begins tomorrow)
Today's contribution: the finalized, consolidated Week 10 write-up (methodology + ATLAS mapping + RAG demonstration) becomes your complete, ready-to-apply toolkit for Week 11's capstone assessment.
### Deliverable
`Day 70: Completed remaining RAG testing; published consolidated Week 10 write-up (playbook + ATLAS mapping + RAG findings); confirmed readiness for Week 11 capstone.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "Walk me through your complete AI red-teaming toolkit and how you'd apply it to a brand-new AI product you've never seen before."
### My Task
1. Walk through your playbook's scope-definition phase.
2. Explain how you'd map relevant categories to ATLAS/OWASP LLM Top 10.
3. Explain your test-case design and reproducibility-scoring approach.
4. Reference your RAG build/exploit/fix as concrete evidence of hands-on capability, including retrieval-layer access control if the hypothetical product uses RAG.
5. Practice this complete narrative out loud, under 3 minutes.
6. Document the result.
### Difficulty
⭐⭐⭐⭐☆ (interview-simulation level)

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
* [ ] Remaining RAG testing completed  * [ ] Consolidated Week 10 write-up published  * [ ] Repository organized
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Interview narrative rehearsed  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow (Start of Week 11)
1. Your complete, consolidated Week 10 toolkit and findings
2. Your interview-ready AI red-teaming narrative
3. Every major Week 9–10 concept at a working professional level
4. How this complete toolkit will be directly applied to Week 11's capstone target
5. What's genuinely still uncertain heading into the capstone (an honest, useful self-assessment)

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link to consolidated write-up]
**Final Status:** 🟢 / 🟡 / 🔴

**🏆 WEEK 10 MILESTONE ACHIEVED: Complete AI red-teaming toolkit (MITRE ATLAS mapping, formal methodology, scoring rubric, reusable playbook, and hands-on RAG build/exploit/fix demonstration) is ready for direct application to Week 11's capstone project.**

---

# 14. Hands-On LAB
**Lab Platform:** Your own RAG application + consolidated documentation
**Lab Name:** "Complete and Consolidate the Week 10 AI Red-Teaming Toolkit"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 2+ hours (the bulk of today's schedule)
### Lab Objective
Finish any remaining RAG testing and produce a complete, polished, consolidated Week 10 write-up ready to directly support Week 11's capstone work.
### Environment
Target: your RAG application + all Week 10 documentation · Tools: full Week 10 toolkit · Prerequisite: Days 64–69
### Lab Tasks
1. Complete the poisoned-knowledge-base RAG test if not yet done.
2. Consolidate ATLAS mapping, playbook, and RAG findings into one document.
3. Organize the complete repository structure.
4. Publish the consolidated write-up.
5. Rehearse your complete Week 10 narrative for interview-readiness.
### What I Need to Discover
Having now completed a full week building genuine AI red-teaming methodology and hands-on skill, does this feel like a comparably solid professional foundation to what Month 1's AppSec capstone (Days 26–28) gave you for traditional web security? That comparison is a meaningful signal of your overall progress at this three-quarter mark of the curriculum.
### Lab Success Criteria
Complete RAG testing, a published consolidated write-up combining all Week 10 elements, and a rehearsed, interview-ready narrative of the complete toolkit.

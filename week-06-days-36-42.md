# 🗓️ WEEK 6 — CI/CD Pipeline Security (Days 36–42)

---

# 🛡️ Day 36 — CI/CD Attack Surface (Secret Extraction, Poisoned Pipelines)

**Date:** 24/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Understanding CI/CD as its own attack surface
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain why CI/CD pipelines are a high-value attack target (they hold secrets and have broad deployment permissions).
* Understand "poisoned pipeline execution" — how untrusted code (e.g., a malicious pull request) can hijack a pipeline's privileges.
* Understand dependency confusion attacks conceptually.

### Success Criteria
* Explain why a CI/CD pipeline is often a more valuable attacker target than the application itself.
* Explain poisoned pipeline execution with a concrete example.
* Explain dependency confusion's mechanism.

---

# 📚 2. Topics to Study
### Primary Topic
**CI/CD Pipeline Attack Surface**
### Secondary Topics
* Poisoned Pipeline Execution (PPE) — direct and indirect
* Dependency confusion attacks (public vs. private package namespace collision)
* Why pipelines often hold more powerful credentials than the application they build

### Priority
🔴 **Must Know:** a CI/CD pipeline typically holds deployment credentials, cloud access keys, and source code access all in one place — compromising the pipeline can be more valuable to an attacker than compromising the running application directly
🟡 **Should Know:** the difference between direct PPE (attacker directly modifies a pipeline config file in a PR) and indirect PPE (attacker's PR doesn't touch pipeline config but still gets executed with pipeline privileges, e.g., via a build script)
🟢 **Nice to Know:** real-world PPE incidents in major CI/CD platforms' history

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Why CI/CD Is a High-Value Target
* What it is: pipelines are automated systems with broad, often over-privileged access — deployment credentials, cloud IAM roles, secrets for third-party services — all triggered by code changes
* Why it matters: this directly recalls the Capital One case study (Day 14) — a compromised pipeline with excessive IAM permissions creates exactly the same blast-radius problem, but the entry point is the build system rather than a running web app
* How it works: an attacker who can get *any* code executed within the pipeline's context (even seemingly harmless test code) inherits all of that pipeline's credentials and permissions
* Real-world example: numerous supply-chain attacks in recent years specifically targeted CI/CD systems rather than application code, because the leverage gained is disproportionately large

### Concept 2 — Poisoned Pipeline Execution (PPE)
* Definition: tricking a CI/CD system into executing attacker-controlled code with the pipeline's full privileges
* Architecture/process (Direct PPE): an external contributor submits a pull request that directly modifies the pipeline's YAML configuration; if the CI system automatically runs pipeline changes from PRs (even from forks) with full secrets access, the attacker's modified pipeline steps run with production credentials
* Architecture/process (Indirect PPE): the attacker doesn't touch the pipeline config at all — they modify a file the pipeline *executes* as part of its normal job (a test script, a build hook, a Makefile target), achieving the same privileged execution without ever touching the obviously-scrutinized pipeline definition itself
* Defense/mitigation: never run pipeline steps with full secrets access against untrusted/fork-originated pull requests; require manual approval before running CI against external contributions

### Concept 3 — Dependency Confusion
* Key terminology: public registry, private/internal package namespace collision
* Practical application: if a company has an internal package named `@company/internal-utils` on a private registry, and an attacker publishes a public package with the *same name* at a higher version number to a public registry (npm, PyPI), some misconfigured build tools will pull the public (malicious) version instead of the intended private one
* Best practices: use scoped private registries correctly configured to never fall through to public registries for internal package names, and pin exact versions/registries explicitly

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Poisoned Pipeline Execution | Getting attacker code executed with pipeline privileges | Directly echoes the Capital One "blast radius" lesson, applied to build systems |
| Direct vs. Indirect PPE | Modifying pipeline config directly vs. modifying something the pipeline executes | Indirect PPE is harder to catch via simple config-file review |
| Dependency confusion | Public package name collision with an internal private package | A supply-chain attack vector distinct from known-CVE dependency risk (Day 34) |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study why CI/CD is a high-value target, direct/indirect PPE, and dependency confusion.
**Output:** A written explanation connecting today's "pipeline as high-value target" concept to Day 14's Capital One "blast radius" lesson.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Review a sample GitHub Actions workflow (yours or a public example) and identify whether it would be vulnerable to direct PPE (does it run untrusted PR code with secrets access?).
2. Identify any indirect PPE risk (does the pipeline execute a script/Makefile target that a PR could modify?).
3. Research (via official npm/PyPI documentation and reputable security write-ups) how dependency confusion attacks were historically demonstrated against major companies, and note the specific misconfiguration pattern involved.

**Expected Result:** A documented PPE risk assessment of a sample workflow + written notes on dependency confusion's real-world pattern.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A GitHub Actions workflow uses `pull_request_target` (which runs with full repo secrets access) combined with checking out and running the PR's own code. Explain exactly why this combination is dangerous.
### Problem 2
Explain why a code reviewer might approve a PR that only "adds a test," not realizing it constitutes indirect PPE.
### Problem 3
Design a dependency confusion defense for a company using both a private npm registry and the public npm registry.
### Challenge
Draft a checklist for reviewing any new or modified GitHub Actions workflow for PPE risk before it's merged.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Week 5 Integrated Assessment (Day 35)
Recall without notes: the SAST+DAST+SCA integrated methodology and its Week 6 pipeline implications.
### Spaced-Repetition Review
* **Yesterday:** Week 5 integrated lab day
* **1 week + 2 days ago:** Secure SDLC models

---

# 🧪 6. Active Recall Exercises
1. Why is a CI/CD pipeline often a more valuable attacker target than the application it builds?
2. What is direct PPE, and what is indirect PPE?
3. Why is indirect PPE harder to catch via simple pipeline-config review?
4. What would an attacker need to exploit dependency confusion (knowledge of an internal package name + ability to publish to the public registry)?
5. What's the impact of a successful PPE attack against a pipeline with cloud deployment credentials?
6. How would you detect a dependency confusion attempt (unexpected package source/version in a build log)?
7. How would you prevent both direct and indirect PPE?
8. Difference between PPE and dependency confusion as attack categories?
9. Real-world example: the general pattern of dependency confusion attacks demonstrated against major companies.
10. Teach why CI/CD is a high-value attack surface to a junior developer in plain language.

### Feynman Test
Explain PPE (direct and indirect) and dependency confusion in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** GitHub Actions workflow syntax review (reading, not yet writing pipelines — that begins Day 39)
### Today's Tool Goal
Read and critically assess existing GitHub Actions YAML for PPE risk patterns (`pull_request_target`, checkout of PR head ref, secrets exposure to untrusted triggers).
### Tool Success Criteria
I can look at a GitHub Actions workflow file and identify whether its trigger/checkout combination creates PPE risk.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (begins construction Day 39)
Today's contribution: write a short "CI/CD Threat Model" document identifying PPE and dependency confusion as explicit threats your upcoming pipeline design must defend against — this becomes part of your pipeline repo's security documentation.
### Deliverable
`Day 36: Documented CI/CD threat model (PPE + dependency confusion) to inform pipeline design starting Day 39.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An open-source project automatically runs its full CI pipeline (including deployment to a staging environment with real cloud credentials) against every pull request, including from first-time external contributors.
### My Task
1. Identify the vulnerability/threat (direct PPE risk).
2. Explain the root cause.
3. Determine the impact (staging environment/cloud credential compromise via a malicious PR).
4. Recommend a mitigation (require maintainer approval before running privileged CI steps against external PRs).
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
* [ ] Study notes  * [ ] PPE risk assessment of a sample workflow  * [ ] Dependency confusion research notes
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Why CI/CD is a high-value attack target
2. Direct vs. indirect PPE
3. Dependency confusion's mechanism
4. Your documented CI/CD threat model
5. How tomorrow's least-privilege-token topic directly mitigates the blast radius of any successful PPE attack

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Self-directed workflow review (real public GitHub Actions examples)
**Lab Name:** "Audit a Real-World Workflow for PPE Risk"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Find and critically review 2–3 real, public GitHub Actions workflows (from open-source repositories) for PPE risk patterns.
### Environment
Target: public GitHub repositories' `.github/workflows/` files · Tools: browser, GitHub search · Prerequisite: today's PPE concepts
### Lab Tasks
1. Find 2–3 public workflows that trigger on `pull_request` or `pull_request_target`.
2. For each, check whether secrets are exposed to the triggered job.
3. Check whether the job checks out and executes the PR's own code.
4. Classify each as low/medium/high PPE risk with reasoning.
5. Document your findings responsibly (this is observational analysis, not exploitation — do not attempt to submit malicious PRs to any real project).
### What I Need to Discover
How common are risky patterns in real, widely-used open-source workflows? Does this match your expectation, or is the ecosystem more/less careful about this than you assumed?
### Lab Success Criteria
Document PPE risk classification for 2–3 real workflows with clear reasoning, without taking any exploitative action against real projects.

---
---

# 🛡️ Day 37 — Dependency Confusion & Least-Privilege CI Tokens

**Date:** 25/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Least-privilege credential design for CI/CD systems
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Deepen yesterday's dependency confusion understanding with hands-on registry configuration practice.
* Explain least-privilege token design specifically for CI/CD (scoped, short-lived, job-specific credentials).
* Configure a GitHub Actions workflow's permissions block to follow least-privilege principles.

### Success Criteria
* Explain least-privilege CI token design without notes, directly connecting it to Day 14's Capital One IAM lesson.
* Configure explicit, minimal `permissions:` scoping in a sample GitHub Actions workflow.
* Explain OIDC-based cloud authentication as an alternative to long-lived static credentials.

---

# 📚 2. Topics to Study
### Primary Topic
**Least-Privilege CI/CD Credentials**
### Secondary Topics
* GitHub Actions' `GITHUB_TOKEN` default permissions and how to restrict them explicitly
* Scoped, job-specific secrets vs. one broad "god" credential
* OpenID Connect (OIDC) for short-lived, keyless cloud authentication from CI/CD

### Priority
🔴 **Must Know:** the same least-privilege principle from Day 14's Capital One case study applies directly here — a pipeline credential should have exactly the permissions its specific job needs, nothing more, so that even a successful PPE compromise (Day 36) has a limited blast radius
🟡 **Should Know:** GitHub Actions' `permissions:` block lets you explicitly restrict the default `GITHUB_TOKEN`'s scope per workflow or per job
🟢 **Nice to Know:** OIDC federation eliminates the need to store long-lived cloud credentials as CI secrets at all — the pipeline requests short-lived, scoped tokens dynamically at run time

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Least Privilege for CI Tokens (Direct Capital One Callback)
* What it is: scoping every credential a pipeline uses down to exactly what that specific job requires — no broader
* Why it matters: this is precisely the lesson from the Capital One breach (Day 14) applied to a different context — an overly broad IAM role turned a network bug into a catastrophe; an overly broad CI token turns a successful PPE attack (Day 36) into a catastrophe
* How it works: a job that only needs to read repository contents should never hold a token that can also push to production or modify repository settings
* Real-world example: revisit Capital One directly — the mitigation principle is identical, just applied one layer earlier in the attack chain (protecting the pipeline itself, not just the cloud environment it deploys to)

### Concept 2 — GitHub Actions `permissions:` Block
* Definition: an explicit configuration restricting what the automatically-provided `GITHUB_TOKEN` can do for a given workflow/job
* Architecture/process: by default, `GITHUB_TOKEN` may have broad read/write permissions; explicitly setting `permissions: contents: read` (and only adding write scopes where genuinely needed, per job) dramatically reduces blast radius
* Best practices: set restrictive permissions at the workflow level, then grant additional specific permissions only to the individual jobs that need them

### Concept 3 — OIDC for Cloud Authentication
* Key terminology: OpenID Connect (OIDC), short-lived token, keyless authentication
* Practical application: instead of storing a long-lived AWS access key as a GitHub secret (which, if leaked via PPE, remains valid indefinitely until manually rotated), configure OIDC federation so GitHub Actions requests a short-lived, narrowly-scoped AWS credential at run time, valid only for that specific job's duration
* Best practices: this directly reduces the value of any secret an attacker might exfiltrate via PPE — a token valid for minutes is far less useful than one valid indefinitely

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Least-privilege CI token | Scoping pipeline credentials to exactly what's needed | The direct Capital One lesson, applied to build systems |
| `permissions:` block | Explicit GitHub Actions token scope restriction | The concrete mechanism for implementing least privilege in GitHub Actions |
| OIDC federation | Short-lived, dynamically-issued cloud credentials for CI | Eliminates the risk of long-lived leaked static credentials |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study least-privilege CI token design, GitHub Actions' `permissions:` block, and OIDC federation.
**Output:** A written explanation directly mapping today's least-privilege CI concept onto Day 14's Capital One IAM lesson — same principle, different layer.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Write a sample GitHub Actions workflow YAML with an explicit, restrictive `permissions:` block.
2. Research and document (via GitHub's official documentation) how to configure OIDC federation for AWS from GitHub Actions, even if you don't have a live AWS account to fully test against — document the configuration steps precisely.
3. Compare a "before" (default/broad permissions) and "after" (least-privilege) version of the same workflow.

**Expected Result:** A documented before/after workflow permissions comparison + OIDC configuration notes.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A workflow's default `GITHUB_TOKEN` has write access to repository contents, but the job only needs to read code and run tests. Write the correct restrictive `permissions:` block.
### Problem 2
Explain, in the context of a successful PPE attack (Day 36), why a short-lived OIDC token limits damage far more than a long-lived static secret would.
### Problem 3
Design a least-privilege token strategy for a pipeline with 3 distinct jobs: run tests (read-only), build and push a Docker image (write to registry), deploy to production (cloud deploy permissions). Should each job share one token, or use separate, scoped tokens?
### Challenge
Draft a complete least-privilege credential policy document for a CI/CD pipeline, covering GitHub token scoping and cloud credential strategy (OIDC preferred over static keys).

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: CI/CD Attack Surface (Day 36)
Recall without notes: direct/indirect PPE and dependency confusion.
### Spaced-Repetition Review
* **Yesterday:** CI/CD attack surface
* **3 weeks + 2 days ago:** Capital One case study (Day 14) — directly relevant again today

---

# 🧪 6. Active Recall Exercises
1. What is least-privilege credential design for CI/CD?
2. How does GitHub Actions' `permissions:` block implement this?
3. Why does OIDC federation reduce risk compared to long-lived static secrets?
4. What would you need to configure OIDC federation for a cloud provider from GitHub Actions?
5. What's the impact of a broadly-scoped CI token being exfiltrated via a successful PPE attack, vs. a narrowly-scoped one?
6. How would you audit an existing pipeline's token scoping for over-permissioning?
7. How would you migrate a pipeline from static cloud credentials to OIDC federation?
8. Difference between workflow-level and job-level `permissions:` scoping?
9. Real-world example: directly connect this to Capital One's IAM over-permissioning lesson.
10. Teach least-privilege CI credential design to a junior developer, using the Capital One case study as the anchor example.

### Feynman Test
Explain least-privilege CI tokens and OIDC federation, directly referencing Capital One, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** GitHub Actions YAML (`permissions:` block configuration)
### Today's Tool Goal
Write correctly-scoped `permissions:` blocks at both workflow and job level.
### Commands / Features to Practice
```yaml
permissions:
  contents: read

jobs:
  deploy:
    permissions:
      contents: read
      id-token: write   # required for OIDC
```
### Tool Success Criteria
I can write a workflow with explicit, minimal permissions and explain why each granted scope is necessary.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline
Today's contribution: draft the least-privilege credential policy (permissions blocks per job, OIDC-preferred cloud auth strategy) that will be directly implemented when you build the actual pipeline starting Day 39.
### Deliverable
`Day 37: Documented least-privilege CI token policy and OIDC federation configuration approach for the upcoming pipeline build.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A startup's single GitHub Actions workflow uses one static AWS access key (stored as a repo secret) with full administrator permissions for every job, including running unit tests.
### My Task
1. Identify the vulnerability/threat (massively over-privileged, long-lived credential exposed to every job, including low-trust ones).
2. Explain the root cause.
3. Determine the impact if this credential were ever exfiltrated (e.g., via a PPE attack).
4. Recommend a mitigation (job-scoped permissions, OIDC federation, separate credentials per job type).
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
* [ ] Study notes  * [ ] Permissions block before/after comparison  * [ ] OIDC configuration notes documented
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Least-privilege CI credential design, connected explicitly to Capital One
2. GitHub Actions `permissions:` block configuration
3. OIDC federation's advantage over static secrets
4. Your documented least-privilege policy for the upcoming pipeline
5. How tomorrow's secret-management topic (trufflehog, git-secrets, Vault) complements today's least-privilege focus

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Lab (GitHub Actions YAML authoring)
**Lab Name:** "Design a Least-Privilege Pipeline Permission Model"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Design and document a complete, job-by-job least-privilege permission model for a realistic multi-job pipeline.
### Environment
Target: a hypothetical (or your emerging real) pipeline with test/build/deploy jobs · Tools: GitHub Actions YAML syntax, official documentation · Prerequisite: today's concepts
### Lab Tasks
1. List every job your future pipeline will need (test, SAST, DAST, SCA, build, deploy).
2. For each job, determine the minimum permissions it genuinely needs.
3. Write the corresponding `permissions:` blocks.
4. Document your OIDC-vs-static-secret decision for any cloud deployment step, with reasoning.
5. Review the complete model for any job that's still over-permissioned.
### What I Need to Discover
When you force yourself to justify every single permission grant explicitly, do you find any job in your design that was implicitly assuming more access than it actually needs? That's exactly the kind of over-permissioning that turned Capital One's incident into a catastrophe.
### Lab Success Criteria
A complete, job-by-job least-privilege permission model documented with clear justification for every granted scope.

---
---

# 🛡️ Day 38 — Secret Management: trufflehog, git-secrets, HashiCorp Vault Basics

**Date:** 26/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Detecting exposed secrets and managing secrets securely
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain why secrets committed to git history remain exposed even after being "deleted" in a later commit.
* Run `trufflehog` and `git-secrets` against a real repository to detect exposed secrets.
* Understand HashiCorp Vault's role as a centralized, dynamic secrets-management system.

### Success Criteria
* Explain why git history retains secrets even after deletion, without notes.
* Successfully run `trufflehog` against a repository and interpret findings.
* Explain Vault's core value proposition (centralized, dynamic, auditable secret issuance) at a conceptual level.

---

# 📚 2. Topics to Study
### Primary Topic
**Secret Detection & Management**
### Secondary Topics
* Why deleting a secret in a new commit doesn't remove it from git history
* `trufflehog` (deep git history scanning) vs. `git-secrets` (pre-commit prevention)
* HashiCorp Vault's dynamic secrets model vs. static, long-lived secrets

### Priority
🔴 **Must Know:** git is an append-only history by design — committing a secret and later "removing" it in a subsequent commit leaves the secret fully recoverable in the repository's history unless that history is explicitly rewritten (and even then, any existing clones/forks retain it)
🟡 **Should Know:** the difference between *detection* tools (trufflehog — finds secrets already committed) and *prevention* tools (git-secrets — blocks secrets from being committed in the first place via a pre-commit hook)
🟢 **Nice to Know:** Vault's "dynamic secrets" concept — generating short-lived, on-demand credentials (e.g., a database password valid for one hour) rather than static, long-lived ones

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Why Git History Retains Secrets Permanently
* What it is: git tracks the complete history of every change; a commit that "removes" a secret from the current file state does not remove the secret from the commit where it was originally introduced
* Why it matters: this is a very common real-world incident pattern — a developer commits a secret, notices the mistake, and pushes a "fix" commit removing it, believing the problem is solved, while the secret remains fully readable in the git log indefinitely
* How it works: `git log -p` or a simple `git show <commit>` on the original commit reveals the secret exactly as it was, regardless of later commits
* Real-world example: numerous public GitHub incidents involve exactly this pattern — developers scanning public repos specifically for this kind of "removed but still in history" secret
* Common mistake: believing that deleting a file or a line containing a secret in a new commit is sufficient remediation — the correct remediation requires rotating the actual credential (assume it's compromised) AND potentially rewriting history

### Concept 2 — Detection vs. Prevention Tooling
* Definition: `trufflehog` scans the full git history (and can scan live repositories, filesystems, and even S3 buckets) for patterns matching known secret formats (API keys, private keys, tokens); `git-secrets` installs a pre-commit hook that blocks a commit from being created at all if it matches a secret pattern
* Architecture/process: prevention (git-secrets) is the ideal first line of defense; detection (trufflehog) is essential for auditing existing repositories (including ones you didn't set up prevention on from day one) and for periodic ongoing scanning
* Best practices: use both — prevention to stop new leaks, detection to catch what prevention missed or what existed before prevention was set up

### Concept 3 — HashiCorp Vault and Dynamic Secrets
* Key terminology: static secret, dynamic secret, secret lease/TTL
* Practical application: rather than an application holding a long-lived database password as an environment variable, Vault can generate a unique, short-lived database credential on demand, automatically revoked after its lease expires
* Best practices: this directly extends yesterday's OIDC/least-privilege discussion — the shorter a credential's useful lifetime, the less damage its exposure can cause

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Git history persistence | Deleted secrets remain in earlier commits indefinitely | A very common, easily-preventable real-world incident source |
| Prevention vs. detection tooling | `git-secrets` blocks commits; `trufflehog` finds existing exposures | Both are necessary; neither alone is sufficient |
| Dynamic secrets (Vault) | Short-lived, on-demand generated credentials | Extends the least-privilege/short-lifetime principle from Day 37 |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study why git history retains secrets, the detection-vs-prevention distinction, and Vault's dynamic secrets concept.
**Output:** A written explanation of exactly why "I deleted the secret in a later commit" is not remediation, using git's data model as the explanation.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Install `trufflehog` and run it against a public repository known to have had historical secret leaks (research a well-documented public example, or create a deliberate test repository with a dummy "secret" committed and later "removed" to demonstrate the concept safely on your own repo).
2. Install `git-secrets` and configure it as a pre-commit hook on a local test repository; attempt to commit a dummy secret pattern and confirm it's blocked.
3. Research HashiCorp Vault's dynamic secrets documentation and note how a database credential lease would work conceptually, even without a full local Vault deployment.

**Expected Result:** Documented `trufflehog` findings (on your safe test repo or a well-known public example) + a working `git-secrets` pre-commit hook demonstration + Vault dynamic secrets notes.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A developer commits an AWS access key, then immediately deletes it in the next commit before pushing both commits together. Is the secret still exposed once pushed? Explain precisely why or why not.
### Problem 2
Design the exact remediation steps (not just "remove it") for a secret discovered in git history that has already been pushed to a public repository.
### Problem 3
Explain why `git-secrets` (prevention) alone is insufficient for a codebase with years of pre-existing history predating its installation.
### Challenge
Design a complete secret-management policy combining `git-secrets` (prevention), scheduled `trufflehog` scans (detection), and Vault-issued dynamic secrets (runtime credential management) for a mid-sized engineering team.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Least-Privilege CI Tokens (Day 37)
Recall without notes: the `permissions:` block and OIDC federation.
### Spaced-Repetition Review
* **Yesterday:** Least-privilege CI tokens
* **2 weeks + 2 days ago:** SCA & SBOM generation (Day 34) — both are "supply chain"-adjacent risk categories

---

# 🧪 6. Active Recall Exercises
1. Why does a "deleted" secret in git history remain exposed?
2. How does `trufflehog` detect secrets in historical commits?
3. How does `git-secrets` prevent new secret commits?
4. What would you need to properly remediate a leaked secret (rotate the actual credential, not just remove it from the file)?
5. What's the impact of a leaked cloud credential remaining valid indefinitely because it was never rotated after "removal"?
6. How would you detect secrets that predate your prevention tooling's installation?
7. How would Vault's dynamic secrets reduce the impact of a leaked credential compared to a static one?
8. Difference between prevention and detection secret-management tooling?
9. Real-world example: the common "committed then deleted" secret exposure pattern.
10. Teach why git history retains secrets permanently to a junior developer in plain language.

### Feynman Test
Explain why deleted secrets remain exposed in git history, and the prevention/detection/dynamic-secrets toolkit that addresses this, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** `trufflehog`, `git-secrets`
### Today's Tool Goal
Run both tools against real (safe, authorized) repositories and interpret their output.
### Commands / Features to Practice
```text
trufflehog git file://./my-test-repo
git secrets --install
git secrets --register-aws
git commit -am "test"   # should be blocked if a secret pattern is present
```
### Tool Success Criteria
I can run both tools correctly and explain the distinct role each plays in a complete secret-management strategy.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (secret scanning becomes a pipeline gate starting Day 39)
Today's contribution: document your `trufflehog` scan configuration and `git-secrets` pre-commit setup — these become both a repository-level safeguard AND a CI pipeline step in the build starting tomorrow.
### Deliverable
`Day 38: Configured and tested trufflehog scanning and git-secrets pre-commit hook; documented secret-management policy for pipeline integration.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A company discovers, via a `trufflehog` scan run for the first time after 3 years of development, that a database password was committed in year one and has been sitting in git history ever since, still valid and unrotated.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause (no historical scanning, no rotation policy).
3. Determine the impact of a 3-year exposure window.
4. Recommend immediate remediation (rotate the credential immediately) AND long-term prevention (git-secrets + scheduled trufflehog scans + Vault dynamic secrets).
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
* [ ] Study notes  * [ ] trufflehog scan run and documented  * [ ] git-secrets hook configured and tested
* [ ] Vault dynamic secrets notes  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Why git history retains secrets permanently
2. `trufflehog` vs. `git-secrets`'s respective roles
3. Vault's dynamic secrets concept
4. Your documented secret-management policy
5. How tomorrow begins the actual pipeline build, incorporating everything from Days 36–38 as concrete pipeline steps

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Lab (trufflehog + git-secrets)
**Lab Name:** "Detect and Prevent: Full Secret-Management Setup"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Set up a complete, working secret-detection and prevention configuration on a real (test) repository, demonstrating both the discovery of an existing (deliberately planted, for safe practice) leaked secret and the prevention of a new one.
### Environment
Target: a dedicated local test git repository (never a real production repo, for safety) · Tools: `trufflehog`, `git-secrets` · Prerequisite: today's concepts
### Lab Tasks
1. Create a test repository and deliberately commit a dummy "secret" (a clearly fake, non-functional pattern like a placeholder AWS key format) in an early commit.
2. "Remove" it in a later commit.
3. Run `trufflehog` and confirm it still finds the dummy secret in history.
4. Install `git-secrets` and attempt to commit a new dummy secret; confirm it's blocked.
5. Document the complete before/after demonstration.
### What I Need to Discover
Does directly seeing the "removed" secret still appear in a trufflehog scan make the git-history-persistence concept concrete in a way pure reading didn't? This kind of hands-on demonstration is often what makes a security concept truly stick.
### Lab Success Criteria
Documented demonstration of trufflehog finding a historical (test) secret and git-secrets blocking a new commit attempt.

---
---

# 🛡️ Day 39 — Build the Secure Pipeline, Part 1: GitHub Actions Skeleton + Semgrep Gate

**Date:** 27/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Building a real GitHub Actions CI/CD pipeline with an integrated security gate
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Build the foundational GitHub Actions workflow skeleton (checkout, dependency install, test job) applying Day 37's least-privilege permissions from the start.
* Add Semgrep as an automated pipeline gate, failing the build on High/Critical findings.
* Confirm the pipeline runs correctly end-to-end on a real push/PR.

### Success Criteria
* A working GitHub Actions workflow exists in a real repository.
* Least-privilege `permissions:` are explicitly set (Day 37).
* Semgrep runs automatically and correctly fails the build when a deliberately-introduced vulnerable pattern is present.

---

# 📚 2. Topics to Study
### Primary Topic
**Building the Pipeline Skeleton with an Integrated SAST Gate**
### Secondary Topics
* GitHub Actions workflow syntax: triggers, jobs, steps
* Integrating Semgrep as a pipeline step with a fail condition
* Structuring the pipeline for future extension (Days 40–41 will add DAST, SCA, secret scanning)

### Priority
🔴 **Must Know:** how to structure a GitHub Actions YAML file — `on:` triggers, `jobs:`, `steps:`, and how to fail a job based on a tool's exit code
🟡 **Should Know:** how to apply Day 37's least-privilege `permissions:` block from the very first version of the pipeline, not as an afterthought
🟢 **Nice to Know:** caching dependencies in GitHub Actions to speed up repeated pipeline runs

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — GitHub Actions Workflow Structure
* What it is: a YAML file in `.github/workflows/` defining what triggers the pipeline (`on:` — push, pull_request, schedule), what jobs run, and what steps each job executes
* Why it matters: this is the actual automation vehicle for everything you've learned in Weeks 5–6 — turning manual tool runs into something that happens automatically on every code change
* How it works: each `step` runs a shell command or a reusable "action"; a step's non-zero exit code fails the job, which fails the overall workflow run
* Real-world example: this exact pattern (checkout → install → test → security gates → build → deploy) is the standard shape of a huge proportion of real-world CI/CD pipelines

### Concept 2 — Integrating Semgrep as a Gate
* Definition: adding a step that runs `semgrep --config=p/owasp-top-ten --error` (the `--error` flag makes Semgrep exit non-zero if findings are present, which fails the job)
* Architecture/process: place this step early in the pipeline (fail fast, before spending time on slower steps like DAST) — connects directly to your Day 30–31 Semgrep configuration work
* Attack scenario this catches: any future commit reintroducing a vulnerable pattern (like Day 1's SQL concatenation) is now automatically caught before merge, not just during manual Week 1 exploitation testing

### Concept 3 — Building for Extension
* Key terminology: modular pipeline design, job dependencies (`needs:`)
* Practical application: structure today's skeleton so that Days 40–41 can cleanly add DAST, SCA, and secret-scanning steps/jobs without needing to rewrite what you build today
* Best practices: use clear job names and comments so the pipeline's structure documents itself for a future reader (or interviewer reviewing your portfolio)

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Workflow trigger (`on:`) | What event starts the pipeline | Determines when your security gates actually run |
| Pipeline gate | A step whose failure blocks the build/merge | The mechanism turning manual security testing into automatic enforcement |
| Fail fast | Ordering steps so fast/cheap checks run before slow ones | Saves CI time and developer feedback latency |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study GitHub Actions workflow syntax in depth, focusing on triggers, jobs, steps, and exit-code-based failure.
**Output:** Sketch (in notes) the full intended pipeline structure across this week (test → Semgrep → today; DAST + secret scan → tomorrow; SCA + final integration → Day 41), even though you're only building the first piece today.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Create a real GitHub repository (or use your existing personal project) and add a `.github/workflows/security-pipeline.yml` file.
2. Build the skeleton: trigger on push/PR, checkout code, install dependencies, run existing tests.
3. Apply Day 37's least-privilege `permissions:` block from the start.
4. Add a Semgrep step configured to fail the build on findings.
5. Deliberately introduce a vulnerable pattern (e.g., Day 1's SQL concatenation) in a test branch/PR and confirm the pipeline correctly fails; then fix it and confirm the pipeline passes.

**Expected Result:** A working, committed GitHub Actions workflow with a demonstrated fail/pass cycle around a real Semgrep finding.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Your Semgrep step runs but doesn't fail the build even when findings are present. What flag or configuration is likely missing?
### Problem 2
Explain why placing Semgrep before a slower DAST step (to be added tomorrow) is the correct ordering, referencing the "fail fast" principle.
### Problem 3
Your workflow's `permissions:` block is too restrictive and the Semgrep step fails for an unrelated permissions reason (e.g., can't post a PR comment). How would you diagnose and fix this without simply granting broad permissions?
### Challenge
Sketch the `needs:` dependency structure for the full multi-job pipeline you'll complete by Day 41 (test → SAST → SCA → DAST → build, with appropriate parallelization where jobs don't depend on each other).

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Secret Management (Day 38)
Recall without notes: why git history retains secrets, and the trufflehog/git-secrets/Vault toolkit.
### Spaced-Repetition Review
* **Yesterday:** Secret management
* **1 week + 4 days ago:** SAST fundamentals with Semgrep (Day 30) — directly applied today

---

# 🧪 6. Active Recall Exercises
1. What is the basic structure of a GitHub Actions workflow (triggers, jobs, steps)?
2. How do you make a step's failure actually fail the overall pipeline job?
3. Why does "fail fast" ordering matter for pipeline design?
4. What would you need to correctly integrate Semgrep as a gate (the right flag to exit non-zero on findings)?
5. What's the impact of a security gate that runs but doesn't actually fail the build on findings — is it providing any real protection?
6. How would you extend today's skeleton with additional jobs in coming days without rewriting it?
7. How would you apply least-privilege permissions from the very start rather than retrofitting them later?
8. Difference between a workflow succeeding "silently" past a security issue vs. genuinely gating on it?
9. Real-world example: how this exact pipeline shape mirrors real-world CI/CD security integration.
10. Teach the concept of a "pipeline security gate" to a junior developer in plain language.

### Feynman Test
Explain how you built today's pipeline skeleton and Semgrep gate, and why the ordering/permissions choices matter, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** GitHub Actions (full workflow authoring and debugging)
### Today's Tool Goal
Author, commit, run, debug, and iterate on a real GitHub Actions workflow until it behaves exactly as intended.
### Commands / Features to Practice
```yaml
name: Security Pipeline
on: [push, pull_request]
permissions:
  contents: read
jobs:
  sast:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Semgrep
        run: |
          pip install semgrep --break-system-packages
          semgrep --config=p/owasp-top-ten --error .
```
### Tool Success Criteria
I have a real, committed, working workflow file that correctly passes and fails based on actual code changes, not just a theoretical example.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (construction begins today — THIS IS the project)
Today's contribution: the initial working pipeline skeleton with an integrated, correctly-failing Semgrep gate, committed to your portfolio repository.
### Deliverable
`Day 39: Built and tested GitHub Actions pipeline skeleton with least-privilege permissions and a working Semgrep SAST gate.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A team adds Semgrep to their pipeline but configures it without the `--error` flag, so it always exits 0 regardless of findings — the step shows a green checkmark even when critical vulnerabilities are detected.
### My Task
1. Identify the vulnerability/threat (a false sense of security from a non-functional gate).
2. Explain the root cause.
3. Determine the impact (vulnerable code merges freely while appearing "scanned and clean").
4. Recommend the fix.
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
* [ ] Study notes  * [ ] Working pipeline skeleton committed  * [ ] Semgrep gate tested (fail + pass demonstrated)
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. GitHub Actions workflow structure
2. How to correctly gate a pipeline on a security tool's findings
3. Least-privilege permissions applied from pipeline inception
4. Your working, tested Semgrep gate
5. How tomorrow adds ZAP/Nuclei (DAST) and secret scanning to this same pipeline

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Real GitHub repository (your emerging Portfolio Piece #2)
**Lab Name:** "Build and Prove the SAST Gate Works"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes (the bulk of today's Session 2)
### Lab Objective
Prove, with a real fail/pass demonstration, that your pipeline's Semgrep gate genuinely blocks vulnerable code and allows fixed code through.
### Environment
Target: your real GitHub repository · Tools: GitHub Actions, Semgrep · Prerequisite: today's fundamentals
### Lab Tasks
1. Push the initial working pipeline to `main`.
2. Create a branch introducing a deliberately vulnerable pattern (e.g., string-concatenated SQL).
3. Open a PR and confirm the pipeline run fails on the Semgrep step.
4. Fix the vulnerable pattern in the same branch.
5. Confirm the pipeline now passes, and merge.
### What I Need to Discover
Seeing your own pipeline actually block a real (test) vulnerability, automatically, without you manually running Semgrep yourself — does this concretely demonstrate the value of "shift left" automation from Day 29 in a way the concept alone didn't?
### Lab Success Criteria
A documented, real fail-then-pass cycle proving the Semgrep gate functions correctly in your actual pipeline.

---
---

# 🛡️ Day 40 — Build the Secure Pipeline, Part 2: Add ZAP + npm audit + Secret Scanning

**Date:** 28/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Extending the pipeline with DAST, SCA, and secret-scanning gates
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Add a headless ZAP baseline scan job to the pipeline (building on Day 33's configuration work).
* Add an `npm audit`/SCA gate (building on Day 34's work).
* Add automated secret scanning (`trufflehog`, building on Day 38's work) as a pipeline step.
* Confirm all gates run correctly together in one integrated workflow.

### Success Criteria
* The pipeline now includes 4 working security gates: SAST (Day 39), DAST, SCA, and secret scanning.
* Each gate correctly fails the build when triggered by a deliberately introduced issue.
* The full pipeline run completes in a reasonable time, informed by yesterday's "fail fast" ordering discussion.

---

# 📚 2. Topics to Study
### Primary Topic
**Extending the Pipeline: DAST, SCA, and Secret-Scanning Gates**
### Secondary Topics
* Running the application under test within the CI environment (needed for DAST to have a live target)
* Job ordering and dependencies (`needs:`) for a multi-gate pipeline
* Structuring output so failures are clearly attributable to a specific gate

### Priority
🔴 **Must Know:** DAST requires the application to actually be running within the CI job before ZAP can scan it — this typically means starting the app as a background process (or using a service container) before the scan step
🟡 **Should Know:** how to structure `needs:` dependencies so independent gates (SAST, SCA, secret scanning) can run in parallel, while DAST (which needs a running app, possibly built from earlier steps) runs appropriately
🟢 **Nice to Know:** uploading scan reports as workflow artifacts for later review, not just pass/fail status

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Running the App for DAST Within CI
* What it is: unlike SAST (works on static source) or SCA (works on a dependency manifest), DAST needs an actual running instance of the application to scan
* Why it matters: this is the trickiest integration of today's three additions — you need to start the app (e.g., `npm start &` or via a Docker container) and wait for it to be ready before invoking ZAP against `http://localhost:PORT`
* How it works: a step starts the app in the background, a subsequent step waits/polls until the app responds, then the ZAP scan step runs against the now-live local instance
* Common mistake: running the ZAP scan before the app has finished starting up, causing a false "target unreachable" failure unrelated to actual security findings

### Concept 2 — Parallelizing Independent Gates
* Definition: SAST, SCA, and secret scanning don't depend on each other or on a running application, so they can run as separate parallel jobs rather than sequential steps within one job, reducing total pipeline time
* Architecture/process: define separate `jobs:` for `sast`, `sca`, `secret-scan`, and `dast`, with `dast` potentially using `needs: [build]` if it depends on a build step, while the others have no interdependency
* Best practices: parallelization is a direct practical benefit of good pipeline architecture — a well-structured pipeline with 4 gates need not take 4x as long as one gate if independent gates run concurrently

### Concept 3 — Clear Failure Attribution
* Key terminology: job name, step name, workflow summary
* Practical application: name every job and step descriptively (`SAST - Semgrep OWASP Scan`, not just `run scan`) so that when the pipeline fails, a developer immediately knows which gate and why, without digging through logs
* Best practices: this is a genuine developer-experience consideration — a security pipeline that's confusing to debug will be worked around or disabled by frustrated developers, echoing the alert-fatigue lesson from Day 24

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Live-target requirement | DAST needs the app actually running to scan it | The key technical challenge distinguishing DAST integration from SAST/SCA |
| Parallel jobs | Independent gates running concurrently rather than sequentially | Reduces total pipeline execution time |
| Failure attribution | Clear naming so a failed gate is immediately identifiable | Prevents the pipeline itself from becoming a source of developer frustration/circumvention |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study how to run an application in the background within a CI job, wait for readiness, and structure parallel job dependencies.
**Output:** Sketch the exact job/step sequence for today's 3 additions, noting which run in parallel and which have dependencies.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Add an `sca` job running `npm audit` (or your project's equivalent), configured to fail on High+ severity findings.
2. Add a `secret-scan` job running `trufflehog` against the repository.
3. Add a `dast` job: start the application in the background, wait for it to be ready, then run a headless ZAP baseline scan (from Day 33's documented configuration) against it.
4. Confirm all 4 jobs (SAST from yesterday + these 3 new ones) run correctly, with the independent ones running in parallel.

**Expected Result:** A working, 4-gate pipeline with correct parallelization and clear job naming.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Your DAST job fails with "connection refused" even though your app normally starts fine locally. What's the most likely cause in a CI environment, and how would you fix it?
### Problem 2
Explain why SAST, SCA, and secret-scanning jobs can safely run in parallel while DAST typically cannot start until the app is built/running.
### Problem 3
A developer complains the pipeline is "too slow" now with 4 gates. Using today's parallelization concept, explain how you'd address this without removing any gate.
### Challenge
Design the complete `needs:` dependency graph for a 6-job pipeline: test, SAST, SCA, secret-scan, build, DAST — explaining which depend on which and why.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Pipeline Skeleton + Semgrep Gate (Day 39)
Recall without notes: workflow structure and how the Semgrep fail/pass cycle was proven.
### Spaced-Repetition Review
* **Yesterday:** Pipeline skeleton + SAST gate
* **1 week ago:** CI/CD attack surface (Day 36)

---

# 🧪 6. Active Recall Exercises
1. Why does DAST require the application to be actually running within the CI job?
2. How do you structure a pipeline so independent gates run in parallel?
3. Why does clear job/step naming matter for developer experience?
4. What would you need to correctly wait for an app to be ready before scanning it (a readiness check/polling step)?
5. What's the impact of a slow, poorly-parallelized pipeline on developer adoption and patience?
6. How would you debug a DAST job that fails due to timing/readiness issues rather than actual findings?
7. How would you structure `needs:` dependencies for a 6-job pipeline?
8. Difference between SAST/SCA (no running app needed) and DAST (running app required)?
9. Real-world example: how a well-structured multi-gate pipeline mirrors real DevSecOps team practice.
10. Teach pipeline parallelization and DAST integration challenges to a junior developer in plain language.

### Feynman Test
Explain how you extended the pipeline with DAST, SCA, and secret scanning today, including the specific DAST readiness challenge, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** GitHub Actions (multi-job workflows, background processes, service readiness)
### Today's Tool Goal
Correctly implement a "start app in background, wait for readiness, then scan" pattern within a CI job.
### Commands / Features to Practice
```yaml
- name: Start app
  run: npm start &
- name: Wait for app
  run: npx wait-on http://localhost:3000
- name: Run ZAP baseline scan
  run: docker run -t zaproxy/zap-stable zap-baseline.py -t http://localhost:3000 -r report.html
```
### Tool Success Criteria
I can reliably start an app, confirm it's ready, and scan it, all within an automated CI job with no manual intervention.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline
Today's contribution: the pipeline now has 4 working, correctly-parallelized security gates (SAST, SCA, secret scanning, DAST) — a substantial, demonstrable piece of your portfolio.
### Deliverable
`Day 40: Extended pipeline with DAST (headless ZAP), SCA (npm audit), and secret scanning (trufflehog) gates, all tested and parallelized.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A pipeline's DAST job intermittently fails in CI (about 1 in 5 runs) with a connection error, even though the exact same code works fine locally and in most CI runs.
### My Task
1. Identify the likely category of problem (a race condition — the scan step sometimes runs before the app has finished starting, especially under CI's variable resource allocation).
2. Explain the root cause.
3. Determine the impact of an unreliable ("flaky") pipeline gate on developer trust in the pipeline.
4. Recommend a fix (a more robust readiness check with retries/timeout, rather than a fixed sleep delay).
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
* [ ] Study notes  * [ ] DAST job added and tested  * [ ] SCA job added and tested  * [ ] Secret-scan job added and tested
* [ ] Parallelization confirmed  * [ ] Practice problems  * [ ] Active-recall answers  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Why DAST needs a live running target, and how to reliably provide one in CI
2. Parallel job structuring for independent gates
3. Clear failure attribution practices
4. Your complete, working 4-gate pipeline
5. How tomorrow's lab day will stress-test and finalize this pipeline before Portfolio Piece #2's milestone

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Real GitHub repository (your emerging Portfolio Piece #2)
**Lab Name:** "Prove All Four Gates Work Together"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 90–120 minutes
### Lab Objective
Demonstrate, with real test cases, that all 4 pipeline gates (SAST, SCA, secret scanning, DAST) independently and correctly detect their respective issue types.
### Environment
Target: your real pipeline repository · Tools: full pipeline toolkit · Prerequisite: Days 39–40
### Lab Tasks
1. Introduce a deliberate SAST-catchable issue on a test branch; confirm only the SAST gate fails.
2. Introduce a deliberate SCA-catchable issue (e.g., downgrade a dependency to a known-vulnerable version); confirm only the SCA gate fails.
3. Introduce a deliberate (dummy, non-functional) secret; confirm only the secret-scan gate fails.
4. Introduce a deliberate DAST-catchable issue (if feasible given your app); confirm the DAST gate fails.
5. Fix all four and confirm the full pipeline passes cleanly.
### What I Need to Discover
Does each gate fail independently and specifically for its own issue type, without false-triggering the others? This confirms your pipeline provides genuinely useful, attributable signal rather than a confusing tangle of failures.
### Lab Success Criteria
Documented proof that all 4 gates independently detect their respective issue types correctly, with a final clean passing run after all fixes.

---
---

# 🛡️ Day 41 — Lab Day: trufflehog Against a Public Repo & Pipeline Hardening

**Date:** 29/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Real-world secret-scanning practice and pipeline resilience review
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Practice `trufflehog` against real, public, authorized-for-scanning repositories to build genuine pattern-recognition experience beyond your own test cases.
* Perform a full resilience/hardening review of your pipeline built in Days 39–40 (catch-up on any incomplete elements, review for any remaining PPE/least-privilege gaps from Days 36–37).
* Prepare the pipeline for Day 42's milestone review.

### Success Criteria
* You've run `trufflehog` against at least 2–3 real public repositories and documented (responsibly, without exposing any real secrets you might find) your findings methodology.
* Your pipeline has been reviewed against the full Days 36–40 checklist (PPE risk, least privilege, secret scanning, all 4 gates working, appropriately parallelized).
* Any remaining gaps or incomplete elements from this week are closed today.

---

# 📚 2. Topics to Study
### Primary Topic
**Real-World Secret Scanning Practice & Pipeline Hardening Review**
### Secondary Topics
* Responsible disclosure practices if a real secret is genuinely discovered during authorized scanning
* A comprehensive self-review checklist covering Days 36–40's concepts
* Catching up on any incomplete lab work from this week

### Priority
🔴 **Must Know:** if you genuinely discover a real, live secret while scanning a public repository (even in an educational/practice context), you do NOT use, test, or publicize it — the responsible action is to consider reporting it via the project's disclosure process if one exists, or simply not engaging further
🟡 **Should Know:** how to review your own pipeline holistically against everything learned this week, not just individually-tested pieces
🟢 **Nice to Know:** public "secret scanning" research/awareness projects and how they respectfully handle discovered live secrets

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Responsible Practice When Scanning Real Public Repositories
* What it is: many public repositories are legitimately scannable (they're public, after all), but discovering a genuine, live, working secret creates an ethical responsibility distinct from finding one in your own deliberately-planted test case
* Why it matters: your professional conduct in security work is part of what employers assess — knowing how to handle an accidental real discovery responsibly is itself a signal of maturity
* How it works: if you discover what appears to be a real, live secret, the appropriate action is to NOT use it for anything, NOT publicize the specific finding publicly, and consider whether the project has a responsible disclosure process; when in doubt, disengage rather than escalate
* Best practices: for today's practice, prefer scanning well-known educational/intentionally-vulnerable public repositories (e.g., OWASP's own example repositories) over scanning random real production projects, to minimize the chance of this situation arising at all

### Concept 2 — Holistic Pipeline Review
* Definition: reviewing your completed pipeline as a whole, not just checking that each individually-built piece works in isolation
* Practical application: re-read Day 36's PPE checklist against your actual triggers (`on: pull_request` — does it expose secrets to fork-originated PRs?); re-confirm Day 37's least-privilege permissions are still correctly scoped after all of Day 40's additions; confirm Day 38's secret-scanning gate is genuinely part of the automated pipeline, not just a manual exercise
* Best practices: a fresh, holistic review often catches integration issues that individual-feature testing misses

### Concept 3 — Closing the Week's Backlog
* Key terminology: technical debt, backlog triage (recalling Day 13/21's earlier lab-consolidation pattern)
* Practical application: if any of Days 36–40's hands-on exercises were left incomplete due to time constraints, today is the buffer to close them before Day 42's milestone
* Best practices: be honest about what's genuinely complete vs. what needs more time — carrying forward an honestly-tracked gap is better than claiming false completion

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Responsible secret discovery | Ethical handling of accidentally-found real secrets | A genuine professional-conduct signal, not just a technical skill |
| Holistic pipeline review | Assessing the complete system, not just individual pieces | Catches integration gaps individual-feature testing misses |
| Honest backlog tracking | Accurately noting incomplete work rather than false completion | Maintains the integrity of your own learning process |

---

# ⏱️ 4. Study Schedule

## Session 1 — Real-World Secret Scanning (45–60 min)
Identify 2–3 appropriate public repositories for practice (favor well-known educational/intentionally-vulnerable projects). Run `trufflehog` against them and document your methodology and any findings responsibly.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Full Pipeline Hardening Review (60–90 min)
1. Re-read your Day 36 PPE risk assessment and verify your actual pipeline's triggers against it.
2. Re-verify Day 37's least-privilege permissions are still correctly scoped after Day 40's additions.
3. Confirm all 4 gates (SAST, SCA, secret scan, DAST) are genuinely automated and correctly parallelized.
4. Close out any incomplete exercises from Days 36–40.

**Expected Result:** A documented holistic pipeline review with any gaps identified and closed.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
While scanning a public repository for practice, you find what appears to be a genuine, currently-valid API key. What is the correct next action?
### Problem 2
Your holistic review reveals that Day 40's DAST job actually has broader permissions than necessary (a regression from Day 37's original design). How would this happen, and how do you fix it?
### Problem 3
Reflect honestly: which of this week's hands-on exercises (Days 36–40) do you feel least confident actually explaining and defending in an interview? Plan a specific review action for it.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full Week 6 recall sweep: CI/CD attack surface → least-privilege tokens → secret management → pipeline skeleton/SAST → DAST/SCA/secret-scan integration. Answer what/why/how for each from memory.
### Spaced-Repetition Review
* **Yesterday:** Pipeline extension (DAST/SCA/secret-scan)
* **This week:** the full CI/CD security arc
* **6 weeks ago:** SQL injection (a good long-interval check)

---

# 🧪 6. Active Recall Exercises
1. Cold-recall direct vs. indirect PPE.
2. Cold-recall least-privilege CI token design and OIDC federation.
3. Cold-recall why git history retains secrets and the prevention/detection toolkit.
4. Cold-recall your pipeline's 4 gates and what each catches.
5. What's the responsible action if you discover a genuine live secret during authorized scanning practice?
6. How would you explain this week's complete pipeline build to an interviewer, end to end?
7. How would you extend this pipeline further if given another week (e.g., container scanning, previewing Week 7)?
8. Difference between individually-tested pipeline pieces and a holistically-reviewed complete pipeline?
9. Real-world example connecting this week's least-privilege work back to Capital One.
10. Teach the complete Week 6 CI/CD security arc to a junior developer in under 4 minutes.

### Feynman Test
Explain your complete pipeline build (all 4 gates, least privilege, PPE mitigation) as one connected story in 5–6 sentences. If unclear, mark 🟡 **Needs Review** before Day 42's milestone.

---

# 🛠️ 7. Tool Practice
**Tool:** `trufflehog` (real-world targets) + full pipeline toolkit review
### Today's Tool Goal
Build genuine pattern-recognition confidence with `trufflehog` beyond your own test cases, and confirm fluency with the complete pipeline toolkit.
### Tool Success Criteria
I can run `trufflehog` against a new, unfamiliar repository and correctly triage its output without hesitation.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (milestone tomorrow)
Today's contribution: the fully hardened, holistically-reviewed pipeline, with all Week 6 concepts genuinely integrated and verified — ready for tomorrow's milestone documentation.
### Deliverable
`Day 41: Completed holistic pipeline hardening review; closed all Week 6 exercise backlog; practiced real-world trufflehog usage.`

---

# 📝 9. Practice / Security Challenge
### Scenario
You're asked in an interview: "Walk me through the complete secure CI/CD pipeline you built, from initial threat modeling through to the final working gates."
### My Task
1. Start with Day 36's threat model (PPE, dependency confusion).
2. Explain Day 37's least-privilege design decisions.
3. Explain Day 38's secret-management approach.
4. Walk through Days 39–40's actual gate implementation.
5. Mention today's holistic hardening review as evidence of thoroughness.
6. Practice this full narrative out loud, under 3 minutes.
7. Document the result.
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
* [ ] Real-world trufflehog practice documented  * [ ] Holistic pipeline review completed
* [ ] All Week 6 backlog closed  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Interview narrative rehearsed  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Your complete Week 6 pipeline story, cold, interview-ready
2. Responsible practice for real-world secret-scanning discoveries
3. Every gap you found and closed during today's holistic review
4. Your full pipeline's architecture end-to-end
5. What tomorrow's milestone documentation will formalize about this project

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Real public repositories (educational/intentionally-vulnerable examples) + your own pipeline repo
**Lab Name:** "Real-World Secret Scanning Practice + Pipeline Self-Audit"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Build real pattern-recognition experience with secret scanning against unfamiliar codebases, and perform a rigorous self-audit of your own completed pipeline.
### Environment
Target: 2–3 public educational repositories + your own pipeline repo · Tools: `trufflehog`, GitHub Actions · Prerequisite: Days 36–40
### Lab Tasks
1. Run `trufflehog` against 2–3 public educational repositories.
2. Document your triage methodology and any (non-sensitive, appropriately handled) findings.
3. Re-audit your own pipeline against the complete Days 36–40 checklist.
4. Close any remaining gaps.
5. Prepare a summary of the week's complete deliverable for tomorrow's milestone.
### What I Need to Discover
Does your pattern-recognition for secrets generalize to codebases you've never seen before, or was it specific to your own deliberately-planted test cases? This is the same "does learning generalize" question from Day 13's TryHackMe exercise, now applied to a different skill.
### Lab Success Criteria
Documented real-world scanning practice with responsible handling of any findings, plus a complete, gap-free holistic pipeline review.

---
---

# 🛡️ Day 42 — Portfolio Piece #2 Milestone: Pipeline Repo Structure Finalized

**Date:** 30/09/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Documenting and finalizing a DevSecOps portfolio deliverable
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Finalize the pipeline repository's documentation (README, architecture explanation) to professional portfolio standard.
* Confirm the repository structure itself is clean and navigable.
* Prepare an interview-ready narrative for this milestone (full completion continues into Week 7–8 with containers/cloud, but this week's pipeline core is now a presentable milestone).

### Success Criteria
* The repository has a clear README explaining the pipeline's purpose, architecture, and each of the 4 security gates.
* An architecture diagram (even simple) visually represents the pipeline flow.
* You can present this milestone confidently in under 3 minutes.

---

# 📚 2. Topics to Study
### Primary Topic
**Finalizing a DevSecOps Portfolio Repository**
### Secondary Topics
* Writing a README that serves both a technical reviewer and a quick-skimming hiring manager
* Creating a simple pipeline architecture diagram
* Distinguishing "this week's milestone" from "the eventual full Portfolio Piece #2" (which will incorporate Week 7's container work and Week 8's cloud work before final completion)

### Priority
🔴 **Must Know:** how to write a README that clearly communicates what the project does, why it matters, and how each security gate works, without requiring the reader to dig through YAML
🟡 **Should Know:** how to create a simple, clear pipeline architecture diagram (even a basic flowchart) that visually communicates the gate structure
🟢 **Nice to Know:** how real open-source DevSecOps tooling projects structure their documentation as a style reference

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Writing an Effective README
* What it is: the first (and often only) document a reviewer reads — it must explain the project's purpose, architecture, and value without requiring them to read the actual pipeline YAML
* Why it matters: recalling Day 28's executive-summary lesson, a strong README serves the same function for a code project that an executive summary serves for a written report — it's the entry point that determines whether a reviewer engages further
* How it works: structure as — project purpose (1 paragraph) → architecture overview (with diagram) → the 4 security gates explained individually (what each catches, why it matters) → how to run it yourself → design decisions and trade-offs (e.g., why you chose Semgrep over an alternative, why least-privilege permissions were prioritized)
* Best practices: write for a reader who has 2 minutes to skim before deciding whether to look closer — front-load the most impressive/relevant information

### Concept 2 — Architecture Diagrams
* Definition: a simple visual representation of the pipeline's flow (trigger → parallel gates → build/deploy), making the structure immediately graspable
* Practical application: even a basic flowchart (using the same diagramming approach from Week 3's threat-modeling exercises) dramatically improves a technical reviewer's ability to quickly understand your work
* Best practices: keep it simple — box-and-arrow diagrams communicate more effectively than elaborate designs for this purpose

### Concept 3 — Milestone vs. Final Completion
* Key terminology: incremental portfolio building
* Practical application: today marks a genuine, presentable milestone (the core secure pipeline with 4 working gates), even though the curriculum's full Portfolio Piece #2 description (Week 8's Month 2 capstone) will add container hardening and cloud deployment on top of this foundation
* Best practices: it's professionally reasonable to describe this honestly in an interview as "I built the core secure CI/CD pipeline in Week 6, then extended it with container and cloud hardening in subsequent weeks" — this narrative arc is itself a demonstration of iterative, professional development practice

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| README-first documentation | Structuring docs for a quick-skimming reviewer's entry point | The same "executive summary" principle applied to a code project |
| Architecture diagram | A simple visual of the pipeline's structure | Dramatically improves reviewer comprehension speed |
| Incremental portfolio milestone | This week's deliverable as a genuine, presentable checkpoint | Demonstrates iterative professional practice, not just a finished/unfinished binary |

---

# ⏱️ 4. Study Schedule

## Session 1 — README Drafting (45–60 min)
Draft the full README structure: purpose, architecture overview, gate-by-gate explanation, how-to-run instructions, design decisions.
**Output:** A complete README draft.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Diagram & Polish (60–90 min)
1. Create a simple architecture diagram showing the pipeline's trigger → parallel gates (SAST/SCA/secret-scan) → DAST → build flow.
2. Insert the diagram into the README.
3. Clean up the repository structure (organize workflow files, custom Semgrep rules from Day 31, any supporting scripts) for clean navigability.
4. Final proofread and polish pass.

**Expected Result:** A polished, professional, published repository milestone.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems / Interview Prep (30–45 min)
### Problem 1
Write your 2–3 sentence elevator pitch for this pipeline project.
### Problem 2
Prepare an answer for: "Why did you choose Semgrep/ZAP/trufflehog specifically, rather than alternative tools?"
### Problem 3
Prepare an answer for: "What would you add to this pipeline if you had another week?" (a genuine, thoughtful answer previewing Week 7's container work is a strong response)
### Challenge
Practice presenting this milestone, live, unaided, in under 3 minutes, including the architecture diagram as a visual aid.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full Week 6 final recall sweep, now framed as "how would I present this in an interview": CI/CD attack surface, least privilege, secret management, and the 4-gate pipeline build.
### Spaced-Repetition Review
* **Yesterday:** Pipeline hardening review
* **This week:** the complete CI/CD security arc
* **4 weeks ago:** Week 2's identity/access material (a good long-interval check)

---

# 🧪 6. Active Recall Exercises
1. What are the 4 security gates in your pipeline, and what does each catch?
2. How does your README communicate this project's value to a quick-skimming reviewer?
3. Why does an architecture diagram improve reviewer comprehension?
4. What would a hiring manager look for first when reviewing this portfolio piece?
5. What's the honest scope of "this week's milestone" vs. the eventual full Portfolio Piece #2?
6. How would you defend your tool choices (Semgrep, ZAP, trufflehog) if challenged in an interview?
7. How would you extend this pipeline in coming weeks (container/cloud hardening)?
8. Difference between a README that requires reading YAML to understand vs. one that stands alone?
9. Real-world parallel: how does this pipeline's structure compare to a real company's DevSecOps tooling?
10. Teach this entire milestone to a junior developer, using it as their own future template, in under 4 minutes.

### Feynman Test
Present your Portfolio Piece #2 milestone (architecture, 4 gates, design decisions) live, unaided, in under 3 minutes. If you stumble, mark 🟡 **Needs Review** and rehearse again.

---

# 🛠️ 7. Tool Practice
**Tool:** Markdown/GitHub (README + diagram embedding) + diagramming tool (from Week 3)
### Today's Tool Goal
Produce a polished, portfolio-ready README with an embedded architecture diagram.
### Tool Success Criteria
An interviewer could open this repository cold and understand its purpose and architecture within 2 minutes, unaided.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (Week 6 milestone COMPLETE)
Today's contribution: the finalized, documented, portfolio-ready pipeline repository milestone, with a clear README and architecture diagram.
### Deliverable
`Day 42: PORTFOLIO PIECE #2 - WEEK 6 MILESTONE COMPLETE. Published finalized pipeline repository with professional README and architecture diagram (4 working security gates: SAST, SCA, secret scanning, DAST).`

---

# 📝 9. Practice / Security Challenge
### Scenario
You're now in a real interview, and the interviewer says: "I see you have a secure CI/CD pipeline in your portfolio — walk me through it."
### My Task
1. Deliver your elevator pitch.
2. Walk through the architecture diagram.
3. Explain each of the 4 gates and what it catches, referencing specific Week 1–5 vulnerability classes as concrete examples.
4. Explain your least-privilege design decisions, referencing Capital One.
5. Mention your plan to extend this with container/cloud hardening.
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
* [ ] README finalized  * [ ] Architecture diagram created and embedded  * [ ] Repository structure cleaned
* [ ] Elevator pitch rehearsed  * [ ] Practice problems  * [ ] Active-recall answers  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow (Start of Week 7)
1. Your complete Portfolio Piece #2 Week 6 milestone, cold, interview-ready
2. Every major Week 6 concept (PPE, least privilege, secret management, gate integration) at a working professional level
3. How Week 6's pipeline foundation sets up Week 7's container/Kubernetes security focus
4. What you'd add to this pipeline given more time
5. Your honest self-assessment of remaining Week 6 gaps to keep in mind during Week 7

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link to finalized Portfolio Piece #2 milestone]
**Final Status:** 🟢 / 🟡 / 🔴

**🏆 WEEK 6 MILESTONE ACHIEVED: Portfolio Piece #2's core secure CI/CD pipeline (4 gates: SAST, SCA, secret scanning, DAST) is complete and published; container and cloud hardening extensions follow in Weeks 7–8.**

---

# 14. Hands-On LAB
**Lab Platform:** N/A — today is a documentation/portfolio-finalization day
**Lab Name:** "Present the Pipeline Milestone: Full Walkthrough"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 45–60 minutes (folded into Session 3 above)
### Lab Objective
Confirm you can present this substantial technical project clearly and confidently, converting completed engineering work into a compelling interview narrative.
### Environment
Target: your own finalized repository · Tools: none, just your voice/notes · Prerequisite: Days 36–42
### Lab Tasks
1. Present the full milestone walkthrough out loud, timed, using the architecture diagram.
2. Answer one self-generated likely interview follow-up question without notes.
3. Get feedback if possible (mentor, peer, or self-review via recording).
4. Note anything you'd tighten in either the documentation or your live delivery.
5. Confirm the GitHub repo link works and displays cleanly, including the diagram rendering correctly.
### What I Need to Discover
Can you talk about this substantial technical project with genuine confidence and clarity, or does part of the architecture still feel shaky to explain under pressure? That gap, if it exists, is worth another review pass.
### Lab Success Criteria
You can present the Week 6 pipeline milestone clearly and confidently, unaided, in under 3 minutes, with the architecture diagram as a supporting visual.

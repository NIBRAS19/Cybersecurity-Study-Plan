# 🗓️ WEEK 7 — Container & Kubernetes Security (Days 43–49)

---

# 🛡️ Day 43 — Docker Security: Container Escape & Privilege Escalation

**Date:** 01/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Understanding container isolation boundaries and their failure modes
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain what a container actually isolates (namespaces, cgroups) and what it does NOT provide (a full security boundary equivalent to a VM).
* Understand container escape mechanics conceptually (privileged containers, mounted Docker socket, kernel exploits).
* Run a container with excessive privileges and observe the escape risk in a safe lab.

### Success Criteria
* Explain what namespaces/cgroups do and don't isolate, without notes.
* Identify why running a container with `--privileged` or a mounted Docker socket is dangerous.
* Demonstrate (in a safe lab) how a privileged container can affect the host.

---

# 📚 2. Topics to Study
### Primary Topic
**Docker Container Isolation and Escape Mechanics**
### Secondary Topics
* Linux namespaces (PID, network, mount, etc.) and cgroups as Docker's underlying isolation mechanisms
* The Docker socket (`/var/run/docker.sock`) as a host-level privilege escalation vector when mounted inside a container
* Why containers share the host kernel (unlike VMs), and what that implies for kernel-exploit risk

### Priority
🔴 **Must Know:** containers are NOT a full security boundary like a virtual machine — they share the host's kernel, and misconfiguration (privileged mode, mounted host resources) can grant a container process effective control over the host
🟡 **Should Know:** why mounting the Docker socket into a container is equivalent to giving that container root access to the host (it can simply ask the Docker daemon to start a new privileged container mounting the host filesystem)
🟢 **Nice to Know:** specific historical container-escape CVEs (e.g., "Dirty COW"-style kernel exploits, `runc` vulnerabilities)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — What Containers Actually Isolate
* What it is: Docker containers use Linux namespaces (process IDs, network, mounts, users, etc.) to make a process believe it has its own isolated view of the system, and cgroups to limit resource usage — but all containers on a host share the same underlying kernel
* Why it matters: this is fundamentally different from a VM, which virtualizes hardware and runs a genuinely separate kernel — a container escape can potentially compromise the host directly, since there's no hypervisor boundary to cross
* How it works: a `--privileged` container disables most of these isolation protections, giving the container near-host-equivalent capabilities
* Real-world example: numerous container-escape CVEs over the years have stemmed from kernel vulnerabilities exploitable specifically because containers share the host kernel

### Concept 2 — The Docker Socket Escalation Path
* Definition: `/var/run/docker.sock` is the Unix socket the Docker CLI uses to communicate with the Docker daemon; if this socket is mounted inside a container, any process in that container can issue Docker commands as if it were the host's Docker daemon user (typically root)
* Architecture/process: a compromised application inside a container with the socket mounted could simply run `docker run -v /:/host --privileged alpine chroot /host`, effectively becoming host root
* Attack scenario: this is a very common real-world misconfiguration, often introduced for convenience (e.g., CI runners that need to build/run Docker images from within a container) without realizing the severity of the privilege granted
* Defense/mitigation: avoid mounting the Docker socket into containers wherever possible; if genuinely required (e.g., Docker-in-Docker CI scenarios), use more restrictive alternatives (rootless Docker, dedicated build isolation)

### Concept 3 — Shared Kernel Risk
* Key terminology: kernel exploit, container breakout
* Practical application: because all containers on a host share one kernel, a kernel-level vulnerability (unrelated to Docker's own configuration) can potentially be exploited from within any container to affect the host or other containers
* Best practices: keep host kernels patched, minimize what's exposed inside containers, and treat container isolation as one layer of defense-in-depth, not an absolute security guarantee

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Namespaces/cgroups | Linux kernel features providing process/resource isolation | The actual mechanism behind container isolation — not a full VM-style boundary |
| Docker socket mount | Exposing `/var/run/docker.sock` inside a container | A common, severe host-privilege-escalation misconfiguration |
| Shared kernel | All containers on a host use the same kernel | Means kernel exploits can potentially cross container boundaries |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study Linux namespaces/cgroups, the Docker socket escalation path, and shared-kernel risk.
**Output:** Write, in your own words, why "containers" and "virtual machines" provide fundamentally different security guarantees.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. In a safe local Docker environment, run a container with the Docker socket mounted (`-v /var/run/docker.sock:/var/run/docker.sock`) and, from inside that container, use the Docker CLI to start a new privileged container mounting the host filesystem — observe the host-level access this grants.
2. Run a container with `--privileged` and observe additional capabilities it has compared to a default container.
3. Document both demonstrations clearly, noting the exact commands and observed effects.

**Expected Result:** Two documented, safely-conducted demonstrations of container escalation vectors on your own local machine.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A CI runner container mounts the Docker socket to allow building Docker images as part of the pipeline. Explain the security trade-off this creates and one safer alternative.
### Problem 2
Explain why patching the host kernel matters for container security even if your container images themselves are perfectly configured.
### Problem 3
A developer argues "containers are basically as secure as VMs since they're isolated." Correct this misconception with specific technical reasoning.
### Challenge
Design a policy (rules) for when, if ever, mounting the Docker socket into a container should be permitted in a production environment, and what compensating controls would be required.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Portfolio Piece #2 Milestone (Day 42)
Recall without notes: your pipeline's 4 gates and least-privilege design.
### Spaced-Repetition Review
* **Yesterday:** Portfolio Piece #2 milestone
* **1 week ago:** CI/CD attack surface (Day 36) — both weeks share the theme of "infrastructure-level trust boundaries," now moving from pipelines to containers

---

# 🧪 6. Active Recall Exercises
1. What do Linux namespaces and cgroups actually provide for container isolation?
2. Why is a container NOT equivalent to a VM in terms of security boundary?
3. Why is mounting the Docker socket into a container dangerous?
4. What would an attacker need to escalate from a mounted-socket container to host root (just the ability to run Docker commands)?
5. What's the impact of a shared-kernel exploit crossing container boundaries?
6. How would you detect a container running with excessive privileges in a production environment?
7. How would you prevent Docker socket exposure and privileged-mode misuse?
8. Difference between what a container isolates and what a VM isolates?
9. Real-world example: a general pattern of container-escape CVEs tied to shared-kernel architecture.
10. Teach container isolation limits to a junior developer in plain language.

### Feynman Test
Explain what containers do and don't isolate, and the Docker socket escalation path, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Docker CLI
### Today's Tool Goal
Practice inspecting a running container's capabilities and privilege level.
### Commands / Features to Practice
```text
docker run --rm -it --privileged alpine sh
docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock alpine sh
docker inspect <container> --format '{{.HostConfig.Privileged}}'
```
### Tool Success Criteria
I can identify, from `docker inspect` output, whether a running container has dangerous privilege configurations.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (container hardening is the Week 7 extension)
Today's contribution: audit your Week 6 pipeline's Docker usage (if any containers are used for building/testing) for Docker-socket or privileged-mode misconfigurations.
### Deliverable
`Day 43: Audited pipeline's container usage for socket-mount/privileged-mode risks; documented container isolation demonstration.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A CI/CD system runs build jobs in containers with the Docker socket mounted "for convenience" so builds can create Docker images without nested virtualization complexity.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact (a compromised build step could gain full host access).
4. Recommend a mitigation (rootless Docker, Kaniko-style daemonless image building, or dedicated isolated build runners).
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
* [ ] Study notes  * [ ] Docker socket escalation demo completed  * [ ] Privileged container demo completed
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. What containers isolate and what they don't
2. The Docker socket escalation path
3. Shared-kernel risk
4. Your audit findings on the pipeline's container usage
5. How tomorrow's hardened-Dockerfile topic directly addresses the risks identified today

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Docker environment
**Lab Name:** "Demonstrate the Docker Socket Escalation Path"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Safely demonstrate, on your own local machine, how a mounted Docker socket grants effective host root access.
### Environment
Target: local Docker Desktop/Engine · Tools: Docker CLI · Prerequisite: today's concepts
### Lab Tasks
1. Start a container with the Docker socket mounted.
2. From inside that container, use the Docker CLI to launch a new privileged container mounting the host's root filesystem.
3. From within that new container, access host files (e.g., read a file outside the original container's filesystem).
4. Document the exact command chain and observed host access.
5. Write the mitigation recommendation.
### What I Need to Discover
How many steps, realistically, separate "convenient CI configuration" from "full host compromise"? Does seeing this chain executed concretely change how you'd evaluate a Docker socket mount request in a real code review?
### Lab Success Criteria
Documented, safely-conducted demonstration of the full escalation chain with clear mitigation recommendation.

---
---

# 🛡️ Day 44 — Hardened Dockerfiles: Minimal Base Images, Non-Root Users, Multi-Stage Builds

**Date:** 02/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Writing security-hardened Dockerfiles
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain why minimal base images (e.g., `alpine`, `distroless`) reduce attack surface compared to full OS images.
* Explain why running container processes as a non-root user limits the impact of a container compromise.
* Write a hardened Dockerfile using multi-stage builds, a minimal base image, and a non-root user, for a real Node.js application.

### Success Criteria
* Explain minimal base images' security benefit without notes.
* Explain why non-root container processes matter, connecting to Day 43's escape-risk discussion.
* Produce a working, hardened Dockerfile with a demonstrated before/after image-size and attack-surface comparison.

---

# 📚 2. Topics to Study
### Primary Topic
**Dockerfile Hardening: Minimal Images, Non-Root Users, Multi-Stage Builds**
### Secondary Topics
* Distroless images (containing only the application and its runtime dependencies, no shell/package manager)
* The `USER` directive and why it matters even given Day 43's shared-kernel caveats
* Multi-stage builds for separating build-time dependencies from the final runtime image

### Priority
🔴 **Must Know:** every unnecessary binary, package, or tool included in a container image is additional attack surface if the application is ever compromised — a minimal image means an attacker who achieves code execution inside the container has fewer tools available to escalate or pivot
🟡 **Should Know:** running as a non-root user inside the container doesn't eliminate Day 43's shared-kernel/escape risks entirely, but it significantly raises the difficulty bar for many escalation techniques and limits the damage of many application-level compromises even without a full escape
🟢 **Nice to Know:** Google's "distroless" image family as a specific minimal-image option

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Minimal Base Images
* What it is: choosing a base image (`alpine`, `distroless`, or a `scratch`-based build) that contains only what's strictly necessary to run the application, rather than a full OS distribution with a shell, package manager, and dozens of unused utilities
* Why it matters: if an attacker achieves code execution within the container (e.g., via an application vulnerability from Weeks 1–3), a minimal image denies them common post-exploitation tools (no `bash`, no `curl`, no package manager to install more tools)
* How it works: `FROM node:20-alpine` instead of `FROM node:20` dramatically reduces image size and included tooling; `distroless` images go further, often lacking even a shell entirely
* Real-world example: numerous real-world container compromises were significantly limited in scope because the compromised container lacked the tools an attacker needed for further lateral movement

### Concept 2 — Non-Root Container Users
* Definition: explicitly setting the container's runtime user to a non-root, unprivileged user via the `USER` directive, rather than defaulting to root
* Architecture/process: `RUN adduser -D appuser && USER appuser` (Alpine syntax) ensures the application process itself runs without root privileges inside the container
* Attack scenario: even without a full container escape (Day 43), a non-root process significantly limits what an attacker can do if they achieve code execution within the container (can't modify system files, can't bind to privileged ports, etc.)
* Defense/mitigation: always set a non-root `USER` in your Dockerfile; combine with minimal base images for maximum effect

### Concept 3 — Multi-Stage Builds
* Key terminology: build stage, runtime stage, `COPY --from=`
* Practical application: use a full-featured image (with compilers, build tools, dev dependencies) for the *build* stage, then copy only the final compiled/built artifacts into a minimal *runtime* stage — the final image never includes the build tools at all
* Best practices: this is the standard modern pattern for producing genuinely minimal production images while still having full tooling available during the build process itself

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Minimal base image | An image containing only what's strictly necessary | Denies attackers common post-exploitation tools if code execution is achieved |
| Non-root `USER` | Running the container process as an unprivileged user | Limits damage even without a full container escape |
| Multi-stage build | Separate build and runtime image stages | Achieves minimal runtime images while retaining full build tooling |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study minimal base images, non-root users, and multi-stage build patterns.
**Output:** Write, in your own words, why a minimal, non-root, multi-stage-built image reduces the practical impact of an application-level compromise (from Weeks 1–3) even without addressing Day 43's deeper container-escape risks.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Take a real Node.js application (your own project or a simple sample app).
2. Write a "before" Dockerfile: full `node` base image, root user, single stage.
3. Write an "after" hardened Dockerfile: `node:alpine` (or distroless) base image, multi-stage build, explicit non-root `USER`.
4. Build both images and compare their sizes.
5. Attempt to run common post-exploitation commands (`curl`, `bash`, `apt`) inside both images' running containers and document the difference.

**Expected Result:** A documented before/after comparison (image size + available tooling) with both Dockerfiles saved.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A Dockerfile uses `FROM node:20` and never sets a `USER` directive. List at least 2 concrete hardening changes you'd make and why.
### Problem 2
Explain why a multi-stage build lets you use a compiler/build tool (like `gcc` or full `npm install` with dev dependencies) without that tooling ending up in the final production image.
### Problem 3
A team resists switching to `distroless` because "we need a shell for debugging in production." Propose an alternative debugging strategy that doesn't require permanently including a shell in the production image.
### Challenge
Write a complete hardened Dockerfile (multi-stage, minimal base, non-root user, and any other hardening you can justify) for a Python Flask application, from scratch.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Docker Security Fundamentals (Day 43)
Recall without notes: namespaces/cgroups, Docker socket escalation, shared-kernel risk.
### Spaced-Repetition Review
* **Yesterday:** Docker security fundamentals
* **1 week + 1 day ago:** Least-privilege CI tokens (Day 37) — the same least-privilege principle applies to container users today

---

# 🧪 6. Active Recall Exercises
1. Why does a minimal base image reduce attack surface?
2. How does a non-root `USER` limit the impact of a compromise?
3. How do multi-stage builds achieve minimal runtime images while retaining full build tooling?
4. What would an attacker need to exploit a compromised container that lacks a shell/package manager (much more effort, likely requiring a full escape rather than simple post-exploitation)?
5. What's the impact difference between a root-user compromise and a non-root-user compromise within the same container?
6. How would you verify a container image doesn't include unnecessary tooling?
7. How would you convert a legacy single-stage Dockerfile into a hardened multi-stage one?
8. Difference between `alpine` and `distroless` base image philosophies?
9. Real-world example: how minimal images limited real-world container compromise scope.
10. Teach Dockerfile hardening to a junior developer in plain language.

### Feynman Test
Explain minimal images, non-root users, and multi-stage builds together in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Docker CLI + Dockerfile authoring
### Today's Tool Goal
Write, build, and compare hardened vs. unhardened Dockerfiles.
### Commands / Features to Practice
```dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
RUN adduser -D appuser
WORKDIR /app
COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
USER appuser
CMD ["node", "dist/index.js"]
```
### Tool Success Criteria
I can write a correct multi-stage, minimal, non-root Dockerfile from scratch without copying a template verbatim.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (this becomes a new pipeline gate/artifact this week)
Today's contribution: write the hardened Dockerfile for your pipeline's application, to be scanned by Trivy/Grype tomorrow and eventually integrated as a build step in your pipeline.
### Deliverable
`Day 44: Wrote hardened multi-stage Dockerfile (minimal base image, non-root user) for pipeline application; documented before/after comparison.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A security review finds a production container running as root, based on a full `ubuntu:latest` image with `curl`, `wget`, `python`, and a full shell all installed, none of which the application itself uses.
### My Task
1. Identify the vulnerability/threat (excessive attack surface + root user).
2. Explain the root cause (likely a Dockerfile written for developer convenience, never hardened for production).
3. Determine the impact if the application were compromised.
4. Recommend a mitigation (minimal base image, multi-stage build, non-root user).
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
* [ ] Study notes  * [ ] Before/after Dockerfile comparison completed  * [ ] Hardened Dockerfile written for pipeline app
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Minimal base images, non-root users, and multi-stage builds
2. Your before/after hardening comparison results
3. How to write a hardened Dockerfile from scratch
4. How today's hardening reduces (but doesn't eliminate) Day 43's escape risks
5. How tomorrow's image-scanning topic (Trivy/Grype) verifies your hardened image is actually free of known vulnerabilities

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Docker environment
**Lab Name:** "Harden a Real Application's Dockerfile: Before/After"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Produce a genuinely hardened Dockerfile for your pipeline's application, with measured before/after improvements.
### Environment
Target: your pipeline project's application · Tools: Docker · Prerequisite: today's concepts
### Lab Tasks
1. Write the "before" Dockerfile if you don't already have one.
2. Build it and record image size.
3. Write the hardened "after" Dockerfile.
4. Build it and record image size.
5. Attempt post-exploitation-style commands (`curl`, `bash`) in both running containers and document the difference.
### What I Need to Discover
How large is the practical difference in both image size and available attacker tooling between an unhardened and hardened image for the same application? Does this concretely demonstrate the value of hardening in a way the concept alone didn't?
### Lab Success Criteria
Documented before/after comparison (size + tooling availability) with both Dockerfiles saved to the pipeline repository.

---
---

# 🛡️ Day 45 — Image Scanning & Supply Chain: Trivy, Grype, Cosign

**Date:** 03/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Container image vulnerability scanning and image signing
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Run Trivy and Grype against your hardened Dockerfile's built image to verify it's free of known-vulnerable packages.
* Explain image-signing (Cosign) and why it matters for supply-chain integrity.
* Understand typosquatting risk in public image registries.

### Success Criteria
* Run and interpret Trivy/Grype scans against a real image.
* Explain what image signing verifies and why it matters.
* Explain typosquatting risk in the context of pulling public base images.

---

# 📚 2. Topics to Study
### Primary Topic
**Container Image Scanning & Supply Chain Security**
### Secondary Topics
* Trivy and Grype: scanning built images for known-vulnerable OS packages and application dependencies (extending Day 34's SCA concept to the container layer)
* Cosign: cryptographically signing images so consumers can verify they haven't been tampered with and genuinely came from the claimed source
* Typosquatting in public image registries (e.g., a malicious `pytnon:latest` image targeting typos of `python`)

### Priority
🔴 **Must Know:** even a hardened Dockerfile (Day 44) can still include vulnerable OS-level packages inherited from its base image — Trivy/Grype specifically scan for these known-CVE packages within the built image itself
🟡 **Should Know:** image signing (Cosign) verifies an image's authenticity and integrity — without it, a compromised registry or man-in-the-middle could substitute a malicious image with no way for the consumer to detect it
🟢 **Nice to Know:** how typosquatted images have been used historically to distribute malware to careless developers

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Image Scanning with Trivy/Grype
* What it is: tools that scan a built container image's layers for OS packages and application dependencies with known CVEs, similar in spirit to Day 34's `npm audit` but operating at the full image level (including the base OS layer, not just application dependencies)
* Why it matters: even your hardened, minimal `node:20-alpine` image from Day 44 could still include a vulnerable version of a system library — image scanning catches this layer that neither SAST, DAST, nor application-level SCA would see
* How it works: `trivy image myapp:latest` downloads/uses a vulnerability database and reports every known-CVE package found within the image's layers, with severity ratings
* Real-world example: numerous organizations have discovered, via image scanning, that their "minimal" base images still contained years-old unpatched OS packages with known critical CVEs

### Concept 2 — Image Signing with Cosign
* Definition: cryptographically signing a container image after building it, so that anyone pulling the image can verify it came from the claimed source and hasn't been tampered with since signing
* Architecture/process: `cosign sign` attaches a signature to the image in the registry; `cosign verify` checks that signature against a known public key before allowing deployment
* Attack scenario this prevents: an attacker who compromises a registry (or intercepts image pulls) and substitutes a malicious image would be caught by signature verification failing
* Defense/mitigation: sign every image your pipeline builds, and configure your deployment environment to refuse unsigned or invalid-signature images

### Concept 3 — Typosquatting in Public Registries
* Key terminology: typosquatting, registry namespace
* Practical application: an attacker publishes a malicious image with a name deliberately similar to a popular legitimate image (a common misspelling), hoping developers will pull it by mistake
* Best practices: always pull from verified/official image namespaces, pin exact image digests (not just tags, which can be reassigned) for production builds, and combine with signature verification

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Image scanning (Trivy/Grype) | Checking a built image's layers for known-CVE packages | Catches vulnerable OS/base-layer packages that other tools don't see |
| Image signing (Cosign) | Cryptographic verification of image authenticity/integrity | Prevents malicious image substitution via compromised registries |
| Typosquatting | Malicious images with names similar to legitimate ones | A supply-chain risk distinct from known-CVE vulnerabilities |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study Trivy/Grype's scanning model, Cosign's signing/verification flow, and typosquatting risk.
**Output:** Write, in your own words, why image scanning is necessary even for a hardened, minimal image from Day 44.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Run Trivy against your Day 44 hardened image and document findings.
2. Run Grype against the same image for comparison.
3. Install Cosign and sign your image (using a local key pair for practice); verify the signature successfully.
4. Deliberately corrupt/re-tag the image and confirm verification now fails.

**Expected Result:** Documented Trivy/Grype scan results + a working sign/verify demonstration (including a deliberate failure case).

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Trivy reports a "Critical" vulnerability in a base-layer OS package that your application doesn't even directly use. Explain why this still matters and how you'd remediate it (likely: update the base image tag/version).
### Problem 2
Explain, step by step, how Cosign's sign/verify process would catch a registry-level image substitution attack.
### Problem 3
A developer runs `docker pull pytnon:latest` due to a typo. Explain the risk and how pinning to a verified, exact image digest would have prevented this class of mistake even without the typo being caught.
### Challenge
Design a complete "trusted image supply chain" policy: base image sourcing, scanning thresholds, signing requirements, and deployment-time verification enforcement.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Hardened Dockerfiles (Day 44)
Recall without notes: minimal base images, non-root users, multi-stage builds.
### Spaced-Repetition Review
* **Yesterday:** Hardened Dockerfiles
* **1 week + 6 days ago:** SCA & SBOM generation (Day 34) — directly extended today to the image layer

---

# 🧪 6. Active Recall Exercises
1. What does Trivy/Grype scan for that application-level SCA (Day 34) doesn't see?
2. How does Cosign's signing/verification process work?
3. Why does typosquatting create risk even for careful developers?
4. What would you need to properly verify image authenticity before deployment (a known public key + enforced verification policy)?
5. What's the impact of deploying an unscanned, unverified image pulled from an untrusted source?
6. How would you integrate Trivy scanning into your Week 6 pipeline as an additional gate?
7. How would you enforce signature verification at deployment time?
8. Difference between application-level SCA (Day 34) and image-level scanning (today)?
9. Real-world example: organizations discovering vulnerable OS packages in "minimal" images via scanning.
10. Teach image scanning and signing to a junior developer in plain language.

### Feynman Test
Explain image scanning, signing, and typosquatting risk together in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Trivy, Grype, Cosign
### Today's Tool Goal
Run all three tools against a real built image and demonstrate a full scan-sign-verify workflow.
### Commands / Features to Practice
```text
trivy image myapp:latest
grype myapp:latest
cosign generate-key-pair
cosign sign --key cosign.key myapp:latest
cosign verify --key cosign.pub myapp:latest
```
### Tool Success Criteria
I can run a complete scan-sign-verify workflow and correctly interpret both success and (deliberately-induced) failure cases.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (image scanning becomes a new pipeline gate)
Today's contribution: add Trivy scanning as a documented step you'll integrate into your pipeline (extending the 4 gates from Week 6 with a 5th: container image scanning), and document your image-signing approach.
### Deliverable
`Day 45: Ran Trivy/Grype scans against hardened image; demonstrated Cosign sign/verify workflow; documented 5th pipeline gate design.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A company deploys container images directly from a public registry without any scanning or signature verification, trusting tags alone (e.g., `myapp:latest`) which can be silently reassigned to point to different image content at any time.
### My Task
1. Identify the vulnerability/threat (no integrity guarantee, no vulnerability visibility).
2. Explain the root cause.
3. Determine the impact (a compromised or malicious image could be deployed without detection).
4. Recommend a mitigation (Trivy scanning gate + Cosign signature enforcement + digest pinning).
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
* [ ] Study notes  * [ ] Trivy/Grype scans completed  * [ ] Cosign sign/verify workflow demonstrated
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Image scanning's role distinct from application-level SCA
2. Cosign's sign/verify workflow
3. Typosquatting risk
4. Your documented 5th pipeline gate design
5. How tomorrow's Kubernetes security topic builds on today's image-integrity foundation (a K8s cluster ultimately pulls and runs these same images)

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Docker environment
**Lab Name:** "Full Image Supply-Chain Workflow: Scan, Sign, Verify, Fail"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Demonstrate a complete image supply-chain security workflow, including a deliberate failure case proving verification actually works.
### Environment
Target: your Day 44 hardened image · Tools: Trivy, Grype, Cosign · Prerequisite: today's concepts
### Lab Tasks
1. Scan the image with both Trivy and Grype; document findings.
2. Sign the image with Cosign using a local key pair.
3. Verify the signature successfully.
4. Modify/re-tag the image (simulating tampering) and confirm verification now fails.
5. Document the complete workflow and the specific failure demonstration.
### What I Need to Discover
Does actually seeing verification fail against a tampered image make the value of signing concrete in a way the concept alone didn't? What would this have caught in a real supply-chain compromise scenario?
### Lab Success Criteria
Documented complete scan-sign-verify workflow with a demonstrated, genuine failure case when the image is tampered with.

---
---

# 🛡️ Day 46 — Kubernetes Security Part 1: Pod Escape & Exposed Dashboards

**Date:** 04/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Kubernetes architecture and common misconfiguration risks
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand Kubernetes' core architecture (pods, nodes, the API server) at a level sufficient to reason about security.
* Explain pod escape risk, extending Day 43's container-escape concepts to a multi-node orchestration context.
* Explain why an exposed, unauthenticated Kubernetes dashboard is a critical vulnerability, directly previewing tomorrow's Tesla case study.

### Success Criteria
* Explain Kubernetes' basic architecture without notes.
* Explain pod escape risk and its relationship to Day 43's container escape concepts.
* Explain why dashboard exposure is catastrophic, in preparation for Day 49's case study.

---

# 📚 2. Topics to Study
### Primary Topic
**Kubernetes Architecture & Pod/Dashboard Security Risks**
### Secondary Topics
* Core Kubernetes concepts: pods, nodes, the control plane/API server, `kubectl`
* Pod escape (a container escape, Day 43, within the additional context of a multi-node cluster where escape could mean lateral movement across nodes)
* Exposed, unauthenticated Kubernetes dashboards as a historically common, severe misconfiguration

### Priority
🔴 **Must Know:** Kubernetes orchestrates many containers (grouped into pods) across many nodes, managed via an API server that `kubectl` and other tools communicate with — if that API server (or a dashboard providing a UI to it) is exposed without authentication, an attacker effectively gains control of the entire cluster
🟡 **Should Know:** a pod escape (breaking out of a container within a pod to affect the underlying node) is fundamentally the same class of risk as Day 43's container escape, just with the added consequence of potentially compromising an entire cluster node that may host many other pods/tenants
🟢 **Nice to Know:** Kubernetes' RBAC (Role-Based Access Control) model, previewed in depth tomorrow

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Kubernetes Core Architecture
* What it is: Kubernetes manages containerized applications across a cluster of machines (nodes); a "pod" is the smallest deployable unit (one or more tightly-coupled containers); the control plane (including the API server) manages the desired state of the entire cluster
* Why it matters: understanding this architecture is essential before reasoning about where security controls need to apply — the API server is the single most powerful control point in a cluster
* How it works: `kubectl` (and any other cluster management tool, including a dashboard) communicates with the API server, which then schedules and manages pods across the cluster's nodes
* Real-world example: this architecture directly explains why the Tesla incident (previewed for tomorrow) was so severe — an exposed dashboard is essentially an exposed API server front-end

### Concept 2 — Pod Escape as Extended Container Escape
* Definition: a container escape (Day 43) occurring within a Kubernetes pod, potentially allowing an attacker to affect not just the container's host but the entire node, and from there potentially other pods scheduled on that same node
* Architecture/process: the same misconfigurations from Day 43 (privileged containers, mounted host paths) apply directly within Kubernetes, often configured via a Pod's `securityContext`
* Attack scenario: a pod running with `privileged: true` or with sensitive host paths mounted creates the same escalation risk as Day 43's standalone Docker example, but now within a shared multi-tenant cluster

### Concept 3 — Exposed Kubernetes Dashboards
* Key terminology: unauthenticated API access, cluster-admin-equivalent access
* Practical application: the Kubernetes Dashboard is a web UI providing broad cluster management capabilities; if deployed without proper authentication (a historically common default-configuration mistake), anyone who can reach it over the network has effective control over the entire cluster — creating, deleting, or modifying any workload
* Best practices: never expose the Kubernetes dashboard (or the API server itself) directly to the public internet without strong authentication; this is the exact setup that led to real, well-documented incidents

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Pod | The smallest deployable unit in Kubernetes, one or more containers | The context within which Day 43's escape risks apply, with added cluster-wide implications |
| API server | The central control-plane component managing cluster state | The single most powerful control point — exposure here means cluster-wide compromise |
| Exposed dashboard | An unauthenticated web UI providing cluster management access | A historically common, catastrophic misconfiguration (previewed for tomorrow's case study) |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study Kubernetes' core architecture (pods, nodes, API server) and pod-escape/dashboard-exposure risks.
**Output:** Draw (or describe in writing) a simple diagram of Kubernetes architecture, labeling where the API server sits and why its exposure is so critical.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Set up a local Kubernetes environment (minikube or kind) if not already available.
2. Deploy a simple pod with a `securityContext` allowing `privileged: true` and observe the additional capabilities available compared to a securely-configured pod.
3. Research (via documentation/reputable case studies) how the Kubernetes Dashboard's authentication model works and what a misconfigured, exposed deployment looks like.

**Expected Result:** A working local Kubernetes environment with a documented privileged-pod demonstration + dashboard-exposure research notes.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A pod's `securityContext` sets `privileged: true` "to allow the application to access certain hardware features." Explain the security trade-off and what alternative approach (fine-grained Linux capabilities instead of full privilege) would be preferable.
### Problem 2
Explain, using today's architecture understanding, exactly why an exposed Kubernetes Dashboard is roughly equivalent to exposing the entire cluster's API server without authentication.
### Problem 3
Draw the escalation path from "compromised container in a pod" to "compromised node" to "potential access to other tenants' pods on that node."
### Challenge
Design a checklist for reviewing a Kubernetes deployment manifest for pod-escape risk (privileged mode, host path mounts, capabilities) before it's applied to a cluster.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Image Scanning & Supply Chain (Day 45)
Recall without notes: Trivy/Grype scanning and Cosign's sign/verify workflow.
### Spaced-Repetition Review
* **Yesterday:** Image scanning & supply chain
* **1 week + 3 days ago:** Docker security fundamentals (Day 43) — directly extended today

---

# 🧪 6. Active Recall Exercises
1. What are the core components of Kubernetes architecture (pods, nodes, API server)?
2. How does pod escape relate to Day 43's container escape concepts?
3. Why is the API server the most critical control point in a cluster?
4. What would an attacker need to exploit an exposed Kubernetes dashboard (just network reachability, if no authentication is configured)?
5. What's the impact of gaining unauthenticated access to a cluster's API server/dashboard?
6. How would you detect a privileged pod running unnecessarily in a cluster?
7. How would you prevent dashboard exposure (network policies, authentication requirements, avoiding public exposure entirely)?
8. Difference between a single-container Docker escape (Day 43) and a Kubernetes pod escape's broader cluster implications?
9. Real-world example: previewing tomorrow's Tesla Kubernetes dashboard breach case study.
10. Teach Kubernetes' core architecture and its security implications to a junior developer in plain language.

### Feynman Test
Explain Kubernetes' architecture and why pod escape/dashboard exposure are severe risks, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** `kubectl`, minikube/kind
### Today's Tool Goal
Deploy and inspect pods, including reviewing their `securityContext` configuration.
### Commands / Features to Practice
```text
minikube start
kubectl apply -f privileged-pod.yaml
kubectl get pod <name> -o yaml | grep -A5 securityContext
kubectl describe pod <name>
```
### Tool Success Criteria
I can deploy a pod and inspect its security configuration via `kubectl`, identifying any dangerous settings.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (Kubernetes deployment hardening is a natural extension, connecting Weeks 6–7)
Today's contribution: document a basic Kubernetes deployment manifest for your pipeline's application, noting the `securityContext` settings you'll harden further tomorrow (RBAC) and Day 48 (network policies).
### Deliverable
`Day 46: Set up local Kubernetes environment; documented pod security context risks; drafted initial deployment manifest for pipeline application.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A DevOps team deploys the Kubernetes Dashboard for convenient cluster monitoring and exposes it via a public LoadBalancer service, without configuring any authentication, planning to "add auth later."
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause.
3. Determine the impact (full, unauthenticated cluster control available to anyone who discovers the exposed IP/port).
4. Recommend an immediate mitigation and a longer-term secure access pattern (e.g., port-forwarding via authenticated `kubectl` access rather than public exposure).
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
* [ ] Study notes  * [ ] Local Kubernetes environment set up  * [ ] Privileged pod demonstration completed
* [ ] Dashboard exposure research notes  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Kubernetes' core architecture
2. Pod escape's relationship to container escape
3. Why exposed dashboards are catastrophic
4. Your local Kubernetes environment and initial deployment manifest
5. How tomorrow's RBAC/network-policy topic provides the fine-grained access control that would have prevented today's exposed-dashboard scenario

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Kubernetes (minikube/kind)
**Lab Name:** "Deploy and Audit a Privileged Pod"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Deploy a deliberately privileged pod in a local cluster and audit its configuration, connecting today's concepts to hands-on `kubectl` practice.
### Environment
Target: local minikube/kind cluster · Tools: `kubectl` · Prerequisite: today's concepts
### Lab Tasks
1. Write a pod manifest with `privileged: true` and a host path mount.
2. Deploy it and confirm it runs.
3. Use `kubectl exec` to access the pod and demonstrate the elevated access it has to the underlying node (e.g., access to host filesystem via the mounted path).
4. Rewrite the manifest with a properly restricted `securityContext` (no privilege, no host mounts) and redeploy.
5. Document the before/after security posture difference.
### What I Need to Discover
How directly does this mirror your Day 43 Docker-level demonstration, just at the Kubernetes orchestration layer? Does seeing the same fundamental risk appear again at a higher level of abstraction reinforce how pervasive this pattern is across container tooling?
### Lab Success Criteria
Documented before/after pod security configuration with demonstrated elevated access in the vulnerable version.

---
---

# 🛡️ Day 47 — Kubernetes Security Part 2: RBAC Hardening & Network Policies

**Date:** 05/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Kubernetes RBAC and network policy configuration
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain Kubernetes RBAC (Roles, RoleBindings, ClusterRoles) and how it implements least-privilege access within a cluster.
* Configure a least-privilege RBAC role for a specific application/service account.
* Configure a NetworkPolicy restricting pod-to-pod communication to only what's necessary.

### Success Criteria
* Explain RBAC's Role/RoleBinding model without notes, connecting directly to Day 37's least-privilege CI token concept.
* Write a working, least-privilege RBAC configuration for a sample service account.
* Write a working NetworkPolicy restricting traffic to only required paths.

---

# 📚 2. Topics to Study
### Primary Topic
**Kubernetes RBAC & Network Policies**
### Secondary Topics
* Roles/ClusterRoles (what actions are permitted) and RoleBindings/ClusterRoleBindings (who gets those permissions)
* Service accounts as the identity Kubernetes workloads use to interact with the API server
* NetworkPolicies for restricting which pods can communicate with which other pods (default-deny plus explicit allow rules)

### Priority
🔴 **Must Know:** RBAC is the exact same least-privilege principle from Day 37's CI tokens and Day 14's Capital One IAM lesson, applied within Kubernetes — every service account should have only the specific API permissions its workload genuinely needs
🟡 **Should Know:** by default, Kubernetes networking often allows any pod to communicate with any other pod in the cluster unless NetworkPolicies explicitly restrict this — a default-allow posture that's rarely appropriate for production
🟢 **Nice to Know:** the difference between namespace-scoped Roles and cluster-wide ClusterRoles

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — RBAC as Least Privilege, Again
* What it is: a Role defines a set of permitted actions (verbs like get/list/create/delete) on specific resources (pods, secrets, deployments); a RoleBinding grants that Role to a specific user or service account
* Why it matters: this is the third time in this curriculum you've encountered the exact same least-privilege principle (Capital One's IAM on Day 14, CI tokens on Day 37, now Kubernetes RBAC) — recognizing this as one recurring pattern across different technology layers is itself a mark of mature security thinking
* How it works: a service account for an application that only needs to read its own ConfigMap should never have a Role granting it permission to list Secrets cluster-wide or delete other deployments
* Real-world example: an over-permissioned service account, if its pod is compromised (via any Week 1–3-style application vulnerability), lets an attacker use that pod's Kubernetes API access to move laterally within the cluster — directly analogous to Capital One's IAM-enabled blast radius

### Concept 2 — Service Accounts
* Definition: the identity a pod uses to authenticate to the Kubernetes API server, distinct from human user accounts
* Practical application: every pod is automatically assigned a service account (often a permissive "default" one if not explicitly configured), and that service account's RBAC permissions determine what the pod's processes can do against the cluster API if compromised
* Best practices: create dedicated, narrowly-scoped service accounts per application/workload rather than relying on defaults

### Concept 3 — NetworkPolicies
* Key terminology: default-deny, ingress/egress rules, pod selector
* Practical application: without NetworkPolicies, a compromised pod can typically reach any other pod in the cluster over the network; a default-deny NetworkPolicy combined with specific allow rules (e.g., "the frontend pod may reach the backend pod on port 8080, nothing else") dramatically limits lateral movement
* Best practices: start with a default-deny-all policy per namespace, then add specific, minimal allow rules — the network equivalent of RBAC's least-privilege principle

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| RBAC (Role/RoleBinding) | Kubernetes' permission model for API access | The third recurrence of the least-privilege pattern from Capital One and CI tokens |
| Service account | The identity a pod uses to authenticate to the API server | Determines what a compromised pod could do against the cluster API |
| NetworkPolicy | Rules restricting pod-to-pod network communication | Limits lateral movement from a compromised pod, network-level least privilege |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study RBAC's Role/RoleBinding model, service accounts, and NetworkPolicy's default-deny pattern.
**Output:** Write, explicitly, the three-way parallel between Capital One's IAM (Day 14), CI tokens (Day 37), and Kubernetes RBAC (today) as the same recurring least-privilege principle.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Create a dedicated service account for your Day 46 sample application.
2. Write a least-privilege Role granting only the specific permissions that application genuinely needs (e.g., read its own ConfigMap, nothing else).
3. Write a RoleBinding connecting the service account to the Role.
4. Write a NetworkPolicy implementing default-deny plus a specific allow rule for necessary traffic.
5. Test that the least-privilege configuration works for legitimate use but denies unauthorized actions/traffic.

**Expected Result:** A working, tested least-privilege RBAC configuration + a working, tested NetworkPolicy.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A pod's default service account has cluster-admin-equivalent permissions (a common default-configuration mistake). Write the least-privilege Role/RoleBinding that should replace this.
### Problem 2
Explain why a default-allow-all networking posture in Kubernetes creates the same kind of risk that a flat, unsegmented network created in the Capital One breach.
### Problem 3
Design the specific NetworkPolicy rules for a 3-tier application (frontend, backend, database) where only frontend→backend and backend→database communication should be permitted.
### Challenge
Draft a complete "Kubernetes least-privilege checklist" combining today's RBAC and NetworkPolicy work with yesterday's pod-security-context hardening, ready to apply to any new deployment.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Pod Escape & Exposed Dashboards (Day 46)
Recall without notes: Kubernetes architecture and dashboard-exposure risk.
### Spaced-Repetition Review
* **Yesterday:** Pod escape & exposed dashboards
* **1 week + 1 day ago:** Least-privilege CI tokens (Day 37) — directly connected to today's RBAC topic

---

# 🧪 6. Active Recall Exercises
1. What is the RBAC Role/RoleBinding model?
2. What is a service account, and why does its scoping matter?
3. How do NetworkPolicies implement network-level least privilege?
4. What would an attacker need to exploit an over-permissioned service account (a compromised pod using that account, plus the account's excessive API permissions)?
5. What's the impact of default-allow-all networking in a cluster if one pod is compromised?
6. How would you audit an existing cluster for over-permissioned RBAC roles?
7. How would you implement a default-deny NetworkPolicy baseline for a namespace?
8. Difference between namespace-scoped Roles and cluster-wide ClusterRoles?
9. Real-world example: connect today's RBAC least-privilege principle explicitly to Capital One's IAM lesson.
10. Teach the three-way least-privilege parallel (IAM, CI tokens, RBAC) to a junior developer in plain language.

### Feynman Test
Explain Kubernetes RBAC and NetworkPolicies, explicitly connecting them to the recurring least-privilege pattern, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** `kubectl` (RBAC and NetworkPolicy authoring)
### Today's Tool Goal
Write, apply, and test RBAC and NetworkPolicy configurations.
### Commands / Features to Practice
```text
kubectl create serviceaccount myapp-sa
kubectl apply -f role.yaml
kubectl apply -f rolebinding.yaml
kubectl apply -f networkpolicy.yaml
kubectl auth can-i get secrets --as=system:serviceaccount:default:myapp-sa
```
### Tool Success Criteria
I can write and apply RBAC/NetworkPolicy configurations and use `kubectl auth can-i` to verify least-privilege enforcement works correctly.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (Kubernetes hardening extends the pipeline's deployment target)
Today's contribution: add the least-privilege RBAC and NetworkPolicy configurations to your pipeline application's Kubernetes manifests, extending Day 46's initial deployment.
### Deliverable
`Day 47: Configured least-privilege RBAC (Role/RoleBinding) and default-deny NetworkPolicy for pipeline application; tested enforcement with kubectl auth can-i.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A company's Kubernetes cluster has no NetworkPolicies configured at all (the default), and every pod's service account uses the namespace's default account, which has been granted broad permissions over time by various past troubleshooting sessions that were never reverted.
### My Task
1. Identify the vulnerability/threat (both network and RBAC over-permissioning, compounding risk).
2. Explain the root cause (permissions creep over time without regular audit).
3. Determine the impact if any single pod is compromised (broad lateral movement both at the network and API level).
4. Recommend a remediation plan (RBAC audit and tightening, default-deny NetworkPolicy rollout).
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
* [ ] Study notes  * [ ] RBAC Role/RoleBinding configured and tested  * [ ] NetworkPolicy configured and tested
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. RBAC's Role/RoleBinding model and its least-privilege parallel to IAM/CI tokens
2. Service account scoping
3. NetworkPolicy's default-deny pattern
4. Your tested RBAC and NetworkPolicy configurations
5. How tomorrow's lab day (KillerCoda scenarios) will stress-test these concepts against guided real-world scenarios

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local Kubernetes (minikube/kind)
**Lab Name:** "Least-Privilege RBAC and Network Policy End-to-End"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Configure and prove, with actual `kubectl` tests, that both RBAC and NetworkPolicy least-privilege configurations work correctly.
### Environment
Target: local cluster · Tools: `kubectl` · Prerequisite: today's concepts
### Lab Tasks
1. Create the dedicated service account, Role, and RoleBinding.
2. Use `kubectl auth can-i` to confirm the account can perform its intended action but NOT unauthorized ones.
3. Apply a default-deny NetworkPolicy to the namespace.
4. Add a specific allow rule and confirm intended traffic works while unintended traffic is blocked.
5. Document all test results.
### What I Need to Discover
Does your least-privilege configuration actually hold up under direct testing, or did you miss a permission the application genuinely needs (a common real-world RBAC tuning challenge)? How would you iterate if a legitimate action was unexpectedly denied?
### Lab Success Criteria
Documented, tested RBAC and NetworkPolicy configurations with confirmed allow/deny behavior matching intent.

---
---

# 🛡️ Day 48 — Lab Day: KillerCoda Kubernetes Security Scenarios

**Date:** 06/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Applying Kubernetes security concepts in guided, scenario-based labs
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Complete KillerCoda's free Kubernetes security scenarios, reinforcing Days 43–47's concepts in guided, differently-structured practice environments.
* Identify and close any remaining backlog from this week's hands-on exercises.
* Build confidence handling Kubernetes security scenarios you didn't design yourself.

### Success Criteria
* Complete at least 2–3 KillerCoda Kubernetes security scenarios.
* All Week 7 backlog (Days 43–47) is closed.
* You can articulate what, if anything, these guided scenarios revealed that your own hands-on practice this week missed.

---

# 📚 2. Topics to Study
### Primary Topic
**Consolidation: Kubernetes Security via Guided Scenarios**
### Secondary Topics
* Reviewing KillerCoda's scenario structure and comparing it to your own local minikube/kind practice
* Closing any incomplete exercises from Days 43–47
* Preparing for tomorrow's Tesla case study by ensuring today's dashboard/RBAC concepts are solid

### Priority
🔴 **Must Know:** be able to complete at least one Kubernetes security scenario without hints, applying Days 43–47's concepts fluently
🟡 **Should Know:** which specific Week 7 concept (container escape, hardened Dockerfiles, image scanning, RBAC, NetworkPolicies) felt least solid this week, for targeted review
🟢 **Nice to Know:** how KillerCoda's scenario-based format compares to PortSwigger/TryHackMe's format from earlier months

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Guided Scenario Practice
* What it is: KillerCoda provides free, browser-based, pre-configured Kubernetes environments with specific security scenarios to work through, similar in spirit to TryHackMe's rooms but Kubernetes-specific
* Why it matters: reinforcing this week's concepts in an environment you didn't set up yourself tests genuine understanding versus familiarity with your own specific local setup
* Best practices: attempt each scenario's tasks before consulting any provided hints, exactly as with previous months' lab practice

### Concept 2 — Backlog Triage (Recurring Pattern)
* Definition: systematically completing any Days 43–47 exercises not finished due to time constraints
* Practical application: this is the same "lab consolidation day" pattern established in Weeks 1–3 (Days 6-7, 13, 21) — by now this should feel like a familiar, comfortable rhythm in your curriculum

### Concept 3 — Preparing for Tomorrow's Case Study
* Key terminology: N/A — mostly a readiness check
* Practical application: confirm your understanding of dashboard exposure (Day 46) and RBAC (Day 47) is solid, since tomorrow's Tesla case study will require applying both concepts to a real historical incident

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Guided scenario practice | Structured labs in an environment you didn't configure | Tests genuine transferable understanding |
| Backlog triage | Completing any remaining exercises from the week | Maintains pace and genuine mastery before moving forward |

---

# ⏱️ 4. Study Schedule

## Session 1 — Backlog Triage (45–60 min)
Review Days 43–47 and complete any outstanding exercises.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: KillerCoda Scenarios (60–90 min)
Complete 2–3 free Kubernetes security scenarios on KillerCoda, attempting each task before consulting hints.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Without notes, explain the container/pod escape risk and its mitigation.
### Problem 2
Without notes, explain RBAC's least-privilege model and its connection to Capital One/CI tokens.
### Problem 3
Reflect: which KillerCoda scenario, if any, revealed a gap in your own hands-on practice this week? Plan a specific review action.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full Week 7 recall sweep so far: Docker security → hardened Dockerfiles → image scanning/signing → pod escape/dashboards → RBAC/NetworkPolicies.
### Spaced-Repetition Review
* **Yesterday:** RBAC & NetworkPolicies
* **This week:** the full container/Kubernetes security arc
* **7 weeks ago:** SQL injection (a good long-interval check)

---

# 🧪 6. Active Recall Exercises
1. Cold-recall Docker's namespace/cgroup isolation and the socket-mount escalation path.
2. Cold-recall hardened Dockerfile principles (minimal image, non-root, multi-stage).
3. Cold-recall image scanning (Trivy/Grype) and signing (Cosign).
4. Cold-recall pod escape and exposed-dashboard risk.
5. Cold-recall RBAC's Role/RoleBinding model and NetworkPolicy's default-deny pattern.
6. Which of this week's five days felt most difficult, and why?
7. How would you brief a development team on container/Kubernetes security in a single 15-minute session?
8. Which concept from this week connects most directly to Capital One (Day 14)?
9. Give a real-world example for at least two of this week's topics.
10. Teach the full Week 7 arc to a junior developer in under 4 minutes.

### Feynman Test
Explain the complete Week 7 container/Kubernetes security arc as one connected story in 5–6 sentences. If unclear, mark 🟡 **Needs Review** before tomorrow's case study.

---

# 🛠️ 7. Tool Practice
**Tool:** Full Week 7 toolkit review — Docker, `kubectl`, Trivy/Grype, Cosign
### Today's Tool Goal
Move fluidly between all Week 7 tools in the KillerCoda scenarios without hesitation.
### Tool Success Criteria
I'm not pausing to relearn any tool's basic mechanics from this week.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (extending into container/K8s hardening)
Today's contribution: consolidate all Week 7 artifacts (hardened Dockerfile, RBAC/NetworkPolicy configs, scan results) into your pipeline repository's documentation, ready for Week 8's Month 2 capstone integration.
### Deliverable
`Day 48: Completed KillerCoda Kubernetes security scenarios; consolidated Week 7 artifacts into pipeline repository documentation.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer says: "Tell me about container and Kubernetes security — what would you check first when reviewing a new deployment?"
### My Task
1. Structure your answer: image (hardened, scanned, signed) → pod configuration (non-privileged, no unnecessary host mounts) → RBAC (least-privilege service account) → network (default-deny policy with specific allows).
2. Practice this explanation out loud, under 2 minutes.
3. Anticipate a likely follow-up question (e.g., "How would you retrofit this onto an existing, unhardened cluster?").
4. Document the result.
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
* [ ] All Week 7 backlog closed  * [ ] KillerCoda scenarios completed  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Project artifacts consolidated  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Every Week 7 concept, cold, ready for interview-style questioning
2. Your consolidated pipeline repository artifacts
3. What's genuinely still shaky, if anything, heading into tomorrow's case study
4. Your interview-ready "what would you check first" framework
5. How tomorrow's Tesla case study will apply this week's exact concepts to a real historical incident

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** KillerCoda
**Lab Name:** Kubernetes Security scenarios (free tier) — 2–3 scenarios of your choosing
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Reinforce Week 7's concepts via guided, differently-structured scenarios, testing genuine transfer of understanding.
### Environment
Target: KillerCoda's browser-based Kubernetes environments · Tools: browser, provided `kubectl` access · Prerequisite: Days 43–47
### Lab Tasks
1. Complete 2–3 scenarios covering topics from this week (pod security, RBAC, network policies, or image security if available).
2. Attempt each task before consulting hints.
3. Note any concept that felt shakier in this new environment than in your own local practice.
4. Document completion and key takeaways.
### What I Need to Discover
Does your Week 7 understanding hold up in an environment you didn't configure yourself? Where specifically did it feel less automatic than in your own local minikube/kind setup?
### Lab Success Criteria
Complete 2–3 scenarios, document key takeaways, and honestly identify any transfer-of-learning gaps.

---
---

# 🛡️ Day 49 — Week 7 Consolidation: Tesla Kubernetes Dashboard Breach Case Study

**Date:** 07/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Synthesizing Week 7's concepts through a real-world breach analysis
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Study the 2018 Tesla Kubernetes dashboard breach in depth and map it onto this week's exact concepts (exposed dashboard, RBAC/least-privilege, container escape implications).
* Produce a polished case-study writeup as a portfolio artifact, following the same structure as Day 14's Capital One case study.
* Consolidate Week 7's findings into your pipeline portfolio's documentation.

### Success Criteria
* You can explain the Tesla incident's attack chain end-to-end from memory.
* A polished, cited case-study document exists in your portfolio repo.
* Week 7's complete container/Kubernetes hardening work is consolidated and ready to feed into Week 8's Month 2 capstone.

---

# 📚 2. Topics to Study
### Primary Topic
**Case Study: The 2018 Tesla Kubernetes Dashboard Breach**
### Secondary Topics
* How an exposed, unauthenticated Kubernetes dashboard (Day 46) led to unauthorized access
* How the incident was used for cryptojacking (unauthorized cryptocurrency mining) rather than direct data theft, an important distinguishing detail from Capital One
* What RBAC and network restrictions (Day 47) would have prevented or limited

### Priority
🔴 **Must Know:** the core chain — an exposed Kubernetes Dashboard, without authentication, was discovered and accessed, providing the attacker access to Tesla's cloud infrastructure, which was then used to run unauthorized cryptocurrency-mining software
🟡 **Should Know:** why this incident, unlike Capital One's data-theft motive, illustrates a different attacker objective (resource theft/cryptojacking) using the same fundamental "exposed management interface" vulnerability class
🟢 **Nice to Know:** how the incident was ultimately discovered and remediated, and Tesla's public response

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — The Tesla Attack Chain
* What it is: researchers (in this case, a security research firm conducting reconnaissance) discovered an exposed, unauthenticated Kubernetes Dashboard belonging to Tesla, providing them (and potentially any attacker who found it first) access to Tesla's Kubernetes cluster
* Why it matters: this is the exact scenario you conceptually studied on Day 46 — a real, documented, named incident demonstrating that "just a misconfigured dashboard" translates directly into meaningful cloud infrastructure compromise
* How it works: dashboard access → cluster credentials/configuration visible → access extended to broader cloud infrastructure → unauthorized workloads (cryptomining) deployed using that access
* Real-world example: this IS the real-world example — study it directly

### Concept 2 — Cryptojacking as a Distinct Attacker Objective
* Definition: using compromised computing resources to mine cryptocurrency for the attacker's benefit, rather than stealing data
* Practical application: this incident is a useful contrast to Capital One (Day 14) — not every breach is about data theft; some attackers simply want compute resources, and a compromised Kubernetes cluster with cloud auto-scaling capability is an especially valuable resource-theft target (the cluster may even auto-scale to provide the attacker MORE mining capacity)
* Best practices: monitoring for unexpected resource usage/scaling is itself a detection mechanism (recalling Week 4's logging/SIEM concepts) distinct from data-exfiltration-focused monitoring

### Concept 3 — What Would Have Prevented/Limited This
* Key terminology: defense-in-depth (recalling Day 14's framing)
* Practical application: Day 46's "never expose the dashboard without authentication" would have prevented initial access entirely; Day 47's RBAC least-privilege would have limited what the compromised access could actually do even if the dashboard were somehow reached; Day 44/45's image hardening/scanning are less directly relevant here but reinforce the general posture of minimizing what a compromised access point can achieve
* Best practices: structure your case-study writeup with this same multi-layer "what would have stopped this at each layer" analysis you used for Capital One

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Exposed dashboard exploitation | Real-world instance of Day 46's conceptual risk | Demonstrates the concept isn't merely theoretical |
| Cryptojacking | Unauthorized use of compromised resources for cryptocurrency mining | A distinct attacker objective from data theft, requiring different detection focus |
| Multi-layer prevention analysis | Mapping specific Week 7 concepts to what would have stopped this incident | The same professional case-study skill developed in Day 14 |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Research the Tesla Kubernetes dashboard breach from primary/reputable sources.
**Output:** A written timeline of the attack chain in your own words.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Map It to Week 7 & Write the Case Study (60–90 min)
1. Annotate the attack chain with specific Week 7 concepts (Day 46's dashboard exposure, Day 47's RBAC).
2. Write the "what would have stopped this at each layer" analysis.
3. Draft and publish the polished case-study document, following Day 14's structural template.

**Expected Result:** A complete, well-sourced, published case-study document.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Compare Tesla's cryptojacking motive to Capital One's data-theft motive — how might your monitoring/detection priorities differ if defending against each?
### Problem 2
If Day 47's RBAC least-privilege had been correctly configured, would the Tesla incident's impact have been meaningfully limited, even with the dashboard still exposed? Explain your reasoning.
### Problem 3
Write a 5-sentence executive summary of the Tesla breach suitable for a non-technical audience.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full Week 7 sweep: Docker security → hardened Dockerfiles → image scanning/signing → pod escape/dashboards → RBAC/NetworkPolicies → today's case study synthesis.
### Spaced-Repetition Review
* **Yesterday:** KillerCoda lab consolidation
* **This week:** the complete container/Kubernetes security arc
* **5 weeks ago:** Capital One case study (Day 14) — directly relevant again today for structural comparison

---

# 🧪 6. Active Recall Exercises
1. What is the full Tesla Kubernetes dashboard attack chain, start to finish?
2. How did the exposed dashboard specifically enable this incident?
3. Why is cryptojacking a meaningfully different attacker objective from data theft?
4. What would an attacker need at each step of this chain?
5. What was the actual business/reputational impact?
6. How would you detect this kind of attack in logs/monitoring (unexpected resource scaling, unusual workload deployments)?
7. How would you prevent it (dashboard authentication + RBAC least privilege)?
8. Difference between this incident and Capital One's, in terms of both root cause and attacker objective?
9. Cite specific, accurate details of the Tesla incident (year, general mechanism).
10. Teach the full Tesla case study to a junior developer in under 3 minutes.

### Feynman Test
Explain the Tesla breach as a connected, multi-layer story in 5 sentences, and compare it to Capital One. If unclear, mark 🟡 **Needs Review** before Week 8.

---

# 🛠️ 7. Tool Practice
**Tool:** Web research methodology (primary sources) + Markdown/GitHub for case-study publishing
### Today's Tool Goal
Practice sourcing this case study from primary/reputable sources, following the same rigor as Day 14.
### Tool Success Criteria
Your case study cites specific, verifiable facts rather than vague generalities.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (Week 7 container/K8s hardening now complete)
Today's contribution: publish `case-study-tesla-kubernetes.md` as a companion piece to Day 14's Capital One case study, and consolidate all Week 7 hardening artifacts (Dockerfile, RBAC, NetworkPolicy, scan results) into your pipeline repository's final Week 7 documentation.
### Deliverable
`Day 49: Published Tesla Kubernetes breach case study; consolidated complete Week 7 container/Kubernetes hardening artifacts into pipeline repository.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "You have two breach case studies in your portfolio — Capital One and Tesla. What's the common thread, and what's different?"
### My Task
1. Identify the common thread (both involve a network-reachable misconfiguration exposing infrastructure-level access; both involve insufficiently scoped permissions compounding the impact).
2. Identify the key difference (data theft vs. resource theft/cryptojacking as attacker objectives).
3. Practice this comparative explanation out loud, under 2 minutes.
4. Document the result.
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
* [ ] Case study researched and written  * [ ] Week 7 artifacts consolidated  * [ ] Practice problems
* [ ] Active-recall answers  * [ ] Portfolio repo updated  * [ ] Comparative interview answer rehearsed  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow (Start of Week 8)
1. The Tesla attack chain, cold, with accurate specifics
2. The comparative analysis between Tesla and Capital One
3. The complete Week 7 container/Kubernetes security arc
4. How Week 8's cloud security topics (IAM, S3, VPC) will directly extend both case studies' lessons to the broader cloud environment
5. What's still shaky from Weeks 5–7 that needs review before Week 8's Month 2 capstone

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link to published case study]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** N/A — today is a research/consolidation day
**Lab Name:** "Case Study Deep Dive: Tesla Kubernetes Dashboard Breach (2018)"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes (folded into Sessions 1–2 above)
### Lab Objective
Produce an accurate, well-sourced breach analysis as a standalone portfolio artifact, directly extending Day 14's case-study skill to a new incident and attacker objective.
### Environment
Target: primary/reputable sources · Tools: web research, Markdown
### Lab Tasks
1. Research the timeline from at least two independent, reputable sources.
2. Draft the technical root-cause chain.
3. Draft the multi-layer "what would have stopped this" analysis.
4. Write the executive summary.
5. Publish to your portfolio repo alongside the Capital One case study.
### What I Need to Discover
Having now written two full breach case studies (Capital One and Tesla), do you notice your own analysis process becoming faster and more structured the second time? That's a genuine, measurable sign of skill development in professional security communication.
### Lab Success Criteria
Document the finding (the full chain), explain root cause and impact accurately, and produce a clear, cited, multi-layer mitigation analysis comparable in quality to Day 14's Capital One writeup.

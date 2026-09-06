# 🗓️ DAYS 91–97 — Extended Module: Broader Offensive Security Toolkit

*An extension beyond the original 90 days, covering industry-standard tools outside the AppSec/DevSecOps/AI track: network recon, exploitation frameworks, traffic analysis, Active Directory attacks, credential attacks, forensics, reverse engineering, and OSINT. These come up in almost any security interview regardless of specialization, and round out your practical toolkit.*

---

# 🛡️ Day 91 — Network Reconnaissance: Nmap, Masscan, Shodan/Censys

**Date:** 18/11/2026
**Phase:** Extended Toolkit — Network & Infrastructure Security
**Primary Skill:** Network scanning, service enumeration, and internet-scale asset discovery
**Estimated Total Time:** 3–4 hours
**Difficulty:** Beginner/Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand Nmap's scanning methodology (host discovery, port scanning, service/version detection, OS fingerprinting, NSE scripting) at a working level.
* Run a complete Nmap scan against an authorized lab target and interpret every part of the output.
* Understand Masscan's role for internet-scale scanning and Shodan/Censys's role for passive, non-intrusive asset discovery.

### Success Criteria
* Explain the difference between a TCP SYN scan, a full connect scan, and a UDP scan without notes.
* Run and interpret a service/version detection scan with NSE scripts against a lab target.
* Explain when you'd reach for Masscan or Shodan instead of Nmap.

---

# 📚 2. Topics to Study
### Primary Topic
**Nmap: The Standard for Network Reconnaissance**
### Secondary Topics
* Masscan for internet-scale port scanning
* Shodan and Censys as passive, pre-indexed internet-device search engines
* Why network recon is foundational even for application-security-focused roles — it's often the first step of any real engagement, and interviewers assume basic fluency regardless of specialization

### Priority
🔴 **Must Know:** the core Nmap scan types — `-sS` (SYN/stealth scan, the default for privileged users), `-sT` (full TCP connect, used when SYN isn't available), `-sU` (UDP scan), `-sV` (service/version detection), `-O` (OS fingerprinting) — and what each actually does at the packet level
🟡 **Should Know:** NSE (Nmap Scripting Engine) extends Nmap with scripts for vulnerability detection, brute-forcing, and enumeration — `--script=vuln` or `--script=default` are common starting points
🟢 **Nice to Know:** Shodan/Censys index the entire internet passively (they've already scanned it for you) — useful for reconnaissance without touching the target directly, which matters for scope/legal boundaries

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Nmap's Scanning Methodology
* What it is: Nmap works through stages — host discovery (is the target alive), port scanning (which ports are open), service/version detection (what's running on those ports), and optionally OS fingerprinting and NSE scripting
* Why it matters: this is the standard first step of almost any authorized security engagement — understanding what's actually reachable and what's running on it is the foundation everything else builds on, directly analogous to how Day 26's OWASP Top 10 checklist requires first understanding the application's surface
* How it works: a SYN scan (`-sS`) sends a SYN packet and analyzes the response (SYN-ACK = open, RST = closed, no response/ICMP unreachable = filtered) without completing the full TCP handshake — faster and less likely to be logged by the target application than a full connect scan
* Real-world example: virtually every professional penetration test begins with an Nmap sweep of the in-scope IP ranges

### Concept 2 — NSE Scripting for Deeper Enumeration
* Definition: the Nmap Scripting Engine runs Lua scripts against discovered services for deeper enumeration, vulnerability checks, and even basic exploitation
* Practical application: `nmap -sV --script=vuln <target>` runs a battery of known-vulnerability checks against detected service versions — a fast way to surface low-hanging fruit
* Best practices: NSE scripts range from purely passive (banner grabbing) to actively intrusive (brute-force scripts) — know which category a script falls into before running it against anything you don't own or have explicit authorization to test aggressively

### Concept 3 — Masscan and Shodan/Censys for Scale
* Key terminology: internet-scale scanning, passive reconnaissance
* Practical application: Masscan can scan the entire IPv4 address space in minutes (at the cost of depth — it's essentially a very fast port scanner, typically paired with Nmap for deeper follow-up on discovered hosts); Shodan/Censys have already scanned the internet and let you search that index (e.g., "find all internet-exposed Kubernetes dashboards" — directly recalling Day 46/49's Tesla case study) without sending a single packet to the target yourself
* Best practices: Shodan/Censys are excellent for reconnaissance-only phases or for understanding your own organization's internet-facing exposure without needing explicit scanning authorization, since you're only querying an existing public index

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| SYN scan (`-sS`) | Half-open TCP scan, doesn't complete the handshake | The default, fastest, most common Nmap scan type |
| NSE (Nmap Scripting Engine) | Lua-scripted extensions for deeper enumeration/vuln checks | Turns Nmap from a port scanner into a lightweight vuln scanner |
| Passive reconnaissance (Shodan/Censys) | Querying a pre-built index instead of scanning the target directly | Useful when direct scanning authorization is unclear or unavailable |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study Nmap's scan types, NSE, and Masscan/Shodan/Censys's roles. Install Nmap if not already available.
**Output:** A written explanation of when you'd choose a SYN scan vs. a full connect scan vs. a UDP scan.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Run a basic Nmap scan (`nmap <target>`) against an authorized lab target (e.g., a local VM, a deliberately vulnerable machine like Metasploitable, or `scanme.nmap.org`, which is explicitly provided by the Nmap project for practice).
2. Run a service/version detection scan with default NSE scripts (`nmap -sV -sC <target>`).
3. Run a vulnerability-focused NSE scan (`nmap --script=vuln <target>`) and interpret the findings.
4. Create a free Shodan account and search for a specific, general device/service category (e.g., "webcam", or a specific product banner) to see how passive indexing works.

**Expected Result:** Documented Nmap scan output across 3 scan types + a Shodan search demonstration.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
An Nmap SYN scan against a target returns no response at all for a specific port. What are the possible explanations (closed vs. filtered), and how would a UDP scan behavior differ?
### Problem 2
Explain why Shodan querying is generally lower-risk, authorization-wise, than actively scanning a target yourself.
### Problem 3
Design a reconnaissance plan (which tool, in what order) for authorized external reconnaissance of a company's internet-facing footprint before a penetration test begins.
### Challenge
Research and explain how Nmap's OS fingerprinting (`-O`) works at a conceptual level — what signals does it use to guess the target's operating system?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Day 90 — Final Portfolio Audit & Graduation
Recall without notes: your final audit process and closing reflection.
### Spaced-Repetition Review
* **Most recent:** Day 90's graduation and closing reflection
* **13 weeks ago:** Day 46/49's Tesla Kubernetes dashboard case study — directly relevant to today's Shodan discussion (exposed dashboards are a classic Shodan-discoverable exposure)

---

# 🧪 6. Active Recall Exercises
1. What are the main Nmap scan types, and what does each detect?
2. How does NSE extend Nmap's capability beyond simple port scanning?
3. Why is a SYN scan generally preferred over a full connect scan for stealth?
4. What would you need to responsibly use Masscan for internet-scale scanning (explicit authorization for any range you actually scan)?
5. What's the value of Shodan/Censys for understanding your own organization's exposure?
6. How would you use Nmap results to plan the next phase of an authorized assessment?
7. How would you responsibly and legally scope any scanning activity?
8. Difference between active reconnaissance (Nmap/Masscan) and passive reconnaissance (Shodan/Censys)?
9. Real-world example: how exposed dashboards (like Tesla's Kubernetes dashboard, Day 49) are often discovered via exactly this kind of tooling.
10. Teach basic Nmap usage to a junior developer moving into a security-adjacent role.

### Feynman Test
Explain Nmap's core scanning methodology and how it differs from Shodan's passive approach, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Nmap, Shodan
### Today's Tool Goal
Run a complete scan workflow and perform a basic passive-recon search.
### Commands / Features to Practice
```text
nmap -sV -sC scanme.nmap.org
nmap --script=vuln <authorized-target>
nmap -p- -T4 <authorized-target>   # full port range scan
```
### Tool Success Criteria
I can run and correctly interpret a service/version detection scan and explain every line of its output.

---

# 🏗️ 8. Project Connection
**Current Project:** Extended toolkit reference (supplementary to your three published portfolio pieces)
Today's contribution: document your Nmap scan methodology and results against an authorized lab target as a supplementary "network reconnaissance" note, referenceable if a future role or interview touches on network-layer assessment.
### Deliverable
`Day 91: Documented Nmap scanning methodology (SYN/service/NSE-vuln scans) against an authorized lab target; demonstrated Shodan passive reconnaissance.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "Before you even get to application-layer testing, how would you understand what's actually exposed on a target network?"
### My Task
1. Explain your Nmap-first methodology.
2. Explain how you'd layer in Shodan/Censys for passive context.
3. Explain how findings here would inform your subsequent AppSec-focused testing (directly connecting back to your Month 1 skillset).
4. Document the result.
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
* [ ] Study notes  * [ ] Nmap scans completed and documented  * [ ] Shodan search demonstration completed
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Nmap's core scan types and NSE
2. Masscan and Shodan/Censys's respective roles
3. How network recon fits before application-layer testing
4. Your documented scan methodology
5. How tomorrow's Metasploit topic builds directly on today's recon — turning discovered services into exploitation targets

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local VM / Metasploitable / scanme.nmap.org
**Lab Name:** "Full Reconnaissance Sweep: Nmap Service and Vulnerability Enumeration"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Perform and document a complete Nmap reconnaissance workflow against an authorized target.
### Environment
Target: an authorized lab VM (Metasploitable or similar) · Tools: Nmap · Prerequisite: today's fundamentals
### Lab Tasks
1. Run host discovery and a full port scan.
2. Run service/version detection.
3. Run an NSE vulnerability script pass.
4. Document every open port, its service, and any flagged vulnerabilities.
5. Note which findings you'd investigate further tomorrow with Metasploit.
### What I Need to Discover
Does the discovered attack surface match what you'd expect from a deliberately vulnerable practice machine? What does a clean, well-organized scan report look like, and how would you hand this off to someone else on a team?
### Lab Success Criteria
A complete, documented Nmap scan report covering open ports, services, versions, and any vulnerability findings.

---
---

# 🛡️ Day 92 — Metasploit Framework: Exploitation & Post-Exploitation

**Date:** 19/11/2026
**Phase:** Extended Toolkit — Exploitation Frameworks
**Primary Skill:** Using Metasploit to exploit a known vulnerability and understand post-exploitation basics
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand Metasploit's architecture: exploits, payloads, encoders, and auxiliary modules.
* Use Metasploit to exploit a known vulnerability on an authorized lab target (e.g., Metasploitable), from discovery through to a shell.
* Understand Meterpreter as a post-exploitation payload and its basic capabilities.

### Success Criteria
* Explain the exploit/payload/module relationship without notes.
* Successfully exploit at least one vulnerability end-to-end using Metasploit against a lab target.
* Explain what Meterpreter provides beyond a basic shell.

---

# 📚 2. Topics to Study
### Primary Topic
**Metasploit Framework**
### Secondary Topics
* Metasploit's module structure: exploits, payloads, auxiliary, encoders, post-exploitation modules
* Meterpreter as an advanced, in-memory post-exploitation payload
* Metasploit's relationship to Nmap (yesterday's recon feeds directly into today's targeting)

### Priority
🔴 **Must Know:** an exploit and a payload are separate, composable pieces in Metasploit — the exploit is the delivery mechanism (the vulnerability being leveraged), the payload is what runs once the exploit succeeds (a shell, Meterpreter, etc.) — this modularity is the core of how Metasploit works
🟡 **Should Know:** Metasploit is explicitly a tool for *authorized* testing — using it against anything without clear permission is illegal; the entire value proposition here is practicing against deliberately vulnerable, purpose-built lab machines
🟢 **Nice to Know:** Meterpreter runs entirely in memory (harder to detect via disk-based antivirus) and provides a rich command set beyond a basic shell (file transfer, privilege escalation modules, pivoting)

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Metasploit's Modular Architecture
* What it is: Metasploit separates the vulnerability being exploited (the "exploit" module) from what happens after successful exploitation (the "payload") — this lets you mix and match, e.g., the same exploit with a simple reverse shell payload or with Meterpreter
* Why it matters: this modularity is what makes Metasploit so widely used — a huge, community-maintained library of exploit modules for known CVEs, each composable with various payload options
* How it works: `use exploit/<path>` selects the exploit, `set PAYLOAD <path>` selects the payload, `set RHOSTS <target>` and other options configure it, `exploit` (or `run`) fires it
* Real-world example: yesterday's Nmap service/version detection is exactly what informs which Metasploit exploit module might apply — a specific outdated service version often maps directly to a known Metasploit module

### Concept 2 — Payloads and Meterpreter
* Definition: a payload is the code that executes once an exploit succeeds; Meterpreter is Metasploit's advanced, extensible, in-memory payload providing a rich post-exploitation command set
* Architecture/process: once you have a Meterpreter session, you can browse the filesystem, dump credentials (with appropriate modules), attempt privilege escalation, pivot to other network segments, and more — directly connecting to concepts from Days 43 (container escape) and 94 (AD attacks) about what an attacker does *after* initial access
* Best practices: understand Meterpreter as the bridge between "I got a shell" and "I understand what a real post-exploitation phase actually looks like," which is valuable context even for a primarily defensive/AppSec career

### Concept 3 — Authorized Use Only
* Key terminology: authorized penetration testing, lab-only practice
* Practical application: every exercise today must be against a machine you own or a deliberately-vulnerable lab environment built for this purpose (Metasploitable, TryHackMe/HTB rooms that explicitly permit Metasploit usage)
* Best practices: this isn't just a legal formality — understanding scope and authorization boundaries is itself a core professional security skill, one you've practiced explicitly since Week 3's Burp Suite scope-configuration lesson (Day 21)

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Exploit module | The code leveraging a specific vulnerability | The "how you get in" half of Metasploit's modularity |
| Payload | The code that runs after successful exploitation | The "what happens next" half — a shell, Meterpreter, etc. |
| Meterpreter | An advanced, in-memory, extensible post-exploitation payload | The bridge from initial access to genuine post-exploitation understanding |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study Metasploit's module architecture and Meterpreter's capabilities. Set up Metasploitable (or an equivalent authorized lab target) if not already available.
**Output:** A written explanation of the exploit/payload relationship using a concrete example.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Using yesterday's Nmap findings against your lab target, identify a known-vulnerable service (Metasploitable is deliberately full of them).
2. Search for and select a matching Metasploit exploit module (`search <service/CVE>`).
3. Configure and run the exploit with a Meterpreter payload.
4. From the resulting Meterpreter session, explore basic post-exploitation commands (`sysinfo`, `getuid`, `ls`, `download`).

**Expected Result:** A documented, successful exploitation from Nmap recon through to a Meterpreter session, with basic post-exploitation commands demonstrated.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Explain how you'd choose between a bind shell and a reverse shell payload, and why reverse shells are often preferred when a target is behind a firewall.
### Problem 2
You've gained a Meterpreter session as a low-privilege user. What's your next logical step, and what Metasploit capability would help?
### Problem 3
Explain why Metasploit's exploit library is directly informed by public CVE databases and disclosed vulnerability research — connecting back to Day 34's SCA/dependency-vulnerability lesson.
### Challenge
Research one specific, well-known Metasploit module (e.g., EternalBlue/MS17-010) and explain, at a high level, what vulnerability it exploits and why it became so widely referenced.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Nmap & Network Reconnaissance (Day 91)
Recall without notes: Nmap's scan types and how findings feed into exploitation planning.
### Spaced-Repetition Review
* **Yesterday:** Nmap and network reconnaissance
* **13 weeks + 1 day ago:** Day 43's container escape — a useful conceptual parallel to today's "what happens after initial access" theme

---

# 🧪 6. Active Recall Exercises
1. What is the relationship between an exploit module and a payload in Metasploit?
2. How does Meterpreter differ from a basic reverse shell?
3. Why must Metasploit only ever be used against explicitly authorized targets?
4. What would you need to select an appropriate exploit module (accurate service/version information, typically from Nmap)?
5. What's the impact of gaining a Meterpreter session as a low-privilege vs. privileged user?
6. How would you use post-exploitation Meterpreter commands to understand a compromised system's context?
7. How would you responsibly practice and demonstrate this skill in a portfolio, given the authorization constraints?
8. Difference between an exploit, a payload, and an auxiliary module?
9. Real-world example: how a widely-known exploit like EternalBlue became a Metasploit module.
10. Teach Metasploit's basic workflow (recon → module selection → configuration → exploitation → post-exploitation) to a junior developer.

### Feynman Test
Explain Metasploit's modular architecture and your lab exploitation walkthrough in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Metasploit Framework (`msfconsole`)
### Today's Tool Goal
Run a complete exploit-to-Meterpreter workflow against an authorized lab target.
### Commands / Features to Practice
```text
msfconsole
search <service name or CVE>
use exploit/<path>
set RHOSTS <target>
set PAYLOAD <meterpreter payload>
exploit
sysinfo
getuid
```
### Tool Success Criteria
I can search for, configure, and run an appropriate exploit module and interact with the resulting Meterpreter session.

---

# 🏗️ 8. Project Connection
**Current Project:** Extended toolkit reference
Today's contribution: a documented exploitation walkthrough (recon → exploit → Meterpreter → basic post-exploitation) against an authorized lab target, demonstrating exploitation-framework fluency alongside your existing AppSec portfolio.
### Deliverable
`Day 92: Documented full Metasploit exploitation workflow against authorized lab target, from Nmap recon through Meterpreter post-exploitation.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "Have you used Metasploit? Walk me through a time you exploited something with it."
### My Task
1. Walk through your Day 91–92 workflow: recon → module selection → exploitation → Meterpreter.
2. Be explicit that this was against an authorized, deliberately-vulnerable lab machine.
3. Connect this to your broader understanding of the exploitation lifecycle, even as an AppSec-focused candidate.
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
* [ ] Study notes  * [ ] Successful lab exploitation documented  * [ ] Meterpreter post-exploitation demonstrated
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Metasploit's exploit/payload architecture
2. Your documented exploitation walkthrough
3. Meterpreter's post-exploitation capabilities
4. Authorized-use boundaries
5. How tomorrow's Wireshark topic lets you see, at the packet level, exactly what today's exploitation traffic actually looked like

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Metasploitable (or equivalent authorized lab VM)
**Lab Name:** "Full Exploitation Chain: Recon to Meterpreter"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Chain yesterday's Nmap recon directly into a successful Metasploit exploitation and Meterpreter session.
### Environment
Target: Metasploitable or equivalent · Tools: Metasploit, your Day 91 Nmap results · Prerequisite: Day 91
### Lab Tasks
1. Review yesterday's Nmap findings for a known-vulnerable service.
2. Search Metasploit for a matching exploit module.
3. Configure and run it with a Meterpreter payload.
4. Explore basic post-exploitation commands.
5. Document the complete chain from initial scan to shell.
### What I Need to Discover
Seeing the direct line from yesterday's Nmap output to today's successful exploit selection — does this make the "recon informs exploitation" relationship concrete in a way it wasn't before?
### Lab Success Criteria
A documented, successful, complete exploitation chain from Nmap recon through Meterpreter post-exploitation commands.

---
---

# 🛡️ Day 93 — Wireshark & Network Traffic Analysis

**Date:** 20/11/2026
**Phase:** Extended Toolkit — Network & Infrastructure Security
**Primary Skill:** Packet-level traffic capture and analysis
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand Wireshark's core capture and filtering workflow.
* Capture and analyze traffic from yesterday's Metasploit exploitation to see, at the packet level, what actually happened.
* Understand `tcpdump` as the CLI equivalent for headless/server environments.

### Success Criteria
* Explain the difference between a capture filter and a display filter without notes.
* Capture and analyze a real traffic sample, identifying protocol-level details.
* Explain when you'd use `tcpdump` instead of Wireshark's GUI.

---

# 📚 2. Topics to Study
### Primary Topic
**Wireshark: Packet Capture and Analysis**
### Secondary Topics
* Capture filters (applied during capture, using BPF syntax) vs. display filters (applied after capture, using Wireshark's own syntax)
* Following a TCP stream to reconstruct an entire conversation (e.g., an HTTP request/response, or plaintext credentials)
* `tcpdump` as the CLI equivalent for servers/headless environments

### Priority
🔴 **Must Know:** the distinction between capture filters (what gets recorded at all, applied before capture starts) and display filters (what you're currently viewing from an already-complete capture) — this trips up almost everyone early on
🟡 **Should Know:** "Follow TCP Stream" is one of Wireshark's most useful features — it reconstructs an entire back-and-forth conversation from individual packets, letting you read something like a full HTTP exchange or a plaintext login attempt in one view
🟢 **Nice to Know:** `tcpdump` uses the same underlying filter syntax (BPF) as Wireshark's capture filters, so learning one transfers directly to the other

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Capture Filters vs. Display Filters
* What it is: a capture filter (e.g., `tcp port 80`) determines what traffic gets recorded at all, applied before or during capture; a display filter (e.g., `http.request.method == "POST"`) determines what you're currently viewing from a capture that's already complete
* Why it matters: confusing these is a very common beginner mistake — you can't "display filter" your way into traffic that a restrictive capture filter already excluded from being recorded in the first place
* How it works: for exploratory analysis, it's often safer to capture broadly (or without a restrictive filter) and use display filters liberally afterward, since you can always narrow your view but can't retroactively capture what you didn't record

### Concept 2 — Following TCP Streams
* Definition: reconstructing the full sequence of a TCP conversation from individual captured packets into one readable view
* Practical application: right-click any packet in a TCP conversation → Follow → TCP Stream reconstructs, for example, an entire unencrypted HTTP request and response, or reveals plaintext credentials sent over an unencrypted protocol
* Attack scenario this illustrates: this directly demonstrates why unencrypted protocols (plain HTTP, FTP, Telnet) are dangerous on a network an attacker can observe — a lesson connecting back to Week 2's session-hijacking discussion (Day 9) about network-position attackers

### Concept 3 — `tcpdump` for Headless Environments
* Key terminology: BPF (Berkeley Packet Filter) syntax
* Practical application: `tcpdump -i eth0 -w capture.pcap` captures traffic on a server with no GUI, saving it to a `.pcap` file you can later open in Wireshark for full analysis — a very common real-world workflow (capture on the server, analyze on your workstation)
* Best practices: know enough `tcpdump` syntax to capture on a remote/headless system even if your day-to-day analysis happens in Wireshark's GUI

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Capture filter | Determines what's recorded, applied before/during capture | Can't be applied retroactively — capture broadly if unsure |
| Display filter | Determines what's shown from an already-complete capture | The main day-to-day analysis tool once you have a capture |
| Follow TCP Stream | Reconstructs a full conversation from individual packets | Reveals plaintext protocol content directly, illustrating unencrypted-traffic risk |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study capture vs. display filters and the TCP stream-following workflow. Install Wireshark if not already available.
**Output:** A written explanation of why a restrictive capture filter can permanently exclude traffic you later wish you'd recorded.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Capture traffic while browsing an unencrypted HTTP site (or your own local test app) and use "Follow TCP Stream" to read the full request/response.
2. Capture traffic while re-running yesterday's Metasploit exploitation against your lab target, and identify the exploit traffic in the capture.
3. Practice basic display filters: `ip.addr == <target>`, `tcp.port == 80`, `http`.
4. Run a basic `tcpdump` capture from the command line and confirm you can open the resulting `.pcap` in Wireshark.

**Expected Result:** Documented packet captures with at least one fully reconstructed TCP stream and cross-tool (`tcpdump` → Wireshark) workflow demonstrated.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
You need to capture only traffic to/from a specific IP on port 443. Write the capture filter.
### Problem 2
Explain what "Follow TCP Stream" would reveal if applied to a plaintext FTP login, and why this illustrates a genuine security risk.
### Problem 3
Explain the workflow for capturing traffic on a remote, GUI-less server and analyzing it locally.
### Challenge
Analyze your Day 92 Metasploit exploitation capture — can you identify the specific packets corresponding to the exploit payload being delivered?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Metasploit Exploitation (Day 92)
Recall without notes: the exploit/payload relationship and your lab exploitation chain.
### Spaced-Repetition Review
* **Yesterday:** Metasploit exploitation
* **12 weeks + 5 days ago:** Day 9's session hijacking — directly relevant to today's unencrypted-traffic discussion

---

# 🧪 6. Active Recall Exercises
1. What's the difference between a capture filter and a display filter?
2. How does "Follow TCP Stream" reconstruct a full conversation from packets?
3. Why is capturing broadly and filtering later often safer than a restrictive capture filter?
4. What would you need to capture traffic on a headless server (`tcpdump` with appropriate flags)?
5. What's the security risk illustrated by reading plaintext credentials via a followed TCP stream?
6. How would you use Wireshark to verify that yesterday's exploitation traffic actually occurred as expected?
7. How would you use display filters to narrow a large capture down to just the traffic you care about?
8. Difference between Wireshark's GUI workflow and `tcpdump`'s CLI workflow, and when you'd use each?
9. Real-world example: how packet capture and analysis supports incident response (connecting to Day 25's IR lifecycle).
10. Teach basic Wireshark usage to a junior developer moving into a security-adjacent role.

### Feynman Test
Explain capture vs. display filters and TCP stream following, using your own capture as an example, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Wireshark, `tcpdump`
### Today's Tool Goal
Capture, filter, and analyze real traffic across both tools.
### Commands / Features to Practice
```text
tcpdump -i eth0 -w capture.pcap
tcpdump -i eth0 tcp port 80 -w http-only.pcap
# In Wireshark display filter bar:
ip.addr == <target> and tcp.port == 80
```
### Tool Success Criteria
I can capture traffic with `tcpdump`, open it in Wireshark, apply meaningful display filters, and follow a TCP stream to read its full content.

---

# 🏗️ 8. Project Connection
**Current Project:** Extended toolkit reference
Today's contribution: a documented packet-capture analysis of your Day 92 exploitation traffic, demonstrating traffic-analysis skill as a complement to your existing logging/SIEM work from Week 4.
### Deliverable
`Day 93: Captured and analyzed network traffic (own HTTP browsing + Day 92 exploitation traffic) using Wireshark and tcpdump; documented a fully reconstructed TCP stream.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "If you suspected data was being exfiltrated from a compromised host, how would you confirm it at the network level?"
### My Task
1. Explain how you'd capture traffic from/to the suspected host (via `tcpdump` if remote, or a network tap/span port).
2. Explain how you'd use display filters to isolate unusual destinations or protocols.
3. Explain how "Follow TCP Stream" or similar reconstruction would let you inspect the actual exfiltrated content if unencrypted.
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
* [ ] Study notes  * [ ] Packet captures completed  * [ ] TCP stream reconstruction demonstrated
* [ ] `tcpdump`-to-Wireshark workflow demonstrated  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Capture vs. display filters
2. TCP stream reconstruction and its security implications
3. `tcpdump` for headless capture
4. Your analyzed exploitation traffic
5. How tomorrow's Active Directory topic covers a domain where traffic analysis and credential attacks (Day 95) become especially relevant

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local capture environment + Day 92's lab VM
**Lab Name:** "Capture and Reconstruct: Full Traffic Analysis Workflow"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Capture real traffic, apply meaningful filters, and fully reconstruct at least one conversation.
### Environment
Target: your own browsing traffic + Day 92's exploitation replay · Tools: Wireshark, `tcpdump` · Prerequisite: Day 92
### Lab Tasks
1. Capture unencrypted HTTP traffic and follow the TCP stream.
2. Re-run Day 92's exploit and capture the traffic live.
3. Apply display filters to isolate the exploitation traffic specifically.
4. Document what you can identify about the exploit from the packet capture alone.
5. Practice the `tcpdump`-capture-then-Wireshark-analyze workflow.
### What I Need to Discover
Can you now "see" what an exploit or an unencrypted credential submission actually looks like at the wire level, rather than just understanding it conceptually? Does this change how viscerally you understand the value of encryption (TLS) as a control?
### Lab Success Criteria
Documented captures with at least one fully reconstructed TCP stream and successfully identified exploitation traffic from Day 92.

---
---

# 🛡️ Day 94 — Active Directory Attacks: BloodHound, Impacket, CrackMapExec

**Date:** 21/11/2026
**Phase:** Extended Toolkit — Internal Network & Identity Attacks
**Primary Skill:** Active Directory enumeration and attack-path analysis
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand Active Directory's core structure (domains, users, groups, group policy) and why it's such a high-value target in internal penetration tests.
* Use BloodHound to visualize AD attack paths from a low-privilege user to Domain Admin.
* Understand Impacket and CrackMapExec's roles in AD enumeration and lateral movement.

### Success Criteria
* Explain why AD is often the single highest-value target in an internal network assessment.
* Use BloodHound to identify at least one privilege-escalation path in a lab AD environment.
* Explain what Impacket and CrackMapExec each provide that complements BloodHound's visualization.

---

# 📚 2. Topics to Study
### Primary Topic
**Active Directory Attack Fundamentals**
### Secondary Topics
* Why AD compromise is often the "final boss" of an internal penetration test — controlling the domain often means controlling everything
* BloodHound's graph-based attack-path visualization
* Impacket (Python toolkit for Windows/AD protocol-level attacks) and CrackMapExec/NetExec (enumeration and lateral-movement swiss-army-knife)

### Priority
🔴 **Must Know:** Active Directory centralizes authentication and authorization for most enterprise Windows networks — compromising it (specifically obtaining Domain Admin or equivalent) typically means an attacker has effective control over the entire environment, which is exactly why real-world attackers and professional red teams prioritize it so heavily
🟡 **Should Know:** BloodHound works by ingesting AD data (via a collector) and building a graph showing every possible privilege-escalation path — often revealing non-obvious chains ("this low-privilege user is in a group that has GenericAll rights over a computer object that a Domain Admin has an active session on") that would be nearly impossible to spot by manual inspection alone
🟢 **Nice to Know:** this entire domain (AD attacks) is often its own specialization within security — today's goal is foundational awareness and hands-on exposure, not mastery, since it sits outside your primary AppSec/DevSecOps/AI target roles

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Why Active Directory Is a High-Value Target
* What it is: AD is Microsoft's directory service, centralizing identity, authentication, and authorization for most enterprise Windows networks — a "Domain Admin" account has broad control across essentially every domain-joined system
* Why it matters: this is the internal-network equivalent of the "excessive agency"/least-privilege theme that's recurred throughout your entire curriculum (Capital One's IAM, CI tokens, Kubernetes RBAC, cloud VPC/security groups, LLM agent permissions) — AD privilege-escalation paths are frequently the result of exactly this same pattern: an account, group, or computer object with more effective privilege than intended
* How it works: attackers who gain any foothold (even a low-privilege domain user account) often find, via tools like BloodHound, a path of misconfigured permissions leading step by step to Domain Admin

### Concept 2 — BloodHound's Attack-Path Graph
* Definition: BloodHound ingests AD object and relationship data (users, groups, computers, sessions, ACLs) via a data collector (e.g., SharpHound), then visualizes this as a graph, letting you query for shortest paths from any starting point to Domain Admin
* Architecture/process: this graph-based approach surfaces genuinely non-obvious attack chains involving nested group memberships, delegated permissions, and active user sessions — exactly the kind of complex, multi-hop path that manual AD review would likely miss
* Real-world example: this directly parallels Day 20's attack-tree methodology — BloodHound is essentially an automated, AD-specific attack-tree/attack-path generator

### Concept 3 — Impacket and CrackMapExec/NetExec
* Key terminology: pass-the-hash, lateral movement
* Practical application: Impacket is a Python library/toolkit implementing Windows/AD network protocols directly, enabling attacks like pass-the-hash (authenticating with a captured password hash without ever knowing the plaintext password) and ticket-based attacks; CrackMapExec/NetExec is a higher-level tool built for rapid enumeration and lateral movement across many hosts at once, often used alongside credentials or hashes obtained elsewhere
* Best practices: these tools are typically used together in sequence — BloodHound to find the path, Impacket/CrackMapExec to actually execute steps along it

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Domain Admin | The highest-privilege AD role, effectively controlling the domain | The typical end-goal of an internal AD attack chain |
| BloodHound | Graph-based AD attack-path visualization tool | Surfaces non-obvious, multi-hop privilege-escalation chains |
| Pass-the-hash | Authenticating with a captured hash instead of a plaintext password | A common AD lateral-movement technique enabled by tools like Impacket |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study AD's structure, why it's a high-value target, and BloodHound's graph model.
**Output:** Write, explicitly, how AD attack-path analysis connects to the recurring least-privilege theme from throughout your curriculum.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. If a pre-built lab AD environment is available (several free/community options exist specifically for this purpose, e.g., "GOAD" — Game of Active Directory), set it up; otherwise, work through BloodHound's own sample/example dataset, which ships with realistic demonstration data.
2. Load AD data into BloodHound and explore the graph interface.
3. Run a "shortest path to Domain Admin" query and trace the resulting chain step by step.
4. Research Impacket and CrackMapExec's basic command syntax, even without a full live lab to run them against, to understand their role in executing a BloodHound-identified path.

**Expected Result:** A documented BloodHound attack-path analysis with a traced privilege-escalation chain, plus notes on how Impacket/CrackMapExec would execute the next steps.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Explain, using a specific BloodHound-style example, how a "harmless-looking" group membership can create an unexpected privilege-escalation path.
### Problem 2
Explain pass-the-hash conceptually — why does it work, and what does it tell you about how Windows authentication handles credentials?
### Problem 3
Connect today's AD least-privilege lessons explicitly back to at least 2 earlier instances of the same pattern in your curriculum (Capital One, CI tokens, K8s RBAC, cloud security groups, LLM agency).
### Challenge
Design a remediation recommendation for a BloodHound-identified attack path you traced today — what specific ACL or group-membership change would break the chain?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Wireshark & Traffic Analysis (Day 93)
Recall without notes: capture/display filters and TCP stream reconstruction.
### Spaced-Repetition Review
* **Yesterday:** Wireshark and traffic analysis
* **11 weeks ago:** Day 47's Kubernetes RBAC — directly reused today as the closest prior parallel to AD's permission-graph structure

---

# 🧪 6. Active Recall Exercises
1. Why is Active Directory such a high-value target for internal attackers?
2. How does BloodHound's graph model surface non-obvious attack paths?
3. What does pass-the-hash exploit about Windows authentication?
4. What would you need to run a BloodHound analysis against a real AD environment (a data collector like SharpHound, executed with appropriate access)?
5. What's the impact of a single misconfigured group membership on overall domain security?
6. How would you use CrackMapExec to move laterally once you have valid credentials or a hash?
7. How would you remediate a BloodHound-identified attack path?
8. Difference between BloodHound (visualization/analysis) and Impacket/CrackMapExec (execution)?
9. Real-world example: connect today's AD least-privilege lesson to the recurring pattern from Capital One (Day 14) forward.
10. Teach the concept of AD attack-path analysis to a junior developer, explicitly using the least-privilege throughline from your curriculum.

### Feynman Test
Explain why AD is a high-value target, how BloodHound reveals attack paths, and the least-privilege connection to your broader curriculum, in 5–6 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** BloodHound, Impacket, CrackMapExec/NetExec
### Today's Tool Goal
Explore BloodHound's graph interface and understand Impacket/CrackMapExec's role at a working conceptual level.
### Commands / Features to Practice
```text
# BloodHound queries (via UI): "Shortest Paths to Domain Admins"
# Impacket example (conceptual):
secretsdump.py <domain>/<user>:<password>@<target>
# CrackMapExec example (conceptual):
crackmapexec smb <target-range> -u <user> -H <hash>
```
### Tool Success Criteria
I can navigate BloodHound's graph interface, run a path-to-Domain-Admin query, and explain what Impacket/CrackMapExec commands would do at each step of executing that path.

---

# 🏗️ 8. Project Connection
**Current Project:** Extended toolkit reference
Today's contribution: a documented BloodHound attack-path analysis with an explicit connection to your curriculum's recurring least-privilege theme — a strong, differentiated talking point tying your AppSec/cloud background to internal network security awareness.
### Deliverable
`Day 94: Documented BloodHound attack-path analysis (sample/lab AD data); explained Impacket/CrackMapExec's role in path execution; connected findings to the recurring least-privilege pattern across the full curriculum.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "We're primarily a cloud-native shop, but we still run some on-prem AD. How would that change your risk assessment approach?"
### My Task
1. Explain AD's centralized-identity risk model and why it deserves the same least-privilege rigor as your cloud IAM work (Day 50).
2. Mention BloodHound as your tool of choice for surfacing non-obvious AD privilege paths.
3. Draw the explicit parallel to your cloud/Kubernetes least-privilege work.
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
* [ ] Study notes  * [ ] BloodHound attack-path analysis completed  * [ ] Impacket/CrackMapExec role documented
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Why AD is a high-value internal target
2. BloodHound's graph-based attack-path model
3. Impacket and CrackMapExec's roles
4. Your traced attack path and remediation recommendation
5. How tomorrow's credential-attack topic (Hydra, John the Ripper, Mimikatz) extends today's AD focus with the specific mechanics of obtaining and cracking credentials in the first place

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** BloodHound (sample dataset or lab AD environment)
**Lab Name:** "Trace a Full Privilege-Escalation Path to Domain Admin"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Identify and fully document a complete attack path using BloodHound, with a specific remediation recommendation.
### Environment
Target: BloodHound sample data or a lab AD environment · Tools: BloodHound · Prerequisite: today's fundamentals
### Lab Tasks
1. Load AD data into BloodHound.
2. Run a shortest-path-to-Domain-Admin query.
3. Trace and document every hop in the resulting chain.
4. Identify the specific misconfiguration enabling each hop.
5. Write a remediation recommendation for the single highest-leverage fix.
### What I Need to Discover
Does tracing this path make the abstract concept of "privilege escalation via chained misconfigurations" concrete in the same way Day 20's attack-tree exercise did for web applications?
### Lab Success Criteria
A fully documented, traced attack path with a specific, justified remediation recommendation.

---
---

# 🛡️ Day 95 — Password & Credential Attacks: Hydra, John the Ripper, Mimikatz

**Date:** 22/11/2026
**Phase:** Extended Toolkit — Credential Attacks
**Primary Skill:** Online brute-forcing, offline password cracking, and Windows credential extraction
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand the distinction between online (live-service) and offline (hash-based) password attacks, and which tools fit each category.
* Use Hydra to perform an authorized online brute-force attempt against a lab service.
* Understand John the Ripper as a second major offline cracker alongside Day 12's `hashcat`, and Mimikatz's role in Windows credential extraction.

### Success Criteria
* Explain online vs. offline credential attacks and which tool category fits each, without notes.
* Successfully run an authorized Hydra attack against a lab service.
* Explain what Mimikatz extracts from Windows memory and why that's possible.

---

# 📚 2. Topics to Study
### Primary Topic
**Online and Offline Credential Attack Tools**
### Secondary Topics
* Hydra/Medusa for online brute-forcing against live authentication services (SSH, RDP, HTTP forms, etc.)
* John the Ripper as an offline cracker, complementing Day 12's `hashcat`
* Mimikatz for Windows in-memory credential extraction (LSASS)

### Priority
🔴 **Must Know:** online attacks (Hydra) test credentials directly against a live, running service — slow, rate-limitable, and noisy/detectable; offline attacks (hashcat/John) crack already-obtained password hashes locally, with no interaction with the target system at all, and are limited only by your own compute power — this distinction directly determines which defenses (Day 12's rate limiting vs. Day 12's adaptive hashing) actually stop each category
🟡 **Should Know:** John the Ripper and hashcat overlap significantly in purpose but differ in design philosophy — John is historically CPU-focused with a huge library of format support, hashcat is GPU-accelerated and often faster for supported hash types; many practitioners use both depending on the situation
🟢 **Nice to Know:** Mimikatz extracts credentials (including cleartext passwords in some configurations, NTLM hashes, and Kerberos tickets) directly from a running Windows system's LSASS process memory — this is exactly the kind of post-exploitation action a Meterpreter session (Day 92) with sufficient privilege could enable

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Online vs. Offline Credential Attacks
* What it is: Hydra attacks a live service directly, submitting login attempts over the network and observing success/failure; hashcat/John crack password hashes you've already obtained (from a database dump, a memory extraction like Mimikatz, or a captured authentication exchange) entirely offline
* Why it matters: this distinction determines defense effectiveness — Day 24's rate limiting and account lockout specifically defend against online attacks like Hydra; Day 12's adaptive hashing (bcrypt/argon2) specifically defends against offline attacks like hashcat/John, since a fast offline crack against a slow hash becomes computationally infeasible regardless of rate limiting
* How it works: `hydra -l admin -P wordlist.txt ssh://<target>` attempts each password in the wordlist against the live SSH service, respecting (or deliberately testing) any rate-limiting in place

### Concept 2 — John the Ripper as hashcat's Sibling
* Definition: another major offline password-cracking tool, historically CPU-based with an extremely broad range of supported hash/cipher formats
* Practical application: `john --wordlist=wordlist.txt hashes.txt` runs a dictionary attack, directly parallel to Day 12's `hashcat` dictionary-attack exercise
* Best practices: know both tools exist and roughly when each shines (hashcat for GPU-accelerated speed on common formats, John for breadth of supported/legacy formats) — in practice, many practitioners default to whichever they're more fluent in for a given hash type

### Concept 3 — Mimikatz and Windows Credential Extraction
* Key terminology: LSASS, NTLM hash, Kerberos ticket
* Practical application: Mimikatz can extract plaintext passwords, NTLM hashes, and Kerberos tickets from a live Windows system's memory (specifically the LSASS process, which handles authentication) — this is the direct mechanism that feeds pass-the-hash attacks (Day 94) and further lateral movement
* Best practices: understanding this tool's existence and function is valuable defensive knowledge even without deep hands-on practice — it directly informs why modern Windows hardening (Credential Guard, LSASS protection) exists

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Online credential attack | Testing credentials against a live service | Defended against by rate limiting/lockout (Day 24) |
| Offline credential attack | Cracking already-obtained hashes locally | Defended against by adaptive hashing (Day 12) |
| Mimikatz / LSASS | Windows in-memory credential extraction | The direct mechanism feeding pass-the-hash attacks (Day 94) |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study the online/offline distinction, John the Ripper's role alongside hashcat, and Mimikatz's function.
**Output:** A written explanation connecting today's online/offline distinction directly back to Day 12's and Day 24's respective defenses.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Set up a lab SSH or FTP service with a deliberately weak password on a VM you control.
2. Run an authorized Hydra dictionary attack against it and confirm successful credential discovery.
3. Run John the Ripper against a sample hash file (reusing or extending Day 12's `hashcat` exercise for direct comparison).
4. Research Mimikatz's core commands and output format (via documentation, since running it meaningfully requires a Windows lab environment) and document what a typical credential-dump result looks like conceptually.

**Expected Result:** A documented, successful Hydra attack + a John the Ripper crack + Mimikatz research notes.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A service has no rate limiting or account lockout. Explain exactly why this makes it especially vulnerable to a Hydra-style attack.
### Problem 2
Compare your Day 12 hashcat results and today's John the Ripper results against the same hash file — did they perform similarly, and why might results differ?
### Problem 3
Explain how a successful Mimikatz credential dump on one compromised host could enable lateral movement via pass-the-hash (Day 94) to other hosts.
### Challenge
Design a complete authentication-hardening policy addressing both attack categories: rate limiting/lockout for online attacks, adaptive hashing for offline attacks, and LSASS protection/Credential Guard for Mimikatz-style extraction.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Active Directory Attacks (Day 94)
Recall without notes: BloodHound's attack-path model and pass-the-hash.
### Spaced-Repetition Review
* **Yesterday:** Active Directory attacks
* **13 weeks ago:** Day 12's password storage and hashcat — directly extended today

---

# 🧪 6. Active Recall Exercises
1. What's the difference between an online and an offline credential attack?
2. Why does rate limiting defend against Hydra but not against hashcat/John?
3. Why does adaptive hashing (bcrypt/argon2) defend against hashcat/John but not against Hydra?
4. What would an attacker need to run a Hydra attack effectively (a wordlist + a lack of rate limiting/lockout)?
5. What's the impact of a successful Mimikatz credential dump on a compromised Windows host?
6. How would you detect a Hydra-style brute-force attempt in logs (recalling Day 22–24's logging/SIEM material)?
7. How would you harden a Windows environment against Mimikatz-style extraction?
8. Difference between hashcat and John the Ripper's typical strengths?
9. Real-world example: how a Mimikatz credential dump directly enables Day 94's pass-the-hash lateral movement.
10. Teach the online/offline credential attack distinction to a junior developer, connecting explicitly to Days 12 and 24's defenses.

### Feynman Test
Explain the online/offline credential-attack distinction and how each is specifically defended against, in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Hydra, John the Ripper
### Today's Tool Goal
Run a successful online and offline credential attack, each against an authorized target.
### Commands / Features to Practice
```text
hydra -l admin -P wordlist.txt ssh://<lab-target>
john --wordlist=rockyou.txt hashes.txt
john --show hashes.txt
```
### Tool Success Criteria
I can run both Hydra and John the Ripper successfully and clearly articulate why each represents a different attack category with a different defense.

---

# 🏗️ 8. Project Connection
**Current Project:** Extended toolkit reference
Today's contribution: a documented online (Hydra) and offline (John the Ripper) credential-attack demonstration, directly extending your existing Day 12 hashcat work into a more complete credential-security narrative.
### Deliverable
`Day 95: Ran authorized Hydra online brute-force and John the Ripper offline crack against lab targets; documented Mimikatz's role in Windows credential extraction and its connection to pass-the-hash lateral movement.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "If I told you a company had both weak rate limiting AND weak password hashing, which would you fix first and why?"
### My Task
1. Reason through the relative risk of each (offline cracking against weak hashes is often the more severe, scalable risk once any breach exposes the hash database).
2. Explain that both should ultimately be fixed, but justify a prioritization.
3. Connect this to your Day 12 and Day 24 material directly.
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
* [ ] Study notes  * [ ] Hydra online attack completed  * [ ] John the Ripper offline crack completed
* [ ] Mimikatz research documented  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Online vs. offline credential attacks and their respective defenses
2. Your Hydra and John the Ripper results
3. Mimikatz's role in Windows credential extraction
4. How this connects to Day 94's pass-the-hash lateral movement
5. How tomorrow's forensics/IR topic covers what happens *after* a credential compromise is discovered

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Local lab VM (SSH/FTP service) + Day 12's hash samples
**Lab Name:** "Full Credential Attack Comparison: Online vs. Offline"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Directly compare an online and offline credential attack against your own lab setup, reinforcing why each requires a different defense.
### Environment
Target: your own lab VM + hash samples · Tools: Hydra, John the Ripper · Prerequisite: Day 12
### Lab Tasks
1. Set up a deliberately weak-password lab service.
2. Run a Hydra attack and record time-to-success.
3. Run John the Ripper against a hash sample and record time-to-success.
4. Add rate limiting to the lab service and re-test Hydra — confirm it's now slower/blocked.
5. Document the complete before/after comparison for both attack categories.
### What I Need to Discover
Does directly comparing these two attack categories side by side make the "different attacks need different defenses" lesson more concrete than studying either in isolation?
### Lab Success Criteria
Documented before/after results for both online and offline attacks, with clear reasoning about why each defense worked for its specific category.

---
---

# 🛡️ Day 96 — Digital Forensics & Incident Response Tools: Volatility, Autopsy, YARA

**Date:** 23/11/2026
**Phase:** Extended Toolkit — Forensics & Incident Response
**Primary Skill:** Memory forensics, disk forensics, and malware pattern-matching
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand memory forensics with Volatility — analyzing a RAM dump for running processes, network connections, and injected code.
* Understand disk forensics fundamentals with Autopsy/The Sleuth Kit.
* Write a basic YARA rule for malware pattern-matching, directly extending Day 24's detection-rule-writing skill to a new artifact type.

### Success Criteria
* Explain what memory forensics can reveal that disk forensics alone cannot.
* Run a basic Volatility analysis against a sample memory image.
* Write and test a working YARA rule.

---

# 📚 2. Topics to Study
### Primary Topic
**Digital Forensics: Memory, Disk, and Malware Pattern-Matching**
### Secondary Topics
* Volatility for memory (RAM) forensics
* Autopsy/The Sleuth Kit for disk forensics
* YARA for writing malware/pattern signatures

### Priority
🔴 **Must Know:** memory forensics captures a system's *live, running* state — processes, network connections, injected code, encryption keys in use — much of which never touches disk and is gone the moment the system powers off, which is exactly why Day 25's evidence-preservation lesson (image before you touch anything) matters so much
🟡 **Should Know:** YARA rules are pattern-matching signatures (similar in spirit to Day 24's detection-rule methodology, but for files/memory rather than logs) — used to identify malware families, suspicious file characteristics, or specific IOCs (indicators of compromise)
🟢 **Nice to Know:** Autopsy provides a GUI wrapper around The Sleuth Kit's disk-forensics capabilities (file recovery, timeline analysis, deleted-file examination) — useful for understanding what happened on a system's persistent storage over time

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Memory Forensics with Volatility
* What it is: Volatility analyzes a captured memory image (a snapshot of a system's RAM at a point in time) to extract running processes, open network connections, loaded DLLs, and even recover encryption keys or malware that never wrote itself to disk
* Why it matters: this directly extends Day 25's evidence-preservation lesson — memory forensics is exactly why capturing a RAM image *before* rebooting or otherwise disturbing a compromised system is so critical; volatile data is genuinely gone once power is lost
* How it works: `volatility -f memory.dmp --profile=<profile> pslist` lists running processes at the time of capture; other plugins reveal network connections, injected code, and more

### Concept 2 — Disk Forensics with Autopsy/The Sleuth Kit
* Definition: analyzing a disk image for file system artifacts — deleted files, timeline of file access/modification, browser history, and other persistent evidence
* Practical application: Autopsy provides a GUI for loading a disk image and browsing its file system, recovering deleted files, and building a timeline of activity
* Best practices: disk forensics complements memory forensics — memory shows what was happening live, disk shows the persistent trail left behind, and together they build a fuller incident timeline (directly recalling Day 25's IR lifecycle)

### Concept 3 — YARA for Pattern-Based Detection
* Key terminology: YARA rule, IOC (indicator of compromise)
* Practical application: a YARA rule defines string/byte patterns and logical conditions to identify files or memory matching a known malware family or suspicious characteristic — `yara my_rule.yar suspicious_file.exe` tests a file against the rule
* Best practices: this is directly analogous to Day 24's detection-rule-writing methodology (methodology-based, not naive, pattern matching) — a good YARA rule is specific enough to avoid false positives while still catching genuine variants

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Memory forensics (Volatility) | Analyzing a RAM snapshot for live system state | Captures volatile data that's gone once power is lost — evidence-preservation critical |
| Disk forensics (Autopsy) | Analyzing persistent file-system artifacts | Complements memory forensics for a full incident timeline |
| YARA rule | A pattern-matching signature for files/memory | Detection-rule methodology (Day 24) applied to malware/artifact identification |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study memory forensics, disk forensics, and YARA's role, connecting explicitly to Day 25's evidence-preservation and Day 24's detection-rule lessons.
**Output:** A written explanation of why capturing memory before rebooting a compromised system is critical, using specific volatile-data examples.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Obtain a sample memory image (several are publicly available specifically for forensics practice/training) and run basic Volatility analysis (`pslist`, `netscan` or equivalent).
2. Load a sample disk image into Autopsy and explore its file browser, timeline, and deleted-file recovery features.
3. Write a basic YARA rule matching a specific string pattern, and test it against both a matching and non-matching sample file.

**Expected Result:** Documented Volatility analysis output + an Autopsy exploration summary + a working, tested YARA rule.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
Explain what specific evidence a memory image could reveal about an in-progress attack that a disk image captured afterward would miss entirely.
### Problem 2
Write a YARA rule matching a specific, simple string pattern, and explain why overly broad patterns risk false positives (directly recalling Day 24's alert-fatigue lesson).
### Problem 3
Design a complete evidence-collection order of operations for a suspected active compromise: what do you capture first, second, third, and why?
### Challenge
Research a real (documented, historical) incident where memory forensics specifically was critical to the investigation's outcome, and summarize why.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Password & Credential Attacks (Day 95)
Recall without notes: online vs. offline attacks and Mimikatz's role.
### Spaced-Repetition Review
* **Yesterday:** Password and credential attacks
* **13 weeks + 2 days ago:** Day 25's incident response basics and evidence preservation — directly extended today

---

# 🧪 6. Active Recall Exercises
1. What can memory forensics reveal that disk forensics alone cannot?
2. Why does evidence-collection order matter (memory before disk, generally)?
3. How does a YARA rule work as a pattern-matching signature?
4. What would you need to properly capture volatile memory evidence during an active incident (a memory-imaging tool + the discipline to do it before any remediation action)?
5. What's the impact of skipping memory capture and going straight to disk imaging or system remediation?
6. How would you use Autopsy to build a timeline of a compromised system's activity?
7. How would you avoid an overly broad YARA rule that generates false positives?
8. Difference between memory forensics, disk forensics, and pattern-based detection (YARA) as complementary techniques?
9. Real-world example: an incident where memory forensics was specifically critical.
10. Teach the evidence-collection priority order (memory → disk → other artifacts) to a junior developer, connecting to Day 25's IR lifecycle.

### Feynman Test
Explain memory forensics, disk forensics, and YARA together as a complementary forensic toolkit, in 5–6 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Volatility, Autopsy, YARA
### Today's Tool Goal
Run a basic analysis with each of the three tools.
### Commands / Features to Practice
```text
volatility -f memory.dmp --profile=<profile> pslist
volatility -f memory.dmp --profile=<profile> netscan
yara my_rule.yar target_file
```
### Tool Success Criteria
I can run a basic Volatility process listing, explore a disk image in Autopsy, and write/test a working YARA rule.

---

# 🏗️ 8. Project Connection
**Current Project:** Extended toolkit reference
Today's contribution: a documented forensics workflow (memory analysis + disk exploration + a written YARA rule), extending your existing Day 22–25 logging/IR material into the forensic-investigation phase specifically.
### Deliverable
`Day 96: Ran Volatility memory analysis and Autopsy disk exploration against sample images; wrote and tested a YARA detection rule; documented evidence-collection priority order.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "Walk me through your first three actions the moment you confirm an active compromise on a production server."
### My Task
1. Explain your evidence-preservation-first approach (memory capture before any remediation).
2. Explain your subsequent disk-imaging step.
3. Connect this directly to Day 25's IR lifecycle (identification → containment, with evidence preservation happening throughout).
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
* [ ] Study notes  * [ ] Volatility analysis completed  * [ ] Autopsy exploration completed
* [ ] YARA rule written and tested  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Memory vs. disk forensics and evidence-collection order
2. YARA's pattern-matching approach
3. Your documented forensics workflow
4. How this connects to Day 25's IR lifecycle
5. How tomorrow's reverse-engineering and OSINT topics round out the extended toolkit's final day

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Sample memory/disk images (publicly available forensics training samples)
**Lab Name:** "Full Forensics Workflow: Memory, Disk, and Signature Detection"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Run a complete, documented forensics pass across memory analysis, disk exploration, and YARA rule-writing.
### Environment
Target: sample forensics images · Tools: Volatility, Autopsy, YARA · Prerequisite: today's fundamentals
### Lab Tasks
1. Run Volatility process/network analysis against a sample memory image.
2. Explore a sample disk image in Autopsy, including any deleted-file recovery.
3. Write and test a YARA rule against sample files.
4. Document all three results as one integrated forensics report.
5. Note the evidence-collection order you'd follow in a real incident.
### What I Need to Discover
Does seeing an actual process list pulled from a memory dump, or a deleted file recovered from a disk image, make the abstract "preserve evidence before remediation" lesson from Day 25 concrete in a new way?
### Lab Success Criteria
A documented, integrated forensics report covering memory, disk, and pattern-matching analysis.

---
---

# 🛡️ Day 97 — Reverse Engineering & OSINT: Ghidra, theHarvester, Maltego

**Date:** 24/11/2026
**Phase:** Extended Toolkit — Reverse Engineering & Open-Source Intelligence
**Primary Skill:** Basic binary reverse engineering and OSINT reconnaissance
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Understand Ghidra's role as a disassembler/decompiler for reverse engineering, and perform a basic analysis of a simple binary.
* Understand OSINT methodology and use theHarvester and Maltego for reconnaissance against a domain you control or are explicitly authorized to research.
* Complete the extended toolkit module with a consolidated reference of all seven days' tools.

### Success Criteria
* Explain what a disassembler/decompiler does and why reverse engineering matters for both offense (malware analysis) and defense (understanding what a compiled binary actually does).
* Perform a basic Ghidra analysis on a simple binary.
* Run theHarvester and explore Maltego against an authorized target domain.

---

# 📚 2. Topics to Study
### Primary Topic
**Reverse Engineering (Ghidra) and OSINT (theHarvester, Maltego)**
### Secondary Topics
* Ghidra's disassembly/decompilation workflow at a beginner level
* theHarvester for automated email/subdomain/employee reconnaissance from public sources
* Maltego for visual link-analysis across OSINT data sources

### Priority
🔴 **Must Know:** reverse engineering is the process of analyzing compiled binaries (with no source code) to understand what they actually do — essential for malware analysis, but also broadly useful for understanding closed-source software's behavior; a disassembler converts machine code to assembly, a decompiler attempts to reconstruct higher-level pseudocode
🟡 **Should Know:** OSINT (Open-Source Intelligence) reconnaissance uses only publicly available information — no direct interaction with the target's systems — making it low-risk from an authorization standpoint, though the information gathered (employee names, email formats, exposed subdomains) directly feeds later phases like phishing-simulation or targeted technical testing
🟢 **Nice to Know:** today's two topics represent genuinely deep specializations in their own right (reverse engineering/malware analysis and OSINT/social-engineering-adjacent recon) — today's goal is foundational exposure and vocabulary, not mastery, exactly like yesterday's forensics module

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Ghidra and Reverse Engineering Fundamentals
* What it is: Ghidra (NSA's free, open-source reverse-engineering suite) disassembles a compiled binary into assembly instructions and attempts decompilation into readable pseudocode, letting you analyze what a program does without its original source code
* Why it matters: this is the primary tool for malware analysis (understanding what a suspicious binary actually does before it's executed anywhere) and is also valuable for understanding closed-source software behavior more generally
* How it works: load a binary into Ghidra, let its auto-analysis run, then navigate the disassembly/decompilation views — function names, control flow, and string references (often the fastest way to get an initial sense of a binary's purpose) are good starting points for a beginner

### Concept 2 — theHarvester for Automated OSINT
* Definition: a tool that automatically gathers emails, subdomains, employee names, and other information from public sources (search engines, certificate transparency logs, public breach data where legally accessible) for a given domain
* Practical application: `theHarvester -d <domain> -b all` runs a broad sweep across its supported sources — useful for understanding an organization's public digital footprint, which is often the very first step of a real-world social-engineering-aware assessment
* Best practices: only run this against domains you own or have explicit authorization to research — even though the technique is passive/public-source-based, the target's identity still matters for authorization purposes

### Concept 3 — Maltego for Visual Link Analysis
* Key terminology: transform, entity graph
* Practical application: Maltego lets you visually build out relationships between entities (domains, IP addresses, people, organizations) by running "transforms" (queries against various OSINT data sources) and graphing the results — useful for seeing non-obvious connections across a large amount of gathered information, directly parallel to how BloodHound (Day 94) visualizes AD relationships or how Nmap+Shodan (Day 91) build up a picture of network exposure
* Best practices: like theHarvester, only use this against explicitly authorized targets

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Disassembler/decompiler (Ghidra) | Converts compiled binaries to assembly/pseudocode | The core tool for malware analysis and closed-source software understanding |
| theHarvester | Automated public-source OSINT gathering | Fast, passive reconnaissance feeding later assessment phases |
| Maltego | Visual, transform-based OSINT link analysis | Surfaces non-obvious relationships across gathered data, similar in spirit to BloodHound's graph model |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study Ghidra's basic workflow and OSINT methodology (theHarvester/Maltego). Install Ghidra if not already available.
**Output:** A written explanation of the difference between disassembly and decompilation, and why both views are useful.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Load a simple, deliberately non-malicious sample binary (e.g., a small compiled "Hello World"-style program you compile yourself, or a widely-used beginner reverse-engineering practice binary) into Ghidra and explore its auto-analysis, disassembly, and decompilation views.
2. Run theHarvester against a domain you own or a domain explicitly provided for practice purposes, and review the gathered information.
3. Explore Maltego's interface with a free/community edition, running a basic transform against an authorized target.

**Expected Result:** Documented Ghidra analysis of a simple binary + theHarvester OSINT results + a basic Maltego graph.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems + Full Extended-Toolkit Consolidation (30–45 min)
### Problem 1
Explain, in your own words, why a decompiler's output is described as "pseudocode" rather than a perfect reconstruction of the original source.
### Problem 2
Explain how theHarvester's gathered email-address patterns could inform a subsequent, explicitly-authorized phishing-simulation exercise.
### Problem 3
Reflect on the full 7-day extended module (Days 91–97) — which tool do you feel most confident with, and which would need more practice before you'd rely on it in a real assessment?
### Challenge
Write a one-paragraph summary connecting all seven days of this extended module into a single narrative: how recon (Nmap/Shodan/OSINT) → exploitation (Metasploit) → traffic analysis (Wireshark) → internal movement (AD attacks/credential attacks) → and forensics (Volatility/Autopsy/YARA) represent the full attacker lifecycle, mirrored by defensive understanding at every stage.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Digital Forensics (Day 96)
Recall without notes: memory/disk forensics and YARA rule-writing.
### Spaced-Repetition Review
* **Yesterday:** Digital forensics tools
* **This entire extended module (Days 91–97):** a full-arc recall check

---

# 🧪 6. Active Recall Exercises
1. What does Ghidra do, and what's the difference between its disassembly and decompilation views?
2. What kind of information does theHarvester gather, and from where?
3. How does Maltego's transform-based graphing help surface non-obvious relationships?
4. What would you need to responsibly run OSINT tools (a domain you own or explicit authorization)?
5. What's the impact of gathered OSINT data (emails, subdomains, employee names) on a subsequent assessment phase?
6. How would you use Ghidra to begin analyzing an unfamiliar, potentially malicious binary safely (in an isolated environment)?
7. How would you summarize the complete Days 91–97 extended module as one coherent attacker-lifecycle narrative?
8. Difference between passive OSINT reconnaissance and active scanning (Day 91)?
9. Real-world example: how OSINT gathering typically precedes a real-world social-engineering or targeted technical assessment.
10. Teach the complete extended toolkit (Days 91–97) to a junior developer as a rapid-fire overview.

### Feynman Test
Explain Ghidra, theHarvester, and Maltego, and then summarize the complete Days 91–97 extended module as one coherent story, in 6–8 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** Ghidra, theHarvester, Maltego
### Today's Tool Goal
Perform a basic analysis with each tool.
### Commands / Features to Practice
```text
# Ghidra: File > Import File, then Auto Analyze
theHarvester -d <authorized-domain> -b all
# Maltego: run a Domain-to-Email transform against an authorized target
```
### Tool Success Criteria
I can load and begin analyzing a simple binary in Ghidra, and run and interpret a basic theHarvester/Maltego OSINT query.

---

# 🏗️ 8. Project Connection
**Current Project:** Extended toolkit reference — final consolidation
Today's contribution: your Ghidra, theHarvester, and Maltego results, plus a final consolidated write-up summarizing the complete Days 91–97 extended module as a supplementary reference alongside your three core portfolio pieces.
### Deliverable
`Day 97: Completed basic Ghidra binary analysis and theHarvester/Maltego OSINT reconnaissance; published consolidated Days 91-97 extended toolkit reference summarizing the full attacker-lifecycle narrative.`

---

# 📝 9. Practice / Security Challenge
### Scenario
An interviewer asks: "Your background is mostly AppSec/DevSecOps/AI security — do you have any exposure to network penetration testing or malware analysis?"
### My Task
1. Honestly and confidently describe your Days 91–97 extended module as deliberate, supplementary exposure beyond your primary specialization.
2. Name specific tools and what you did with each (Nmap recon, Metasploit exploitation, Wireshark analysis, BloodHound AD path-tracing, Hydra/John credential attacks, Volatility/Autopsy/YARA forensics, Ghidra/OSINT).
3. Be honest that this is foundational exposure, not deep specialization — and explain that distinction clearly.
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
* [ ] Study notes  * [ ] Ghidra binary analysis completed  * [ ] theHarvester/Maltego OSINT completed
* [ ] Full Days 91-97 module consolidated and published  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Going Forward
1. Every tool across Days 91–97, at a working foundational level
2. The complete attacker-lifecycle narrative connecting all seven days
3. How to honestly and confidently discuss this supplementary exposure in an interview, distinct from your primary specialization
4. Which of these seven days you'd want to develop further if a future role required it
5. How this extended module complements, without replacing, your core 90-day AppSec/DevSecOps/AI security portfolio

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

**🏆 EXTENDED TOOLKIT MODULE COMPLETE (Days 91–97). Foundational, hands-on exposure across network reconnaissance, exploitation frameworks, traffic analysis, Active Directory attacks, credential attacks, digital forensics, reverse engineering, and OSINT — supplementing your core three-piece AppSec/DevSecOps/AI security portfolio with broader industry-standard tool fluency.**

---

# 14. Hands-On LAB
**Lab Platform:** Ghidra + theHarvester + Maltego
**Lab Name:** "Reverse Engineering and OSINT: Final Extended-Module Lab"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 90 minutes
### Lab Objective
Complete basic hands-on analysis with each tool and produce the final consolidated Days 91–97 write-up.
### Environment
Target: a simple compiled binary + an authorized domain · Tools: Ghidra, theHarvester, Maltego · Prerequisite: today's fundamentals
### Lab Tasks
1. Analyze a simple binary in Ghidra.
2. Run theHarvester against an authorized domain.
3. Explore Maltego's transform-based graphing.
4. Write the final consolidated summary connecting all of Days 91–97 into one attacker-lifecycle narrative.
5. Publish the complete extended-module reference document.
### What I Need to Discover
Having now touched every stage of a full attacker lifecycle — recon, exploitation, traffic analysis, internal movement, forensics, reverse engineering, and OSINT — does your understanding of *why* your primary AppSec/DevSecOps/AI security specialization matters feel sharper, now that you can see where it sits within the bigger picture?
### Lab Success Criteria
Completed hands-on work across all three of today's tools, and a published, consolidated Days 91–97 extended toolkit reference document.

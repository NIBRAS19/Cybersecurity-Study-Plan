# 🗓️ WEEK 8 — Cloud Security Fundamentals (AWS/GCP) & Month 2 Capstone (Days 50–56)

---

# 🛡️ Day 50 — IAM & Least Privilege (Cloud)

**Date:** 08/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Cloud IAM privilege escalation paths and least-privilege policy design
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Deepen your understanding of cloud IAM (returning to the exact system implicated in the Capital One breach, Day 14) at a hands-on policy-writing level.
* Understand common IAM privilege escalation paths (e.g., a user with `iam:PassRole` combined with permission to create compute resources).
* Write a genuinely least-privilege IAM policy for a specific task, rather than relying on broad managed policies.

### Success Criteria
* Explain at least one concrete IAM privilege escalation path without notes.
* Write a custom, least-privilege IAM policy (JSON) for a specific scenario.
* Explain permission boundaries and Service Control Policies (SCPs) as organization-wide guardrails.

---

# 📚 2. Topics to Study
### Primary Topic
**Cloud IAM & Least Privilege (AWS-focused, with GCP conceptual parallels)**
### Secondary Topics
* IAM policy structure (JSON: `Effect`, `Action`, `Resource`, `Condition`)
* Privilege escalation paths within IAM itself (not just IAM protecting against external attackers, but IAM misconfigurations allowing a low-privilege identity to escalate to a high-privilege one)
* Permission boundaries and Service Control Policies (SCPs) as guardrails limiting the maximum possible permissions, even if a specific policy is misconfigured

### Priority
🔴 **Must Know:** this week returns directly to the exact vulnerability class from the Capital One breach (Day 14) — you're now going deeper into actually writing and reasoning about IAM policies, not just understanding the concept
🟡 **Should Know:** at least one concrete IAM self-escalation path (e.g., a user permitted to attach policies to themselves, or `iam:PassRole` combined with the ability to launch compute resources that assume a more privileged role)
🟢 **Nice to Know:** GCP's equivalent IAM model (roles bound to members) shares the same core least-privilege principles despite different terminology

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — IAM Policy Structure and Least Privilege
* What it is: an IAM policy document specifies which actions (`Action`) are allowed or denied (`Effect`) on which resources (`Resource`), optionally with further restrictions (`Condition`)
* Why it matters: the Capital One breach specifically involved a role with permissions far broader than its actual function required — this week you learn to write policies precisely scoped to avoid recreating that exact mistake
* How it works: rather than attaching a broad AWS-managed policy like `AmazonS3FullAccess`, write a custom policy granting only `s3:GetObject` on a specific bucket ARN, for example
* Real-world example: revisit Capital One directly — the compromised role's permissions went well beyond what its function needed

### Concept 2 — IAM Privilege Escalation Paths
* Definition: certain combinations of IAM permissions allow an identity to escalate its own privileges, even without any external vulnerability — this is a misconfiguration risk distinct from (but related to) the "over-permissioned by design" problem
* Architecture/process: a well-known example pattern is a user with `iam:CreatePolicyVersion` or `iam:AttachUserPolicy` permission on their own user — they can simply grant themselves more permissions directly; another is `iam:PassRole` combined with permission to launch a compute instance that can assume a more privileged role than the user directly holds
* Defense/mitigation: audit for these specific dangerous permission combinations; tools exist specifically to detect IAM privilege-escalation paths in a real AWS account

### Concept 3 — Permission Boundaries and SCPs
* Key terminology: permission boundary (a maximum-permissions ceiling for a single IAM entity), Service Control Policy (an organization-wide maximum-permissions ceiling across many accounts)
* Practical application: even if a specific IAM policy is accidentally too broad, a permission boundary or SCP can act as a hard ceiling preventing that mistake from actually granting dangerous access
* Best practices: use permission boundaries/SCPs as defense-in-depth against the inevitable human error of writing an imperfect policy — this is the cloud-IAM equivalent of Week 3's CSP-as-defense-in-depth concept

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| IAM policy | JSON document defining allowed/denied actions on resources | The fundamental unit of cloud access control |
| Privilege escalation path | A combination of permissions allowing self-escalation | A distinct, specific risk beyond simple over-permissioning |
| Permission boundary / SCP | A hard ceiling on maximum possible permissions | Defense-in-depth against imperfect policy-writing, echoing CSP's role |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study IAM policy JSON structure, at least 2 specific privilege escalation path patterns, and permission boundaries/SCPs.
**Output:** Write, explicitly, how today's deeper IAM study directly extends your Day 14 Capital One analysis with concrete, actionable policy-writing skill.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Using an AWS Free Tier account (or the AWS Policy Simulator/documentation if you prefer not to use a live account), write a custom least-privilege IAM policy for a specific scenario: "an application needs to read objects from one specific S3 bucket and write CloudWatch logs, nothing else."
2. Test/validate the policy using the AWS IAM Policy Simulator.
3. Research and document one specific IAM privilege-escalation pattern in detail, including exactly which permission combination enables it.

**Expected Result:** A validated, working least-privilege IAM policy JSON document + documented research on one escalation pattern.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A developer requests "S3 access" for their application. Write the specific, minimal IAM policy rather than attaching `AmazonS3FullAccess`.
### Problem 2
A user has `iam:AttachUserPolicy` permission scoped to their own IAM user. Explain exactly how this enables self-privilege-escalation.
### Problem 3
Explain how a permission boundary would have limited (even if not fully prevented) the impact of Capital One's overly broad IAM role.
### Challenge
Design a complete least-privilege IAM strategy (custom policies + permission boundaries) for a 3-tier application (web tier, application tier, database tier), each with different, minimal required permissions.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Tesla Kubernetes Case Study (Day 49)
Recall without notes: the Tesla attack chain and its comparison to Capital One.
### Spaced-Repetition Review
* **Yesterday:** Tesla case study
* **6 weeks ago:** Capital One case study (Day 14) — directly extended today with hands-on policy work

---

# 🧪 6. Active Recall Exercises
1. What is the structure of an IAM policy document?
2. How does a privilege-escalation path within IAM work (give one specific example)?
3. Why do permission boundaries/SCPs matter as defense-in-depth?
4. What would you need to write a genuinely least-privilege policy (precise understanding of exactly what actions/resources a workload needs)?
5. What's the impact of an over-permissioned role, directly referencing Capital One?
6. How would you audit an existing AWS account for IAM privilege-escalation paths?
7. How would you use permission boundaries to guard against imperfect policy-writing?
8. Difference between a permission boundary (single entity) and an SCP (organization-wide)?
9. Real-world example: Capital One's IAM over-permissioning, now analyzed at the policy-writing level.
10. Teach least-privilege IAM policy writing to a junior developer in plain language.

### Feynman Test
Explain IAM least privilege, privilege-escalation paths, and permission boundaries together in 4–5 sentences, directly referencing Capital One. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** AWS IAM Policy Simulator, AWS CLI
### Today's Tool Goal
Write and validate custom IAM policies.
### Commands / Features to Practice
```text
aws iam simulate-custom-policy --policy-input-list file://policy.json --action-names s3:GetObject
```
### Tool Success Criteria
I can write a custom least-privilege policy from scratch and validate it correctly grants intended access while denying everything else.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (Month 2 capstone integration this week)
Today's contribution: write the least-privilege IAM policy your pipeline's deployment step will actually use (extending Day 37's OIDC federation discussion with the concrete policy document itself).
### Deliverable
`Day 50: Wrote and validated least-privilege IAM policy for pipeline deployment role; documented IAM privilege-escalation research.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A company's cloud team, under deadline pressure, grants a new microservice's IAM role `PowerUserAccess` "temporarily" to unblock a launch, planning to scope it down later — a plan that never happens.
### My Task
1. Identify the vulnerability/threat (directly recalling Capital One's pattern).
2. Explain the root cause (temporary broad access becoming permanent).
3. Determine the impact if this microservice is ever compromised.
4. Recommend a mitigation (scoped policy from day one + permission boundary as a safety net + a process ensuring "temporary" broad grants are actually revisited).
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
* [ ] Study notes  * [ ] Custom IAM policy written and validated  * [ ] Privilege-escalation pattern researched
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. IAM policy structure and least-privilege writing
2. IAM privilege-escalation paths
3. Permission boundaries and SCPs
4. Your validated least-privilege pipeline deployment policy
5. How tomorrow's S3/storage-misconfiguration topic extends today's IAM foundation to a specific, historically very common cloud vulnerability class

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** AWS Free Tier (or IAM Policy Simulator if avoiding a live account)
**Lab Name:** "Write and Validate a Least-Privilege IAM Policy"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Produce a genuinely minimal, validated IAM policy for a realistic scenario, directly applying Capital One's lesson at the hands-on policy-writing level.
### Environment
Target: AWS IAM (live account or simulator) · Tools: AWS CLI/Console, IAM Policy Simulator · Prerequisite: today's concepts
### Lab Tasks
1. Define the specific scenario (e.g., "read from one S3 bucket, write CloudWatch logs").
2. Write the custom policy JSON.
3. Validate it with the Policy Simulator against both intended and unintended actions.
4. Refine until it's both sufficient and minimal.
5. Document the final policy and validation results.
### What I Need to Discover
How much more precise can you make a policy compared to the broad managed policies AWS offers by default? Does this exercise change how you'd evaluate a real production IAM policy in a code/infrastructure review?
### Lab Success Criteria
A validated, genuinely least-privilege IAM policy document, confirmed via simulator testing to grant intended access and deny unintended access.

---
---

# 🛡️ Day 51 — S3/Storage Misconfigurations

**Date:** 09/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Cloud storage misconfiguration risks and hardening
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain why public S3 (or equivalent cloud storage) buckets have historically been one of the most common, severe cloud misconfiguration classes.
* Understand Block Public Access and bucket policy configuration.
* Practice safely enumerating publicly accessible storage buckets (in an authorized/educational context only).

### Success Criteria
* Explain the public-bucket misconfiguration pattern without notes.
* Configure Block Public Access and a restrictive bucket policy correctly.
* Explain encryption-at-rest's role as a complementary (not substitute) control.

---

# 📚 2. Topics to Study
### Primary Topic
**S3/Cloud Storage Misconfigurations**
### Secondary Topics
* Bucket policies vs. IAM policies (resource-based vs. identity-based access control)
* AWS Block Public Access as an account/bucket-level safety override
* Object-level ACLs and why they're increasingly discouraged in favor of bucket policies

### Priority
🔴 **Must Know:** an S3 bucket (or GCS bucket, or Azure Blob container) accidentally configured for public read (or worse, public write/list) access has been responsible for an enormous number of real-world data breaches over the years — this is one of the single most common cloud misconfigurations in the industry
🟡 **Should Know:** Block Public Access is a safety mechanism that can override individual bucket/object permission mistakes at the account or bucket level, providing defense-in-depth against exactly this class of error
🟢 **Nice to Know:** the historical trend of S3-bucket-focused security research tools/scanners specifically built to find these exposures

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — The Public Bucket Misconfiguration Pattern
* What it is: a storage bucket intended to be private is instead configured (via bucket policy, ACL, or a simple checkbox mistake) to allow public read (or list, or write) access
* Why it matters: this single misconfiguration class has been responsible for numerous major, widely-reported data exposures across many industries and company sizes
* How it works: a bucket policy with `"Principal": "*"` and `"Effect": "Allow"` for read actions grants access to literally anyone on the internet who discovers the bucket's name (bucket names are often guessable or discoverable via various enumeration techniques)
* Real-world example: numerous well-documented incidents across many sectors (government, healthcare, finance) have stemmed from exactly this misconfiguration

### Concept 2 — Block Public Access as a Safety Override
* Definition: an AWS account/bucket-level setting that, when enabled, prevents public access from being granted at all, regardless of what an individual bucket policy or ACL might otherwise allow
* Architecture/process: enabling Block Public Access account-wide means even a future mistaken bucket policy cannot accidentally expose data publicly — a genuine defense-in-depth safety net
* Best practices: enable Block Public Access by default at the account level, and only selectively disable it for the specific, deliberate, reviewed cases where public access is genuinely intended (e.g., a public static website bucket)

### Concept 3 — Bucket Policies vs. IAM Policies
* Key terminology: resource-based policy (bucket policy, attached to the resource) vs. identity-based policy (IAM policy, attached to the user/role)
* Practical application: both can grant or restrict access to the same bucket, and the *combination* of both determines final effective access — a common source of confusion and misconfiguration is not realizing both policy types interact
* Best practices: understand that a maximally restrictive posture requires both the IAM policy (identity side) and the bucket policy (resource side) to be correctly scoped

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Public bucket misconfiguration | Storage accidentally exposed for public read/write/list | One of the most common, historically severe cloud misconfiguration classes |
| Block Public Access | An override preventing public exposure regardless of policy mistakes | Defense-in-depth safety net against this exact error class |
| Bucket policy vs. IAM policy | Resource-based vs. identity-based access control | Both interact to determine effective access; understanding both is essential |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study the public bucket misconfiguration pattern, Block Public Access, and bucket-policy vs. IAM-policy interaction.
**Output:** Write, in your own words, why "we'll just remember to keep this bucket private" is an inadequate control compared to Block Public Access as an enforced safety mechanism.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Create a test S3 bucket in your AWS Free Tier account (or equivalent).
2. Deliberately configure it with an overly permissive bucket policy (public read) in a controlled, isolated test environment, and observe the access this grants (using only test/dummy data, never real sensitive information).
3. Enable Block Public Access and confirm it correctly prevents the public access even with the permissive policy still attached.
4. Correct the bucket policy to a properly restrictive configuration.

**Expected Result:** A documented demonstration of public exposure risk, Block Public Access's protective effect, and a properly hardened final configuration.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A bucket policy grants `s3:GetObject` to `"Principal": "*"`. Explain exactly who can access this bucket's objects and how they might discover its existence.
### Problem 2
Explain how Block Public Access, enabled at the account level, would have prevented Problem 1's misconfiguration from ever taking effect.
### Problem 3
A team disables Block Public Access account-wide "temporarily" to fix an urgent issue, and never re-enables it. What ongoing risk does this create?
### Challenge
Design a complete storage security policy for an organization: default Block Public Access posture, exception process for genuinely public buckets (e.g., static website hosting), and periodic audit cadence for reviewing any exceptions.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Cloud IAM & Least Privilege (Day 50)
Recall without notes: IAM policy structure, privilege-escalation paths, and permission boundaries.
### Spaced-Repetition Review
* **Yesterday:** Cloud IAM & least privilege
* **6 weeks + 1 day ago:** Capital One case study (Day 14) — this incident also involved S3 data specifically, directly relevant again today

---

# 🧪 6. Active Recall Exercises
1. Why has the public-bucket misconfiguration been so common historically?
2. How does Block Public Access provide defense-in-depth beyond correct bucket-policy configuration alone?
3. How do bucket policies and IAM policies interact to determine effective access?
4. What would an attacker need to exploit a public bucket (just its name, often discoverable via various enumeration techniques or accidental disclosure)?
5. What's the impact of a public bucket containing sensitive customer data?
6. How would you audit an AWS account for buckets with risky public-access configurations?
7. How would you design an exception process for legitimately public buckets while maintaining a secure default posture?
8. Difference between resource-based and identity-based access control?
9. Real-world example: connect this directly to Capital One's S3 data exfiltration.
10. Teach public-bucket misconfiguration risk and Block Public Access to a junior developer in plain language.

### Feynman Test
Explain the public-bucket misconfiguration pattern and Block Public Access's protective role in 4–5 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** AWS CLI/Console (S3 bucket policy and Block Public Access configuration)
### Today's Tool Goal
Configure and test bucket policies and Block Public Access settings.
### Commands / Features to Practice
```text
aws s3api put-bucket-policy --bucket my-test-bucket --policy file://policy.json
aws s3api put-public-access-block --bucket my-test-bucket --public-access-block-configuration BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
```
### Tool Success Criteria
I can configure a bucket policy and Block Public Access settings, and correctly predict the resulting effective access.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (Month 2 capstone integration)
Today's contribution: if your pipeline uses any cloud storage (e.g., for artifacts or logs), configure and document a properly hardened bucket policy with Block Public Access enabled.
### Deliverable
`Day 51: Configured and tested hardened S3 bucket policy with Block Public Access; documented public-bucket risk demonstration.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A healthcare startup stores patient document uploads in an S3 bucket. A routine security audit discovers the bucket has been configured for public read access since launch, six months ago.
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause (likely a default configuration never reviewed, or a well-intentioned but mistaken policy change).
3. Determine the impact given the sensitivity of healthcare data specifically (also connecting to regulatory/compliance implications).
4. Recommend immediate remediation (restrict access immediately, assess exposure scope, follow incident-response/breach-notification obligations as applicable) and long-term prevention (Block Public Access + periodic bucket audits).
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
* [ ] Study notes  * [ ] Public-bucket demonstration completed  * [ ] Block Public Access tested
* [ ] Hardened bucket policy documented  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. The public-bucket misconfiguration pattern and its historical prevalence
2. Block Public Access as a defense-in-depth safety mechanism
3. Bucket-policy vs. IAM-policy interaction
4. Your hardened storage configuration
5. How tomorrow's network security topic (VPC, security groups) extends this week's cloud-hardening focus to the network layer

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** AWS Free Tier
**Lab Name:** "Demonstrate and Remediate a Public Bucket Misconfiguration"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Safely demonstrate the public-bucket risk pattern end-to-end, using only dummy test data, and confirm Block Public Access's protective effect.
### Environment
Target: your own AWS Free Tier test bucket (never a real/production bucket) · Tools: AWS CLI/Console · Prerequisite: today's concepts
### Lab Tasks
1. Create a test bucket with only dummy, non-sensitive test data.
2. Apply an overly permissive bucket policy and confirm (from an unauthenticated context) that the dummy data is publicly readable.
3. Enable Block Public Access and confirm the same access attempt now fails, even with the permissive policy still attached.
4. Correct the bucket policy to a properly restrictive configuration.
5. Document the complete before/after demonstration.
### What I Need to Discover
How immediately did Block Public Access override the permissive policy? Does seeing this defense-in-depth mechanism work directly change how confidently you'd recommend it as a baseline organizational control?
### Lab Success Criteria
Documented, safely-conducted demonstration of public exposure, Block Public Access's protective override, and a final properly hardened configuration — using only dummy data throughout.

---
---

# 🛡️ Day 52 — Network Security: VPC Design, Security Groups, Flow Logs

**Date:** 10/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Cloud network segmentation and traffic monitoring
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Explain VPC (Virtual Private Cloud) design principles for network segmentation (public/private subnets).
* Configure security groups following least-privilege network access principles.
* Understand VPC Flow Logs as the network-layer equivalent of Week 4's application logging concepts.

### Success Criteria
* Explain public/private subnet segmentation without notes.
* Configure a security group allowing only necessary traffic (extending the least-privilege pattern to the network layer).
* Explain what VPC Flow Logs capture and how they'd be used for detection, connecting to Week 4's logging/SIEM material.

---

# 📚 2. Topics to Study
### Primary Topic
**VPC Design, Security Groups, and Flow Logs**
### Secondary Topics
* Public vs. private subnets and when each is appropriate
* Security groups (stateful, instance-level firewalls) vs. Network ACLs (stateless, subnet-level)
* VPC Flow Logs for network traffic visibility and detection

### Priority
🔴 **Must Know:** the principle of network segmentation — not every resource needs to be reachable from the public internet, and internal resources (databases, internal services) should live in private subnets with no direct internet route
🟡 **Should Know:** security groups are stateful (a response to an allowed inbound request is automatically allowed back out) and evaluated per-instance, while Network ACLs are stateless and evaluated per-subnet — both provide network-layer least privilege, at different granularities
🟢 **Nice to Know:** VPC Flow Logs can be fed into the same ELK/Splunk-style pipeline from Week 4 for network-layer detection engineering

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Public/Private Subnet Segmentation
* What it is: a VPC is divided into subnets; public subnets have a route to an internet gateway (directly internet-reachable), while private subnets do not (often routing outbound traffic only through a NAT gateway, with no direct inbound path from the internet)
* Why it matters: a database or internal application tier has no legitimate reason to be directly reachable from the public internet — placing it in a private subnet removes an entire category of direct-exposure risk, regardless of how well its own access controls (IAM, application auth) are configured
* How it works: a typical 3-tier architecture places the load balancer/web tier in public subnets, and the application and database tiers in private subnets, with security groups controlling exactly which tier can reach which
* Real-world example: numerous incidents have involved databases mistakenly left directly internet-reachable, entirely bypassing whatever access controls existed at the application layer

### Concept 2 — Security Groups as Least-Privilege Network Firewalls
* Definition: security groups act as a stateful, instance-level firewall specifying exactly which inbound/outbound traffic (by port, protocol, and source/destination) is permitted
* Architecture/process: rather than a broad "allow all inbound" rule, a properly configured security group for a database tier would allow inbound traffic only on the specific database port, only from the specific application tier's security group (not from the entire internet or even the entire VPC)
* Attack scenario this addresses: this directly extends Day 47's NetworkPolicy concept (Kubernetes pod-to-pod) to the broader cloud VPC network layer — the same least-privilege networking principle, one level up

### Concept 3 — VPC Flow Logs
* Key terminology: flow log, network-layer telemetry
* Practical application: Flow Logs record metadata about network traffic (source/destination IP, port, protocol, accepted/rejected) flowing through your VPC — directly analogous to Week 4's application-layer security logging, but at the network layer
* Best practices: feed Flow Logs into your logging/SIEM pipeline (Week 4) to detect anomalous network patterns (e.g., an internal instance suddenly making connections to an unusual external IP, potentially indicating compromise)

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Public/private subnet | Internet-reachable vs. internally-routed network segments | Removes direct-exposure risk for resources that never need public reachability |
| Security group | Stateful, instance-level network access control | Least-privilege networking, extending Day 47's NetworkPolicy concept to VPCs |
| VPC Flow Logs | Network-layer traffic telemetry | The network equivalent of Week 4's application security logging |

---

# ⏱️ 4. Study Schedule

## Session 1 — Fundamentals (45–60 min)
Study public/private subnet design, security groups vs. Network ACLs, and VPC Flow Logs.
**Output:** Write, explicitly, how today's security-group least-privilege concept directly parallels Day 47's Kubernetes NetworkPolicy concept — the same principle, different infrastructure layer.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. In your AWS Free Tier account, design (and optionally provision, if within free-tier limits) a simple VPC with one public and one private subnet.
2. Configure security groups: a web-tier security group allowing inbound HTTP/HTTPS from the internet, and a database-tier security group allowing inbound access only from the web-tier security group's ID (not from the internet or a broad CIDR range).
3. Enable VPC Flow Logs and review the captured traffic metadata format.

**Expected Result:** A documented VPC design with least-privilege security groups + a working Flow Logs configuration.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
A database's security group allows inbound traffic on its port from `0.0.0.0/0` (anywhere). Rewrite this to allow access only from the application tier's security group.
### Problem 2
Explain why placing a database in a private subnet provides protection even if its security group were somehow misconfigured (defense-in-depth, echoing today's recurring theme).
### Problem 3
Design a Flow-Logs-based detection rule (conceptually, extending Day 24's methodology) for identifying an internal instance communicating with an unexpected external IP.
### Challenge
Design a complete VPC architecture (subnets, security groups, Flow Logs) for a 3-tier application, explicitly connecting each design decision back to the least-privilege principle recurring throughout this curriculum.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: S3/Storage Misconfigurations (Day 51)
Recall without notes: the public-bucket pattern and Block Public Access.
### Spaced-Repetition Review
* **Yesterday:** S3/storage misconfigurations
* **1 week + 3 days ago:** Kubernetes RBAC & NetworkPolicies (Day 47) — directly paralleled today at the VPC layer

---

# 🧪 6. Active Recall Exercises
1. Why does public/private subnet segmentation matter even with strong application-layer access controls?
2. How do security groups implement least-privilege network access?
3. How do VPC Flow Logs relate to Week 4's application logging concepts?
4. What would you need to design a properly segmented VPC (understanding of which tiers genuinely need internet reachability)?
5. What's the impact of a database mistakenly placed in a public subnet with an overly permissive security group?
6. How would you use Flow Logs to detect anomalous network behavior?
7. How would you audit an existing VPC for overly permissive security group rules?
8. Difference between security groups (stateful, instance-level) and Network ACLs (stateless, subnet-level)?
9. Real-world example: databases mistakenly left directly internet-reachable, bypassing application-layer controls entirely.
10. Teach VPC segmentation and security groups to a junior developer, explicitly connecting to Day 47's Kubernetes NetworkPolicy parallel.

### Feynman Test
Explain VPC segmentation, security groups, and Flow Logs together in 4–5 sentences, connecting to the Kubernetes NetworkPolicy parallel. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** AWS CLI/Console (VPC, security group, Flow Logs configuration)
### Today's Tool Goal
Design and configure a segmented VPC with least-privilege security groups and Flow Logs.
### Commands / Features to Practice
```text
aws ec2 create-vpc --cidr-block 10.0.0.0/16
aws ec2 create-security-group --group-name db-tier --description "Database tier" --vpc-id vpc-xxxx
aws ec2 authorize-security-group-ingress --group-id sg-xxxx --protocol tcp --port 5432 --source-group sg-yyyy
aws ec2 create-flow-logs --resource-type VPC --resource-ids vpc-xxxx --traffic-type ALL --log-destination-type cloud-watch-logs
```
### Tool Success Criteria
I can design and configure a segmented VPC with correctly-scoped security groups referencing other security groups (not broad CIDR ranges) where appropriate.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (Month 2 capstone integration)
Today's contribution: document the VPC/network architecture your pipeline's deployment target would use, with security groups following least-privilege design, as part of your Month 2 capstone's infrastructure documentation.
### Deliverable
`Day 52: Designed and documented least-privilege VPC architecture (subnets, security groups, Flow Logs) for pipeline deployment target.`

---

# 📝 9. Practice / Security Challenge
### Scenario
A company's database instance has a security group allowing inbound traffic on its database port from `0.0.0.0/0`, "because it was easier to set up during initial development and no one revisited it before production launch."
### My Task
1. Identify the vulnerability/threat.
2. Explain the root cause (development convenience never hardened for production).
3. Determine the impact (direct internet-reachable database, bypassing application-layer access controls entirely).
4. Recommend a mitigation (restrict security group to specific application-tier security group, move to a private subnet).
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
* [ ] Study notes  * [ ] VPC/subnet design documented  * [ ] Security groups configured and tested
* [ ] Flow Logs enabled and reviewed  * [ ] Practice problems  * [ ] Active-recall answers
* [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Public/private subnet segmentation
2. Least-privilege security group design
3. VPC Flow Logs and their connection to Week 4's logging concepts
4. Your documented VPC architecture
5. How tomorrow's flaws.cloud CTF will let you apply this week's complete IAM/storage/network knowledge hands-on, offensively

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** AWS Free Tier
**Lab Name:** "Design and Configure a Segmented, Least-Privilege VPC"
**Difficulty:** ⭐⭐⭐☆☆
**Estimated Time:** 60–90 minutes
### Lab Objective
Configure a working, properly segmented VPC with least-privilege security groups and Flow Logs, directly applying this week's IAM/storage lessons to the network layer.
### Environment
Target: AWS Free Tier · Tools: AWS CLI/Console · Prerequisite: today's concepts
### Lab Tasks
1. Create a VPC with public and private subnets.
2. Configure security groups referencing other security groups (not broad CIDR ranges) for tier-to-tier communication.
3. Enable VPC Flow Logs.
4. Test that the security group rules correctly allow intended traffic and deny unintended traffic.
5. Document the complete architecture and test results.
### What I Need to Discover
Does designing this network architecture yourself, rather than just reading about it, change how automatically you now think "should this actually be publicly reachable?" for any new resource you might provision in the future?
### Lab Success Criteria
A working, documented, tested VPC architecture with least-privilege security groups and enabled Flow Logs.

---
---

# 🛡️ Day 53 — flaws.cloud CTF Part 1

**Date:** 11/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Applying cloud security concepts offensively in a guided CTF
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Complete the first several levels of flaws.cloud, a free, purpose-built AWS security CTF.
* Apply this week's IAM/S3/network concepts offensively, reinforcing understanding through hands-on exploitation.
* Document each level's vulnerability, exploitation path, and defensive lesson.

### Success Criteria
* Complete at least 3–4 levels of flaws.cloud.
* For each completed level, document the specific misconfiguration exploited and the corresponding defensive fix.
* Connect at least one level explicitly back to this week's IAM or S3 concepts.

---

# 📚 2. Topics to Study
### Primary Topic
**flaws.cloud: Applied AWS Security CTF**
### Secondary Topics
* flaws.cloud's structure (a series of progressively harder levels, each demonstrating a real AWS misconfiguration class)
* Connecting each level's specific vulnerability to Days 50–52's conceptual material
* Using AWS CLI tools for reconnaissance and exploitation within an authorized CTF context

### Priority
🔴 **Must Know:** flaws.cloud is specifically designed to teach real AWS misconfiguration patterns hands-on — every level maps to a genuine, common real-world mistake class
🟡 **Should Know:** how to use AWS CLI tools (`aws s3 ls`, etc.) for reconnaissance against a target you're authorized to test (flaws.cloud is explicitly built for this purpose)
🟢 **Nice to Know:** flaws.cloud's own hint system and how to use it as a genuine last resort, not a first response

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — flaws.cloud's Educational Design
* What it is: a deliberately vulnerable AWS environment, freely available, explicitly designed and authorized for security education — each level demonstrates a specific, realistic AWS misconfiguration
* Why it matters: this is your first opportunity this week to move from "configuring things correctly yourself" (Days 50–52) to "finding and exploiting the same misconfiguration classes when someone else got them wrong" — directly mirroring the Breaker-Defender framework at the cloud-infrastructure level
* How it works: each level typically requires finding some piece of exposed information or misconfigured access control that leads to the next level's credentials/access

### Concept 2 — Applying This Week's Concepts Offensively
* Definition: several flaws.cloud levels directly involve public S3 bucket enumeration (Day 51) and IAM misconfiguration exploitation (Day 50)
* Practical application: rather than just reading about "public buckets are dangerous," you'll directly experience discovering and exploiting one in a realistic (if deliberately constructed) scenario
* Best practices: for each level, before looking at any hint, explicitly ask yourself "which of this week's concepts (IAM, S3, VPC) does this level likely involve?" — this builds the pattern-recognition instinct real assessments require

### Concept 3 — AWS CLI for Reconnaissance
* Key terminology: anonymous/unauthenticated AWS API access, bucket enumeration
* Practical application: some flaws.cloud levels can be approached with unauthenticated AWS CLI calls (e.g., `aws s3 ls s3://bucket-name --no-sign-request` against a public bucket) — practice this exact reconnaissance technique
* Best practices: this technique is directly relevant to real-world authorized cloud security assessments

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| flaws.cloud | A free, authorized, purpose-built AWS security CTF | Lets you practice exploiting real misconfiguration classes hands-on |
| `--no-sign-request` | AWS CLI flag for unauthenticated requests | The technique for testing public bucket access without your own credentials |
| Breaker-Defender at cloud layer | Applying offense/defense framework to infrastructure, not just app code | This week's IAM/S3/VPC hardening (defense) now paired with CTF exploitation (offense) |

---

# ⏱️ 4. Study Schedule

## Session 1 — Setup & Early Levels (45–60 min)
Set up your environment for flaws.cloud (AWS CLI configured, or using the browser where applicable) and begin working through the first levels.

## ☕ Break (10–15 min)

## Session 2 — Hands-On Practice (60–90 min)
1. Continue working through flaws.cloud levels, attempting each before using hints.
2. For each completed level, document: what was misconfigured, how you found/exploited it, and the correct fix.
3. Explicitly connect at least one level to Day 50 (IAM) or Day 51 (S3) concepts.

**Expected Result:** 3–4 completed levels with full documentation.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems (30–45 min)
### Problem 1
For the first level you completed, explain the misconfiguration in the same actionable-finding format you've used throughout the curriculum (title, root cause, impact, fix).
### Problem 2
Which level, if any, required using an AWS CLI reconnaissance technique you hadn't used before this week?
### Problem 3
How does completing this CTF change your confidence in being able to recognize similar misconfigurations in a real cloud security assessment?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: Network Security (Day 52)
Recall without notes: VPC segmentation, security groups, Flow Logs.
### Spaced-Repetition Review
* **Yesterday:** Network security
* **8 weeks ago:** Week 1's injection material (a good long-interval check)

---

# 🧪 6. Active Recall Exercises
1. What is flaws.cloud, and what is it designed to teach?
2. For each level completed today, what was the specific misconfiguration?
3. How did Day 50/51's concepts directly help you recognize and exploit these misconfigurations?
4. What would you need to perform this kind of reconnaissance in a real authorized cloud assessment (proper scope authorization + the right CLI techniques)?
5. What's the impact of each misconfiguration you found, in a real-world context?
6. How would you detect these kinds of misconfigurations proactively (before a CTF/pentest finds them)?
7. How would you fix each one?
8. Difference between reading about a misconfiguration and actually exploiting one yourself?
9. Real-world example: connect today's levels to actual documented cloud misconfiguration incidents.
10. Teach your favorite flaws.cloud level's lesson to a junior developer in plain language.

### Feynman Test
Explain the misconfigurations from at least 2 completed flaws.cloud levels, and their fixes, in 4–5 sentences total. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** AWS CLI (unauthenticated/reconnaissance usage)
### Today's Tool Goal
Practice AWS CLI reconnaissance techniques against an authorized CTF target.
### Commands / Features to Practice
```text
aws s3 ls s3://flaws.cloud --no-sign-request
aws s3 cp s3://flaws.cloud/secret-... . --no-sign-request
```
### Tool Success Criteria
I can use AWS CLI reconnaissance techniques confidently and know when/why `--no-sign-request` is relevant.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (Month 2 capstone integration)
Today's contribution: document your flaws.cloud progress and lessons learned as a supplementary "applied cloud security" artifact, reinforcing your Month 2 capstone's credibility with hands-on CTF evidence.
### Deliverable
`Day 53: Completed 3-4 flaws.cloud levels; documented each misconfiguration, exploitation method, and fix in actionable format.`

---

# 📝 9. Practice / Security Challenge
### Scenario
This IS today's practice challenge — the flaws.cloud levels themselves.
### My Task
1. Identify the vulnerability/threat at each level.
2. Explain the root cause.
3. Determine the impact.
4. Reproduce the exploitation.
5. Recommend a mitigation.
6. Document each result.
### Difficulty
⭐⭐⭐☆☆ (progressive, increasing per level)

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
* [ ] 3-4 flaws.cloud levels completed  * [ ] Each level documented in actionable format
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. The misconfigurations from every level completed today
2. How Day 50/51's concepts directly applied
3. AWS CLI reconnaissance techniques
4. Your documented findings in actionable format
5. How tomorrow continues with the remaining flaws.cloud levels and flaws2.cloud

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** flaws.cloud
**Lab Name:** flaws.cloud Levels 1–4 (or as many as time allows)
**Difficulty:** ⭐⭐⭐☆☆ (progressive)
**Estimated Time:** 2+ hours (the bulk of today's schedule)
### Lab Objective
Complete the initial levels of flaws.cloud, directly applying this week's IAM and S3 concepts to real (if deliberately constructed) exploitation scenarios.
### Environment
Target: flaws.cloud (explicitly authorized for this purpose) · Tools: AWS CLI, browser · Prerequisite: Days 50–52
### Lab Tasks
1. Attempt each level before using hints.
2. Document the misconfiguration and exploitation for each completed level.
3. Explicitly connect at least one level to this week's IAM/S3 concepts.
4. Track progress and note where you'll continue tomorrow.
### What I Need to Discover
Does having spent this week configuring these exact services correctly yourself (Days 50–52) make you faster at spotting the misconfigurations here than you would have been without that hands-on defensive practice first?
### Lab Success Criteria
3–4 levels completed with full documentation in actionable finding format.

---
---

# 🛡️ Day 54 — flaws.cloud Part 2 / flaws2.cloud

**Date:** 12/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security
**Primary Skill:** Continued applied cloud security CTF practice
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate/Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Complete the remaining levels of flaws.cloud.
* Begin flaws2.cloud if time allows, which introduces additional/different AWS misconfiguration scenarios.
* Consolidate all CTF findings into one complete write-up.

### Success Criteria
* flaws.cloud is fully completed (all levels).
* At least the first 1–2 levels of flaws2.cloud are attempted.
* A complete, consolidated write-up of all CTF findings exists.

---

# 📚 2. Topics to Study
### Primary Topic
**Completing flaws.cloud & Introducing flaws2.cloud**
### Secondary Topics
* flaws2.cloud's different scenario focus (often involving different AWS services/misconfiguration classes than flaws.cloud)
* Consolidating a complete CTF write-up as a portfolio-quality artifact

### Priority
🔴 **Must Know:** be able to complete the full flaws.cloud CTF and articulate every level's lesson
🟡 **Should Know:** flaws2.cloud often introduces slightly different scenario types — approaching it with the same "which concept class does this involve" reasoning as yesterday
🟢 **Nice to Know:** how flaws.cloud/flaws2.cloud compare in difficulty and scenario design philosophy

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Completing the Full CTF
* What it is: finishing flaws.cloud's remaining levels, which typically increase in difficulty and may involve additional AWS services beyond S3/IAM (e.g., Lambda, CloudFront)
* Why it matters: completing the full CTF (not just the easier early levels) demonstrates genuine persistence and depth, both valuable qualities to convey in a portfolio
* Best practices: don't abandon a level after a quick attempt — the harder later levels are often where the most valuable, less-obvious lessons live

### Concept 2 — flaws2.cloud's Different Focus
* Definition: a follow-up CTF by the same creator, often built around a different architectural pattern (reportedly focusing more on container/Lambda-based scenarios in some versions) — approach it fresh rather than assuming it's simply "more of the same"
* Practical application: apply the same systematic reasoning ("what AWS service is this level built around, and what's this week's or earlier weeks' relevant concept") rather than expecting identical patterns to flaws.cloud

### Concept 3 — Consolidating the Complete Write-Up
* Key terminology: portfolio-quality CTF write-up
* Practical application: combine all completed levels (from both flaws.cloud and flaws2.cloud) into one polished document, following the same actionable-finding format used throughout your curriculum
* Best practices: this consolidated write-up becomes genuine, credible evidence of hands-on cloud security skill for your job search

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Full CTF completion | Finishing all levels, not just the easier early ones | Demonstrates genuine depth and persistence |
| flaws2.cloud | A related but distinct CTF with different scenario focus | Tests whether your reasoning generalizes beyond flaws.cloud's specific patterns |
| Consolidated write-up | A single polished document covering all completed CTF levels | A credible, portfolio-quality artifact of hands-on cloud security skill |

---

# ⏱️ 4. Study Schedule

## Session 1 — Complete flaws.cloud (45–60 min)
Finish any remaining flaws.cloud levels from yesterday.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: flaws2.cloud (60–90 min)
Begin flaws2.cloud, attempting its early levels with the same systematic, concept-first reasoning approach as yesterday. Document findings as you go.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems / Consolidation (30–45 min)
### Problem 1
Compare flaws.cloud and flaws2.cloud's scenario design — what's similar, what's different?
### Problem 2
Which single level across both CTFs taught you the most valuable, previously-unfamiliar lesson?
### Problem 3
Draft the outline for your consolidated CTF write-up document.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
### Previous Topic: flaws.cloud Part 1 (Day 53)
Recall without notes: the misconfigurations found in yesterday's completed levels.
### Spaced-Repetition Review
* **Yesterday:** flaws.cloud, part 1
* **1 week + 4 days ago:** Kubernetes RBAC (Day 47) — a good check on whether this concept remains solid

---

# 🧪 6. Active Recall Exercises
1. Summarize every flaws.cloud level's misconfiguration from memory.
2. How does flaws2.cloud's scenario design differ from flaws.cloud's?
3. What new AWS service or misconfiguration class, if any, did flaws2.cloud introduce?
4. What would you need to approach an unfamiliar CTF scenario systematically (concept-first reasoning, not pattern memorization)?
5. What's the cumulative impact story across all completed levels, if you imagine them as one real organization's infrastructure?
6. How would you use this CTF experience as evidence of hands-on skill in a job interview?
7. How would you continue building this skill further (additional free cloud security CTFs, previewed as an ongoing professional development habit)?
8. Difference between completing early/easy levels and persisting through harder ones?
9. Real-world example: connect at least one level to a documented real-world incident pattern.
10. Teach your single most valuable CTF lesson to a junior developer in plain language.

### Feynman Test
Summarize your complete flaws.cloud/flaws2.cloud experience and its most valuable lessons in 5–6 sentences. If unclear, mark 🟡 **Needs Review**.

---

# 🛠️ 7. Tool Practice
**Tool:** AWS CLI (continued reconnaissance practice)
### Today's Tool Goal
Apply reconnaissance techniques to new, unfamiliar scenario types in flaws2.cloud.
### Tool Success Criteria
I can approach an unfamiliar CTF level systematically, without needing yesterday's exact techniques to apply directly.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (Month 2 capstone integration)
Today's contribution: begin drafting the consolidated CTF write-up document combining all completed flaws.cloud/flaws2.cloud levels.
### Deliverable
`Day 54: Completed remaining flaws.cloud levels; began flaws2.cloud; drafted consolidated CTF write-up outline.`

---

# 📝 9. Practice / Security Challenge
### Scenario
This IS today's practice challenge — the remaining flaws.cloud levels and flaws2.cloud's initial levels.
### My Task
1. Identify the vulnerability/threat at each level.
2. Explain the root cause.
3. Determine the impact.
4. Reproduce the exploitation.
5. Recommend a mitigation.
6. Document each result in your consolidated write-up.
### Difficulty
⭐⭐⭐⭐☆ (progressive, increasing difficulty)

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
* [ ] flaws.cloud fully completed  * [ ] flaws2.cloud initial levels attempted  * [ ] Consolidated write-up drafted
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Project contribution  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Every completed level from both CTFs and their lessons
2. How your reasoning approach generalized (or didn't) across the two different CTFs
3. Your drafted consolidated write-up
4. How this hands-on cloud security practice strengthens your Month 2 capstone
5. How tomorrow's Month 2 capstone will integrate this week's cloud work with Weeks 5–7's pipeline/container work

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** flaws.cloud (completion) + flaws2.cloud (initial levels)
**Lab Name:** "Complete the Cloud Security CTF Arc"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 2+ hours (the bulk of today's schedule)
### Lab Objective
Fully complete flaws.cloud and begin flaws2.cloud, building a complete, portfolio-quality CTF write-up.
### Environment
Target: flaws.cloud, flaws2.cloud · Tools: AWS CLI, browser · Prerequisite: Day 53
### Lab Tasks
1. Complete any remaining flaws.cloud levels.
2. Attempt flaws2.cloud's initial levels.
3. Document every level in actionable format.
4. Consolidate into one draft write-up document.
5. Note progress for potential continuation if time runs short.
### What I Need to Discover
Having now worked through a substantial, multi-day cloud security CTF, do you feel genuinely more confident approaching an unfamiliar cloud environment for a real assessment than you did at the start of this week?
### Lab Success Criteria
flaws.cloud fully completed, flaws2.cloud initial levels attempted, complete draft write-up documenting all findings.

---
---

# 🛡️ Day 55 — Month 2 Capstone: Integrate Pipeline + Hardened Container + Cloud Config

**Date:** 13/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security (Capstone)
**Primary Skill:** Integrating all of Month 2's work into one cohesive, deployable secure system
**Estimated Total Time:** 4 hours
**Difficulty:** Advanced

---

## 🎯 1. Daily Goal / Expected Outcome
* Integrate Week 6's pipeline, Week 7's hardened container/Kubernetes configuration, and Week 8's cloud IAM/network hardening into one cohesive, end-to-end system.
* Confirm the complete integrated system (pipeline → hardened image → secure deployment target) works together.
* Identify and resolve any integration friction between the individually-built pieces.

### Success Criteria
* All three major Month 2 components (pipeline, container/K8s hardening, cloud IAM/network) are demonstrably connected in one system, not three separate, disconnected artifacts.
* Any integration issues discovered are documented and resolved (or honestly logged as known limitations).
* You can explain the complete, integrated architecture end-to-end.

---

# 📚 2. Topics to Study
### Primary Topic
**Integrating Month 2's Complete DevSecOps Toolchain**
### Secondary Topics
* Connecting pipeline outputs (a scanned, signed, hardened image) to a deployment target (Kubernetes with RBAC/NetworkPolicies, or cloud infrastructure with least-privilege IAM)
* Identifying and resolving real integration friction (individually-tested pieces often reveal new issues only when connected)
* Structuring the capstone narrative for interview presentation

### Priority
🔴 **Must Know:** integration is where individually-solid pieces often reveal unexpected friction — today's real work is making Weeks 6–8's separate accomplishments function as one genuinely connected system
🟡 **Should Know:** how to trace a single artifact (your application) through its complete lifecycle: code commit → pipeline gates (Week 6) → hardened, scanned, signed image (Week 7) → deployed with least-privilege IAM/RBAC and network segmentation (Weeks 7–8)
🟢 **Nice to Know:** how real DevSecOps teams structure exactly this kind of end-to-end secure delivery pipeline

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Tracing the Complete Artifact Lifecycle
* What it is: following your application from source code through to a running, deployed instance, verifying every security control from Weeks 6–8 genuinely applies at the correct stage
* Why it matters: this is the actual "Month 2 capstone" the original curriculum describes — not three separate deliverables, but one integrated demonstration of secure software delivery
* How it works: commit → Week 6's pipeline gates (SAST/SCA/secret-scan/DAST) → Week 7's hardened Dockerfile build → Week 7's image scanning/signing → deployment to Week 7's hardened Kubernetes configuration (RBAC/NetworkPolicy) or Week 8's hardened cloud infrastructure (IAM/VPC)

### Concept 2 — Resolving Integration Friction
* Definition: issues that only become apparent when previously-separate, individually-tested components are connected (e.g., the pipeline's build step producing an image tag format that doesn't match what your Kubernetes manifests expect, or an IAM role scoped for the pipeline not having quite the right permission for a new deployment step)
* Practical application: today is explicitly about finding and fixing these seams, not just re-confirming each piece works in isolation (which you've already done in Weeks 6–8)
* Best practices: document any friction found and how you resolved it — this itself is valuable, realistic DevSecOps experience to discuss in interviews

### Concept 3 — Structuring the Integrated Capstone Narrative
* Key terminology: end-to-end secure delivery pipeline
* Practical application: prepare to describe this not as "I did three separate things" but as "I built one complete secure software delivery system, and here's how each piece connects to the next"
* Best practices: this integrated framing is significantly more compelling in an interview than a list of disconnected accomplishments

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Artifact lifecycle | Tracing one application through its complete secure delivery path | The actual substance of "integration," not just individually-tested pieces |
| Integration friction | Issues only visible when components are connected | Realistic, valuable DevSecOps experience beyond isolated feature-testing |
| Integrated narrative | Presenting the work as one connected system | Significantly more compelling than a list of separate accomplishments |

---

# ⏱️ 4. Study Schedule

## Session 1 — Mapping the Complete Lifecycle (45–60 min)
Diagram (reusing your Week 3 diagramming skill) the complete artifact lifecycle from commit to deployment, marking exactly which Week 6/7/8 control applies at each stage.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Integrate and Test (90–120 min)
1. Confirm your Week 6 pipeline correctly builds using Week 7's hardened Dockerfile.
2. Confirm the pipeline's built image is scanned (Trivy) and signed (Cosign) as part of the automated flow, not just manually as in Week 7.
3. Confirm the image can be deployed to your Week 7 hardened Kubernetes configuration (RBAC/NetworkPolicy) or connects correctly to Week 8's IAM/VPC configuration if deploying to cloud infrastructure directly.
4. Identify and resolve any friction discovered during this integration.

**Expected Result:** A demonstrably connected, end-to-end system with documented integration fixes.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems / Reflection (30–45 min)
### Problem 1
What integration friction, if any, did you discover today that wasn't apparent when testing each piece individually in Weeks 6–8?
### Problem 2
Trace one specific security control (e.g., least-privilege IAM) through the complete lifecycle — at which exact stage does it apply, and how do you know it's actually being enforced there?
### Problem 3
If you had to explain this integrated system's value in one sentence to a non-technical hiring manager, what would you say?

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
Full Month 2 recall sweep: Secure SDLC → SAST/DAST/SCA → CI/CD security → Container/Kubernetes security → Cloud IAM/storage/network. This is your last comprehensive check before tomorrow's final Portfolio Piece #2 publication.
### Spaced-Repetition Review
* **Yesterday:** flaws.cloud/flaws2.cloud CTF practice
* **This month:** the complete DevSecOps arc

---

# 🧪 6. Active Recall Exercises
1. Trace your application's complete lifecycle from commit to deployment, naming every security control applied at each stage.
2. What integration friction did you discover and resolve today?
3. How does Week 6's pipeline connect concretely to Week 7's container hardening?
4. How does Week 7's container/Kubernetes work connect concretely to Week 8's cloud IAM/network work?
5. What's the overall business value of this integrated system, in plain language?
6. How would you extend this system further if given another month (e.g., runtime security monitoring, previewed as a natural next step)?
7. How would you present this integrated capstone in an interview, as one connected story rather than three separate projects?
8. Difference between individually-tested components and a genuinely integrated system?
9. Real-world parallel: how does this integrated pipeline compare to a real company's DevSecOps toolchain?
10. Teach your complete Month 2 capstone story to a junior developer in under 4 minutes.

### Feynman Test
Explain your complete, integrated Month 2 capstone system in 5–6 sentences, tracing the full artifact lifecycle. If unclear, mark 🟡 **Needs Review** before tomorrow's final publication.

---

# 🛠️ 7. Tool Practice
**Tool:** Full Month 2 toolkit — GitHub Actions, Docker, Trivy, Cosign, kubectl, AWS CLI
### Today's Tool Goal
Move fluidly across the complete toolkit to trace and test the integrated lifecycle.
### Tool Success Criteria
I can demonstrate the complete, connected flow without needing to context-switch awkwardly between "Week 6 mode," "Week 7 mode," and "Week 8 mode" — it should feel like one coherent system.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — Secure CI/CD Pipeline (final integration before tomorrow's publication)
Today's contribution: the fully integrated, demonstrably connected system spanning pipeline, container/Kubernetes hardening, and cloud configuration — this IS the complete Portfolio Piece #2.
### Deliverable
`Day 55: Completed Month 2 capstone integration - demonstrated full artifact lifecycle from commit through hardened, scanned, signed deployment with least-privilege IAM/RBAC enforcement.`

---

# 📝 9. Practice / Security Challenge
### Scenario
This IS today's practice challenge — the complete integration work itself.
### My Task
1. Trace the complete artifact lifecycle.
2. Identify every security control and confirm it's genuinely enforced at its intended stage.
3. Find and resolve integration friction.
4. Document the complete, connected system.
5. Prepare the integrated narrative for tomorrow's presentation.
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
* [ ] Complete artifact lifecycle diagrammed  * [ ] Integration completed and tested  * [ ] Friction documented and resolved
* [ ] Practice problems  * [ ] Active-recall answers  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow
1. Your complete, integrated Month 2 system end-to-end
2. Every specific integration issue you found and how you resolved it
3. How each Week 6/7/8 control maps to a specific lifecycle stage
4. Your interview-ready integrated narrative
5. How tomorrow's final documentation/publication step will present this as one cohesive Portfolio Piece #2

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link]
**Final Status:** 🟢 / 🟡 / 🔴

---

# 14. Hands-On LAB
**Lab Platform:** Your complete Portfolio Piece #2 repository/system
**Lab Name:** "Month 2 Capstone: End-to-End Integration Test"
**Difficulty:** ⭐⭐⭐⭐☆
**Estimated Time:** 3+ hours (the bulk of today's schedule)
### Lab Objective
Prove, with a real end-to-end demonstration, that your complete secure software delivery system works as one integrated whole, not three separate pieces.
### Environment
Target: your complete pipeline + container + cloud infrastructure · Tools: full Month 2 toolkit · Prerequisite: Weeks 6–8
### Lab Tasks
1. Trigger the complete pipeline from a real code commit.
2. Confirm every gate runs correctly (SAST, SCA, secret-scan, DAST).
3. Confirm the built image is hardened, scanned, and signed automatically.
4. Confirm deployment to your hardened Kubernetes/cloud target succeeds with least-privilege IAM/RBAC enforced.
5. Document the complete, successful end-to-end run.
### What I Need to Discover
Does the complete system genuinely work end-to-end when triggered by one real action (a commit), or did today's integration work reveal that some pieces were only ever tested in isolation? This is the real test of whether Month 2 produced a genuine system or just a collection of separately-functioning parts.
### Lab Success Criteria
A documented, successful, complete end-to-end run of the entire integrated system, triggered by a single real commit.

---
---

# 🛡️ Day 56 — Portfolio Piece #2 Finalized: Publish the Complete Secure Pipeline

**Date:** 14/10/2026
**Phase:** Month 2 — DevSecOps & Infrastructure Security (Capstone Completion)
**Primary Skill:** Final professional documentation and portfolio publication for a complex, multi-part project
**Estimated Total Time:** 3–4 hours
**Difficulty:** Intermediate

---

## 🎯 1. Daily Goal / Expected Outcome
* Finalize all documentation for the complete, integrated Portfolio Piece #2, following the same professional standards established for Portfolio Piece #1 (Day 28).
* Publish the complete repository with a polished README, architecture diagram, and all supporting case-study/CTF artifacts linked.
* Prepare and rehearse the complete interview narrative.

### Success Criteria
* A complete, polished README exists explaining the full integrated system (pipeline + container/K8s + cloud), with an architecture diagram.
* All supporting artifacts (Capital One case study, Tesla case study, flaws.cloud write-up, custom Semgrep rules) are clearly linked/referenced from the main README.
* You can present the complete Portfolio Piece #2, live, in under 4 minutes (slightly longer than Portfolio Piece #1's 3 minutes, given its greater scope).

---

# 📚 2. Topics to Study
### Primary Topic
**Finalizing a Complex, Multi-Part DevSecOps Portfolio Piece**
### Secondary Topics
* Structuring documentation for a project with multiple interconnected components (unlike Portfolio Piece #1's single-assessment-report structure)
* Linking supporting artifacts (case studies, CTF write-ups) without cluttering the primary README
* Preparing a longer, multi-part interview narrative while keeping it concise and compelling

### Priority
🔴 **Must Know:** how to structure documentation for a genuinely more complex project than Portfolio Piece #1 — this requires a clear primary README with an architecture overview, linking out to supporting detail rather than cramming everything into one document
🟡 **Should Know:** how to sequence an interview narrative for a multi-part project so it builds logically (threat model → pipeline → container hardening → cloud hardening → integration) rather than jumping between pieces
🟢 **Nice to Know:** how real open-source DevSecOps projects structure documentation for complex, multi-component systems as a style reference

---

# 🧠 3. Specific Subtopics & Concepts

### Concept 1 — Documentation Structure for a Complex Project
* What it is: a primary README providing the architecture overview and high-level narrative, with clearly linked supporting documents (case studies, CTF write-up, detailed configuration notes) for readers who want to go deeper
* Why it matters: unlike Portfolio Piece #1's single report, this project has genuinely multiple interconnected parts — cramming everything into one document would overwhelm a quick-skimming reviewer, exactly the failure mode Day 28's README lesson warned against
* How it works: README structure — project purpose and architecture diagram (the "what and why") → the 4 (now effectively 5, with image scanning) security gates explained briefly → container/Kubernetes hardening summary → cloud IAM/network hardening summary → links to: Capital One case study, Tesla case study, flaws.cloud/flaws2.cloud write-up, custom Semgrep rules directory
* Best practices: the primary README should be fully comprehensible in under 3 minutes of reading, with everything else available one click away for a deeper reviewer

### Concept 2 — Sequencing the Interview Narrative
* Definition: presenting this multi-part project in a logical build-up sequence rather than jumping between components
* Practical application: "I started by threat-modeling CI/CD-specific risks like poisoned pipeline execution (Week 6, Day 36), then built a pipeline with four automated security gates, then hardened the container layer with minimal images and Kubernetes RBAC, then extended the same least-privilege principle to cloud IAM and network segmentation — and I validated my cloud security understanding hands-on through the flaws.cloud CTF"
* Best practices: this sequencing naturally demonstrates depth and connects individual pieces into one coherent narrative, exactly like today's Day 55 integration work

### Concept 3 — Final Portfolio Quality Check
* Key terminology: portfolio-readiness (recalling Day 28)
* Practical application: verify every link works, every diagram renders correctly, and the complete repository would make a strong first impression on a hiring manager with only 2–3 minutes to review it
* Best practices: this is your second time doing this exact exercise (after Day 28), so it should feel more efficient and practiced now

### Key Terms to Remember

| Term | Meaning | Why It Matters |
|---|---|---|
| Layered documentation | Primary README + linked supporting detail documents | Prevents overwhelming a quick-skimming reviewer while still providing depth |
| Sequenced narrative | Presenting a multi-part project in logical build-up order | Demonstrates depth and coherence rather than a disconnected list |
| Portfolio-readiness (2nd iteration) | The same Day 28 quality bar, now applied to a more complex project | Should feel more practiced and efficient the second time |

---

# ⏱️ 4. Study Schedule

## Session 1 — README Structure & Architecture Diagram (45–60 min)
Draft the complete README structure and finalize the architecture diagram (extending Day 42's initial diagram with Week 7/8's container/cloud additions).
**Output:** A complete README draft with embedded architecture diagram.

## ☕ Break (10–15 min)

## Session 2 — Hands-On: Link Supporting Artifacts & Polish (60–90 min)
1. Link the Capital One case study, Tesla case study, and flaws.cloud/flaws2.cloud write-up clearly from the main README.
2. Clean up the complete repository structure (organize pipeline configs, Dockerfiles, Kubernetes manifests, IAM policies) for clean navigability.
3. Final proofread and polish pass across all documents.
4. Publish everything to GitHub.

**Expected Result:** A complete, polished, published, multi-part portfolio repository.

## ☕ Break (10–15 min)

## Session 3 — Practice Problems / Interview Prep (30–45 min)
### Problem 1
Write your elevator pitch for this complete, integrated Month 2 capstone (slightly longer than Portfolio Piece #1's, given the greater scope — aim for 3–4 sentences).
### Problem 2
Prepare an answer for: "This is a much bigger project than a typical portfolio piece — walk me through how it all fits together."
### Problem 3
Prepare an answer for: "What was the hardest part of integrating all these pieces together?" (Day 55's integration friction discussion is your genuine, specific answer here)
### Challenge
Practice presenting this complete capstone, live, unaided, in under 4 minutes, using the architecture diagram and linked artifacts as supporting reference.

---

# 🔄 5. Revision of Previously Studied Material
**Time: 20–30 minutes**
This IS the revision — finalizing documentation requires recalling and correctly representing every Month 2 concept. Use this time to proofread for technical accuracy against your actual Weeks 5–8 notes.
### Spaced-Repetition Review
* **Yesterday:** Month 2 capstone integration
* **This entire month:** the complete DevSecOps arc, now crystallized into one published deliverable

---

# 🧪 6. Active Recall Exercises
1. What is the complete structure of your Portfolio Piece #2 documentation (primary README + linked artifacts)?
2. How does layered documentation prevent overwhelming a quick-skimming reviewer?
3. Why does sequencing the interview narrative in build-up order matter?
4. What would a hiring manager look for first when reviewing this more complex portfolio piece?
5. What's the overall business value narrative this project tells?
6. How would you defend your integration approach if challenged on a specific design decision?
7. How would you extend this project further (runtime security monitoring, additional cloud services, previewing potential future learning)?
8. Difference between Portfolio Piece #1's single-document structure and Portfolio Piece #2's layered, multi-document structure?
9. Real-world parallel: how does this project's scope compare to a genuine junior-to-mid-level DevSecOps engineer's real-world project scope?
10. Teach your complete Month 2 capstone story to a junior developer, using it as their own future template, in under 4 minutes.

### Feynman Test
Present your Portfolio Piece #2's complete architecture and narrative, live, unaided, in under 4 minutes. If you stumble or need notes, mark 🟡 **Needs Review** and rehearse again.

---

# 🛠️ 7. Tool Practice
**Tool:** Markdown/GitHub (final complex-project documentation and publishing)
### Today's Tool Goal
Produce clean, professional, layered documentation for a genuinely complex multi-part project.
### Commands / Features to Practice
```text
git add . && git commit -m "Month 2 Capstone: Complete Secure CI/CD Pipeline with Container/Kubernetes and Cloud Hardening (Portfolio Piece #2)" && git push
```
### Tool Success Criteria
An interviewer could open this repository cold and, within 3 minutes, understand the complete system's architecture and be able to navigate to any supporting detail they want to explore further.

---

# 🏗️ 8. Project Connection
**Current Project:** Portfolio Piece #2 — COMPLETE
Today's contribution: the final, polished, published, layered documentation for the complete Month 2 capstone.
### Deliverable
`Day 56: PORTFOLIO PIECE #2 COMPLETE - Published complete Secure CI/CD Pipeline with Container/Kubernetes hardening and Cloud IAM/network security, including linked case studies (Capital One, Tesla) and applied CTF practice (flaws.cloud/flaws2.cloud).`

---

# 📝 9. Practice / Security Challenge
### Scenario
You're now in a real interview, and the interviewer says: "I see you have a secure CI/CD pipeline project in your portfolio that looks quite extensive — walk me through it."
### My Task
1. Deliver your elevator pitch.
2. Walk through the architecture diagram, sequencing the narrative: threat model → pipeline gates → container hardening → cloud hardening → integration.
3. Reference the linked case studies (Capital One, Tesla) as evidence of your analytical depth.
4. Reference the flaws.cloud CTF as evidence of hands-on applied skill.
5. Discuss the integration friction from Day 55 as a genuine, specific technical story.
6. Document the result.
### Difficulty
⭐⭐⭐⭐☆ (interview-simulation level, higher stakes given project scope)

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
* [ ] README finalized with architecture diagram  * [ ] All supporting artifacts linked  * [ ] Repository structure cleaned
* [ ] Complete narrative rehearsed  * [ ] Practice problems  * [ ] Active-recall answers  * [ ] Self-assessment

---

# 🧠 12. What I Should Be Able to Explain Tomorrow (Start of Month 3)
1. Your complete Portfolio Piece #2, cold, interview-ready
2. Every major Month 2 concept (SDLC, SAST/DAST/SCA, CI/CD security, container/K8s hardening, cloud IAM/network security) at a working professional level
3. How Month 2's infrastructure/DevSecOps focus sets up Month 3's AI security specialization
4. What you'd add to this project given more time (runtime monitoring, additional cloud services)
5. Your honest self-assessment of remaining Month 2 gaps to keep in mind during Month 3

---

# 🚀 13. Daily Completion Summary
**Today I learned / practiced / built / struggled with / need to review:** [fill in]
**GitHub Evidence:** [link to complete, finalized Portfolio Piece #2]
**Final Status:** 🟢 / 🟡 / 🔴

**🏆 MONTH 2 MILESTONE ACHIEVED: Portfolio Piece #2 (Secure CI/CD Pipeline with Container/Kubernetes and Cloud Hardening) is complete and published. Two of three portfolio pieces are now done — Month 3's AI security capstone is the final piece.**

---

# 14. Hands-On LAB
**Lab Platform:** N/A — today is a documentation/portfolio-finalization day
**Lab Name:** "Present the Complete Month 2 Capstone: Full Walkthrough"
**Difficulty:** ⭐⭐☆☆☆
**Estimated Time:** 45–60 minutes (folded into Session 3 above)
### Lab Objective
Confirm you can present this substantial, multi-part technical project clearly and confidently — converting two months of accumulated engineering work into a compelling, coherent interview narrative.
### Environment
Target: your own finalized, complete repository · Tools: none, just your voice/notes · Prerequisite: Days 29–55
### Lab Tasks
1. Present the full capstone walkthrough out loud, timed, using the architecture diagram.
2. Answer one self-generated likely interview follow-up question without notes.
3. Get feedback if possible (mentor, peer, or self-review via recording).
4. Note anything you'd tighten in either the documentation or your live delivery.
5. Confirm all GitHub repo links work and all diagrams/artifacts display cleanly.
### What I Need to Discover
Having now done this exact "present the completed capstone" exercise twice (Day 28 and today), do you notice yourself moving faster and more confidently through the process? That's genuine, measurable evidence of professional growth over these two months, worth recognizing explicitly as you head into Month 3.
### Lab Success Criteria
You can present the complete Portfolio Piece #2 clearly and confidently, unaided, in under 4 minutes, with the architecture diagram and linked artifacts as supporting reference.

# 📣 The Parallel Track — Visibility & Career Materials Guide

**Runs underneath Days 1–90 of your cybersecurity curriculum.**

---

## Why this exists

The 90-day curriculum is built to produce deep technical skill and three complete portfolio pieces. But nothing in it runs *continuously* — visibility work happens in bursts (three publish days, plus a concentrated Week 12 polish pass). That leaves value on the table:

- **A public learning journey is itself a credibility signal.** A hiring manager who sees 12 weeks of consistent, specific posts before you ever apply is a warmer lead than a cold application.
- **GitHub commit history is a signal independent of the code itself.** Steady activity across 90 days reads as discipline; three giant commits on publish days reads as a rush job, even when the underlying work is excellent.
- **Memory decays.** Reconstructing "what did I actually accomplish in Week 4" on Day 78, ten weeks later, is much harder than capturing it in five minutes at the time.
- **Networking compounds with time in the pipeline.** A connection made in Week 3 has nine more weeks to warm up before you need it in Week 12. A connection made in Week 12 has none.

None of this competes with the 5-hours/day study schedule. Total weekly overhead is **20–30 minutes**, mostly at natural breakpoints (end of each lab-consolidation day) that already exist in the curriculum.

---

## The four components, at a glance

| Component | Cadence | Time cost | Where it lives |
|---|---|---|---|
| LinkedIn post | Weekly (end of each week, Days 7/14/21.../84) | ~10 min | LinkedIn |
| GitHub hygiene | Continuous (every lab day) + weekly push | ~2 min/day | Your repos |
| Brag document | Weekly (same day as the LinkedIn post) | ~5 min | A single running doc |
| Networking touch | Weekly, ramping up in Month 3 | ~10 min | LinkedIn, communities |

---

## 1. Weekly LinkedIn Posts

**Rule of thumb:** post at the end of each week — the same day as that week's lab-consolidation/review day (Days 7, 14, 21, 28, 35, 42, 49, 56, 63, 70, 77, 84). You already have a natural "what did I do this week" reflection built into those days — the post is just that reflection, shortened and made public.

### Format (keep it under ~150 words)
1. **One-line hook** — what you did, stated plainly.
2. **One specific technical detail** — the thing that makes it credible, not generic ("I found X vulnerable to Y because Z" beats "learned a lot about security this week").
3. **One honest note** — something that was hard, or a mistake you caught yourself making. This is what makes these posts different from marketing copy, and it's what people actually engage with.
4. **A forward-looking line** — what's next.

Skip the hashtag-stuffing. Two or three relevant ones (`#cybersecurity #appsec #100DaysOfCode`-style) are plenty.

### Week-by-week hooks

Use these as starting prompts — don't copy them verbatim, write them in your own voice using your actual findings.

| Week | Hook |
|---|---|
| 1 | "Started a 90-day transition from full-stack dev into security. Week 1: exploited my first SQL injection end-to-end — union-based, blind, the works — and rebuilt the same endpoint with parameterized queries to see the fix actually close it." |
| 2 | "This week: forged a JWT by exploiting algorithm confusion, and found my first real IDOR. The pattern underneath both — trusting something the client controls — is the same one I kept seeing all week." |
| 3 | "Went from exploiting XSS/CSRF to building my first formal threat model (STRIDE + attack trees) on my own past project. Comparing what the threat model *predicted* against what I actually found by testing was the most useful exercise of the week." |
| 4 | "🏆 **Milestone:** published Portfolio Piece #1 — a full OWASP Top 10 assessment of OWASP Juice Shop, 15+ findings with CVSS scoring, MITRE ATT&CK mapping, and a remediation roadmap. Link in comments." |
| 5 | "Month 2 starts: shifting from finding bugs in a running app to preventing them before they ship. Wrote my first custom Semgrep rule this week — took the SQLi pattern from Week 1 and turned it into something a CI pipeline can catch automatically." |
| 6 | "Built a CI/CD pipeline with four automated security gates this week (SAST, SCA, secret scanning, DAST) — and specifically designed it around least-privilege tokens after digging into how the Capital One breach happened." |
| 7 | "Spent this week on container and Kubernetes security — demonstrated a Docker socket escalation from inside a container to full host access, then hardened it. Also wrote up the Tesla Kubernetes dashboard breach as a case study." |
| 8 | "🏆 **Milestone:** published Portfolio Piece #2 — a complete secure CI/CD pipeline with container hardening, image scanning/signing, and least-privilege cloud IAM, validated hands-on against flaws.cloud. Link in comments." |
| 9 | "Month 3: AI security. Spent this week on the OWASP Top 10 for LLM Applications and got hands-on with prompt injection — cleared several Lakera Gandalf levels and logged which techniques worked against which defenses." |
| 10 | "Mapped my prompt-injection work to MITRE ATLAS this week (the AI-specific sibling of ATT&CK), then built a small RAG app, broke its retrieval-layer access control myself, and fixed it — same IDOR pattern from Week 2, just applied to vector search instead of a database." |
| 11 | "🏆 **Milestone:** published Portfolio Piece #3 — a full security assessment of an AI-integrated web app, combining traditional OWASP Top 10 testing with an LLM red-team (with reproducibility scoring, not just single-shot findings) and an integrated threat model. This is the capstone. Link in comments." |
| 12 | "90 days, three complete security assessments, and I'm now actively interviewing. Reflecting on the whole arc: [your own honest 2–3 sentence takeaway]." |

For the three milestone weeks (4, 8, 11), this is your highest-leverage post of the entire 90 days — spend the extra few minutes to actually link the repo, and consider a short comment-thread follow-up highlighting your single best finding.

---

## 2. GitHub Hygiene

The goal: your commit history should look like what it actually is — 90 days of sustained, real work — not three commits on the days you happened to publish something.

### Daily habit (during any day that produces an artifact — most of them)
- Commit your day's work to the relevant repo **before you close your laptop**, even if it's a findings-log entry, a lab writeup, or a half-finished Dockerfile.
- Write commit messages that describe what you actually did, not `update` or `wip`: `Day 11: documented IDOR + vertical escalation findings on Juice Shop admin routes`.
- If a day's work doesn't belong in a portfolio repo yet (early exploration, a throwaway test), it's fine to keep it in a private scratch repo — but push *something* most days.

### Weekly habit (pair with your LinkedIn post)
- Push everything from the week.
- Update the relevant portfolio repo's README if the week added a new capability, finding, or artifact — don't let the README go stale relative to what's actually in the repo.
- Skim your commit history for the week — does it read as coherent progress to an outside viewer, or as noise? Squash/clean up only if it's genuinely confusing, not for cosmetic perfection.

### Structural habits (set up once, early)
- **Three portfolio repos**, one per piece, each with its own README from the day you start it — not retrofitted on publish day (Days 28/56/77 already cover the *polish* pass; this is about the README existing and being roughly accurate from day one).
- **A personal profile README** (the special repo matching your GitHub username) — even a bare-bones version from Week 1 is better than none until Week 12's proper pass.
- **Pin your three portfolio repos** as soon as each one has enough content to not look empty — don't wait until Day 79 to do this for the first time; update the pins as each new piece comes online.

---

## 3. The Brag Document

A single running file (in your portfolio repo, a private doc, wherever) — one entry per week, written the same day as your LinkedIn post while it's fresh. This is what turns Day 78's resume-writing from archaeology into assembly.

### Format per entry
```
## Week N (Days X-Y) — [theme]
- What I built/found: [1-2 sentences, specific]
- Skills demonstrated: [tools, techniques]
- Toughest problem solved: [1 sentence]
- Portfolio artifact: [link, if applicable]
```

### Why weekly beats "I'll remember it later"
By Day 78, you're eleven weeks past Week 1. You will not accurately recall the exact CVSS vector you assigned to your first SQLi finding, or the specific phrase you used to describe your JWT algorithm-confusion exploit. Capturing it in the moment means Day 78's resume work is a matter of skimming twelve entries and lifting the strongest lines — not trying to remember what you did in September.

### Bonus use
This document is also your raw material for:
- Day 80–81's mock interview answers (pull real specifics instead of generic summaries)
- Day 87's second mock interview, where you'll want fresh specific detail for weaker areas
- Any interview follow-up question that asks "can you give me another example"

---

## 4. Networking Cadence

Networking is lightest in Months 1–2 and ramps up deliberately in Month 3, when you have something increasingly substantial to point people to.

### Weeks 1–8 (light touch, ~10 min/week)
- Follow and genuinely engage (a real comment, not "Great post!") with 2–3 security people/accounts whose content you actually find useful.
- If a security subreddit, Discord, or local UAE tech/security meetup group looks relevant, join it now — lurk if you want, but join. Showing up in week 9 as a total stranger is harder than showing up as a name that's been quietly present since week 1.

### Weeks 9–11 (moderate, ~15–20 min/week)
- Start commenting more substantively — you now have real, specific things to say about prompt injection, RAG security, or AI red-teaming that most people in general security spaces haven't hands-on tried yet. This is your most differentiated material — use it.
- If there's a UAE-specific or MENA-region security meetup, conference, or Discord, consider attending or engaging directly — local presence matters disproportionately for a UAE job search.

### Week 12 and Days 85–90 (full activation)
- This is where Day 88's dedicated outreach sprint lives — see that day's full plan for direct messaging, informational interviews, and community contribution at volume.
- Your earlier, lighter-touch engagement (Weeks 1–11) is what makes Week 12's outreach land better — you're not a cold stranger messaging people out of nowhere; you're someone whose name and posts they may have already seen.

---

## Putting it together: a sample week

Using Week 4 (your first portfolio milestone) as an example:

- **Days 22–27:** normal curriculum work; commit daily as you go (`Day 22: built structured security-logging demo`, `Day 23: ELK dashboard analyzing login attack data`...).
- **Day 26–27 (capstone testing):** commit findings incrementally, not in one giant dump.
- **Day 28 (already in the curriculum):** publish Portfolio Piece #1, per the existing plan.
- **Same day, +15 min:** write your brag-document entry for Week 4. Post the Week 4 LinkedIn milestone post, linking the repo. Pin the repo to your GitHub profile if not already done.
- **That's it.** Total parallel-track overhead for the week: roughly 20–30 minutes, entirely absorbed into a day that was already a "wrap up and publish" day.

---

## A note on what NOT to do

- **Don't post daily.** Weekly is the right cadence — daily posting about a 5-hour study day reads as noise, not signal, and isn't sustainable for 90 days anyway.
- **Don't wait for perfection.** A slightly rough weekly post beats a perfect one you talked yourself out of writing.
- **Don't fabricate or round up.** Everything in this guide — the posts, the brag document, the outreach — should describe real work, honestly. The entire value of this parallel track is that it's *true*; a hiring manager who later finds a gap between your public narrative and your actual portfolio does more damage than the narrative ever helped.
- **Don't let this compete with the study time.** If a week is genuinely too heavy (capstone weeks especially) and something has to give, skip the LinkedIn post before you skip actual lab work — but still take the 5 minutes for the brag-document entry, since that's the cheapest, highest-leverage piece of this whole system.

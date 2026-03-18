# Secure Software Development Lifecycle (SDLC)

**Module 2 — Secure SDLC**

---

## The problem we're solving

Security is often bolted on at the end—or only in production.

- <span class="fragment">❌ How do we integrate security into each phase?</span>
- <span class="fragment">❌ How do we prioritise risks?</span>
- <span class="fragment">❌ How do we automate security in the pipeline?</span>

**Today:** Threat modeling, risk assessment, and security in CI/CD.

---

## Why this matters

- <span class="fragment">Late security findings are expensive and painful to fix.</span>
- <span class="fragment">Without shared risk language, everything becomes “critical”.</span>
- <span class="fragment">Manual reviews alone don’t scale with modern delivery.</span>

<span class="fragment">Secure SDLC gives you **where** and **when** security fits your existing workflow.</span>

---

## Security in every phase

- <span class="fragment">Requirements: what needs protecting? compliance, data classifications.</span>
- <span class="fragment">Design: data‑flow, trust boundaries, “what could go wrong?”.</span>
- <span class="fragment">Implementation: secure coding patterns and library choices.</span>
- <span class="fragment">Review & test: code review checklists, unit/integration/security tests.</span>
- <span class="fragment">Deploy & operate: secrets, configs, monitoring, incident response.</span>

<span class="fragment">Goal: **shift left** without blocking delivery.</span>

---

## Reality: where security often is

- <span class="fragment">A single “security review” before go‑live.</span>
- <span class="fragment">Annual pen test or audit, then a long list of findings.</span>
- <span class="fragment">Security team seen as an external gatekeeper.</span>

<span class="fragment">We want security to be **part of the flow**, not an afterthought.</span>

---

## Shift left and ownership

- <span class="fragment">**Shift left** = find and fix earlier (design, PR, CI).</span>
- <span class="fragment">**Ownership** = dev + product own security outcomes.</span>
- <span class="fragment">Security team provides policy, tooling, and escalation.</span>

<span class="fragment">Question: where does security currently show up in *your* process?</span>

---

## Example pipeline (simple view)

```text
dev → commit → pull request → build & unit tests → deploy to test → deploy to prod
```

- <span class="fragment">Where could we add a **dependency scan**?</span>
- <span class="fragment">Where could we run **SAST**?</span>
- <span class="fragment">Where would a **DAST** or ZAP scan fit?</span>

<span class="fragment">We’ll refine this map in later modules.</span>

---

## Threat modelling: the basic idea

- <span class="fragment">Draw how data flows (e.g. Browser → API → DB).</span>
- <span class="fragment">Mark where input is **untrusted** and where **sensitive data** lives.</span>
- <span class="fragment">Ask: “what could go wrong here?” at each boundary.</span>

<span class="fragment">You don’t need a big document—a simple sketch is enough.</span>

---

## Threats and risk

- <span class="fragment">Threat: specific “what could go wrong?” (e.g. SQLi, IDOR, data leak).</span>
- <span class="fragment">Risk = likelihood × impact (or a simple High/Medium/Low).</span>
- <span class="fragment">Use risk to decide **what to fix first** and what to accept or mitigate.</span>

<span class="fragment">Without this, everything is “P1” and nothing gets done.</span>

---

## Example: simple system diagram

```text
[User Browser] → [Web/App] → [API] → [Database]
                          ↘ [External Service]
```

- <span class="fragment">Where is input untrusted?</span>
- <span class="fragment">Where is sensitive data stored or processed?</span>
- <span class="fragment">What happens if auth/validation is missing at a boundary?</span>

<span class="fragment">We’ll use this pattern for the “threat sketch” exercise.</span>

---

## Exercise: Security in our pipeline

- <span class="fragment">Sketch your current pipeline (commit → PR → build → test → deploy).</span>
- <span class="fragment">Mark **one** place where you could add a security check.</span>
- <span class="fragment">Write what it would do (e.g. “npm audit on PR, fail on high”).</span>

<span class="fragment">We’ll share a few examples and revisit them in Module 7.</span>

---

## Threat sketch exercise

- <span class="fragment">Take Browser → API → DB (or your own app).</span>
- <span class="fragment">Individually, list **three threats** you can think of (one sentence each).</span>
- <span class="fragment">Decide for yourself: which would you fix first, and why?</span>


---


- <span class="fragment">**S – Spoofing**: pretending to be someone/something else (e.g. login bypass).</span>
- <span class="fragment">**T – Tampering**: changing data or code (e.g. modifying requests or DB records).</span>
- <span class="fragment">**R – Repudiation**: denying actions without a reliable audit trail.</span>
- <span class="fragment">**I – Information disclosure**: exposing data to someone who shouldn’t see it.</span>
- <span class="fragment">**D – Denial of service**: making a system or feature unavailable.</span>
- <span class="fragment">**E – Elevation of privilege**: gaining more rights than intended (e.g. user → admin).</span>

<span class="fragment">Use it as a **checklist** when you’re asking “what could go wrong?”—but don’t let it slow you down.</span>


---

## Automating security in CI/CD

- <span class="fragment">**Dependency scanning** (npm audit, Dependabot, Snyk).</span>
- <span class="fragment">**SAST** (static analysis for injection, unsafe APIs, secrets).</span>
- <span class="fragment">**Secret detection** (leaked keys/passwords in repo).</span>
- <span class="fragment">**DAST** (ZAP) on a running app—later modules.</span>

<span class="fragment">Aim: checks on **every change**, not once a year.</span>

---

## Tuning and gates

- <span class="fragment">Start with **warnings** while you tune noise.</span>
- <span class="fragment">Agree when to **fail the build** (e.g. critical/high only).</span>
- <span class="fragment">Document who can accept risk and how.</span>

<span class="fragment">The goal is trust in the tooling, not alert fatigue.</span>

---

## Exercise: First security gate

- <span class="fragment">Pick **one**: dependency scan, secret scan, or SAST rule.</span>
- <span class="fragment">Describe in one sentence what it checks and when it runs.</span>
- <span class="fragment">Example: “Secret scan on every PR; block merge if a key is found.”</span>

<span class="fragment">We’ll build on this when we add ZAP and other tools.</span>

---

## What we covered

1. <span class="fragment">**Security in each phase** — Requirements, design, code, review, test, deploy.</span>
2. <span class="fragment">**Threat modelling & risk** — Diagram + “what could go wrong?” + prioritisation.</span>
3. <span class="fragment">**Automation in CI/CD** — Dependency, SAST, and secret scans on every change.</span>

---

## Key takeaways (Module 2)

1. <span class="fragment">Security belongs in **every phase** of the SDLC, not just at the end.</span>
2. <span class="fragment">Simple threat modelling and risk assessment focus effort on what matters.</span>
3. <span class="fragment">Automated checks in the pipeline make “we checked” repeatable and scalable.</span>
4. <span class="fragment">Next: we’ll exploit real vulnerabilities (Module 3) before fixing and automating them.</span>

---

## Bridge to Module 3

- <span class="fragment">You now have a framework for **when and where** security fits your workflow.</span>
- <span class="fragment">Next: Module 3 — **Common vulnerabilities and exploitation** using OWASP Top 10 labs.</span>
- <span class="fragment">You’ll see what “broken” looks like so we can fix and automate in later modules.</span>

---

## Questions?

*Secure SDLC — Module 2*


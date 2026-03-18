# Introduction to Application Security

**Module 1 — Introduction to Application Security**

---

## The problem we're solving

Applications are everywhere—web, mobile, APIs—and so are threats.

- <span class="fragment">❌ How do I know what to secure first?</span>
- <span class="fragment">❌ What are the main risks?</span>
- <span class="fragment">❌ How does security fit into delivery?</span>

**Today:** Set the scene—why app security matters, common threats, and risks in web and mobile apps.

---

## Why this matters

- <span class="fragment">Most breaches start in application code, not firewalls or OS.</span>
- <span class="fragment">A single unchecked input or missing ownership check can expose real data.</span>
- <span class="fragment">Developers are the first line of defence; security teams can’t review every line.</span>

<span class="fragment">Goal for this module: a shared picture of **where attacks touch your systems** and **what kinds of threats exist**.</span>

---

## The modern attack surface

Applications are the main target: web apps, mobile apps, APIs, and the data they handle.

- <span class="fragment">Web apps (forms, URLs, cookies, headers).</span>
- <span class="fragment">Mobile apps (APIs, device data, stored tokens).</span>
- <span class="fragment">APIs (JSON/XML, headers, partner integrations).</span>

<span class="fragment">Attackers go where the value is—credentials, PII, business logic.</span>

---

## Key concepts: attack surface & trust boundaries

- <span class="fragment">**Attack surface** = every place an attacker can send data or influence behaviour.</span>
- <span class="fragment">Examples: forms, query params, JSON bodies, headers, file uploads, webhooks.</span>
- <span class="fragment">**Trust boundary** = where data crosses from untrusted (user/partner/internet) to trusted (backend/DB).</span>
- <span class="fragment">Every trust boundary is a place to **validate input and enforce policy**.</span>

---

## Reality: what teams often do

- <span class="fragment">Focus on infra security (firewalls, TLS, patching) and **assume the app is trusted**.</span>
- <span class="fragment">Rely on client-side validation and “internal” APIs.</span>
- <span class="fragment">Treat security findings as “someone else’s problem” (security team, ops, auditors).</span>

<span class="fragment">In practice, small app-level mistakes cause the biggest incidents.</span>

---

## Developers and vulnerabilities

Vulnerabilities are usually **unintentional**:

- <span class="fragment">Missing validation (accepting whatever the client sends).</span>
- <span class="fragment">Copy‑pasted code (string‑built SQL, unsafe HTML rendering).</span>
- <span class="fragment">Assumptions about “who can call this API”.</span>
- <span class="fragment">Outdated or vulnerable dependencies.</span>

<span class="fragment">Same people who introduce issues are best placed to **prevent and fix** them.</span>

---

## Normal code, security impact

- <span class="fragment">Search box concatenates user input into SQL → SQL injection.</span>
- <span class="fragment">`GET /api/ticket?id={id}` with no ownership check → IDOR / broken access control.</span>
- <span class="fragment">Comment rendered with `innerHTML` → stored XSS.</span>

<span class="fragment">Each looks like “normal feature work” until someone exploits it.</span>

---

## Exercise (short): “What could go wrong?”

- <span class="fragment">Look at a simple API snippet: `GET /api/ticket?id={id}` returning a ticket.</span>
- <span class="fragment">In pairs/groups (or chat): list **what an attacker could try**.</span>
- <span class="fragment">No exploit details yet—just hypotheses.</span>

<span class="fragment">This primes the mindset for later hands‑on exploitation.</span>

---

## Common threat families (high level)

- <span class="fragment">**Injection** — user input ends up in queries/commands/HTML.</span>
- <span class="fragment">**Broken access control (IDOR)** — no check that “this user owns this resource”.</span>
- <span class="fragment">**Auth/session flaws** — weak login, bad session handling, token leaks.</span>
- <span class="fragment">**Sensitive data exposure** — PII in logs, over‑broad API responses, missing TLS.</span>

<span class="fragment">We’ll go hands‑on with these in Module 3.</span>

---

## Exercise: Security news → threat family

- <span class="fragment">Pick a recent security story (or I’ll provide one).</span>
- <span class="fragment">In pairs/groups: answer</span>
  - <span class="fragment">**What was the impact?** (data loss, account takeover, outage, fraud)</span>
  - <span class="fragment">**Which threat family fits best?** (injection / IDOR / auth-session / data exposure)</span>
  - <span class="fragment">**What single control would have reduced risk most?**</span>

<span class="fragment">Goal: practise “spot the pattern” before we touch any tools.</span>

---

## Threats vs. attack vectors

- <span class="fragment">**Attack vector** = how an attacker reaches you (form, API, upload, header, webhook).</span>
- <span class="fragment">**Threat** = what they’re trying to do (steal data, escalate, disrupt).</span>
- <span class="fragment">Same threat can use many vectors; same vector can enable many threats.</span>

<span class="fragment">Use both when prioritising: “internet‑facing API + PII” is higher risk than an internal admin tool.</span>

---

## Where you’ve seen this

- <span class="fragment">Security reports mentioning SQL injection or XSS in “boring” endpoints.</span>
- <span class="fragment">Bugs that crashed or mis‑behaved but “nobody exploited them (as far as we know)”.</span>
- <span class="fragment">“Internal‑only” APIs that quietly became internet‑facing.</span>

<span class="fragment">This course connects those experiences to concrete patterns and fixes.</span>

---

## Web, mobile, and APIs: same ideas, different edges

- <span class="fragment">**Web** — forms, cookies, headers; XSS and CSRF are key concerns.</span>
- <span class="fragment">**Mobile** — network APIs, local storage, logs, tokens on device.</span>
- <span class="fragment">**APIs** — tokens/keys, rate limiting, over‑broad responses, IDOR.</span>

<span class="fragment">Core principles (validate, authenticate, authorise, encode, protect data) stay the same.</span>

---

## “Internal” doesn’t mean safe

- <span class="fragment">“It’s only for our frontend” → ends up exposed through a misconfigured gateway.</span>
- <span class="fragment">“It’s behind VPN” → a compromised laptop or partner can still call it.</span>
- <span class="fragment">“It’s just a debug endpoint” → forgotten and left enabled in production.</span>

<span class="fragment">Design for explicit **trust boundaries**, not wishful thinking.</span>

---

## Lab: Map Your Attack Surface

- <span class="fragment">Sketch a simple system (e.g. TrustyTickets or your app): browser → API → DB (+ external services).</span>
- <span class="fragment">Mark **attack surface** (inputs, calls, uploads, headers).</span>
- <span class="fragment">Mark at least **two trust boundaries** and what’s untrusted at each side.</span>
- <span class="fragment">Ask “what could go wrong?” at 2–3 key points; pick the highest‑risk one.</span>

<span class="fragment">You’ll reuse this map in Module 2 (threat modelling) and Module 3 (exploitation).</span>

---

## What we covered

1. <span class="fragment">**Modern attack surface** — apps (web, mobile, APIs) are primary targets.</span>
2. <span class="fragment">**Developer role** — most vulns are unintentional and fixable by dev teams.</span>
3. <span class="fragment">**Threat families** — injection, access control, auth/session, data exposure.</span>
4. <span class="fragment">**Context** — web, mobile, and APIs share principles but differ in edges.</span>

---

## Key takeaways (Module 1)

1. <span class="fragment">App security matters because **application code is where most breaches start**.</span>
2. <span class="fragment">Think in terms of **attack surface** and **trust boundaries** when designing and reviewing.</span>
3. <span class="fragment">Small, “normal” coding decisions can become serious vulnerabilities.</span>
4. <span class="fragment">Next modules will let you **exploit** these issues (Module 3) and then **fix and automate** (Modules 4–7).</span>

---

## Bridge to Module 2

- <span class="fragment">Here we focused on **where** attacks touch your system and **what kinds** of threats exist.</span>
- <span class="fragment">Next: Module 2 — **Secure SDLC**: where security fits into your lifecycle and how to model threats and risks.</span>
- <span class="fragment">That gives structure for **when** to think about trust boundaries and **when** to add checks and automation.</span>

---

## Questions?

*Introduction to Application Security — Module 1*


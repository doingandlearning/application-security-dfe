# Understanding Common Security Vulnerabilities

**Module 3 — Common vulnerabilities**

---

## The problem we're solving

Developers hear about "OWASP" and "vulnerabilities" but may not see how they show up in code.

- <span class="fragment">❌ What are the top risks and how do they happen?</span>
- <span class="fragment">❌ What do they look like in real apps?</span>
- <span class="fragment">❌ How do we safely practice finding them?</span>

**Today:** OWASP Top 10 (focus set), common coding flaws, and hands-on exploitation in a safe lab.

---

## Why this matters

- <span class="fragment">Reading about SQL injection or XSS is not the same as **exploiting** them.</span>
- <span class="fragment">Once you’ve done the attack yourself, you recognise the same pattern in code review.</span>
- <span class="fragment">Hands‑on experience makes later “fix and automate” modules feel necessary, not theoretical.</span>

<span class="fragment">Goal: you leave having **broken** a lab app and knowing which code made it possible.</span>

---

## OWASP Top 10 (high-level)

OWASP Top 10 = most critical web app security risks.

- <span class="fragment">We’ll focus on:</span>
  - <span class="fragment">**Injection (A03)** — untrusted data in queries/commands/templates.</span>
  - <span class="fragment">**Broken Access Control (A01)** — IDOR and missing checks.</span>
  - <span class="fragment">**Cross‑Site Scripting (XSS)** — untrusted data as HTML/JS.</span>
  - <span class="fragment">We’ll **mention** authentication/session issues and cryptographic failures/data exposure (we’ll go deeper later).</span>

<span class="fragment">The rest we’ll name briefly and point you to docs for self‑study.</span>

---

## Focus for this module

- <span class="fragment">**Injection** → make the app run attacker‑controlled queries.</span>
- <span class="fragment">**Broken access control (IDOR)** → access someone else’s data.</span>
- <span class="fragment">**XSS** → run attacker‑controlled JS in a victim’s browser.</span>

<span class="fragment">We’ll do 1–3 of these in DVWA/WebGoat/TrustyTickets depending on time.</span>

---

## Injection (SQLi) – concept

- <span class="fragment">Untrusted input concatenated into a SQL query or command.</span>
- <span class="fragment">Examples: search boxes, filters, login forms.</span>
- <span class="fragment">Impact: read/modify data, sometimes entire database compromise.</span>

```sql
-- vulnerable pattern
SELECT * FROM Tickets WHERE Title LIKE '%{search}%';
```

<span class="fragment">In lab: you’ll use payloads to pull back data you shouldn’t see.</span>

---

## Broken access control / IDOR – concept

- <span class="fragment">Endpoint uses an ID from the request but never checks **ownership**.</span>
- <span class="fragment">Example: `/api/tickets/123` returns ticket 123 for any logged‑in user.</span>
- <span class="fragment">Impact: read or modify other users’ data.</span>

```http
GET /api/tickets/123        # what if you change 123 → 124?
```

<span class="fragment">In lab: you’ll change IDs to view or act as another user.</span>

---

## XSS – concept

- <span class="fragment">Untrusted input rendered as HTML/JS, often via `innerHTML` or unescaped templates.</span>
- <span class="fragment">Stored XSS: payload saved in DB and shown to others.</span>
- <span class="fragment">Impact: session theft, keylogging, defacement, phishing.</span>

```html
<!-- vulnerable pattern -->
<div id="comment"></div>
<script>
  comment.innerHTML = userComment;  <!-- userComment is untrusted -->
</script>
```

<span class="fragment">In lab: you’ll craft a payload that pops an alert or steals a cookie (in theory).</span>

---

## Lab environment and safety

- <span class="fragment">We use **deliberately vulnerable apps** (DVWA, WebGoat, TrustyTickets).</span>
- <span class="fragment">They run in an **isolated lab** (local VM/container, no real data).</span>
- <span class="fragment">Techniques are for authorised testing only; unauthorised use is illegal.</span>

<span class="fragment">Rule: “Break only what you own or are explicitly allowed to test.”</span>

---

## Demo: one exploit together

- <span class="fragment">We’ll walk one example as a group, e.g.:</span>
  - <span class="fragment">SQL injection in a search box, or</span>
  - <span class="fragment">IDOR by changing an ID, or</span>
  - <span class="fragment">Stored XSS in a comment field.</span>

<span class="fragment">Then you’ll repeat and extend the attack in your own lab instance.</span>

---

## Lab: Exploit Common Vulnerabilities

- <span class="fragment">Work in pairs/small groups in DVWA/WebGoat/TrustyTickets.</span>
- <span class="fragment">Targets (depending on time):</span>
  - <span class="fragment">SQL injection: read data that isn’t yours.</span>
  - <span class="fragment">IDOR: access another user’s record via ID change.</span>
  - <span class="fragment">Stored XSS: inject a script that runs when the page loads.</span>

<span class="fragment">Use the task sheet in `exercises/` for details, hints, and optional solutions.</span>

---

## After the lab: connect to code

- <span class="fragment">Look at the code behind at least one exploit.</span>
- <span class="fragment">Ask: “What one line allowed this to work?”</span>
- <span class="fragment">Write one sentence: “To fix this, I would …”</span>

<span class="fragment">This sets up Module 4 (secure coding and fixes).</span>

---

## Coding patterns behind the vulns

- <span class="fragment">Injection → string concatenation into SQL/commands/templates.</span>
- <span class="fragment">IDOR → using IDs from the client with no ownership/role check.</span>
- <span class="fragment">XSS → rendering untrusted input as HTML/JS.</span>
- <span class="fragment">Auth/data issues → weak hashing, noisy errors, logs/ APIs that leak too much.</span>

<span class="fragment">Different symptoms, **same handful of coding mistakes**.</span>

---

## Exercise (short): One-line fix

- <span class="fragment">For each exploit you did, write one sentence:</span>
  - <span class="fragment">“To fix this, I would …” (e.g. parameterised query, ownership check, output encoding).</span>
- <span class="fragment">Share a couple with the group.</span>

<span class="fragment">We’ll turn these into concrete patterns in Module 4.</span>

---

## What we covered

1. <span class="fragment">**OWASP Top 10 overview** with a focus set (Injection, Broken Access Control, XSS, Auth, Data Exposure).</span>
2. <span class="fragment">**Hands‑on exploitation** in a safe lab environment.</span>
3. <span class="fragment">**Coding flaws** that lead to these vulnerabilities.</span>

---

## Key takeaways (Module 3)

1. <span class="fragment">Exploiting vulnerabilities yourself makes them **stick**.</span>
2. <span class="fragment">A small set of coding patterns (concatenation, missing checks, unsafe output) cause most issues.</span>
3. <span class="fragment">You can now recognise Injection, IDOR, and XSS when you see them in code and behaviour.</span>
4. <span class="fragment">Next: we’ll fix these patterns in Module 4 with secure coding practices.</span>

---

## Bridge to Module 4

- <span class="fragment">You’ve seen what “broken” looks like and how easy it is to exploit.</span>
- <span class="fragment">Next: Module 4 — **Input validation and secure coding** (parameterised queries, output encoding, auth/authorisation).</span>
- <span class="fragment">We’ll fix the same vulnerabilities you just exploited in TrustyTickets or example code.</span>

---

## Questions?

*Common vulnerabilities — Module 3*


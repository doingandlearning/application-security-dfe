# Security Testing and Vulnerability Assessment

**Module 6 — Security testing**

---

## The problem we're solving

We've built defences—how do we check they work?

- <span class="fragment">❌ What tools do I use to test my app?</span>
- <span class="fragment">❌ What's the difference between SAST and DAST?</span>
- <span class="fragment">❌ When is pen testing appropriate for developers?</span>

**Today:** OWASP ZAP, Burp Suite, SAST/DAST, and pen-testing fundamentals.

---

## Why this matters

- <span class="fragment">You’ve now **exploited** vulnerabilities (Module 3) and **fixed** them (Modules 4–5).</span>
- <span class="fragment">Next step: use tools to find issues at scale and **keep them from coming back**.</span>
- <span class="fragment">Security testing turns “we think we’re secure” into “we checked.”</span>

---

## DAST tools: OWASP ZAP and Burp Suite

- <span class="fragment">Both send requests to a **running app** and analyse responses.</span>
- <span class="fragment">They look for patterns: injection, XSS, insecure headers, auth issues, etc.</span>
- <span class="fragment">ZAP is free/open source; Burp is widely used in industry.</span>

Rule of thumb: **ZAP** for automation/CI; **Burp** for interactive/manual testing.

---

## ZAP / Burp in one slide

| Tool | Strengths | Typical use |
|------|-----------|-------------|
| ZAP  | Free, scriptable, headless, CI-friendly | Baseline scans, quick coverage |
| Burp | Proxy, Repeater, Intruder, scanner | Manual testing, complex auth/flows |

---

## SAST vs DAST vs SCA (high-level)

- <span class="fragment">**SAST** — Static Application Security Testing (code/binary analysis).</span>
- <span class="fragment">**DAST** — Dynamic testing of a running app (ZAP/Burp).</span>
- <span class="fragment">**SCA** — Software Composition Analysis (dependency CVEs).</span>

No single tool finds everything. You want **at least SCA + one of SAST/DAST**, ideally all three.

---

## What each tool sees

| Question                    | SAST | DAST | SCA |
|----------------------------|:----:|:----:|:---:|
| Sees vulnerable code paths |  ✅  |  ⚪  | ⚪  |
| Needs a running app        |  ⚪  |  ✅  | ⚪  |
| Finds misconfig at runtime |  ⚪  |  ✅  | ⚪  |
| Finds dependency CVEs      |  ⚪  |  ⚪  | ✅  |


---

## ZAP demo: baseline scan against TrustyTickets

- <span class="fragment">Start TrustyTickets (`dotnet run`).</span>
- <span class="fragment">Point ZAP at the app URL </span>
- <span class="fragment">Run an automated or baseline scan.</span>
- <span class="fragment">Look at Alerts: group by risk level; open one finding.</span>

Key message: “Scanners find **candidates**. We still need to reproduce and triage.”

---

## Burp demo (optional)

- <span class="fragment">Configure browser to proxy via Burp.</span>
- <span class="fragment">Log in and click around the app.</span>
- <span class="fragment">Send an interesting request to **Repeater**, tweak params, resend.</span>

Tie it back to Module 3:

- “This is how we manually test for IDOR and injection when automation can’t guess the business logic.”

---

## SAST: what it looks like to a developer

- <span class="fragment">Finds patterns in code: concatenated SQL, unsafe APIs, tainted input.</span>
- <span class="fragment">Usually integrated into IDE or CI (e.g. PR comments).</span>

Example narrative:

- “Tool flags this line as potential SQL injection.”
- “We check: can user input reach this SQL? If yes, fix as in Module 4.”

---

## SCA: dependency scanning

- <span class="fragment">Scans packages (NuGet, npm, etc.) for **known CVEs**.</span>
- <span class="fragment">Examples: Dependabot, Snyk, `npm audit`.</span>
- <span class="fragment">Great first win: easy to add, high signal.</span>

Bridge to Module 7:

- “Even if you don’t run SAST/DAST yet, SCA is low-hanging fruit in CI.”

---

## Pen test fundamentals (for developers)

- <span class="fragment">Authorised, scoped, time-boxed testing of your app.</span>
- <span class="fragment">Output: report with findings, severity, and reproduction steps.</span>
- <span class="fragment">Your role: understand, reproduce, fix, and prevent regressions.</span>

Link to Module 3:

- “Pentesters do the same things we did: inject, change IDs, test authz, but at scale and with more tools.”

---

## Reading a pen test finding (example)

Structure to show:

- **Title**: “IDOR on /api/tickets/{id}”
- **Risk**: High
- **Description**: “User A can view User B’s ticket by changing `id`.”
- **Steps to reproduce**: request details.

---

Then:

- Reproduce in browser/Burp.
- Connect the fix back to Module 4 (ownership check).

---

## Triage and false positives

- <span class="fragment">Tools will produce **noise** (false positives, informational alerts).</span>
- <span class="fragment">If everything is “critical”, nothing is.</span>

---

Core process:

1. **Reproduce**: follow the scanner’s request/response.
2. **Decide**: true positive or false positive?
3. **Act**:
   - True positive → fix or accept with justification.
   - False positive → document + suppress in tooling.
4. **Baseline**: fail builds only on *new* critical/high findings.

---

## Exercise ideas (pick 2–3)

- <span class="fragment">**ZAP baseline scan**: run against TrustyTickets; pick one finding to reproduce and map to a fix.</span>
- <span class="fragment">**Tool matrix**: fill in what SAST/DAST/SCA can and cannot see.</span>
- <span class="fragment">**Reproduce a pen test finding**: given a short description, replay and fix.</span>
- <span class="fragment">**Triage three findings**: decide true/false positive and next action.</span>

---

## What we covered

1. <span class="fragment">ZAP and Burp as DAST tools for running apps.</span>
2. <span class="fragment">SAST, DAST, and SCA: different lenses on security.</span>
3. <span class="fragment">Pen test fundamentals and how devs engage with reports.</span>
4. <span class="fragment">Triage, false positives, and baselines.</span>

---

## Key takeaways (Module 6)

1. <span class="fragment">Security tools are **helpers**, not oracles—reproduce and triage.</span>
2. <span class="fragment">Combine SAST, DAST, and SCA for better coverage.</span>
3. <span class="fragment">Make scans **repeatable** (CI, baselines) so you see regressions, not just noise.</span>

---

## Bridge to Module 7

- <span class="fragment">You now know **what to run** (ZAP, SAST, SCA) and how to interpret results.</span>
- <span class="fragment">Next: Module 7 — **Implementing Secure DevOps**: wiring these into the pipeline, plus container/cloud checks and incident response.</span>

---

## Questions?

*Security testing and vulnerability assessment — Module 6*

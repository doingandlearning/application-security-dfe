# Module 2 — Exercises

This module has three short, discussion‑friendly exercises. You can run 1–2 of them depending on time.

- **Lab 2.1 — Security in our pipeline** (map current pipeline and add one security check).
- **Lab 2.2 — Threat sketch** (diagram + “what could go wrong?” + prioritisation).
- **Lab 2.3 — First security gate** (pick one automated check and define when it runs).

Standalone lab descriptions follow, using the general lab template style.

---

## Lab 2.1: Security in Our Pipeline

```markdown
# Lab 2.1: Security in Our Pipeline

## Objective
In this lab, you'll **map your current delivery pipeline** and identify **one concrete place** to add or improve a security check. You'll practice thinking about **where security fits** into your existing workflow instead of treating it as a separate phase.

You will:
1. Sketch your current or typical CI/CD pipeline.
2. Mark existing security‑relevant steps (if any).
3. Choose one realistic security check to add.
4. Decide when it should run and what it should do (warn vs. block).

---

## Scenario: Your Team’s Pipeline

You're working on a service that ships regularly. Commits go to a shared repo; changes are built, tested, and deployed via some mix of manual and automated steps.

The pipeline might include:
- Developer machines, local builds, and tests.
- A shared repository with pull requests.
- One or more build/test jobs.
- Deployments to test/staging/production.

You want to make security checks **part of this flow**, not an add‑on at the end.

---

## Task 1: Sketch the Current Pipeline

**Your task:**

- In small groups (or individually, then share), draw your current or a typical pipeline:
  - Developer → commit → pull request → build → test → deploy (test/prod).
  - Include any manual sign‑offs or approvals.
- Mark where code or artefacts move between stages (e.g. “build artefact pushed to registry”).

Hints:
- Keep it simple—5–8 boxes is enough.
- If your team doesn’t use CI yet, sketch what *should* happen when you go from “code on laptop” to “running in production”.

---

## Task 2: Mark Existing Security Steps

**Your task:**

- On your diagram, mark any existing steps that relate to security, for example:
  - Code review checklists.
  - Dependency scanning (Dependabot, Snyk, npm audit, etc.).
  - Static analysis or linters with security rules.
  - Manual pen test or “security review” step.
- Add a short note for each: “what does this do?”.

Hints:
- Be honest—if nothing is automated yet, that’s useful information.
- Include informal practices (“Alice always checks auth in PRs”) if they matter.

---

## Task 3: Add One New Security Check

**Your task:**

- Choose **one** check you could realistically add in the next 1–2 sprints, such as:
  - Dependency scan (e.g. npm audit, Dependabot PRs).
  - Secret scan (e.g. git‑secrets, truffleHog, built‑in provider).
  - A small SAST rule set focused on 1–2 patterns (e.g. SQL injection sinks).
- Decide:
  - **Where** in the pipeline it should run (commit, PR, nightly, pre‑prod).
  - **What** it should do (warn vs. fail build / block merge).

Write this as a one‑sentence policy, e.g.:
- “Run npm audit on every PR; fail the build on high/critical vulns, warn on medium.”
- “Run secret scan on every push; block merge if a key is detected.”

---

## Example Output

```text
- A simple pipeline diagram with 5–8 steps.
- Existing security steps marked with short notes.
- One new security check described in a single sentence (where, what, and severity).
```

---

## Key Concepts Demonstrated

- **Security in every phase**: thinking about where checks belong in the existing flow.
- **Pragmatic shift left**: starting with one realistic, high‑value check.
- **Policy clarity**: defining when a check warns vs. when it blocks.

---

## Next Steps

You’ll revisit this pipeline when we add concrete tools in Modules 6 and 7 (ZAP, SAST/DAST, container and cloud checks).
```

---

## Lab 2.2: Threat Sketch

```markdown
# Lab 2.2: Threat Sketch

## Objective
In this lab, you'll **draw a simple data‑flow diagram** and list **3 concrete threats** against it. You'll practice connecting architecture to “what could go wrong?” and deciding which threat you would fix first.

You will:
1. Draw a simple system diagram (e.g. Browser → API → DB).
2. Mark untrusted inputs and sensitive data.
3. List 3 threats in one sentence each.
4. Vote on which threat you’d address first and why.

---

## Scenario: Simple Web/API/DB System

Consider a typical web application:
- A user’s browser talks to a web or SPA frontend.
- The frontend calls a backend API.
- The API stores and reads data from a database.
- Optionally, the API calls an external service (email, payment, third‑party API).

This maps closely to many real systems (including TrustyTickets).

---

## Task 1: Draw the Data Flow

**Your task:**

- Draw a diagram with 4–6 boxes, for example:
  - User Browser
  - Web/App Frontend
  - Backend API
  - Database
  - External Service (optional)
- Add arrows to show data flow between them.
- Mark where input is **untrusted** (user, partner, internet) and where **sensitive data** is stored or processed.

Hints:
- Don’t overcomplicate—boxes and arrows are enough.
- “Untrusted” includes your own frontend; don’t assume it’s safe because you built it.

---

## Task 2: List Three Threats

**Your task:**

- For the diagram you just drew, list **three threats**, one sentence each.
- Use the pattern: “An attacker could [do X] by [leveraging Y], which would [impact Z].”
- Example categories (don’t feel limited by them):
  - Injection (SQLi, XSS, command injection).
  - Broken access control / IDOR.
  - Auth/session issues.
  - Sensitive data exposure.

Hints:
- Focus on realistic threats for your system, not just textbook examples.
- It’s fine if multiple groups pick similar threats; the discussion will compare context.

---

## Task 3: Prioritise by Risk

**Your task:**

- For your three threats, quickly rate **likelihood** and **impact** as High/Medium/Low.
- Pick **one** threat you would address first and be ready to say why.
- If working in a larger group, vote (hands/chat) on which threat is the top priority.

Hints:
- Don’t overthink the scoring; the goal is the habit of comparing, not a perfect matrix.
- Ask: “If this happened tomorrow, how bad would it be?” and “How likely is it given our controls?”.

---

## Example Output

```text
- A simple Browser → API → DB diagram with untrusted inputs and sensitive data marked.
- Three one‑sentence threats tied to that diagram.
- A quick High/Medium/Low view of risk and one “fix first” choice.
```

---

## Key Concepts Demonstrated

- **Threat modelling** as “diagram + what could go wrong?”.
- **Risk thinking** using quick, qualitative likelihood/impact.
- **Prioritisation**: picking one threat to fix first based on context.

---

## Next Steps

You’ll use this exact mental model when you look at real vulnerabilities in Module 3 and when you design fixes and tests in later modules.
```

---

## Lab 2.3: First Security Gate

```markdown
# Lab 2.3: First Security Gate

## Objective
In this lab, you'll choose **one automated security check** to add to your pipeline and define **when it runs** and **how strict** it should be. You’ll practice thinking about automation as part of CI/CD, not just ad‑hoc tools.

You will:
1. Pick a single check (dependency scan, secret scan, or SAST rule set).
2. Decide when it should run (on commit/PR/nightly).
3. Decide what severity should **block** vs. just **warn**.
4. Capture this as a short, clear policy sentence.

---

## Scenario: Adding One Gate

Your team wants to start automating security in CI/CD but can’t add everything at once. You agree to start with **one gate** that provides real value without overwhelming the team.

Candidates include:
- Dependency vulnerability scanning.
- Secret detection.
- A focused SAST ruleset (e.g. SQL injection, unsafe deserialisation).

---

## Task 1: Choose Your Check

**Your task:**

- As a group, choose **one** of:
  - Dependency scanning.
  - Secret scanning.
  - A small SAST or lint ruleset.
- Note any tools you already have available (e.g. Dependabot, Snyk, GitHub Advanced Security, git‑secrets).

Hints:
- Start where you have the most pain today (e.g. frequent vulnerable libraries) or where tooling is easiest to enable.

---

## Task 2: Define When It Runs

**Your task:**

- Decide when this check should run, for example:
  - On every push or pull request.
  - Nightly or on a schedule.
  - Before deploying to production.
- Consider trade‑offs:
  - How often do you change dependencies or secrets?
  - How long does the check take?

Write this down as part of your policy, e.g. “Run on every PR” or “Run nightly on default branch”.

---

## Task 3: Set Severity and Action

**Your task:**

- Decide what happens at different severities:
  - Which severities **block** the build or merge? (e.g. critical/high).
  - Which severities just create **warnings**? (e.g. medium/low).
- Capture this in a single sentence, for example:
  - “Block merges on critical/high; warn only on medium/low.”
  - “Block if a secret is detected anywhere; all findings must be resolved or explicitly accepted.”

---

## Example Output

```text
- One chosen check (dependency, secret, or SAST).
- A written decision on when it runs (PR / nightly / pre‑prod).
- A one‑sentence policy defining which severities block vs. warn.
```

---

## Key Concepts Demonstrated

- **Automation**: integrating a security check into CI/CD instead of ad‑hoc runs.
- **Signal vs. noise**: tuning severity thresholds to keep trust in the tool.
- **Policy thinking**: making explicit what “fails the build” vs. “just warns”.

---

## Next Steps

Later modules (6–7) will give you concrete examples of these checks (ZAP, SAST/DAST, container and cloud security) so you can refine or expand your first gate.
```


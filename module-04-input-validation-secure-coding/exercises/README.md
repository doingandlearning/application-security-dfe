# Lab 4.1: Fix the Vulnerabilities (TrustyTickets)

## Objective
In this lab, you'll **fix real vulnerabilities** in TrustyTickets using secure coding patterns: parameterised queries/ORM for SQLi, safe rendering for XSS, validation for robustness, and server‑side authz checks for IDOR.

You will:
1. Fix SQL injection in ticket search and reports.
2. Fix stored XSS in comments and description rendering.
3. Add validation to at least one endpoint (type/length/format).
4. Fix IDOR by enforcing ownership/role checks.
5. Re-run the Module 3 payloads to prove the fixes work.

---

## Scenario: You inherited a vulnerable app

TrustyTickets is intentionally vulnerable. Your job is to make it safe enough that:
- Injection payloads no longer change query behaviour.
- XSS payloads render as harmless text (or are sanitised appropriately).
- Users cannot access other users’ tickets/attachments by changing IDs.

---

## Prereqs

- TrustyTickets runs locally: `cd TrustyTickets && dotnet run`
- You can reproduce at least one exploit from Module 3 (SQLi/IDOR/XSS) before you start fixing.

---

## Core tasks (must-do)

These match the patterns demonstrated in Module 4 demos, but you’ll apply them broadly and prove they hold.

### Core Task 1: Fix SQL injection (search + reports)

**Your task:**

- Locate the vulnerable SQL concatenation:
  - Ticket search (`GET /api/tickets?search=`) and/or reports (`GET /api/reports/assignees?q=`).
- Replace raw SQL concatenation with:
  - EF Core query APIs, or
  - parameterised SQL.

**Verify:**

- The Module 3 SQLi payload no longer returns extra rows and does not break the query.

Hints:
- Search for `FromSqlRaw` or string interpolation in SQL.
- Prefer an ORM query over raw SQL for this lab.

---

### Core Task 2: Fix stored XSS (comments + description)

**Your task:**

- Find where comments and/or ticket description are rendered with `innerHTML`.
- Change to safe rendering:
  - `textContent`, safe DOM construction, or framework auto-escaping.
- If you want to allow limited formatting, use a strict sanitiser allowlist (optional).

**Verify:**

- A payload like `<img src=x onerror=alert(1)>` no longer executes when viewing the ticket.

---

### Core Task 3: Add validation (one endpoint)

**Your task:**

- Pick one input surface (e.g. ticket title, search string, assign request).
- Add basic validation:
  - Type checks
  - Length limits
  - Format constraints where relevant
- Return 400 with a clear error on invalid input.

**Discuss:**

- Validation improves UX and stability but is not a replacement for parameterisation/encoding.

---

### Core Task 4: Fix IDOR (ticket-by-id + attachment download)

**Your task:**

- Add an ownership/role check for:
  - `GET /api/tickets/{id}`
  - `GET /api/attachments/{id}/download`
- Ensure unauthorised access returns 403/404 (depending on your policy).

**Verify:**

- As Bob, you cannot access Alice’s ticket or attachment by changing IDs.

---

## Constraints (to make it non-trivial)

These are here specifically to prevent the lab from becoming “copy the demo”.

1. **Don’t remove functionality**
   - Search must still work (no “disable search” fixes).
   - Ticket description/comments must still display text sensibly.

2. **Fix it in two places**
   - SQLi: fix both the tickets search and the reports search.
   - XSS: fix both the comment surface and the description surface.
   - IDOR: fix both ticket-by-id and attachment download.

3. **Decide and document a policy**
   - For IDOR, choose **403 vs 404** for unauthorized access and apply it consistently.
   - Write a one-line rationale in your notes: “We chose X because …”.

---

## Proof (deliverable)

Produce one of the following:

### Option A: Scripted “before/after” checks (recommended)

Write a short checklist or script that a teammate can run to verify:

- SQLi payload no longer expands results.
- IDOR no longer returns another user’s ticket/attachment.
- XSS payload is rendered as text and does not execute.

### Option B: A minimal automated test

Add one small automated check (unit/integration) for at least one fix, e.g.:

- “returns 403/404 when a user requests a ticket they don’t own”
- “XSS payload is encoded/escaped in output”

---

## Stretch goals (if you finish early)

Pick 1–2:

1. **Dynamic sorting allowlist**
   - If you have any “sort by field” behaviour, implement an allowlist of safe sort fields.

2. **Better error handling**
   - Replace 500s caused by bad input with clean 400s (where appropriate).

3. **Consistency pass**
   - Find one additional endpoint that takes an `id` and ensure it enforces authz consistently.

4. **Security notes**
   - Add a short “regression prevention” note: “How would we stop this returning?” (SAST rule, code review checklist, CI gate).

## Example Output

```text
- PR/patch showing:
  - Parameterised/ORM queries replacing concatenated SQL.
  - Safe rendering replacing innerHTML usage.
  - Validation added to at least one endpoint.
  - Ownership checks added for ticket and attachment access.
- Either: a scripted before/after verification checklist, or one minimal automated test.
- A one-line decision note for your IDOR policy (403 vs 404) and why.
```

---

## Key Concepts Demonstrated

- **Parameterisation/ORM** prevents injection.
- **Safe rendering/encoding** prevents XSS.
- **Validation** improves robustness and error handling.
- **Server-side authz** prevents IDOR.

---

## Next Steps

In Module 5 and beyond, you’ll harden sessions/tokens and start adding tooling (SAST/DAST) to catch regressions automatically.

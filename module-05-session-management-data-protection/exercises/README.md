# Lab 5.1: Choose 2–3 Security Improvements

## Objective
Pick **2–3 small, high‑leverage improvements** around sessions, data, and APIs. For each chosen area, you’ll:

1. Inspect the current behaviour (in code and/or browser).
2. Decide on at least one concrete improvement.
3. Capture the change and a short “why this matters” note.

---

## Option A — Harden the session cookie (hands-on)

### Your task

- In your app (or in TrustyTickets as a reference), check:
  - Session/auth cookie name(s).
  - Flags: `HttpOnly`, `Secure`, `SameSite`, `Max-Age`/expiry.
- Decide and document:
  - Which flags should be set for **production**.
  - Whether you need different settings for local dev vs prod.

If you’re using TrustyTickets:

- File: `TrustyTickets/Controllers/AuthController.cs`
- Endpoint: `POST /api/auth/login` (sets the `Session` cookie).

### Deliverable

- A short note (1–2 sentences) on:
  - Your desired cookie flags.
  - Any trade-offs (e.g. SSO flows that need `SameSite=None`).

---

## Option B — JWT/token hygiene checklist (conceptual)

### Your task

- Pick one real system you work on that uses JWT or other tokens (or use a provided example).
- For that system:
  - How long do access tokens live?
  - What claims are in the payload (any PII/secrets)?
  - Where are tokens stored on the client (cookie/localStorage/mobile secure storage)?
  - How is the signature/expiry verified server-side?
  - How do you revoke tokens?

### Deliverable

- A 4–5 point checklist for **your context**, e.g.:
  - “Access tokens: 15 min; refresh: 7 days with rotation.”
  - “Payload: only user id + role; no email/PII.”
  - “Verify signature + `exp` on every request.”
  - “Store in httpOnly cookie for browser; secure storage on mobile.”
  - “Revocation via short expiry and rotating refresh tokens.”

---

## Option C — Log/PII audit (quick review)

### Your task

- Pick **one** logging or error-handling path in your code (or a sample snippet).
- Identify:
  - What gets logged on success and on error.
  - Whether any of it might be:
    - Passwords/tokens/session ids.
    - PII (email, names, addresses, phone numbers, customer ids).
- Decide:
  - One thing you will **never log** (e.g. “We never log passwords or full tokens.”).
  - One improvement (e.g. “Mask emails”, “Log user id only, not email”, “Strip query strings from error logs”).

### Deliverable

- A one-line **“never log X”** rule and one concrete change you’d make.

---

## Option D — API response review (data exposure & abuse)

### Your task

- Pick **one** API response from your system (or from TrustyTickets).
- For that response:
  - List the fields returned.
  - Mark which are required for the UI/business need.
  - Mark which are internal / sensitive / “nice to have”.
- Decide:
  - At least one field to remove or restrict (or justify keeping everything).
  - One potential abuse scenario (e.g. enumeration, brute force) and how rate limiting or paging might help.

If you’re using TrustyTickets:

- Endpoint: `GET /api/tickets/{id}`
- Files:
  - `TrustyTickets/Controllers/TicketsController.cs`
  - `TrustyTickets/Services/TicketService.cs`

### Deliverable

- A short “before vs after” summary:
  - “Before: Ticket response included X, Y, Z.”
  - “After: we plan to return only A, B, C to this client; X/Y will be restricted or moved.”

---

## Wrap-up (group discussion)

In small groups or plenary:

- Share **which 2–3 options** you chose.
- For each:
  - What was the **most surprising** thing you found (e.g. a field in a response, something in logs, a long token lifetime)?
  - What is **one change** you could realistically make in your team in the next sprint?

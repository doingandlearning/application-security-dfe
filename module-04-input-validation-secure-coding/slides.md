# Input Validation and Secure Coding Practices

**Module 4 — Input validation and secure coding**

---

## The problem we're solving

We've seen how injection and XSS work—how do we prevent them?

- <span class="fragment">❌ How do I validate and sanitise input safely?</span>
- <span class="fragment">❌ How do I implement auth and authz correctly?</span>
- <span class="fragment">❌ What patterns should I use (and avoid)?</span>

**Today:** Fix the vulnerabilities you just exploited using secure coding patterns.

---

## Why this matters

- <span class="fragment">Module 3 made the risks real; this module turns that into **habits and patterns**.</span>
- <span class="fragment">Most vulns come from a small set of mistakes: concatenation, unsafe rendering, missing authz checks.</span>
- <span class="fragment">The goal is “secure by default” code and reviews that don’t reintroduce the same bugs.</span>

---

## What we’ll fix (from Module 3)

- <span class="fragment">**SQL injection** → stop concatenating untrusted input into SQL.</span>
- <span class="fragment">**Stored XSS** → stop rendering untrusted input as HTML/JS.</span>
- <span class="fragment">**IDOR / broken access control** → enforce ownership/role checks server-side.</span>
- <span class="fragment">**Validation** → reject invalid inputs early (but don’t confuse validation with “security”).</span>

---

## Preventing SQL injection (SQLi)

- <span class="fragment">Rule: user input must be treated as **data**, never as **code**.</span>
- <span class="fragment">Use ORM query APIs or parameterised queries.</span>
- <span class="fragment">Avoid raw SQL; if you must use it, use parameters only.</span>

---

## SQLi: unsafe vs safe patterns

```csharp
// BAD: concatenation
var sql = "SELECT * FROM Tickets WHERE Title LIKE '%" + search + "%'";
```

```csharp
// GOOD: ORM query API
var tickets = await db.Tickets
  .Where(t => t.OwnerId == userId && t.Title.Contains(search))
  .ToListAsync();
```

```csharp
// GOOD: Raw ADO.net/Dapper
using var conn = new SqlConnection(connString);
using var cmd = new SqlCommand(
    "SELECT * FROM Tickets WHERE OwnerId = @ownerId AND Title LIKE @search",
    conn);

cmd.Parameters.AddWithValue("@ownerId", ownerId);
cmd.Parameters.AddWithValue("@search", "%" + search + "%");

using var reader = await cmd.ExecuteReaderAsync();
```

---

## SQLi isn’t solved by “sanitising”

- <span class="fragment">Regex/blocklists are brittle and bypassable.</span>
- <span class="fragment">Escaping is easy to get wrong and depends on context.</span>
- <span class="fragment">Validation improves UX/stability, but **parameterisation prevents injection**.</span>

---

## Preventing XSS: the core idea

- <span class="fragment">XSS happens when untrusted data is interpreted as code (HTML/JS).</span>
- <span class="fragment">Default to safe rendering (`textContent`, auto-escaping templates).</span>
- <span class="fragment">Avoid dangerous sinks (`innerHTML`) for untrusted input.</span>
- <span class="fragment">If you truly need HTML, use strict sanitisation + still encode where appropriate.</span>

---

## XSS: unsafe vs safe patterns

```javascript
// BAD
div.innerHTML = comment.body;
```

```javascript
// GOOD
div.textContent = comment.body;
```

---

## Encoding depends on context

- <span class="fragment">HTML body vs HTML attribute vs URL vs JavaScript string are different contexts.</span>
- <span class="fragment">Use framework/library encoding for the *context you’re outputting to*.</span>
- <span class="fragment">Rule of thumb: **escape on output**, not “clean on input”.</span>

---

## Validation vs sanitisation (don’t mix them up)

- <span class="fragment">**Validation**: “is this allowed?” (type, length, format, business rules).</span>
- <span class="fragment">**Sanitisation/encoding**: “make it safe for this use” (output context).</span>
- <span class="fragment">Validation helps stability; encoding/parameterisation prevents injection.</span>

---

## Authentication vs authorisation

- <span class="fragment">**Authentication** = who is this? (login, session, token).</span>
- <span class="fragment">**Authorisation** = are they allowed? (roles, ownership, permissions).</span>
- <span class="fragment">Most real breaches are authz failures (IDOR).</span>

---

## Fixing IDOR: the pattern

```csharp
// BAD: no ownership check
var ticket = await db.Tickets.FindAsync(id);
return Ok(ticket);
```

```csharp
// GOOD: enforce ownership (or role)
if (ticket.OwnerId != currentUserId && !currentUser.IsAdmin) return Forbid();
```

---

## Where should authz live?

- <span class="fragment">Ideally in a shared layer (service/policy) so it’s hard to forget.</span>
- <span class="fragment">Controller-level checks are easy to miss on new endpoints.</span>
- <span class="fragment">Tests help: “returns 403 when user doesn’t own the resource”.</span>

---

## Lab: Fix TrustyTickets (secure coding)

We’ll fix the same issues we exploited:

- <span class="fragment">**Fix SQLi** in ticket search and reports endpoints.</span>
- <span class="fragment">**Fix XSS** in comments and ticket description rendering.</span>
- <span class="fragment">**Add validation** (length/type/format) on at least one endpoint.</span>
- <span class="fragment">**Fix IDOR** on ticket-by-id and attachment download.</span>

<span class="fragment">Then rerun the Module 3 payloads to prove the fixes.</span>

---

## What we covered

1. <span class="fragment">SQL injection prevention (parameters/ORM, no concatenation).</span>
2. <span class="fragment">XSS prevention (safe rendering + encoding by context).</span>
3. <span class="fragment">Validation as correctness (not as a substitute for security).</span>
4. <span class="fragment">Auth/authz patterns (ownership checks to stop IDOR).</span>

---

## Key takeaways (Module 4)

1. <span class="fragment">Injection is prevented by parameterisation/ORM—not by regex “sanitising”.</span>
2. <span class="fragment">XSS is prevented by safe rendering and context-aware encoding.</span>
3. <span class="fragment">Validation improves robustness; auth/authz enforce who can do what.</span>
4. <span class="fragment">Fix the app, then rerun the exploits to prove it.</span>

---

## Bridge to Module 5

- <span class="fragment">We fixed coding patterns in your app and API.</span>
- <span class="fragment">Next: Module 5 — sessions, cookies, tokens/JWT, data protection, and API hardening.</span>

---

## Questions?

*Input validation and secure coding — Module 4*

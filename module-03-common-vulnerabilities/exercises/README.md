
# Lab 3.1: Exploit Common Vulnerabilities

## Objective
In this lab, you'll **exploit common web vulnerabilities** in a deliberately vulnerable application (DVWA, WebGoat, or TrustyTickets). You'll practice SQL injection, broken access control (IDOR), and stored XSS, and connect each exploit to the coding pattern that enabled it.

You will:
1. Perform a basic SQL injection to read data you shouldn’t see.
2. Exploit an IDOR / broken access control issue to access another user’s resource.
3. Trigger a stored XSS payload that runs in a victim’s browser.
4. Identify the underlying coding mistake for each vulnerability.

---

## Scenario: Deliberately Vulnerable App

You have access to a lab environment running one or more of:
- **DVWA** (Damn Vulnerable Web Application).
- **WebGoat**.
- **TrustyTickets** (intentionally vulnerable sample app).

The app is:
- Running locally or in a safe lab network.
- Populated with sample data and test users (e.g. `alice`, `bob`).
- Explicitly configured for training/exploitation.

---

## Task 1: SQL Injection (SQLi)

### Goal

Use SQL injection to retrieve data you **should not** be able to see as a normal user.

### Your task:

- Navigate to a page or feature that allows searching or filtering, for example:
  - A “search tickets” or “search products” box.
  - A login form in DVWA/WebGoat (if that’s the recommended exercise).
- Start with a **simple test** payload, e.g.:
  - `'` or `"`.
  - `test'` or `test"`.
- Observe whether:
  - You see SQL error messages.
  - The number of results changes unexpectedly.
- Use hints/task text provided by the instructor or app to build a payload that:
  - Returns additional rows (e.g. tickets for other users), or
  - Bypasses a WHERE clause.

### Hints:

- Try adding `' OR 1=1 --` or `' OR 'a'='a` in a text field that ends up in a query.
- Look for parameters in the URL or form that seem to map directly to a query.
- If using TrustyTickets, start with the known vulnerable routes (see “TrustyTickets route targets” below).

<details>
<summary>Possible Solution (Conceptual)</summary>

```text
- Input: ' OR 1=1 --
- Result: the app returns many more rows than expected, including items you shouldn’t see.
- Conclusion: the input is being concatenated directly into a SQL query.
```

</details>

---

## Task 2: Broken Access Control / IDOR

### Goal

Access another user’s resource by manipulating an identifier in the request.

### Your task:

- Find a feature that fetches or updates a single item by ID, for example:
  - `GET /tickets/123`
  - `GET /api/ticket?id=123`
  - `GET /profile?userId=123`
- While logged in as one user (e.g. `alice`), change the ID:
  - `123 → 124`, or another value you suspect belongs to a different user.
- Observe whether you can:
  - See another user’s data (ticket, profile, etc.).
  - Perform actions on their behalf (e.g. update/delete).

### Hints:

- Look at the URL bar: any numeric or GUID‑looking parameters?
- Use the browser dev tools or a proxy (Burp/ZAP) to intercept and edit requests.
- In many labs, IDs are sequential; try `id+1` or `id-1`.

<details>
<summary>Possible Solution (Conceptual)</summary>

```text
- Request: GET /api/ticket?id=101  (Alice’s ticket)
- Modified: GET /api/ticket?id=102 (Bob’s ticket)
- Result: as Alice, you see Bob’s ticket details.
- Conclusion: the server does not check that the current user owns the ticket (IDOR).
```

</details>

---

## Task 3: Stored XSS

### Goal

Inject a script into the application so that **another visit** to the page causes the script to run.

### Your task:

- Find a place where you can submit content that is later displayed, for example:
  - Comments.
  - Messages.
  - Profile “bio” fields.
- Submit a harmless test value (e.g. `TEST123`) and see where it appears.
- Replace your input with a simple XSS payload.

```html
<script>alert('XSS');</script>
```

- Reload or view the page as another user if the lab supports multiple accounts.
- Confirm whether the script runs (e.g. an alert box appears).

### Hints:

- If script tags are blocked, try payloads using event handlers (e.g. `<img src=x onerror=alert(1)>`) depending on lab guidance.
- Look for any field that is rendered without escaping (you may see HTML tags rendered as HTML, not text).

In TrustyTickets specifically, a reliable stored‑XSS test is:

```html
<img src=x onerror=alert(1)>
```

<details>
<summary>Possible Solution (Conceptual)</summary>

```text
- Input: <script>alert('XSS');</script> as a comment.
- Result: every user who views the comments sees a popup.
- Conclusion: user input is being rendered as HTML/JS without encoding or sanitisation.
```

</details>

---

## Task 4: Link Each Exploit to Code

### Goal

Identify the **coding pattern** behind each vulnerability.

### Your task:

- For each exploit (SQLi, IDOR, XSS), answer:
  - “What code pattern made this possible?”
  - “What one change would have stopped me?”
- Examples:
  - “SQLi: string concatenation of input into SQL; should use parameterised query/ORM.”
  - “IDOR: resource fetched by ID without ownership check; should compare owner to current user.”
  - “XSS: unescaped output with `innerHTML`; should use encoding or safe rendering.”

Write one sentence per vuln; you’ll use these in Module 4.

---

## Example Output

```text
- Screenshots or notes confirming:
  - SQL injection returning unexpected data.
  - IDOR showing another user’s data.
  - Stored XSS payload executing.
- A short note per vuln:
  - The endpoint/feature exploited.
  - The input/payload used.
  - The one-line coding pattern that enabled it.
```

---

## Key Concepts Demonstrated

- **Injection**: untrusted input in queries/commands/templates.
- **Broken access control / IDOR**: missing ownership/role checks.
- **Stored XSS**: untrusted input rendered as executable HTML/JS.
- **Exploit ↔ code link**: every attack maps back to a specific coding pattern.

---

## Next Steps

Module 4 will show how to **fix** these vulnerabilities with parameterised queries, output encoding, proper auth/authorisation, and validation. Keep your notes; they will help when mapping from exploit to fix.
```

---

## TrustyTickets route targets (recommended)

If you’re using **TrustyTickets**, the demos you just saw map cleanly to the lab tasks. After each successful exploit, challenge yourself to find **one more route** with the same weakness.

### SQL injection targets

- **Teaching**: `GET /api/tickets?search=...` (use the search box on `tickets.html`)
- **Lab**: `GET /api/reports/assignees?q=...` (use `reports.html`)

Suggested payloads to try (start simple, then try a bypass):

- `'`
- `%' ) OR 1=1 -- `

### IDOR / broken access control targets

- **Teaching**: `GET /api/tickets/{id}` (UI: `ticket.html?id=...`)
- **Lab**: `GET /api/attachments/{id}/download` (from attachments list on ticket page)

### Stored XSS targets

- **Teaching**: comments rendering in `ticket.html` (comment box)
- **Lab**: ticket description rendering in `ticket.html` (create a ticket with HTML in description)

Recommended safe demo payload:

```html
<img src=x onerror=alert(1)>
```

### “Explore another route” challenge

After you complete each of the 3 core tasks, pick one:

- Find another input that reaches a query (SQLi-style)
- Find another “get/download by id” endpoint (IDOR-style)
- Find another place untrusted content is rendered (XSS-style)

Write down:

- The route / UI feature
- The payload or change you made
- The result

---

## Lab 3.2: One‑Line Fix Summary

```markdown
# Lab 3.2: One‑Line Fix Summary

## Objective
In this short follow‑up lab, you'll summarise **how to fix** each vulnerability you exploited in Lab 3.1 in a single, clear sentence. This creates a direct bridge from “I broke it” to “here’s how we prevent it,” ready for Module 4.

You will:
1. For each exploit, write a one‑line fix.
2. Share and compare fixes with the group.
3. Group fixes into common secure‑coding patterns.

---

## Scenario: Right After Exploitation

You’ve just finished Lab 3.1, where you exploited:
- A SQL injection.
- An IDOR / broken access control issue.
- A stored XSS (if time).

Now you want to capture **what good looks like** before moving on.

---

## Task 1: Write One‑Line Fixes

**Your task:**

- For each exploited vuln, write exactly **one sentence** describing the fix, e.g.:
  - “Use a parameterised query/ORM instead of concatenating user input into SQL.”
  - “Check that the ticket’s owner matches the current user (or admin) before returning it.”
  - “Encode user input before rendering and avoid using innerHTML with untrusted data.”

Use the pattern: “To fix this, we would …”.

---

## Task 2: Share and Group

**Your task:**

- Share your one‑line fixes in chat or on a whiteboard.
- As a group, cluster them into themes:
  - Parameterised queries / avoiding concatenation.
  - Ownership/authorisation checks.
  - Output encoding / safe rendering.
  - Stronger auth/session handling.

Optional:
- Highlight any duplicates to show how often the same few patterns appear.

---

## Example Output

```text
- 3–6 one-line fixes, one per exploit.
- A grouped list of fix patterns (SQLi, IDOR, XSS, etc.).
```

---

## Key Concepts Demonstrated

- **Exploit-to-fix mapping**: every attack has a corresponding defensive pattern.
- **Pattern recognition**: many vulnerabilities share the same underlying fix.
- **Preparation for Module 4**: you already have a list of fixes before seeing detailed code.

---

## Next Steps

Module 4 will turn these one‑line summaries into concrete secure‑coding examples and exercises (parameterised queries, encoding, validation, and auth/authorisation patterns).

rp

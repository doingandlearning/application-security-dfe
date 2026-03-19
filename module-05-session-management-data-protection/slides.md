# Session Management and Data Protection

**Module 5 — Session management and data protection**

---

## The problem we're solving

Sessions and data are high-value targets. How do we get them right?

- <span class="fragment">❌ How do I secure cookies, tokens, and JWT?</span>
- <span class="fragment">❌ How do I encrypt and store data safely?</span>
- <span class="fragment">❌ How do I build and protect APIs?</span>

**Today:** Session security, encryption and storage, and secure API design.

---

## Why this matters

- <span class="fragment">After Module 4 (secure coding), we must secure **how identity is maintained** (sessions/tokens) and **how data is handled**.</span>
- <span class="fragment">Weak session handling → hijacking/impersonation.</span>
- <span class="fragment">Weak data protection → exposure in transit, at rest, or through APIs and logs.</span>

---

## Session security: cookie-based sessions (overview)

- <span class="fragment">Browser holds a cookie; server uses it to identify the user/session.</span>
- <span class="fragment">Security depends on cookie flags + server-side session handling.</span>

Rule of thumb: assume **XSS and CSRF exist** somewhere; design sessions to reduce blast radius.

---

## Cookie flags you should know

| Flag | What it does | Why it matters |
|------|--------------|----------------|
| `HttpOnly` | JS can’t read the cookie | reduces session theft via XSS |
| `Secure` | cookie only sent over HTTPS | prevents interception over HTTP |
| `SameSite` | limits cross-site sending | mitigates CSRF in many cases |

---

## SameSite: practical guidance

- <span class="fragment">`Lax` is often a good default for app sessions.</span>
- <span class="fragment">`Strict` can break legitimate flows; use carefully.</span>
- <span class="fragment">`None` requires `Secure` and often needs CSRF/state protections for cross-site flows.</span>

---

## Session fixation and lifecycle

- <span class="fragment">Regenerate session id after login (prevents fixation).</span>
- <span class="fragment">Short idle timeouts for sensitive apps; clear logout behaviour.</span>
- <span class="fragment">Decide how you revoke sessions (server-side store vs stateless tokens).</span>

---

## Cookies vs tokens 

| Topic | Cookie  | Token / JWT |
|------|-----------------|------------|
| State | server-side | often stateless |
| Revocation | easier (server can invalidate) | harder (needs expiry/blacklist) |
| Browser storage | cookie (can be HttpOnly) | often `localStorage` or cookie |
| Common risks | CSRF (mitigate with SameSite + anti-forgery) | XSS token theft if stored in JS-readable storage |

---

## JWT: what it is (and isn’t)

- <span class="fragment">JWT is **signed**, not encrypted by default.</span>
- <span class="fragment">Anyone who has the token can **base64-decode the payload**.</span>
- <span class="fragment">Security relies on: signature verification + expiry + safe storage.</span>

---

## JWT best practices (practical checklist)

- <span class="fragment">Short expiry for access tokens (minutes to hours).</span>
- <span class="fragment">Refresh tokens: rotate and revoke; treat as high-value secrets.</span>
- <span class="fragment">Payload: identifiers only (no PII, no secrets).</span>
- <span class="fragment">Always verify signature + expiry server-side.</span>

---

## Token storage: the core trade-off

- <span class="fragment">`localStorage` / JS memory: easy, but **XSS = token theft**.</span>
- <span class="fragment">HttpOnly cookie: harder for JS to steal, but needs CSRF mitigations.</span>
- <span class="fragment">Mobile: use platform secure storage (Keychain/Keystore).</span>

---

## Data protection: in transit (TLS)

- <span class="fragment">HTTPS everywhere (no auth endpoints over HTTP).</span>
- <span class="fragment">Consider HSTS to enforce HTTPS in browsers.</span>
- <span class="fragment">Treat internal traffic as hostile unless you control the network boundary.</span>

---

## Data protection: at rest (encryption + keys)

- <span class="fragment">Encrypt sensitive data at rest (DB/volume/field where needed).</span>
- <span class="fragment">Key management matters more than the algorithm.</span>
- <span class="fragment">Keys belong in a vault/managed service, not in source code.</span>

---

## Logs and errors: avoid self-inflicted leaks

- <span class="fragment">Never log passwords, tokens, session ids, or secrets.</span>
- <span class="fragment">Be careful with PII (emails, addresses, identifiers).</span>
- <span class="fragment">Client errors should be generic; detailed errors belong in server logs.</span>

---

## Secure API design: treat APIs as a trust boundary

- <span class="fragment">Authenticate every request that touches sensitive data/state.</span>
- <span class="fragment">Authorize per resource/action (ownership/role checks).</span>
- <span class="fragment">Return minimal fields (DTOs; avoid “entire object graph”).</span>
- <span class="fragment">Handle abuse: rate limiting + payload size limits.</span>

---

## API abuse patterns to mention

- <span class="fragment">Enumeration (IDOR + guessing IDs).</span>
- <span class="fragment">Brute force (login, password reset endpoints).</span>
- <span class="fragment">Mass assignment / over-posting.</span>
- <span class="fragment">Overly verbose errors exposing internals.</span>

---

## Exercise ideas (pick 2–3)

- <span class="fragment">**Harden session cookie**: HttpOnly/Secure/SameSite; verify in devtools.</span>
- <span class="fragment">**JWT checklist**: expiry, payload, storage, verification.</span>
- <span class="fragment">**Log audit**: identify sensitive data; define “we never log X”.</span>
- <span class="fragment">**API response review**: remove/restrict one field; discuss rate limiting.</span>

---

## What we covered

1. <span class="fragment">Cookie sessions: flags + lifecycle (fixation, timeouts).</span>
2. <span class="fragment">Tokens/JWT: expiry, payload hygiene, verification, storage.</span>
3. <span class="fragment">Data protection: TLS, encryption at rest, key handling, safe logging.</span>
4. <span class="fragment">API hardening: auth/authz, minimal responses, abuse protection.</span>

---

## Key takeaways (Module 5)

1. <span class="fragment">Session/token security is mostly about **storage, transport, and lifecycle**.</span>
2. <span class="fragment">Encrypt in transit and at rest; protect keys; don’t leak secrets/PII in logs.</span>
3. <span class="fragment">APIs need auth, authz, minimal responses, and abuse controls.</span>

---

## Bridge to Module 6

- <span class="fragment">Now we have patterns and checklists for secure sessions + data handling.</span>
- <span class="fragment">Next: Module 6 — ZAP/Burp, SAST/DAST: how to **find what we missed** and automate checks.</span>

---

## Questions?

*Session management and data protection — Module 5*

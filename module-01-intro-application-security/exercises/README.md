# Module 1 — Exercises

This module has two variants of the same exercise:

- **`live_exercise`** — For in‑person delivery with small groups.
- **`remote_exercise`** — For remote delivery over Teams without breakout rooms.

Both share the same learning goals; only the facilitation format differs.

---

## live_exercise

Use this version when you have participants in the room and can put them into small groups around a flipchart or whiteboard.

```markdown
# Lab 1: Map Your Attack Surface (In‑Person)

## Objective
In this lab, you'll **map the attack surface and trust boundaries** of a simple (or real) system. You'll practice identifying **untrusted inputs**, **trust boundaries**, and **sensitive assets** – the mental model you’ll reuse throughout the course.

You will:
1. Identify external actors and entry points into your system.
2. Mark trust boundaries where untrusted data crosses into trusted components.
3. Highlight the most sensitive assets (data, functionality).
4. Brainstorm “what could go wrong?” at 2–3 key points.
5. Share one improvement you’d make based on your map.

---

## Scenario: Simple Ticketing System (or Your App)

You're part of a team building a **web‑based ticketing system** called *TrustyTickets* (or use your own app if you prefer). The system needs to:
- Let users sign up, log in, and create tickets.
- Allow users to view and comment on their own tickets.
- Expose a simple API that the mobile app and partners can call.
- Store tickets and comments in a database.

This is similar to most CRUD‑style business apps: some UI (web/mobile), some APIs, a database, and integrations.

---

## Task 1: Draw the High‑Level Architecture

Start by sketching the big pieces and how they talk to each other.

**Your task:**

- In small groups, draw a simple diagram with:
  - External users (browser, mobile app, partner system).
  - Frontend (web app / mobile app).
  - Backend/API.
  - Database and any external services (email, payment, logging, third‑party APIs).
- Draw arrows showing how data flows between them.
- Don’t worry about perfect notation – boxes and arrows are fine.

**Hints:**

- Keep it to 5–8 boxes; this is a conversation tool, not an enterprise diagram.
- Think “what talks to what, and over which channel?” (browser → HTTPS → API, API → DB, etc.).
- If you’re using your real system, focus on one slice (e.g. “user logs in and creates a ticket”).

<details>
<summary>Possible Solution for Task 1</summary>

```text
[User Browser] --HTTPS--> [Web App] --HTTPS/API--> [Backend Service] --SQL--> [Database]

[Mobile App] --HTTPS/API--> [Backend Service]

[Partner System] --HTTPS/API (Partner Key)--> [Backend Service]

[Backend Service] --SMTP/HTTP--> [Email Service / Third‑party API]
```

</details>

---

## Task 2: Mark Attack Surface and Trust Boundaries

Now add security‑relevant detail: where untrusted data enters and where it crosses into trusted components.

**Your task:**

- On your diagram, **highlight attack surface** by marking:
  - Inputs (forms, APIs, file uploads, headers, cookies, mobile app calls, partner calls).
  - Any admin or elevated‑privilege interfaces.
- Draw or label at least **two trust boundaries**, for example:
  - Internet ↔ your frontend / edge.
  - Frontend ↔ backend/API.
  - Backend ↔ database or third‑party services.
- For each trust boundary, add a short note: “what do we assume is untrusted here?”

**Hints:**

- Attack surface = anywhere an attacker can send data or influence behaviour.
- Trust boundaries = where you cross from “we don’t control this” to “we trust this more.”
- Think about APIs as much as UIs: partner or mobile calls are just as interesting as browser forms.

<details>
<summary>Possible Solution for Task 2</summary>

```text
Attack surface:
- Login, signup, ticket creation and comment forms (browser).
- Mobile app API calls (JSON bodies, headers, tokens).
- Partner API endpoint for ticket creation.

Trust boundaries:
- Internet → Web App / API (untrusted users, networks).
- Web App / Mobile App → Backend Service (don’t trust client validation or identity claims alone).
- Backend Service → Database (validate queries, enforce authz before hitting DB).
```

</details>

---

## Task 3: “What Could Go Wrong?” at 2–3 Points

Turn your map into a threat brainstorm without yet doing any exploitation.

**Your task:**

- Pick **2–3 attack surface points** from your diagram (e.g. ticket creation form, comment API, partner API).
- For each, in one or two bullet points, answer:
  - “If I were an attacker, what would I try here?”
  - “What bad thing could happen if this input is not validated or access‑controlled?”
- Circle or star the **one** you think is the highest‑risk in your context.

**Hints:**

- Think in families: injection, broken access control, auth/session flaws, data exposure.
- Don’t worry about technical exploit details yet – that’s what Modules 3 and 4 are for.
- Example question: “Could I see someone else’s ticket if I change an ID in the URL/API body?”

<details>
<summary>Possible Solution for Task 3</summary>

```text
Examples:
- Ticket creation form: try SQL injection in the title/description to read other tickets.
- Comment box: try an XSS payload so other users’ browsers run my script.
- Partner API: try calling it without a key or with a guessed key to create tickets as another tenant.

Highest‑risk (example):
- Partner API, because it’s internet‑facing, machine‑to‑machine, and might bypass normal auth checks.
```

</details>

---

## Example Output

```text
- A simple architecture diagram with users, frontend, backend, DB, and one external service.
- Attack surface marked (forms, APIs, partner calls).
- At least two trust boundaries labelled with what’s untrusted on each side.
- A short list (6–9 bullets) of “what could go wrong?” ideas, with one circled as highest risk.
```

---

## Key Concepts Demonstrated

- **Attack surface**: every point where an attacker can send data or influence the system.
- **Trust boundaries**: where data crosses from untrusted to trusted components, and where you should validate and enforce policy.
- **Threat brainstorming**: turning a system sketch into concrete “what could go wrong?” scenarios.
- **Prioritisation mindset**: picking the highest‑risk area instead of treating all inputs as equal.

---

## Next Steps

You’ll reuse this map and mindset in Module 2 (secure SDLC and threat modelling) and Module 3 (hands‑on exploitation with DVWA/WebGoat and TrustyTickets). Keep your diagram – you can refine it as you learn more.
```

---

## remote_exercise

Use this version when running the course remotely over Teams **without breakout rooms**. Participants work individually first, then build a shared view in plenary using chat and a shared whiteboard.

```markdown
# Lab 1 (Remote): Map Your Attack Surface (Teams, No Breakouts)

## Objective
In this remote lab, you'll **map the attack surface and trust boundaries** of a simple (or real) system. You’ll do a short individual sketch, then build a shared view in plenary using a whiteboard and chat.

You will:
1. Sketch your system and identify external actors and entry points.
2. Mark trust boundaries where untrusted data crosses into trusted components.
3. Contribute attack surface and “what could go wrong?” ideas to a shared board.
4. Discuss which points feel highest‑risk in your real systems.

---

## Scenario: Simple Ticketing System (or Your App)

Same as the in‑person exercise:
- Use the *TrustyTickets* mental model (web app + API + DB), **or**
- Use your own real app if that makes the discussion richer.

---

## Step 1: Individual Sketch (3–5 minutes)

**Your task:**

- On paper or in a simple drawing tool, sketch:
  - External users (browser, mobile app, partner system).
  - Frontend (web app / mobile app).
  - Backend/API.
  - Database and one external service (email, payment, logging, third‑party API).
- Draw arrows showing how data flows between them.

You don’t need to share yet – this is just to organise your own thinking.

---

## Step 2: Shared Board – Attack Surface and Trust Boundaries (5–10 minutes)

The instructor will open a Teams Whiteboard / Miro / Excalidraw with:
- A simple skeleton diagram (User → Web/App → API → DB).
- Two areas/columns: **Attack surface** and **What could go wrong?**

**Your task:**

- Add 1–2 sticky notes under **Attack surface** for places where an attacker could send data or influence behaviour (forms, APIs, file uploads, headers, cookies, partner calls, etc.).
- Add 1–2 sticky notes under **What could go wrong?** describing:
  - What an attacker might try.
  - What bad thing could happen if input isn’t validated or access‑controlled.

Hints:
- Think about both browser and API calls.
- Keep each note to a single idea so it’s easy to discuss.

---

## Step 3: Plenary Debrief and Prioritisation (10–15 minutes)

**Your task:**

- In chat, answer: **“Which point feels highest‑risk in *your* world?”**  
  e.g. type `login`, `comments`, `partner API`, or `other + short note`.
- Follow the discussion as the instructor:
  - Groups sticky notes into themes (injection, broken access control, auth/session, data exposure).
  - Connects those themes to later modules (Module 3 exploitation, Module 4 fixes).

Optional (time‑permitting):
- 1–2 volunteers briefly share their individual sketch via screen share to compare with the shared board.

---

## Example Output

```text
- Individual sketches (not collected) for each participant.
- A shared whiteboard with:
  - Attack surface notes (forms, APIs, partner calls, etc.).
  - “What could go wrong?” notes grouped by theme.
- A quick chat poll on which point feels highest‑risk in participants’ real systems.
```

---

## Key Concepts Reinforced

- **Attack surface and trust boundaries** applied to remote participants’ real systems.
- **Collaborative threat brainstorming** without needing breakout rooms.
- **Prioritisation**: focusing on the riskiest points to carry into later modules.
```

# Lab 1: Map Your Attack Surface

## Objective
In this lab, you'll **map the attack surface and trust boundaries** of a simple (or real) system. You'll practice identifying **untrusted inputs**, **trust boundaries**, and **sensitive assets** – the mental model you’ll reuse throughout the course.

You will:
1. Identify external actors and entry points into your system.
2. Mark trust boundaries where untrusted data crosses into trusted components.
3. Highlight the most sensitive assets (data, functionality).
4. Brainstorm “what could go wrong?” at 2–3 key points.
5. Share one improvement you’d make based on your map.

---

## Scenario: Simple Ticketing System (or Your App)

You're part of a team building a **web‑based ticketing system** called *TrustyTickets* (or use your own app if you prefers). The system needs to:
- Let users sign up, log in, and create tickets.
- Allow users to view and comment on their own tickets.
- Expose a simple API that the mobile app and partners can call.
- Store tickets and comments in a database.

This is similar to most CRUD‑style business apps: some UI (web/mobile), some APIs, a database, and integrations.

---

## Task 1: Draw the High‑Level Architecture

Start by sketching the big pieces and how they talk to each other.

**Your task:**

- In small groups, draw a simple diagram with:
  - External users (browser, mobile app, partner system).
  - Frontend (web app / mobile app).
  - Backend/API.
  - Database and any external services (email, payment, logging, third‑party APIs).
- Draw arrows showing how data flows between them.
- Don’t worry about perfect notation – boxes and arrows are fine.

**Hints:**

- Keep it to 5–8 boxes; this is a conversation tool, not an enterprise diagram.
- Think “what talks to what, and over which channel?” (browser → HTTPS → API, API → DB, etc.).
- If you’re using your real system, focus on one slice (e.g. “user logs in and creates a ticket”).

<details>
<summary>Possible Solution for Task 1</summary>

```text
[User Browser] --HTTPS--> [Web App] --HTTPS/API--> [Backend Service] --SQL--> [Database]

[Mobile App] --HTTPS/API--> [Backend Service]

[Partner System] --HTTPS/API (Partner Key)--> [Backend Service]

[Backend Service] --SMTP/HTTP--> [Email Service / Third‑party API]
```

</details>

---

## Task 2: Mark Attack Surface and Trust Boundaries

Now add security‑relevant detail: where untrusted data enters and where it crosses into trusted components.

**Your task:**

- On your diagram, **highlight attack surface** by marking:
  - Inputs (forms, APIs, file uploads, headers, cookies, mobile app calls, partner calls).
  - Any admin or elevated‑privilege interfaces.
- Draw or label at least **two trust boundaries**, for example:
  - Internet ↔ your frontend / edge.
  - Frontend ↔ backend/API.
  - Backend ↔ database or third‑party services.
- For each trust boundary, add a short note: “what do we assume is untrusted here?”

**Hints:**

- Attack surface = anywhere an attacker can send data or influence behaviour.
- Trust boundaries = where you cross from “we don’t control this” to “we trust this more.”
- Think about APIs as much as UIs: partner or mobile calls are just as interesting as browser forms.

<details>
<summary>Possible Solution for Task 2</summary>

```text
Attack surface:
- Login, signup, ticket creation and comment forms (browser).
- Mobile app API calls (JSON bodies, headers, tokens).
- Partner API endpoint for ticket creation.

Trust boundaries:
- Internet → Web App / API (untrusted users, networks).
- Web App / Mobile App → Backend Service (don’t trust client validation or identity claims alone).
- Backend Service → Database (validate queries, enforce authz before hitting DB).
```

</details>

---

## Task 3: “What Could Go Wrong?” at 2–3 Points

Turn your map into a threat brainstorm without yet doing any exploitation.

**Your task:**

- Pick **2–3 attack surface points** from your diagram (e.g. ticket creation form, comment API, partner API).
- For each, in one or two bullet points, answer:
  - “If I were an attacker, what would I try here?”
  - “What bad thing could happen if this input is not validated or access‑controlled?”
- Circle or star the **one** you think is the highest‑risk in your context.

**Hints:**

- Think in families: injection, broken access control, auth/session flaws, data exposure.
- Don’t worry about technical exploit details yet – that’s what Modules 3 and 4 are for.
- Example question: “Could I see someone else’s ticket if I change an ID in the URL/API body?”

<details>
<summary>Possible Solution for Task 3</summary>

```text
Examples:
- Ticket creation form: try SQL injection in the title/description to read other tickets.
- Comment box: try an XSS payload so other users’ browsers run my script.
- Partner API: try calling it without a key or with a guessed key to create tickets as another tenant.

Highest‑risk (example):
- Partner API, because it’s internet‑facing, machine‑to‑machine, and might bypass normal auth checks.
```

</details>

---

## Example Output

```text
- A simple architecture diagram with users, frontend, backend, DB, and one external service.
- Attack surface marked (forms, APIs, partner calls).
- At least two trust boundaries labelled with what’s untrusted on each side.
- A short list (6–9 bullets) of “what could go wrong?” ideas, with one circled as highest risk.
```

---

## Key Concepts Demonstrated

- **Attack surface**: every point where an attacker can send data or influence the system.
- **Trust boundaries**: where data crosses from untrusted to trusted components, and where you should validate and enforce policy.
- **Threat brainstorming**: turning a system sketch into concrete “what could go wrong?” scenarios.
- **Prioritisation mindset**: picking the highest‑risk area instead of treating all inputs as equal.

---

## Next Steps

You’ll reuse this map and mindset in Module 2 (secure SDLC and threat modelling) and Module 3 (hands‑on exploitation with DVWA/WebGoat and TrustyTickets). Keep your diagram – you can refine it as you learn more.
```

---


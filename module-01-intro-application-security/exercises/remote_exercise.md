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


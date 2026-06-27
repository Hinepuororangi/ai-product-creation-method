# AI Product Creation Method
## A Repeatable Framework for Building AI-Powered Products in a Day

*Developed through the build of Parikino OS — June 2026.*
*Updated with Four Fibres OS learnings — June 2026.*

---

## What This Is

A step-by-step method for taking any product idea from concept to working, deployed application — using AI as your architect, developer, and advisor throughout.

This is not a coding tutorial. It is a **product creation operating system.**

Anyone with an idea, a phone, and access to Claude can follow this method. You do not need to know how to code.

---

## The Core Principle

**Architecture before code. Always.**

Most people start building too early. They jump into code before they understand what they're building, who it's for, and how the data fits together. This creates expensive rework.

This method forces the right thinking first. The code comes last — and when it does, it builds quickly because the foundation is solid.

---

## The Six Phases

```
Phase 1 — ASSESS      Is this worth building?
Phase 2 — ARCHITECT   What exactly are we building?
Phase 3 — SPECIFY     What does the data look like?
Phase 4 — BUILD       Write the code
Phase 5 — DEPLOY      Get it live
Phase 6 — ITERATE     Add features without breaking things
```

---

## Phase 1 — ASSESS

### The AI Boardroom

Before writing a single line of code, convene the AI Boardroom.

**How to use it:**

Open a new Claude chat. Paste this prompt:

> "You are an AI Boardroom made up of world-class experts including a Product Strategist, Systems Architect, UX Designer, Software Engineer, Data & AI Specialist, Finance & Commercial Advisor, Marketing & Growth Advisor, Operations & Process Advisor, Legal & Privacy Advisor, and Cultural Advisor. Assess this idea as if we are deciding whether to invest time and money into building it. Do not simply agree. Challenge weak assumptions. Identify blind spots. Keep the discussion practical."

Then describe your idea in plain language.

**What the Boardroom produces:**

1. Problem restatement — what are we actually solving?
2. Is this worth solving — would people genuinely use it?
3. Product strategy — what is the simplest MVP?
4. Systems design — what are the major modules?
5. User experience — what should the first screen show?
6. Technical review — recommended stack and risks
7. Commercial review — can this generate income?
8. Risk assessment — what could cause failure?
9. Recommendation — build now, prototype first, or stop?
10. Action plan — next ten practical steps

**The test:** If the Boardroom cannot clearly articulate the problem and who experiences it, the idea is not ready. Do not proceed to Phase 2.

**Boardroom tip:** Go through all steps and record them. These become the Build Record — a document that captures every decision and why it was made. Keep it alongside the Build Brief.

**Parikino example:** The board identified that the real problem was not lack of tools — it was fragmented knowledge and no shared source of truth. That reframe shaped everything that followed.

**Four Fibres example:** Initial brief was a static requirements tracker. Boardroom identified the real need was a two-sided request workflow — Darren submits, the right person actions it, both parties see live status. Completely different product, same data.

---

## Phase 2 — ARCHITECT

### The Product Architecture Review

Once the Boardroom says build, produce a full architecture before any code.

**Prompt Claude:**

> "You are a senior Product Architect, Systems Designer, UX Designer, Solution Architect, and Technical Lead. Review this product brief. Challenge assumptions. Identify missing pieces. Recommend improvements. The goal is to produce a robust product specification before any development begins."

**What to define in this phase:**

**Modules** — What are the major sections of the product? Group them into domains. Remove anything that belongs in Phase 2 of the roadmap, not the MVP.

**User types** — Who uses this? What does each person need to see and do? Define 2-4 user types maximum for MVP.

**Permissions** — Who can see what? Who can create, edit, delete each type of record? Define this before building — retrofitting permissions is expensive.

**Routing logic** — If different users see different content (e.g. finance requests go to finance team), define the routing rules here. This becomes a hardcoded map in the app.

**Success definition** — What does a successful interaction look like for each user type? Write it out as a short story. This becomes your UX brief.

**The test:** Can you describe every screen in the app without opening a code editor? If yes, proceed to Phase 3.

**Parikino example:** 23 modules in the brief. Architecture reduced to 5 core domains and 4 MVP modules.

**Four Fibres example:** Two clear user journeys — Darren submits a typed request, picks a category, adds detail. Admin/finance/systems sees it in their queue and taps once to action it. Requirements tracker carried from v1 as a second tab.

---

## Phase 3 — SPECIFY

### The Data Model and Build Brief

This is the most important phase. Get this right and the build is fast. Get this wrong and you rebuild.

**Part A — Data Model**

List every object (entity) in your product. For each one define:
- What fields does it have?
- What is its lifecycle (draft → active → archived)?
- What other objects does it relate to?
- Who owns it?
- Who can see it?

**Prompt Claude:**

> "Based on this product architecture, produce a complete entity-relationship diagram. Identify missing entities, unnecessary entities, relationships, ownership, permissions, and lifecycle for each object."

**Shared Supabase project tip:** If you're adding a new app to an existing Supabase project, prefix all table names with your app identifier (e.g. `ff_` for Four Fibres). This prevents conflicts with existing tables and keeps the schema clean.

**Part B — The Build Brief**

The Build Brief is a single document that contains everything Claude needs to build the product in any future session without re-explaining context.

A good Build Brief contains:

```
1. What the product is (2-3 sentences)
2. Live URL and repo location
3. Tech stack (every library, every CDN, every pinned version)
4. Database tables and their purpose
5. Current version and what's built
6. What's not built yet (and why)
7. Known constraints and gotchas
8. Hardcoded values that need to become dynamic
9. Next session priorities in order
10. How to start the next session (exact prompt)
```

**The test:** Could a developer who has never seen this product pick up the Build Brief and know exactly what to build next? If yes, proceed to Phase 4.

---

## Phase 4 — BUILD

### The Single-File Prototype

For MVP, build in the simplest possible way that still deploys.

**Recommended MVP stack:**

| Layer | Tool | Pinned Version | Why |
|-------|------|----------------|-----|
| Frontend | React (CDN) | 18.3.1 | No build step, instant deploy |
| Compiler | Babel Standalone (CDN) | 7.24.7 | Compiles JSX in browser |
| Database | Supabase | — | Postgres + auth + storage |
| Hosting | Vercel | — | Free, auto-deploys from GitHub |
| Styling | Inline CSS / CSS variables | — | No dependencies |

**Pin your CDN versions.** Never use `@18` or `@latest`. Always use exact versions like `@18.3.1` and `@7.24.7`. Unpinned versions can change and break your app.

**The single-file approach:**

Put everything — HTML, CSS, JavaScript, React components — in one `index.html` file. This sounds wrong but it is right for MVP because:
- Zero build complexity
- One file to deploy
- One file to hand to Claude for updates
- Works on mobile browsers

---

### Critical Technical Constraints

These are learned the hard way. Follow all of them.

**1. File size limit**
Mobile Chrome crashes Babel standalone above ~100KB of JSX. Keep the file under this. Move large static data to a plain `<script>` tag before the Babel script tag.

**2. Single Babel script only**
Never split into multiple `<script type="text/babel">` tags. Each tag compiles independently and cannot share variables. One Babel script only.

**3. No IIFE in JSX**
Immediately-invoked functions `(()=>{ })()` inside JSX break Babel standalone on mobile. Use proper React components instead.

**4. Always use data-presets="react"**
The Babel script tag must include `data-presets="react"`:
```html
<script type="text/babel" data-presets="react">
```
Without this, JSX may fail to compile on some mobile browsers.

**5. Error boundary always**
Always wrap the app in a React ErrorBoundary. Without it, any JavaScript crash shows a blank white screen with no information. The ErrorBoundary catches the error and shows a message.

**6. Do not use supabase-js on mobile**
The `@supabase/supabase-js` CDN library causes silent crashes on mobile Chrome. Instead, use **raw fetch calls directly to the Supabase REST API**, exactly as Parikino OS does. This is more reliable and gives you complete control.

```javascript
// DO NOT USE:
// <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
// const sb = supabase.createClient(url, key)
// sb.from('table').select()

// USE INSTEAD — raw fetch to Supabase REST API:
async function dbFetch(path, options = {}) {
  const session = getSession();
  const token = (session && session.access_token) || SUPABASE_KEY;
  const res = await fetch(`${SUPABASE_URL}/rest/v1/${path}`, {
    ...options,
    headers: {
      apikey: SUPABASE_KEY,
      Authorization: `Bearer ${token}`,
      "Content-Type": "application/json",
      Accept: "application/json",
      ...(options.method === "POST" || options.method === "PATCH"
        ? { Prefer: "return=representation" } : {}),
      ...options.headers,
    },
  });
  const text = await res.text();
  if (!res.ok) throw new Error(`${res.status}: ${text}`);
  return text ? JSON.parse(text) : [];
}
```

**7. Authentication — use raw fetch, not supabase-js auth**
Sign in via the Supabase Auth REST endpoint directly. Store the session in localStorage.

```javascript
const SESSION_KEY = "myapp_session";

function getSession() {
  try { return JSON.parse(localStorage.getItem(SESSION_KEY)); } catch(e) { return null; }
}
function saveSession(data) { localStorage.setItem(SESSION_KEY, JSON.stringify(data)); }
function clearSession() { localStorage.removeItem(SESSION_KEY); }

async function signIn(email, password) {
  const res = await fetch(`${SUPABASE_URL}/auth/v1/token?grant_type=password`, {
    method: "POST",
    headers: { apikey: SUPABASE_KEY, "Content-Type": "application/json" },
    body: JSON.stringify({ email, password }),
  });
  const data = await res.json();
  if (!res.ok) throw new Error(data.error_description || "Sign in failed");
  saveSession(data);
  return data;
}
```

**8. Use the publishable key, not the JWT anon key**
Supabase now issues a `sb_publishable_...` key. Use this — not the legacy `eyJ...` JWT key. It works with the raw fetch approach above.

**9. GitHub mobile upload**
Never paste `index.html` into GitHub's mobile editor — it strips the `<` from `<!DOCTYPE html>`, breaking the file. Always use **Upload Files**.

**10. Supabase RLS — always enable**
Enable Row Level Security on every table and create explicit policies. Without policies, authenticated users cannot read or write data even if they're logged in.

---

### Build Order

1. Login and authentication first — confirm the session works before anything else
2. Dashboard and navigation shell
3. One complete module (data in, data out, working)
4. Test on a real mobile phone before adding more
5. Add modules one at a time

**The test:** Does it load on a mobile phone and show real data from the database? If yes, proceed to Phase 5.

---

## Phase 5 — DEPLOY

### GitHub + Vercel

**Setup (one time):**
1. Create a **private** GitHub repository per product (never share repos between products)
2. Connect to Vercel — import the GitHub repo, leave all settings as default
3. Vercel auto-deploys every time you push to main
4. Do not add a `vercel.json` — it can interfere with static file serving

**Deploying updates:**
1. Download updated `index.html` from Claude
2. Go to GitHub repo → Add file → Upload files
3. Upload the file — do not use the GitHub in-browser editor (it corrupts DOCTYPE)
4. Commit — Vercel deploys automatically in ~30 seconds

**If the app downloads instead of loading:**
This means Vercel is serving the file with the wrong content-type. Causes include:
- File was saved without `.html` extension (must be exactly `index.html`)
- A `vercel.json` is conflicting with the static file server
- Delete any `vercel.json` and redeploy

**If the app shows a blank white screen:**
This means the HTML is loading but JavaScript is crashing. Check:
- Babel script tag has `data-presets="react"`
- CDN versions are pinned (not `@18` or `@latest`)
- You are not using supabase-js (use raw fetch instead)
- File is under 100KB
- There is an ErrorBoundary wrapping the app

**Supabase free tier limit:**
Supabase allows 2 active projects per account on the free tier. If you hit this limit:
- Option A: Pause a project you don't currently need
- Option B: Share a Supabase project between apps using table name prefixes (e.g. `ff_` for Four Fibres, `pk_` for Parikino)
- Option C: Upgrade to Pro ($25/month)

Option B is recommended for related apps or apps owned by the same organisation.

**Security checklist before going live:**
- [ ] Row Level Security enabled on every Supabase table
- [ ] Explicit RLS policies created for each table (read, insert, update as needed)
- [ ] GitHub repo set to Private
- [ ] Publishable key used (safe to expose with correct RLS)
- [ ] No service_role key in any client-side code — ever

**The test:** Does the app load at its public URL on a phone you haven't used before, with no errors? If yes, the product is live.

---

## Phase 6 — ITERATE

### Adding Features Without Breaking Things

Once the MVP is live, every new feature follows this pattern:

**1. State the feature clearly**
Write one sentence: "I want Darren to be able to attach a file to a payment request."

**2. Check the Build Brief**
Is the database ready? Does a new table need to be created? Are there permissions implications?

**3. Give Claude the context**
Start the session with:
- The Build Brief
- The current `index.html` from GitHub
- The one-sentence feature description

**4. Build in isolation first**
Ask Claude to write the new component or function separately before integrating it. Test the logic before wiring it to the app.

**5. Integrate and test on mobile**
Always test on a real phone, not a browser simulator.

**6. Update the Build Brief**
After every session, update the Build Brief to reflect what was built, what changed, and what's next.

---

## The Session Template

Use this structure for every build session:

**Opening prompt:**
> "This is the Build Brief for [product name]. I need you to act as the lead developer. Read the brief fully. Then I will tell you what I want to build today. Do not start coding until we agree on scope."

**Then upload:** current `index.html` + Build Brief

**Then say:** what you want to build

**Closing ritual:**
Before ending the session, ask Claude to:
1. Update the Build Brief with what was built
2. Note any new constraints discovered
3. Confirm the next session priorities
4. Output the updated `index.html` ready for upload

---

## The Rinse and Repeat Cycle

```
New idea
    ↓
AI Boardroom Assessment (Phase 1)
    ↓
Product Architecture Review (Phase 2)
    ↓
Data Model + Build Brief (Phase 3)
    ↓
Build MVP (Phase 4)
    ↓
Deploy (Phase 5)
    ↓
Iterate with Build Brief (Phase 6)
    ↓
New feature → return to Phase 3
New product → return to Phase 1
```

---

## The Standard Tech Stack

Copy these exact CDN links into every new project:

```html
<script src="https://unpkg.com/react@18.3.1/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@18.3.1/umd/react-dom.production.min.js"></script>
<script src="https://unpkg.com/@babel/standalone@7.24.7/babel.min.js"></script>
```

And always open the Babel script tag with:
```html
<script type="text/babel" data-presets="react">
```

Do not add the supabase-js CDN. Use raw fetch instead.

---

## Scaling the Method

**When to stay in single-file:**
- Under 5 users
- Under ~100KB of JSX
- No server-side secrets needed
- No real-time features

**When to migrate to Next.js:**
- File size approaching 100KB
- Need to call AI APIs securely (API key must be server-side)
- Need real-time updates
- Multiple user types with complex routing
- Going to second customer

**The migration is not a rebuild.** All components, all CSS, all Supabase logic carries across. It is a restructure — same rooms, new building.

---

## What This Method Produces

By the end of a single day following this method you will have:

- A working, deployed web application
- Real data flowing from a production database
- Authentication and security in place
- A Build Brief for fast iteration
- A clear roadmap for the next session

By the end of three sessions you will have:

- A product people are using
- A foundation ready to scale to a second customer
- A Build Brief that lets any future developer (or AI) pick up exactly where you left off

---

## Proven On

**Parikino OS** — Marae operating system for Parikino Marae Reservation Trust
- Built: June 2026
- Time: One day
- Result: Live application with authentication, 8 modules, real trustee data, hui register, skills register, bookings, stocktake
- URL: parikino-os-v2.vercel.app
- Repo: Hinepuororangi/parikino-os (private)
- Next: Minutes upload with AI extraction, Next.js migration

**Four Fibres OS** — Two-sided request platform for Four Fibres working relationship
- Built: June 2026
- Time: One day
- Result: Live application with role-based authentication, request submission and routing, 5 request types, team queue, requirements tracker
- URL: four-fibres-os.vercel.app
- Repo: Hinepuororangi/FOUR-FIBRES (private)
- Database: Shared Parikino Supabase project with `ff_` prefix tables
- Key learning: supabase-js CDN crashes on mobile — use raw fetch to Supabase REST API

---

## Critical Lessons Summary

| # | Lesson | Why it matters |
|---|--------|----------------|
| 1 | Pin CDN versions exactly | Unpinned versions can change and break your app |
| 2 | Add `data-presets="react"` to Babel script | Without it, JSX fails on some mobile browsers |
| 3 | Never use supabase-js on mobile | It causes silent crashes; use raw fetch instead |
| 4 | Use publishable key, not JWT anon key | Works with raw fetch approach |
| 5 | Store session in localStorage | Persistent login without supabase-js auth module |
| 6 | Always use ErrorBoundary | Blank screen = invisible crash without it |
| 7 | Upload files via GitHub Upload Files | Mobile editor strips DOCTYPE |
| 8 | No vercel.json for static HTML | Can cause file to download instead of render |
| 9 | Keep file under 100KB | Mobile Chrome crashes Babel above this |
| 10 | Prefix shared DB tables | Prevents conflicts when sharing a Supabase project |
| 11 | Run full Boardroom even for small features | The real product is often different from the first idea |
| 12 | RLS policies must be explicit | Enabling RLS without policies blocks all queries |

---

*This method was developed through the AI Boardroom process and proven through two production deployments. Every lesson in this document was earned.*

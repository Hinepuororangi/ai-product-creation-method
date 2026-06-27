# AI Product Creation Method
## A Repeatable Framework for Building AI-Powered Products in a Day

*Developed through the build of Parikino OS — June 2026.*
*Updated with Four Fibres OS learnings — June 2026.*
*Updated with App Icon step and Fast & Flawless OS discovery — June 2026.*

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

**Fast & Flawless OS example:** Initial research showed lash education was saturated. Boardroom identified the gap — no one had built a structured system around the four specific things that kill artist confidence: Retention, Speed, Design and Consistency. Completely different positioning.

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
The supabase-js CDN causes silent crashes on mobile Chrome. Use raw fetch to the Supabase REST API instead.

```javascript
// Instead of supabase.from('table').select()
// Use this:
const response = await fetch(
  `${SUPABASE_URL}/rest/v1/table_name?select=*`,
  {
    headers: {
      'apikey': SUPABASE_KEY,
      'Authorization': `Bearer ${SUPABASE_KEY}`
    }
  }
);
const data = await response.json();
```

**7. Use publishable key not JWT anon key**
The publishable key works with raw fetch and RLS. Never use the service_role key in client-side code.

**8. Store session in localStorage**
Without supabase-js auth, store the user session manually in localStorage so login persists across page refreshes.

---

## Phase 5 — DEPLOY

### Getting the App Live

**Standard deploy process:**

1. Go to github.com — open your repo
2. Tap Add file → Upload files
3. Upload `index.html` (do NOT use the mobile editor — it strips DOCTYPE)
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

### App Icon — Make It a PWA

Every deployed app should have an icon so it can be saved to the home screen like a native app. Claude outputs all three files at once. You only convert and upload.

**Step 1 — Describe the icon to Claude**

Tell Claude:
- Background colour
- Text or symbol (e.g. "spell out PARIKINO")
- Accent colour
- Style (e.g. tāniko pattern background, minimal, bold)

**Step 2 — Claude outputs three files at once**

Claude produces:
- `icon.svg` — the master icon file
- `manifest.json` — PWA configuration
- `head-tags.html` — the snippet to paste into your `<head>`

**Step 3 — Convert SVG to PNG**

Go to **svgtopng.com** and export at four sizes:

| Size | File name | Purpose |
|------|-----------|---------|
| 512×512 | icon-512.png | App stores, PWA |
| 192×192 | icon-192.png | Android PWA |
| 180×180 | icon-180.png | Apple touch icon |
| 32×32 | icon-32.png | Favicon |

**Step 4 — Upload to GitHub**

Upload all files to your repo root:
- `icon.svg`
- `icon-512.png`
- `icon-192.png`
- `icon-180.png`
- `icon-32.png`
- `manifest.json`

**Step 5 — Add head tags to index.html**

Paste the head-tags snippet inside your `<head>` in `index.html`:

```html
<!-- PWA Manifest -->
<link rel="manifest" href="/manifest.json">

<!-- Theme colour (browser bar) -->
<meta name="theme-color" content="#YOUR_COLOUR">

<!-- Apple touch icon -->
<link rel="apple-touch-icon" href="/icon-180.png">

<!-- Favicon -->
<link rel="icon" type="image/png" sizes="32x32" href="/icon-32.png">

<!-- Apple PWA settings -->
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="YOUR APP NAME">
```

**Step 6 — Commit and deploy**

Upload the updated `index.html` via GitHub Upload Files. Vercel redeploys in 30 seconds. Open the app on your phone, tap Share → Add to Home Screen.

**The test:** Does your app icon appear when saved to the home screen? If yes, it's a PWA.

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
Deploy + App Icon (Phase 5)
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

**Fast & Flawless OS** — Lash artist confidence and business system
- Discovery: June 2026
- Method used: Full 10-phase AI Product Engineering Methodology
- Vision: Builds confident lash artists by solving the four things that hold them back — Retention, Speed, Design and Consistency
- Target: Beginners and struggling artists wanting to quit
- Key learning: Discovery-first methodology identified a gap no competitor had filled — a named, structured framework around the four confidence killers
- Status: Build Brief approved, development pending

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
| 13 | Claude outputs SVG + manifest.json + head-tags in one go | Saves steps, keeps the process repeatable |
| 14 | Convert SVG at svgtopng.com to 512, 192, 180, 32px | All four sizes needed for full PWA and favicon support |
| 15 | Discovery before architecture | The real product is always different from the first idea — Phase 0 proves it |

---

*This method was developed through the AI Boardroom process and proven through three production deployments. Every lesson in this document was earned.*

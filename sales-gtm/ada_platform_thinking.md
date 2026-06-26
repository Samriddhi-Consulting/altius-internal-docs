# ADA Learning Simulator — Project Thinking Document

*Structured analysis across three personas: Learner, Client Admin, Internal ADA Specialist*

---

## 1. The Learner

### 1A. Free Scenario

**What you've said:** Easy sign up, fair use policy, fast interaction.

**Sign-up flow — keep it ruthlessly simple:**
- **Magic link login** for all users (free and enterprise). Enter email → receive a link → click to enter. Zero passwords to remember. Especially good for L&D where people do a simulation once, then come back weeks later.
- No phone number, no company name, no onboarding survey. Every field you add loses ~15% of sign-ups.
- After sign-up, drop them straight into a scenario picker. No welcome wizard, no tutorial. The simulation itself *is* the onboarding.

> ✅ **DECIDED:** Magic link for all users — free and enterprise. No passwords.

**Fair use — configurable turn limits + feature gating:**

> ✅ **DECIDED:** Turn limits apply to ALL users (free and paid). Configurable per scenario via the ADA specialist dashboard.

**How turn limits work:**
- Each scenario has a `max_turns` field set by the ADA specialist (e.g., 15 turns for a quick discovery call, 25 for a complex negotiation)
- The system counts learner messages (user turns only, not AI responses)
- At `max_turns`, the simulation auto-completes and triggers the coaching evaluation
- This is a pedagogical design choice, not just a cost lever — it forces learners to be efficient and purposeful in their conversations, just like real-world interactions have time pressure

**Free vs. Paid differentiation (feature-gated):**
- Free users get access to 1-2 "showcase" scenarios (your best ones), same turn limits as paid
- Free users get a basic debrief ("You completed the simulation. Here's a summary.")
- **No** detailed coaching evaluation, **no** scoring breakdown, **no** PDF export for free users
- This lets them experience the magic (the conversation feels real) while withholding the professional value (the evaluation)

**Rate limiting for abuse prevention:**
- Max 5 simulations per day per free account (prevents bot abuse)
- Max 100 LLM calls per day per free account (prevents API cost runaway)
- IP-based throttling for unauthenticated requests

---

### 1A-ii. Input Abuse Monitoring (Pre-LLM Gate)

> ✅ **DECIDED:** All user messages are screened before being sent to the LLM.

The system must detect and block content that isn't genuine human-to-human conversation. This runs as middleware *before* the LLM call, adding negligible latency (~5ms) since it's rule-based, not AI-based.

**What to detect:**

| Abuse Type | Detection Method | Response |
|---|---|---|
| **Copy-pasted structured data** (tables, JSON, CSV, markdown tables) | Regex: detect `|---|` table separators, `{"key":`, repeated `\t` tab characters, `<tag>` HTML. Also detect abnormally long single messages (>500 chars with low whitespace ratio) | Reject with: *"It looks like you pasted structured data. Try speaking to [Persona] naturally, as you would in a real conversation."* |
| **Off-topic content** (asking about the weather, telling jokes, testing the AI) | Lightweight keyword/pattern check + a short LLM classifier call (optional, can use a tiny model). Flag messages that have zero semantic overlap with the scenario context | Gentle redirect: *"[Persona] seems confused. Try steering the conversation back to [scenario topic]."* |
| **Prompt injection attempts** ("Ignore your instructions", "You are now...", "System prompt:") | Regex for known injection patterns: `ignore.*instructions`, `you are now`, `system:`, `\[INST\]`, `act as`, `pretend you are` | Hard block: *"This message couldn't be sent."* Log the attempt for review |
| **Gibberish / keyboard mashing** ("asdfghjkl", "aaaaaa") | Entropy check: if the character distribution is extremely uneven or the message has no dictionary words | Reject: *"That doesn't look like a message. Try again."* |
| **Extremely short messages** ("ok", "yes", "hmm" repeatedly) | Count consecutive low-effort messages (< 5 words). Allow 1-2, flag on 3+ | Gentle nudge: *"[Persona] is waiting for a more detailed response. What specifically would you like to discuss?"* |

**Escalation model:**
```
First offense  → Gentle redirect (message not sent, helpful hint shown)
Second offense → Warning ("Your responses are being reviewed")
Third offense  → Session flagged for admin review
Fifth offense  → Session locked ("Please contact your administrator")
```

**Implementation notes:**
- Flag count stored on the session record (`flag_count` integer field)
- Each flagged message is stored with `is_flagged: true` and a `flag_reason` field in the messages table
- Admin dashboard shows a "Flagged Sessions" view so they can review learners who may need coaching on how to use the tool
- The ADA specialist dashboard shows aggregate flag data across clients ("Client X has 12% flagged sessions — may need better onboarding")

---

### 1B. Paid Scenario (Enterprise Learners)

**What you've said:** Users added by admin via dashboard.

**How the learner enters:**
- Admin adds learners by email (or bulk CSV)
- Learner receives an email: *"You've been invited to [Scenario Name] by [Company]. Click to start."*
- First click → magic link auth → land directly in their assigned scenario
- **No scenario picker needed** — enterprise learners are assigned a specific simulation by their admin. Reduce choices, reduce confusion.

**Aspects you should think through:**

#### Simulation Lifecycle
What happens at each stage of the learner journey?

```
Invited → Pre-read → Simulation → Debrief → (Done)
```

> ✅ **DECIDED:** Learners cannot reset their own simulations. Resets are admin/specialist-only actions.

| Stage | What the learner sees | Design considerations |
|---|---|---|
| **Invited** | Email with a CTA button | Should the email preview the scenario topic? ("Practice handling a pricing objection with a fleet customer") — Yes, it sets expectations |
| **Pre-read** | Scenario briefing (static content) | This is your SSG sweet spot. Pre-rendered HTML, loads instantly. Include: context, your role, the persona you'll meet, what success looks like |
| **Simulation** | Chat interface with AI persona | The core experience. Auto-completes at `max_turns`. See performance section below |
| **Debrief** | Coaching feedback, scores, strengths/weaknesses | **This is the product.** The simulation is the hook; the debrief is what L&D pays for |
| **Done** | "Simulation complete" — no retry button | If a retry is needed (e.g., learner was just testing), the admin or ADA specialist resets the session from their dashboard. The learner's previous transcript and scores are archived, not deleted |

#### The "Instantaneous Response" Problem
You've said this is critical. Here's how to architect it:

**The mix of static + LLM components:**

| Component | Static or LLM? | Why |
|---|---|---|
| Pre-read content | **Static** (SSG at build time) | Never changes per user. Serve from CDN. <50ms globally |
| Scenario picker UI | **Static** (SSR) | Server-rendered HTML, minimal JS |
| AI persona's first message | **Pre-generated** (stored in DB) | The opening line ("Hello, I'm Vikram. What brings you in today?") is always the same. Don't waste an LLM call on it |
| AI responses during chat | **LLM (streamed)** | Stream tokens as they arrive. User sees first word in ~300ms. Full response appears over 1-3 seconds, feels like natural typing |
| Typing indicator | **Static animation** | Show a "Vikram is typing..." bubble for 500ms before streaming begins. Feels human |
| Coaching debrief | **LLM (background)** | Trigger the evaluation call the moment the simulation ends. Show a "Generating your coaching report..." animation while it processes. 3-5 seconds is acceptable here because the user expects it |
| Score visualizations | **Static** (client-side rendering of LLM output) | Once the LLM returns scores, render them as charts/progress bars instantly |

**Latency budget per chat turn:**
```
User sends message
  → Network to server:     ~50ms  (Vercel Edge)
  → LLM API call (Groq):   ~200ms (time-to-first-token)
  → First token to user:   ~300ms total
  → Full response streamed: 1-3 seconds
```
Groq is the fastest LLM provider currently available (~200 tokens/sec on Llama models). This is why both existing apps use it.

#### Other Learner Aspects to Consider

**1. Mobile experience**
L&D cohorts in industries like auto, FMCG, and pharma access training on phones. Your chat UI *must* work well on mobile. This means:
- Full-width chat bubbles
- Bottom-anchored input (like WhatsApp/iMessage)
- No horizontal scrolling
- Touch-friendly buttons (min 44px tap targets)

**2. Language and accessibility**
- Will you support non-English simulations? Not now, but your DB schema should store a `locale` field on scenarios from day 1
- Basic accessibility: keyboard navigation, screen reader support on the chat. Don't gold-plate this initially, but don't make it impossible to add later

**3. Session recovery**
- If a learner's browser crashes mid-simulation, they should be able to return and pick up from their last message. This is already solved in the CC codebase (Supabase persistence). Carry it forward.

**4. Learner data privacy**
- Chat transcripts contain learner behavior data. You need a clear data retention policy.
- Enterprise clients *will* ask: "Where is our data stored? Who can see it? How long do you keep it?"
- Design for this early: `org_id` on every table, row-level security, data export/deletion endpoints.

**5. Emotional safety**
- Simulations can be stressful (that's partly the point). Consider adding a subtle "I need help" or "Exit simulation" escape hatch that doesn't feel like failure.
- The AI persona should never be cruel, only challenging. Your system prompts must include guardrails: "Never use profanity. Never make personal attacks. Stay in character but remain professional."

**6. Completion certificate** *(tech debt — not in initial build)*
- After the debrief, offer a simple downloadable certificate: "Praneet completed the BANT Lead Qualification simulation on May 17, 2026." L&D teams love these for compliance tracking.
- **Deferred** to a later phase. Nice-to-have, not a launch blocker.

---

## 2. The Client Admin (L&D Specialist)

### What you've said:
- Not a programmer
- Manage users, reset scenarios, get basic reports
- Don't need to create scenarios (use provided ones)
- Dashboard that starts basic but scales to complex

### The Admin's Core Jobs-to-be-Done

| Job | What they need | Priority |
|---|---|---|
| **Invite learners** | Add by email (single or CSV bulk upload). Assign to a scenario. | P0 — Day 1 |
| **Monitor progress** | See who has started, who has completed, who hasn't logged in yet | P0 — Day 1 |
| **View results** | Client admin: operational session data (status, transcript). ADA specialist/faculty: full debrief with scores on `/internal/reports` | P0 — Day 1 |
| **Reset a learner** | Let someone redo the simulation (e.g., they were just testing) | P0 — Day 1 |
| **Export data** | Download a CSV of all learner scores for reporting to stakeholders | P1 — Week 2 |
| **Cohort analytics** | Aggregate view: "What's the average score? What's the most common weakness?" | P1 — Month 1 |
| **Schedule a cohort** | "Open the simulation on June 1, close on June 15" — time-bound access | P2 — Month 2 |
| **Compare cohorts** | "How did Batch 1 compare to Batch 2?" | P2 — Quarter 2 |
| **Custom branding** | "Can we put our logo on it?" | P3 — When they ask |

### The Dashboard Architecture (Scaling from Basic to Complex)

Your instinct to treat the dashboard as a "separate module" is correct. Here's how to architect it:

```
/admin                → Admin layout (sidebar + content area)
/admin/learners       → User management (invite, list, reset)
/admin/reports        → Client admin: session status + transcript (no scores)
/internal/reports     → ADA specialist / faculty: full debrief + scores (assigned orgs)
/admin/analytics      → Cohort-level dashboards (P1)
/admin/settings       → Org settings, branding, access windows (P2)
```

**Why this scales:**
- Each route is a separate page/component. Adding a new dashboard view = adding a new route.
- The data layer is just SQL queries against the existing tables (sessions, evaluations, users). No new infrastructure needed.
- You can progressively add complexity: start with a table of learner scores, later add charts (Recharts/Chart.js), later add filters and drill-downs.

### Other Admin Aspects to Consider

**1. Multi-scenario assignment**
An admin might have 3 scenarios from ADA. They should be able to:
- See which scenarios are available to their org
- Assign different learners to different scenarios
- Or assign ALL learners to the same scenario

Design the data model as: `org_scenarios` (which scenarios an org has access to) + `learner_assignments` (which scenario a specific learner is assigned to).

**2. Notification management**
- "Send a reminder to all learners who haven't started"
- Initially: a button that triggers a batch email
- Later: automated reminders (e.g., "3 days before deadline, send a nudge")

**3. Admin ≠ Super Admin**
From day 1, separate roles:
- **Admin** (L&D client): Can manage learners, view reports, reset simulations *for their own org only*
- **Super Admin** (ADA internal): Can see all orgs, create scenarios, manage subscriptions

This is just a `role` field on the user record + middleware checks, but it's much easier to add now than retrofit later.

**4. The "Show my boss" problem**
L&D admins will always need to justify the platform to their management. Give them:
- A one-page "Executive Summary" export (PDF): *"42 learners completed the simulation. Average score: 7.2/10. Top strength: Rapport building. Top gap: Quantifying customer impact."*
- This single feature can drive renewals.

**5. Real-time vs. batch reporting**
- Individual learner results → real-time (available the moment the debrief completes)
- Cohort analytics → can be slightly delayed (a background job that aggregates every hour, or on-demand refresh). Don't over-engineer real-time dashboards early.

---

## 3. The Internal ADA Client Servicing Specialist

### What you've said:
- Non-technical
- Creates scenarios and evaluation logic for clients
- Trains clients on using the tools
- Interfaces with the client

### This Person's Workflow (Current vs. Target)

**Today (without a platform):**
```
Client says "We want a negotiation simulation"
  → ADA specialist describes the scenario in a Word doc
  → Developer reads the Word doc
  → Developer writes system prompts in Python
  → Developer writes evaluation rubric in Python
  → Developer deploys
  → Specialist tests
  → "Can you change the persona's tone?" → Developer edits → redeploys
  → Repeat 3-5 times until the specialist is happy
  → Go live
```
**Time: 2-4 weeks. Bottleneck: developer availability.**

**Target (with internal tools):**
```
Client says "We want a negotiation simulation"
  → ADA specialist opens the Scenario Builder
  → Fills in: persona name, backstory, scenario context, key behaviors, evaluation criteria
  → Clicks "Preview" → chats with the AI persona in a sandbox
  → Adjusts tone/behavior → previews again
  → Clicks "Publish to [Client Org]"
  → Go live
```
**Time: 1-3 days. Bottleneck: the specialist's own iteration, not developer availability.**

### How to Make This Person Maximally Productive

**1. Scenario Builder (Internal Tool — Not client-facing)**

This is the most important internal tool. It should be a form-based UI where the specialist fills in structured fields that get assembled into a system prompt.

**Instead of writing raw prompts, the specialist fills in:**

| Field | Example | How it maps to the system prompt |
|---|---|---|
| Persona Name | Vikram Singh | "You are {persona_name}." |
| Persona Role | Fleet Owner, Falcon Freight | "You are the {role} at {company}." |
| Persona Personality | Tough, skeptical, results-oriented, impatient | "Your personality traits are: {traits}. Respond accordingly." |
| Backstory | Owns 50 trucks. Had a bad experience with delayed spare parts last quarter. | "Your backstory: {backstory}. Reference this naturally in conversation." |
| Hidden Concerns | Worried about switching costs. Won't mention this unless asked directly. | "You have hidden concerns: {concerns}. Only reveal these if the learner asks probing questions." |
| Success Behaviors | Active listening, empathy statements, quantifying impact | Used to build the evaluation rubric |
| Failure Behaviors | Jumping to solutions, ignoring emotions, being pushy | Used to build the evaluation rubric |
| Opening Line | "I've been waiting for this meeting. Let's get straight to the point." | Stored as `first_message` in the scenario record — no LLM call needed |
| Scenario Context (Pre-read) | Markdown-formatted briefing for the learner | Rendered as the pre-read page |
| Max Turns | 20 | Session ends after N turns; triggers evaluation |

**The system assembles these fields into a prompt template:**
```
You are {persona_name}, {role} at {company}.

PERSONALITY: {traits}
BACKSTORY: {backstory}
HIDDEN CONCERNS: {concerns}

RULES:
- Stay in character at all times
- Never break the fourth wall
- If the learner asks good probing questions, reveal hidden concerns gradually
- If the learner is pushy or dismissive, become more guarded
- Keep responses to 2-4 sentences unless the learner asks an open-ended question
```

The specialist never sees this template. They just fill in the fields and click "Preview."

**2. Live Preview / Sandbox**

The single most productivity-enhancing feature. The specialist should be able to:
- Click "Preview" on any scenario
- Chat with the AI persona as if they were a learner
- See how the persona responds to different approaches
- Make adjustments and preview again instantly
- **Without deploying anything, without involving a developer**

This is technically simple: it's just the same chat API but called with the draft scenario config instead of the published one.

**3. Evaluation Rubric Builder**

Similar form-based approach:

| Field | Example |
|---|---|
| Criteria 1 | "Did the learner demonstrate active listening?" |
| Criteria 2 | "Did the learner identify the hidden concern?" |
| Criteria 3 | "Did the learner offer a solution that addressed the customer's specific situation?" |
| Scoring scale | 1-5 per criteria |
| Overall scoring guidance | "8+/15 = Good, 5-7 = Needs Improvement, <5 = Failed" |

These get assembled into the evaluation prompt that's sent to the LLM after the simulation.

**4. Template Library**

Over time, ADA will build dozens of scenarios. The specialist should be able to:
- Browse a library of past scenarios
- Clone an existing scenario as a starting point
- Modify the clone for a new client
- This turns a 1-day task into a 2-hour task

**5. Client Handoff Kit**

When a scenario goes live, the specialist needs to train the admin. Give them:
- An auto-generated "Quick Start Guide" (branded PDF) that they can send to the admin
- A 2-minute screen recording walkthrough (or a guided tour built into the admin UI)
- A pre-written email template: "Hi [Admin], your simulation is live! Here's how to invite your team..."

**6. Client Health Dashboard (Internal)**

The specialist manages multiple clients. They need a single view:

| Client | Active Scenarios | Learners Invited | Learners Completed | Avg Score | Last Activity | Status |
|---|---|---|---|---|---|---|
| Ashok Leyland | 3 | 150 | 89 | 7.1 | 2 days ago | 🟢 Active |
| Gulab & Glow | 1 | 30 | 30 | 6.8 | 14 days ago | 🟡 Needs follow-up |
| New Client | 0 | 0 | 0 | — | — | 🔴 Onboarding |

This prevents clients from going silent and helps the specialist proactively reach out.

---

## 4. Data Model (Simplified — Supports All Three Personas)

```
orgs
  id, name, logo_url, plan (free/paid), created_at

users
  id, email, org_id (nullable for free users), role (learner/admin/ada_specialist/super_admin),
  created_at

scenarios
  id, title, pre_read_markdown, persona_name, persona_config (JSON),
  first_message, max_turns, eval_rubric (JSON), status (draft/published),
  created_by, created_at

org_scenarios
  org_id, scenario_id  (which scenarios an org can access)

learner_assignments
  user_id, scenario_id, assigned_by, assigned_at, deadline

sessions
  id, user_id, scenario_id, status (active/completed/abandoned),
  turn_count, flag_count, is_flagged, started_at, completed_at

messages
  id, session_id, role (user/assistant), content,
  is_flagged (boolean), flag_reason (nullable string), created_at

evaluations
  id, session_id, scores (JSON), summary, strengths (JSON),
  improvements (JSON), created_at

specialist_org_assignments
  specialist_id, org_id  (which orgs an ada_specialist can view on /internal/reports)

session_resets
  id, session_id, reset_by (user_id of admin/specialist), reason, reset_type, new_session_id, reset_at
```

**Key design decisions:**
- `org_id` is nullable on `users` → free users don't belong to any org
- `persona_config` is JSON → stores the structured fields (traits, backstory, hidden concerns) without needing a migration every time you add a field
- `eval_rubric` is JSON → same flexibility for evaluation criteria
- `org_scenarios` is a join table → one org can have many scenarios; one scenario can be shared across orgs (template reuse)
- `max_turns` on scenarios → configurable per scenario by ADA specialist; enforced at the API layer
- `flag_count` / `is_flagged` on sessions → tracks input abuse; surfaced in admin and specialist dashboards
- `is_flagged` / `flag_reason` on messages → individual message-level abuse tracking
- `session_resets` audit table → tracks who reset what and why (admin/specialist accountability)
- Learners have **no** self-service reset capability. **full_redo** archives the old session and creates a new one; **flag_unlock** clears lockout on the existing session (same session_id).

---

## 5. Phased Build Order

> **Status (2026-06):** Phase 0 and Phase 1 shipped (auth, core simulation, debrief). **Phase 2 partial** (reports, reset, role split) shipped 2026-06-21 — see checkboxes below.  
> **Operational docs:** [SCENARIO_MODULE_GUIDE.md](app/scenarios/SCENARIO_MODULE_GUIDE.md), [SCENARIO_INTAKE_EDUCATION.md](app/scenarios/SCENARIO_INTAKE_EDUCATION.md), [ACCREDITATION_EVIDENCE.md](ACCREDITATION_EVIDENCE.md), [DOCS_INDEX.md](DOCS_INDEX.md).  
> **GTM:** [altius_corporate_explainer.md](altius_corporate_explainer.md), [altius_education_explainer.md](altius_education_explainer.md), [altius_shared_blocks.md](altius_shared_blocks.md).

Corporate L&D and higher-ed faculty share the same platform phases; admin in Phase 2 serves both ("L&D dashboard" = "faculty dashboard").

### Phase 0: Foundation (Week 1-2) — shipped
- [x] Next.js project setup (App Router, TypeScript)
- [x] Database schema + Drizzle ORM
- [x] Auth (Supabase — **magic link** + Microsoft OAuth)
- [x] Basic layout: learner portal + admin shell + specialist shell
- [x] Role-based middleware (learner / admin / ada_specialist / super_admin)

### Phase 1: Core Simulation (Week 2-4) — shipped
- [x] Scenario modules (Vikram ×3, FlowBridge discovery)
- [x] Chat UI with streaming responses (Vercel AI SDK)
- [x] **Input abuse monitoring middleware** (pre-LLM gate)
- [x] **Configurable turn limits** (auto-complete at `max_turns`)
- [x] Pre-read page
- [x] Session persistence (resume from last message)
- [x] Pre-generated first message (no LLM call)
- [x] Coaching debrief with per-scenario rubric

### Phase 2: Admin MVP (Week 4-6) — partial
- [ ] Admin: invite learners (single + CSV — magic link invites)
- [ ] Admin: view learner progress (started/completed/not started)
- [x] View session results: admin operational data (status, transcript — no scores) on /admin/reports; specialist/faculty full debrief (transcript + scores) on /internal/reports for assigned orgs — faculty review path
- [x] Admin/specialist: reset learner simulation (flag_unlock + full_redo; audit in session_resets; Resend notifies assigned specialists)
- [ ] Admin: view flagged sessions (input abuse alerts)
- [ ] Admin: export CSV of all scores — **basis for accreditation / CO attainment worksheets** ([ACCREDITATION_EVIDENCE.md](ACCREDITATION_EVIDENCE.md))
- [x] Role-based access: admin org-scoped via users.org_id; specialist via specialist_org_assignments; super_admin all orgs

### Phase 3: Free Tier + Polish (Week 6-8)
- [ ] Free sign-up flow (no org, limited showcase scenarios)
- [ ] Fair use enforcement (feature-gated: no detailed debrief for free)
- [ ] Rate limiting (5 sims/day, 100 LLM calls/day)
- [ ] Mobile-responsive chat UI

### Phase 4: Internal Tooling (Week 8-12)
- [ ] Scenario Builder (form-based, for ADA specialists) — replaces manual module authoring for most fields
- [ ] **Turn limit configuration** per scenario in Scenario Builder
- [ ] Live preview / sandbox
- [ ] Evaluation rubric builder (until then: [SCENARIO_INTAKE.md](app/scenarios/SCENARIO_INTAKE.md) + code module)
- [ ] Template library (clone existing scenarios)
- [ ] Client health dashboard (internal — includes flag rate monitoring)
- [ ] ADA specialist: reset learner simulations (cross-org) — *Org-scoped specialist reset shipped Phase 2 (P2-004/P2-009 via specialist_org_assignments); super_admin can reset any org. True cross-org specialist tooling remains future.*

### Phase 5: Advanced Analytics (Month 3+)
- [ ] Cohort analytics dashboard — criterion-level patterns ("most of batch weak on probing depth")
- [ ] Cohort comparison
- [ ] Executive summary PDF export — L&D and accreditation one-pager
- [ ] Scheduled cohorts (time-bound access)
- [ ] Automated reminder emails
- [ ] Completion certificates (tech debt from Phase 3)
- [ ] *(Optional, HEI)* CO attainment views / crosswalk export with cohort data

---

## 6. What NOT to Build Yet

| Temptation | Why to resist |
|---|---|
| Multi-language support | Get the English experience perfect first. The DB schema supports `locale` from day 1, so adding it later is straightforward |
| Custom branding/white-labeling | Only build when a paying client asks. It's just CSS theming but it's a rabbit hole |
| LMS integrations (SCORM, xAPI) | Enterprise ask that matters later. Keep the data model clean so you *can* export xAPI statements, but don't build the integration |
| Video/voice simulations | Totally different stack (WebRTC, TTS). Stay text-first |
| AI-generated scenarios | Let the internal specialist design scenarios for now. "AI creates the simulation" is a v3 feature |
| Payment/billing system | Use Stripe Checkout or similar when you have paying customers. Don't build billing infrastructure before you have revenue |

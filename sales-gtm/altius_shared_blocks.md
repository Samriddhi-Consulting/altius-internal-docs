# Altius — Shared GTM Blocks

Canonical copy reused across explainer documents. When updating proof points, platform capabilities, or brand footer, edit here first, then sync [altius_corporate_explainer.md](./altius_corporate_explainer.md) and [altius_education_explainer.md](./altius_education_explainer.md). Proof points and deployment stats also feed JSON-LD, `llms.txt`, and AEO citation anchors — update [Altius SEO + AEO Strategy.md](./Altius%20SEO%20+%20AEO%20Strategy.md) Part 5.3 when stats or deployment facts change materially.

---

## Value proposition (canonical)

**What Altius is:** Practice **and** coaching debrief—not practice alone. The debrief is what turns a rehearsal into behaviour change.

**The loop (do not collapse to “practice” only):**

1. **Briefing** — context before the conversation  
2. **Conversation** — practise the interaction under realistic pressure  
3. **Coaching debrief** — behaviour-level feedback immediately after (what landed, what to do differently)  
4. **Evidence** — transcript and scores for the learner, cohort, or accreditation file  

**One-line definition (titles, hero, `llms.txt` lead):**

> Altius helps professionals and students practise high-stakes conversations, receive a structured coaching debrief right after, and change how they perform—not just what they know.

**Tagline (LinkedIn, pitch decks, header copy):**

> Some lessons you can't afford to learn on the job. Altius™ is how your people don't have to.

**LinkedIn About Us (company page, ~2000 chars):**

A surgeon practises on a cadaver before operating on a patient. A pilot practises on a simulator before flying a plane. But when it comes to the human side of professional work, we still rely on classroom role-plays, e-learning modules, and hope.

Your people probably know what a good discovery question sounds like. They know they should listen before pitching. They know the SBI model for giving feedback. The problem is rarely knowledge. It's that knowing and doing are two completely different things, and professional training has never really cracked the second one.

That gap is what Altius is built for.

Altius is a simulation platform that gives professionals a safe space to practise high-stakes conversations before they face them for real. Sales pitches. Customer escalations. Difficult negotiations. Discovery calls where the first few minutes decide everything. Learners talk with AI personas that push back, go quiet when they should engage, and respond the way real people do. It feels like messaging a real person. That's the point.

When a simulation ends, your L&D team gets a proper coaching debrief: strengths, gaps, a behaviour-level score against a rubric built for your context. The kind of insight that usually requires a senior coach in the room, available for every learner, at any scale.

There's something else worth naming. Adults don't want to role-play in front of colleagues. The social risk kills practice quality. Every Altius simulation is private, just the learner and the AI. People try harder, fail more honestly, and learn faster when nobody is watching.

Altius is live, with active scenarios across automotive, B2B sales, customer service, banking, and leadership. No app to install. No LMS to navigate. A link, a brief, and a conversation.

We work with L&D leaders, HRBPs, and functional heads who are tired of training that moves the completion rate but not the capability.

If that's the problem you're trying to solve, we'd like to talk.

**Messaging rules:**

| Lead with (benefits) | Not in headlines / meta / hero |
|----------------------|--------------------------------|
| Knowing–doing gap, behaviour change, coaching debrief | “AI-powered” as the promise |
| Performance under pressure, evidence, ROI | “AI simulation platform” as the category hook |
| Practise → debrief → do differently next time | Feature lists that skip the debrief |

**When “AI” is OK:** solution-aware pages, schema/`llms.txt` secondary clauses, product mechanics (how the persona adapts), comparison content—not page titles, ProofBar, or primary CTAs.

**Proof stat framing:** “90% changed their approach after one session” implies the full loop (conversation + debrief)—do not attribute behaviour change to practice alone.

**Cohort separation (schema + marketing):** Home and `/corporate` use **corporate pilot** stats (n=20, 4.6/5 relevance). `/education` uses **student pilot** stats (n=201, 4.4/5 overall). Never merge cohorts in `aggregateRating`.

---

## What we've built

This isn't a concept. Altius is live, deployed, and tested.

- ✅ **Customer-centricity simulations** for a leading Indian automotive OEM. 40 managers across Parts, Sales, and Service practised retention conversations with an AI fleet operator and received individualised coaching debriefs.
- ✅ **B2B discovery simulation.** Learners practise qualification conversations with a C-suite buyer mid-crisis: customs hold, a Diwali deadline, operational chaos. The system evaluates crisis-appropriate tone, probing depth, and stakeholder awareness. Deployed with a business school cohort and in corporate pilots.
- ✅ **GCC cross-cultural discovery simulation.** Client Solutions teams scope a Norwegian client's GCC workplace fit-out: discovery under Nordic directness, honest expectation-setting, and scoping without over-promising. Live as *The Nordic Standard* on `/demos`.
- ✅ **Leadership and governance simulation.** Regional banking leaders practise difficult feedback when a star RM kept a borrower green through covenant breaches: SBI, empathy without collusion, and shared accountability before Credit closes the book. Live as *The Green RAG* on the homepage DemoTeaser and `/demos`.
- ✅ **Modular scenario architecture.** Each simulation is self-contained: persona, scene, briefing, and rubric. New scenarios can be designed, tested, and deployed in days.
- ✅ **Production-grade platform.** Magic-link authentication, role-based access, organisation-level data isolation, streaming AI responses, and automated coaching evaluation.

---

## ProofBar copy (microsite)

Canonical stat strings per page — do not paraphrase. Sync to Astro `ProofBar.astro` props after edits here.

| Page | Stat string |
|------|-------------|
| Home (`/`) | 90% changed their approach after one session · Rated 4.6/5 for programme relevance |
| Corporate (`/corporate`) | 94% of participants recommend to peers · Rated 4.6/5 for programme relevance |
| Education (`/education`) | 94% rated it good or excellent · 8 in 10 said it deepened understanding vs a lecture alone |

**Education proof band:** 94% good/excellent · 8 in 10 understanding · footnote Rated 4.4/5 by students in pilot cohorts.

**Factual anchors:** Ashok Leyland deployment — **40 managers** (not "150+", not "hundreds"). Corporate: 90% behaviour change · 94% recommend · 4.6/5 (n=20). Education: 94% · 8 in 10 · 4.4/5 (n=201). Do not merge cohorts.

**Hero microcopy (home):** "Sign in with just your email. Takes 60 seconds."

**CTA verb (demo):** "Try a scenario" — not "Try a demo", "Try the demo", or any variation.

---

## Default OG image copy (`altius-practice-doing-difficult.png`)

1200×630 PNG. **Source:** [`altius-practice-doing-difficult.png`](./altius-practice-doing-difficult.png) → copy to `landing-page/public/`. Do not edit `public/` by hand.

| Element | Copy |
|---------|------|
| Headline | People learn to do difficult things by doing difficult things. |
| Subline (benefit) | Close the gap between knowing and doing. |
| Social proof | ● LIVE · 90% changed their approach after one session |
| CTA | Try a scenario · 60 seconds |

Subline is benefit-only (no AI, no process). Social proof and trial deepen on the site.

---

## About A Degree Above™ (ADA) — common

**A Degree Above** builds simulation-based capability tools for organisations and educational institutions that take human performance seriously. **Altius** is our flagship platform: scenario design and behavioural science, brought together so people can practise, get coached, and change how they perform. Altius™ and A Degree Above™ are trademarks of Samriddhi Consulting.

---

## About ADA — corporate closing

Because the only way to change how people perform is to practise the performance. Not just learn about it.

We work with L&D teams, HRBPs, and functional leaders. Not instead of them. You bring the domain expertise: the understanding of what excellent performance looks like in your organisation. We build the Altius simulation that lets your entire team practise it, at scale, with consistency, and with data.

---

## About ADA — education closing

Because the only way to develop professional competence is to practise the competence. Not just study it.

We work with faculty and programme leaders, not instead of them. You bring the domain expertise: the understanding of what professional excellence looks like in your discipline. We build the Altius simulation that lets your students practise it, at scale, with consistency, and with evidence that holds up to scrutiny.

---

## Contact and trademark footer

*To set up a conversation: [insert contact details]*

*Altius™ and A Degree Above™ are trademarks of Samriddhi Consulting. All rights reserved.*

---

## Off-site entity log (SEO / knowledge graph)

Maintain with [Altius SEO + AEO Strategy.md](./Altius%20SEO%20+%20AEO%20Strategy.md) Part 4.4 when listings go live.

| Platform | Status | URL |
|----------|--------|-----|
| Google Search Console | Done | Sitemap submitted June 2026 |
| LinkedIn — A Degree Above | Done | `https://www.linkedin.com/company/a-degree-above/` |
| LinkedIn — Altius showcase | Done | `https://www.linkedin.com/showcase/altius-by-ada/` |
| GitHub — public docs mirror | Done | `https://github.com/Samriddhi-Consulting/altius-public-docs` (auto-sync via `sync-public-docs.yml`) |
| Google Business Profile | Done | [Maps listing](https://www.google.com/maps/place/A+Degree+Above/data=!4m2!3m1!1s0x0:0x1c0738c025962a93) — service-area; `adegreeabove.org`; first post 2026-07-09 |
| Wikidata — A Degree Above | Deleted | Q140329925 removed (RfD spam/advertising, 2026-07-18). Do not recreate without independent sources. |
| Wikidata — Altius | Deleted | Q140469863 removed (RfD notability, 2026-07-19). Do not recreate without independent sources. |
| Crunchbase | Pending | A Degree Above |

---

## Microsite performance (SEO-003)

Shipped 2026-07-10 on the Astro microsite (implementation detail; copy above unchanged):

- Fraunces WOFF2 preload; decorative Δ watermarks use system serif (`.bg-watermark`) — not Fraunces — to avoid CLS
- Education page: measured `content-visibility` on below-fold sections; accreditation logos lazy-loaded
- `/demos`: hydrate on load when URL filters are present, otherwise on visible
- Gate: `npm run verify:lighthouse` in `landing-page/`; re-check production after deploy

Canonical tracker: [Altius SEO + AEO Strategy.md](./Altius%20SEO%20+%20AEO%20Strategy.md) §3.1 · [BACKLOG.md](./BACKLOG.md) SEO-003

# Altius SEO + AEO Strategy

*Complete Growth Architecture · June 2026*

> **Canonical source.** This is the single source of truth for Altius SEO and AEO (Answer Engine Optimization). Execution plans, agent guides, and microsite work must align with this document.
>
> **When to update this file** (same change set as the product edit):
> - New or removed marketing routes, sub-pages, or blog posts
> - Changes to proof points, entity copy, or FAQ content used in schema / `llms.txt`
> - SEO infrastructure shipped or changed (`SeoHead`, JSON-LD, sitemap, `robots.txt`, `llms.txt`, Core Web Vitals fixes)
> - Scenario gallery topology changes that affect crawl strategy (SSR, per-scenario pages, ItemList schema)
> - Cross-host or canonical URL changes
>
> **Referenced by:** [DOCS_INDEX.md](./DOCS_INDEX.md) · [AGENTS.md](./AGENTS.md) · [landing-page/AGENTS.md](./landing-page/AGENTS.md) · [doc-sync skill](../.cursor/skills/doc-sync/SKILL.md)
>
> **Technical execution checklist:** [`.cursor/plans/landing_page_seo_94ae9e46.plan.md`](../.cursor/plans/landing_page_seo_94ae9e46.plan.md) — use this strategy for *what* and *why*; use the plan for Phase 1–4 microsite mechanics.
>
> **Implementation status (SEO infra):**
> - **Phase 1 complete** (2026-06-23) — sitemap, `SeoHead`, `robots.txt`, `llms.txt`, benefit titles/meta, OG image, DemoGallery URL filters (`?topic=` / `?audience=` / `?function=`).
> - **Phase 2 core complete** (2026-06-23) — `src/lib/seo/schema.ts` JSON-LD `@graph`, Tier 1 FAQs (14 corporate + 19 education) with `FAQPage` schema, `/about`, proof stats + `aggregateRating`/`review[]`, footer entity copy. **HowTo + ItemList shipped** (2026-07-09). **Operator HowTo on `/corporate` and `/education`** (SEO-018, 2026-07-10) — 6-step setup HowTo distinct from home learner HowTo; no new route. **Optional / deferred:** Service types on corporate/education.
> - **Phase 3 CWV (SEO-003) shipped** (2026-07-10) — Fraunces preload; `.bg-watermark` CLS fix; education `content-visibility`; demos conditional hydration.
> - **`llms.txt` upgraded** (2026-07-10) — rewritten to [llmstxt.org](https://llmstxt.org/) (H1 + blockquote + curated H2 link lists); live at `/llms.txt`.
> - **GSC:** `https://altius.adegreeabove.org/sitemap-index.xml` submitted (June 2026).
> - **Off-site entity (partial):** LinkedIn + Wikidata + GBP live (ADA [Q140329925](https://www.wikidata.org/wiki/Q140329925), Altius [Q140469863](https://www.wikidata.org/wiki/Q140469863), [GBP Maps](https://www.google.com/maps/place/A+Degree+Above/data=!4m2!3m1!1s0x0:0x1c0738c025962a93)); `sameAs` in `schema.ts`. Crunchbase — pending. Log: Part 4.4.

---

## Preamble: What's Wrong With the Existing Plan
The existing plan is a technically correct execution checklist. It is not a strategy. It treats SEO as a metadata problem. The actual challenge is this: your buyers don't search for Altius — they search for the pain Altius solves. Before they know a product like yours exists, they are searching for things like "why doesn't sales training stick" and "how to prove training ROI to leadership." The existing plan would make your current 4 pages discoverable. This plan will make Altius findable at every point in the buyer's journey, from the moment they feel the pain to the moment they book a call.

Part 1: Persona Architecture & Journey Maps
There are 8 buying personas across two markets. Each has a distinct vocabulary, pain hierarchy, and search behaviour. Every content and technical decision in this strategy maps to one or more of these personas.

Corporate Track
Persona C1 — The L&D Head
Head of L&D, VP Sales Enablement, GM Training, CLO

Their core anxiety: "I can scale training content. I cannot scale the practice that makes content stick. And I cannot prove impact to my CFO."

Funnel Stage What they're experiencing What they search Content needed
Unaware Watching training budgets get cut after flat post-training metrics "why doesn't sales training work" · "training transfer problem" · "L&D ROI measurement" · "Kirkpatrick level 3 assessment" Blog: The Knowing-Doing Gap: Why Your Training Budget Isn't Changing Performance
Problem-aware Realising classrooms/e-learning aren't enough, looking for solutions "practice-based learning corporate" · "sales role play at scale" · "behavioural learning software India" · "AI for L&D" Blog: Beyond the Workshop: 5 Ways to Build Practice into Your Learning Architecture
Solution-aware Evaluating simulation/practice technology "AI conversation simulation training" · "sales practice software" · "simulation based learning platform India" · "virtual role play corporate training" Corporate product page (expanded), comparison content
Decision Evaluating Altius specifically "Altius ADA demo" · "AI training simulation pricing India" · "conversation simulation corporate pilot" Demo request page, pilot brief, case study (Ashok Leyland)
Persona C2 — The HRBP
HR Business Partner, Senior HRBP

Their core anxiety: "The manager knows the SBI feedback framework. They still avoid the conversation. The coaching model isn't building the muscle."

Funnel Stage What they search Content needed
Unaware "why managers avoid difficult conversations" · "feedback conversation training" · "performance conversation coaching" Blog: The Manager Who Knows But Doesn't: Why HRBPs Need a New Tool
Problem-aware "difficult feedback training for managers" · "AI leadership coaching tool" · "manager capability building India" Use-case page: Leadership & People conversations
Solution-aware "AI coaching simulation for managers" · "difficult conversations practice tool" Feature page: Coaching debrief, evidence output
Decision "HR leadership simulation tool pilot" Contact / pilot
Persona C3 — The Functional Head
Sales Director, VP Customer Success, Head of Procurement, Quality Head

Their core anxiety: "My team knows what to do. They don't do it when the customer is actually in the room."

This persona rarely searches for "training tools." They search for the business outcome: "how to improve sales conversion", "reducing customer churn". Your content must meet them at the business problem, not the training solution. This is the most important persona for organic discovery.

Funnel Stage What they search Content needed
Unaware "sales team not closing deals" · "reduce customer churn" · "how to improve discovery calls" · "sales coaching India" Blog: Why Your Reps Know the Discovery Framework and Still Jump to Pitch
Problem-aware "sales enablement tools India" · "conversation coaching for sales teams" · "how to improve first call conversion" Industry-specific pages: Sales, Customer Success, Procurement
Solution-aware "sales simulation tool" · "conversation practice for sales reps India" Scenario gallery, use case content
Decision "try AI sales simulation" Demo → app funnel
Persona C4 — The GCC L&D / HR Lead
L&D Business Partner in a Global Capability Centre

A distinct sub-persona. GCCs in India have large L&D budgets, sophisticated buyers, and a specific pain: cross-cultural communication, influencing without authority, managing global stakeholders. There are 1,600+ GCCs in India. This is a reachable, high-value segment.

Pain vocabulary: "cross-cultural communication training", "GCC soft skills", "influencing stakeholders without authority India", "communication training for offshore teams"

Education Track
Persona E1 — The Programme Director
MBA Director, BBA Chair, PGDM Head

Their core anxiety: "We claim our graduates are industry-ready. Our placement feedback says they can't hold a professional conversation. I need something that closes that gap and shows up in our brochure."

Funnel Stage What they search Content needed
Unaware "employability gap B-school India" · "how to improve student professional skills" · "MBA soft skills training" Blog: The Employability Gap: What Recruiters Say vs. What We Teach
Problem-aware "beyond case study method" · "conversation skills assessment for MBA students" · "professional competence pedagogy" Blog: When the Case Study Talks Back: Moving Students from Analysis to Action
Solution-aware "AI simulation for MBA teaching" · "student communication practice tool" · "conversation simulation higher education" Education product page (expanded), programme-type pages
Decision "Altius MBA pilot" · "AI conversation practice for B-school" Pilot brief, contact
Persona E2 — The Accreditation Lead / IQAC
IQAC Coordinator, Director Quality Assurance, Dean Academic

This persona has the highest-intent, lowest-competition search queries in your entire addressable market. They are searching for very specific things that almost no tool talks about directly. Altius should own these searches entirely.

Funnel Stage What they search Content needed
Unaware "NAAC criterion 2 experiential learning evidence" · "NBA CO PO attainment communication skills" · "NEP 2020 outcome based education implementation" Blog: What NAAC Actually Wants as Proof of Experiential Learning
Problem-aware "how to document CO attainment for soft skills" · "NAAC experiential learning tools" · "NBA programme outcome assessment tool" · "AACSB assurance of learning direct assessment" Accreditation-specific landing pages (NAAC, NBA, NEP, AACSB)
Solution-aware "simulation based assessment NAAC evidence" · "AI tool for CO attainment" Education product page with accreditation mapping
Decision "Altius NAAC accreditation" · "Altius higher education pilot" Pilot brief
Persona E3 — The Faculty
Professor, Asst. Prof., Course Coordinator (Sales, Communication, Negotiation, HRM)

Their core anxiety: "I teach consultative selling for 14 weeks. My students can write about it. I cannot assess whether 60 students can actually do it."

Searches: "how to assess 60 students on role play" · "communication skills assessment tool students" · "AI feedback for student presentations" · "alternative to viva voce for soft skills"

Persona E4 — The Dean / VC
Dean, Vice-Chancellor, Institution Director

Their real need is differentiation in a market of 4,000 B-schools. They are not searching for training tools — they are searching for rankings, reputation, and differentiation strategies. The content for this persona must speak brand and reputation, not pedagogy.

Searches: "how to differentiate MBA programme India" · "edtech innovation for rankings" · "NIRF ranking improvement strategy" · "what makes a top B-school different"

Part 2: Pain-Point Content Architecture
What needs to be built
The existing 4-page site covers your product. It does not cover the buyer's journey. Here is the full content architecture mapped to persona journeys.

Tier 1: Top-of-Funnel Content (Blog / Resources)
These pages drive organic discovery at the awareness stage. They do not pitch Altius — they provide genuine value on the pain. The CTA is soft (download a guide, try a demo scenario).

Corporate pillar:

Post Target Persona Core pain addressed SEO target
The Knowing-Doing Gap: Why Your Training Budget Isn't Changing Behaviour C1, C2 Training transfer failure "training transfer problem" · "why training doesn't stick"
Why Your Sales Team Knows the Framework and Still Caves on Price C3 Sales training to performance gap "sales training effectiveness" · "why salespeople don't use training"
The Scale Problem: Why Classroom Role-Play Fails Past 30 People C1 Scaling practice "scale sales training" · "soft skills training at scale India"
Measuring What Matters: Moving L&D from Smile Sheets to Behavioural Evidence C1, C4 ROI and proof "L&D ROI measurement" · "Kirkpatrick level 3 India"
The Peer Role-Play Problem: Why Adults Don't Stretch in Front of Colleagues C2 Embarrassment/honesty in training "role play training problems" · "why role play doesn't work"
Cross-Cultural Communication in GCCs: The Gap Nobody Measures C4 GCC-specific pain "cross-cultural communication training GCC India"
Sales Coaching at Scale: From the Best Regional Manager to Every Rep C1, C3 Coaching consistency "sales coaching at scale India"
Education pillar:

Post Target Persona Core pain addressed SEO target
The Employability Gap: What Recruiters Say vs. What We Teach in B-Schools E1, E4 Graduate employability "MBA employability gap India" · "B-school placement skills"
When the Case Study Talks Back: Moving MBA Students from Analysis to Action E1, E3 Case method limitation "beyond case study method" · "MBA pedagogy innovation"
What NAAC Actually Wants as Evidence of Experiential Learning E2 NAAC compliance anxiety "NAAC experiential learning evidence" · "NAAC criterion 2 tools"
CO/PO Attainment for Soft Skills: From Vague Claim to Measurable Evidence E2, E3 NBA/NEP compliance "CO PO attainment soft skills" · "NBA programme outcome assessment"
60 Students, One Faculty, Zero Time: Scaling Practice in Higher Education E3 Faculty bandwidth "how to assess 60 students communication" · "faculty bandwidth assessment"
Why Patient Communication Training Still Relies on Standardised Patients — and What's Changed E3 (medical) Medical education gap "patient communication simulation medical college India"
From Internship Certificate to Verifiable Evidence: The New Standard for Experiential Learning E2, E1 Accreditation evidence gap "experiential learning documentation accreditation India"
Tier 2: Middle-of-Funnel Landing Pages (Product + Use Case)
These pages live on the main microsite and target solution-aware buyers. Each needs full SEO treatment (title, meta, schema, internal links from blog posts).

New corporate sub-pages:

Page URL Primary target query Who it's for
Sales Enablement Simulations /corporate/sales-enablement "AI sales training simulation India" C1, C3 (Sales Director)
Leadership & People Conversations /corporate/leadership "difficult conversation practice training" C2, C3 (People managers)
Customer Success & Retention /corporate/customer-success "customer retention training simulation" C3 (CS Head)
GCC Communication Practice /corporate/gccs "cross-cultural communication training GCC" C4
Banking & BFSI Simulations /corporate/banking "customer advisory simulation banking India" C3 (BFSI)
Pharma & Healthcare Rep Training /corporate/pharma "medical rep communication training AI" C3 (Pharma)
New education sub-pages:

Page URL Primary target query Who it's for
NAAC Compliance & Evidence /education/naac "NAAC experiential learning evidence tool" E2
NBA CO/PO Attainment /education/nba "NBA CO PO attainment soft skills assessment" E2
NEP 2020 Alignment /education/nep "NEP 2020 outcome based education tools" E2, E1
MBA & PGDM Programmes /education/mba "AI simulation for MBA teaching India" E1, E3
Law Schools /education/law "client counselling simulation law school India" E3 (Law faculty)
Medical & Nursing Colleges /education/medical "patient communication simulation medical college" E3 (Medical faculty)
Engineering Colleges /education/engineering "professional communication skills engineering students NEP" E1
Tier 3: Bottom-of-Funnel Conversion Pages
These don't exist today and are critical conversion infrastructure.

Page Purpose
/about **Shipped** — entity anchor; LinkedIn About copy, what we've built, AboutPage schema (`https://altius.adegreeabove.org/about`)
/case-studies/ashok-leyland Only live proof point — must be a full page, not a stat in a ProofBar
/pilot A dedicated "start a pilot" landing page with frictionless contact form
/pricing Even a rough "per scenario, per cohort" page reduces sales friction
/compare Comparison with alternatives (classroom training, generic AI chatbots, video role-play platforms)
/scenarios (expanded) Scenario library with per-scenario pages (Phase 5B)
Part 3: Technical SEO Foundation
3.1 Core Web Vitals (India 4G Priority)
This is non-negotiable given the brand guidelines' Indian 4G, mid-range Android target. Lighthouse mobile 90+ is the standard.

LCP (Largest Contentful Paint < 1.5s)

fetchpriority="high" + <link rel="preload" as="image"> on all hero .webp images
Confirm public/fonts/fraunces-variable.woff2 is committed and served correctly — a missing font that falls back to system-ui is invisible in dev but kills Lighthouse
Inline the critical CSS above the fold (astro:inline on BaseLayout's above-fold styles)
Serve hero images from Cloudflare CDN (already on Cloudflare Pages — ensure cache headers are correct)
CLS (Cumulative Layout Shift < 0.05)

Every <img> needs explicit width + height attributes, or aspect-ratio in CSS
Fraunces WOFF2 must have @font-face { ascent-override; size-adjust } (already in brand guidelines — verify it's implemented)
Reserve space for the ProofBar stats if they load client-side
FID/INP (Interaction to Next Paint < 100ms)

DemoGallery.tsx is the primary offender — it is a React island with full re-renders on filter. Convert filters to URL params (already planned) and ensure the initial paint is the complete unfiltered list from SSR HTML
Verify SSR output (critical blocker):

astro build && grep -c "scenario-card" dist/demos/index.html
If this returns 0, all scenarios in `SCENARIOS` are client-only rendered and invisible to crawlers. This must be confirmed before any other SEO work.

3.2 Crawl Infrastructure
XML Sitemap (via @astrojs/sitemap)

Current: shipped in `landing-page/public/robots.txt` (2026-06-23)
Target: https://altius.adegreeabove.org/sitemap-index.xml auto-generated at build time
As new pages are added, ensure all are included (exclude any thin filter-variant URLs if added)
Priority values: / → 1.0, main audience pages → 0.9, sub-pages → 0.8, blog → 0.7
HTML Sitemap

Add /sitemap as a human-readable page — helps both crawlers and users, especially once the site grows to 20+ pages
Structure it as the full content hierarchy:
Home
Corporate → Sales Enablement · Leadership · Customer Success · GCCs · by Industry
Education → NAAC · NBA · NEP · by Programme Type
Demo Scenarios
Blog / Resources
About · Pilot · Contact
robots.txt

User-agent: \*
Allow: /
Disallow: /admin/
Disallow: /internal/
Sitemap: https://altius.adegreeabove.org/sitemap-index.xml
Note: include specific bot permissions if needed for GPTBot, ClaudeBot, PerplexityBot (for AEO — see Part 5).

Internal Linking Architecture
This is where most sites fail. Every blog post and sub-page must link to adjacent content and up to the primary product page. The hub-and-spoke model:

Home (hub)
├── /corporate (hub)
│ ├── Blog posts link back to /corporate
│ ├── /corporate/sales-enablement links to relevant scenarios on /demos
│ └── /case-studies/ashok-leyland links to /corporate
├── /education (hub)
│ ├── Blog posts link back to /education
│ ├── /education/naac links to /education
│ └── All accreditation pages link to /pilot
└── /demos (hub)
└── Each scenario links to the relevant corporate/education sub-page
Internal link anchor text must use descriptive, keyword-relevant phrases — not "click here" or "learn more."

Canonical Tags

Every page must have a self-referencing canonical
If filter-variant URLs are ever added (e.g., /demos?audience=corporate), canonical must point to /demos
Cross-host: if the app host ever renders marketing-like pages, canonical back to the microsite
3.3 Page-Level SEO Fixes (From Existing Plan — Confirmed)
These remain valid and unchanged:

Issue Fix
Title double-suffix bug Remove "Altius |" prefix from page title props; layout appends | Altius by A Degree Above
Missing home description Write dedicated 150–160 char description using ProofBar proof points
Missing OG/Twitter tags SeoHead.astro component
Missing canonical Emit on every page
theme-color #003f88
DemoGallery URL params Fix ?audience= and ?function= params
Part 4: Schema & Knowledge Graph
4.1 Entity Map
Before writing schema, map the entities and their relationships. This is what you're trying to teach Google's knowledge graph:

Entities:

Entity Type Key properties
A Degree Above (ADA) Organization Operating company; `@id` `https://adegreeabove.org/#organization`; India
Altius Brand + WebApplication Product (2026); microsite `altius.adegreeabove.org`
Samriddhi Consulting — Trademark proprietor only (footer line on site; **not** in JSON-LD as `parentOrganization`)
The Knowing-Doing Gap Concept (no schema type — use editorial ownership) The problem Altius solves
Simulation-Based Learning Concept Category Altius belongs to
Ashok Leyland (reference) Organization Client reference — do NOT claim as schema entity, reference in content only
Behavioural Evidence Concept Output category of Altius
Relationships (edges to establish):

Altius → brand / application → A Degree Above (operator)
A Degree Above → `sameAs` → LinkedIn company page; `url` → `https://adegreeabove.org`
Altius showcase → `sameAs` → `https://www.linkedin.com/showcase/altius-by-ada/`
Altius → applicationCategory → "Educational Technology", "Corporate Training Software"
Altius → audience → L&D professionals, Higher Education institutions
Altius → areaServed → India (IN)
Altius → operatingSystem → "Browser-based"
4.2 Schema Per Page
Site-wide (every page via BaseLayout):

{
"@context": "https://schema.org",
"@graph": [
{
"@type": "Organization",
"@id": "https://adegreeabove.org/#organization",
"name": "A Degree Above",
"alternateName": "ADA",
"url": "https://adegreeabove.org",
"logo": "https://altius.adegreeabove.org/altius-tm-logo.png",
"sameAs": [
"https://www.linkedin.com/company/a-degree-above/"
],
"contactPoint": {
"@type": "ContactPoint",
"email": "connect@adegreeabove.org",
"contactType": "sales"
}
},
{
"@type": "Brand",
"@id": "https://altius.adegreeabove.org/#brand",
"name": "Altius",
"sameAs": "https://www.linkedin.com/showcase/altius-by-ada/"
},
{
"@type": "WebSite",
"@id": "https://altius.adegreeabove.org/#website",
"name": "Altius by A Degree Above",
"url": "https://altius.adegreeabove.org",
"publisher": { "@id": "https://adegreeabove.org/#organization" }
}
]
}
Home page (/) — additional schema:

{
"@type": "SoftwareApplication",
"@id": "https://altius.adegreeabove.org/#software",
"name": "Altius",
"applicationCategory": ["EducationalApplication", "BusinessApplication"],
"operatingSystem": "Browser-based (iOS, Android, Windows, macOS)",
"offers": {
"@type": "Offer",
"description": "Per-scenario, per-cohort pricing. No platform licence.",
"areaServed": "IN"
},
"featureList": [
"AI-powered conversation simulations",
"Custom AI personas with emotional range",
"Automated coaching debrief",
"Behavioural evidence and scoring",
"Cohort-level analytics dashboard"
],
"audience": {
"@type": "Audience",
"audienceType": "L&D leaders, HRBPs, higher education faculty, programme directors"
}
}
Home — HowTo schema (maps to How It Works section):

{
"@type": "HowTo",
"name": "How to run an AI conversation simulation with Altius",
"description": "Run an Altius simulation in four steps: brief, conversation, coaching debrief, dashboard.",
"image": "https://altius.adegreeabove.org/altius-how-it-works.png",
"inLanguage": "en-IN",
"step": [
{
"@type": "HowToStep",
"name": "The Briefing",
"text": "Learners receive a contextual scenario brief — the professional context, their role, and who they'll meet."
},
{
"@type": "HowToStep",
"name": "The Conversation",
"text": "Learners practice with a realistic AI persona that adapts in real time to what they say, how they say it, and what they leave out."
},
{
"@type": "HowToStep",
"name": "The Coaching Debrief",
"text": "Immediately after the conversation, Altius generates a structured behaviour-level coaching evaluation with scores, strengths, and areas to improve."
},
{
"@type": "HowToStep",
"name": "The Dashboard",
"text": "L&D leaders and faculty see individual and cohort-level data: who completed, individual scorecards, and aggregate capability gap patterns."
}
]
}
/corporate — additional schema:

{
"@type": "Service",
"name": "Altius Corporate Conversation Practice Simulations",
"provider": { "@id": "https://adegreeabove.org/#organization" },
"serviceType": "Corporate Training Simulation",
"description": "AI-powered practice simulations for sales teams, managers, and functional leaders across Indian organisations.",
"areaServed": "IN",
"audience": { "@type": "Audience", "audienceType": "L&D leaders, HR Business Partners, Sales Directors" }
}
Add FAQPage schema (source from the 6 pain points in the corporate explainer — convert each to Q&A format).

/education — additional schema:

{
"@type": "Service",
"name": "Altius Higher Education Practice Simulations",
"provider": { "@id": "https://adegreeabove.org/#organization" },
"serviceType": "Higher Education Assessment Tool",
"description": "AI-powered student simulations with accreditation-ready behavioural evidence for NAAC, NBA, NEP 2020, AACSB, and AMBA.",
"areaServed": "IN"
}
Add FAQPage schema sourced from the 5 problems section and the accreditation mapping table. These are high-value for AI Overviews.

/demos — ItemList schema:

{
"@type": "ItemList",
"name": "Altius AI Conversation Scenario Library",
"description": "{N} AI-powered professional conversation scenarios across sales, leadership, operations, and academic disciplines.",
"numberOfItems": "{N}",
"itemListElement": [
// One ListItem per scenario with name, description, url
]
}
/case-studies/ashok-leyland (new page) — schema:

{
"@type": "Article",
"articleSection": "Case Study",
"name": "Ashok Leyland: AI Conversation Practice at Scale Across Dealer Network",
"about": { "@id": "https://altius.adegreeabove.org/#software" },
"description": "40 managers across Parts, Sales, and Service practised retention conversations with an AI persona. 94% recommended Altius to peers.",
"datePublished": "2026-06"
}
Per-accreditation pages (/education/naac, /education/nba):
Add FAQPage schema per page with 5–8 Q&As targeting the exact questions IQAC coordinators and faculty search.

4.3 Knowledge Graph Integration Tactics
Getting into Google's knowledge graph requires authority signals across multiple platforms:

**Google Business Profile** for **A Degree Above** (online-only / service-area business OK) — anchors verified business identity; website `https://adegreeabove.org`. Do not list Samriddhi as the operating business on GBP. **Status: done (July 2026)** — [Maps listing](https://www.google.com/maps/place/A+Degree+Above/data=!4m2!3m1!1s0x0:0x1c0738c025962a93); description, logo, phone, first post live (public crawl may lag).
**LinkedIn company page** (A Degree Above) — live with logo, description, website → `adegreeabove.org`. **About Us copy:** [altius_shared_blocks.md](./altius_shared_blocks.md) § Value proposition. **Status: live.**
**Altius showcase page** — `https://www.linkedin.com/showcase/altius-by-ada/` linked to microsite; matches `Brand.sameAs` in JSON-LD. **Status: URL shared publicly (June 2026).**
**Wikidata** — ADA [Q140329925](https://www.wikidata.org/wiki/Q140329925) + Altius [Q140469863](https://www.wikidata.org/wiki/Q140469863); linked via `developer` / `product or material produced`. **Status: done (July 2026).**
**Crunchbase / AngelList** listing for A Degree Above — free tier sufficient for cross-reference. **Status: pending.**
**Wikipedia** — strongest signal when secondary coverage exists. Not yet feasible.
**Press coverage / PR** — Ashok Leyland pilot is the natural hook (ET L&D, YourStory, HR Katha).
**Author schema** on blog posts — deferred until blog launches (Phase 5).

4.4 Off-site entity log (maintain when listings go live)

| Platform | Entity | URL / ID | Status | Notes |
|----------|--------|----------|--------|-------|
| Google Search Console | Microsite | Property: `altius.adegreeabove.org` | **Done** | Sitemap submitted June 2026 |
| LinkedIn company | A Degree Above | `linkedin.com/company/a-degree-above/` | **Done** | `sameAs` in schema |
| LinkedIn showcase | Altius | `linkedin.com/showcase/altius-by-ada/` | **Done** | Shared publicly; `Brand.sameAs` |
| Google Business Profile | A Degree Above | [Maps listing](https://www.google.com/maps/place/A+Degree+Above/data=!4m2!3m1!1s0x0:0x1c0738c025962a93) | **Done** | Service-area; `adegreeabove.org`; first post 2026-07-09 |
| Wikidata | A Degree Above | [Q140329925](https://www.wikidata.org/wiki/Q140329925) | **Done** | P856, P973 org `/about/`, P4264 company, P1056 → Altius |
| Wikidata | Altius | [Q140469863](https://www.wikidata.org/wiki/Q140469863) | **Done** | P178 → ADA, P973 product `/about`, P4264 `altius-by-ada` showcase |
| Crunchbase | A Degree Above | — | Pending | Optional |
| `adegreeabove.org` | Parent org site | Organization markup on parent host | Future | Reinforces `@id` |
Part 5: AEO — Answer Engine Optimization
AEO is not a future consideration for Altius. Your buyers — L&D heads, HRBPs, programme directors, IQAC coordinators — are highly educated professionals who actively use Perplexity, ChatGPT, and Google AI Overviews for vendor research. They will ask questions like:

"What are the best AI sales training tools in India?"
"How do I demonstrate NAAC CO attainment for communication skills?"
"Is there software for practising difficult conversations at work?"
If Altius is not in the answer, it does not exist for this class of buyer.

5.1 /llms.txt (Required — not optional)
**Shipped** at [`landing-page/public/llms.txt`](./landing-page/public/llms.txt) → `https://altius.adegreeabove.org/llms.txt`.

Follow the [llmstxt.org](https://llmstxt.org/) format (not free-form marketing prose):

1. `#` H1 — site/product name  
2. `>` blockquote — canonical one-liner from [altius_shared_blocks.md](./altius_shared_blocks.md)  
3. Free body (no headings) — citation rules, cohort-separated proof, entity split (Altius product / A Degree Above operator)  
4. `##` sections of absolute Markdown links: `- [Title](https://…): notes`  
5. Final `## Optional` for secondary URLs (sitemap, robots, public docs mirror, contact)

When proof points, routes, or entity URLs change, update `llms.txt` in the same change set as the page/schema edit.

5.2 AI Crawler Access Policy
Update robots.txt to explicitly allow AI crawlers while keeping app paths blocked:

# Standard crawlers

User-agent: \*
Allow: /
Disallow: /admin/
Disallow: /internal/

# AI crawlers — explicitly allow for AEO

User-agent: GPTBot
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Googlebot
Allow: /

Sitemap: https://altius.adegreeabove.org/sitemap-index.xml
5.3 Content Structure for AI Citation
AI answer engines pull from:

FAQ schema — structure your content as questions and answers; Google AI Overviews and Perplexity directly pull from FAQPage schema
Definition statements — open each major section with a direct, citable definition. "Altius is an AI-powered conversation simulation platform that..." is more citable than an implied description
Lists and tables — AI models prefer structured content. The comparison tables in the corporate/education explainers are already in this format — they need to be on the live web pages
Statistic anchors — specific, cited numbers ("90% of participants changed their approach after one session", "40 managers across Parts, Sales, and Service", "94% recommend to peers", "8 in 10 said it deepened understanding") are highly citable. They must appear on the live pages, not just in internal documents. **Cohort separation:** corporate n=20 (4.6/5) vs education n=201 (4.4/5) — never merge in schema or marketing copy.
FAQ content — **shipped (Tier 1, June 2026):** 14 Q&As on `/corporate`, 19 on `/education` (visible accordions + matching `FAQPage` schema). Source: [altius_faq_drafts.md](./altius_faq_drafts.md) → `landing-page/src/data/faqs/`. Original six-pain-point set (reference):

/corporate FAQs (from the 6 pain points):

Q: Why doesn't corporate training change how people behave under pressure? A: [The knowing-doing gap paragraph]
Q: How can I run quality role-play practice with a team of 50+ people across geographies? A: [Scale problem paragraph]
Q: How do I prove to leadership that training is improving performance? A: [Proof gap paragraph]
Q: Why do adults play it safe in peer role-plays? A: [Embarrassment problem paragraph]
Q: How long does it take to deploy an Altius simulation? A: [One to two weeks from brief to live]
Q: Does Altius require an LMS integration? A: [No app, no download, browser-based]
/education FAQs:

Q: How does Altius help with NAAC accreditation evidence? A: [Accreditation mapping paragraph]
Q: How do I assess 60 students on communication skills? A: [Faculty bandwidth problem]
Q: What is CO/PO attainment evidence for soft skills? A: [OBE compliance section]
Q: Is Altius a replacement for the case method? A: [Case method ceiling section]
Q: How does Altius differ from a viva voce? A: [Comparison table row]
Q: What frameworks does Altius map to for accreditation? A: NAAC, NBA, NEP 2020, AACSB, EQUIS, AMBA
Part 6: Social Media Integration
Social does not directly move search rankings. It does five things that matter for this strategy: builds brand awareness at the top of the funnel before prospects search, generates inbound links from social posts that get shared, drives brand search volume (which is a positive ranking signal), builds the authority of named authors (E-E-A-T), and distributes the blog content you're creating.

6.1 Platform Strategy
LinkedIn — Primary channel. Non-negotiable.

Your buyers live here. L&D heads, HRBPs, MBA programme directors, IQAC coordinators — all on LinkedIn, all consuming professional content. LinkedIn is where Altius's content will get discovered, shared, and discussed before a buyer ever types a search query.

Two LinkedIn entities to establish:

A Degree Above company page — fully completed, with logo, banner, description, website link → `adegreeabove.org`. **Status: live.**
Altius showcase page — `https://www.linkedin.com/showcase/altius-by-ada/` → `altius.adegreeabove.org`. **Status: live; URL shared publicly (June 2026).**
Content pillars for LinkedIn:

Pillar Cadence Format Example
The Knowing-Doing Gap 2x/week Short post (150–200 words) "A sales rep learned the discovery framework in the workshop. Two weeks later they jumped straight to pitch. This isn't a training failure. It's a practice gap. [Link to blog]"
Simulation Snapshot 1x/week Carousel / screenshot Show an excerpt of an Altius conversation. The AI persona, the learner's message, the adaptive response. Prove the product works without claiming anything.
Proof Point 1x/week Stat + context "90% of participants changed their approach after one session. Not after a curriculum. After one 20-minute conversation with an AI persona. That's what practice does."
Accreditation Hook (education) 1x/week Direct post "Your SSR claims experiential learning. Your evidence is attendance registers and internship certificates. NAAC panels know the difference. There is a third category now."
Industry Use Case 1x/week Case micro-story The fleet operator scenario. The CPO in crisis. The first-time manager. Narrate a scenario as a story.
Behind the Build Monthly Longer post How a scenario gets built. The persona design process. What makes an AI counterpart feel real. Builds credibility and interest.
Personal thought leadership (from founders/specialists): In India B2B, personal profiles outperform company pages 5:1. The ADA specialist team and founders should post individually, not just through the company page. LinkedIn prioritises personal content. Even 2 posts/week per founder dramatically outperforms company-only posting.

Twitter/X — Secondary. Thought leadership only.

Post condensed versions of LinkedIn content. Follow and engage with L&D Twitter: Josh Bersin, ATD, NASSCOM L&D conversations, SHRM India. Do not invest heavily here until LinkedIn is working.

YouTube — Future (Phase 5+)

Once the product is more polished visually, a 3-minute product demo video (showing an actual conversation + debrief) will be the single highest-conversion piece of content you can create. Defer until the free-tier experience is live and the UI is camera-ready.

6.2 Social → SEO Virtuous Loop
The social strategy feeds SEO in this order:

Blog post published → shared on LinkedIn
L&D practitioners share the post → brand mentions increase
Brand search volume increases → Google sees rising entity interest
Some shares lead to links from other websites → builds domain authority
Author of the blog post builds a named author profile → E-E-A-T score improves
Perplexity and ChatGPT pull from LinkedIn posts as secondary citations when answering questions about AI training tools → AEO value
6.3 Social Schema Integration
Every blog post needs:

<meta property="og:type" content="article" />
<meta property="article:author" content="[Author LinkedIn URL]" />
<meta property="article:published_time" content="[ISO date]" />
Every author page (bio) needs Person schema with sameAs pointing to their LinkedIn profile. This connects the content to the named author entity in the knowledge graph.

Part 7: Implementation Phasing
Given the scope, here is the prioritised execution order based on ROI-per-effort.

Week 1–2 (Unblock everything) — **done (2026-06-23):**

Verify DemoGallery SSR output ✓
Confirm all assets exist in public/ ✓
OG default image ✓
@astrojs/sitemap, robots.txt, SeoHead.astro ✓
A Degree Above LinkedIn company page ✓
/llms.txt (llmstxt.org format) and robots.txt AI crawler rules ✓
GSC sitemap submitted ✓

Week 3–4 (Schema foundation + AEO) — **core done (2026-06-23); partial deferrals:**

Site-wide Organization + WebSite + Brand JSON-LD ✓
Home: WebApplication + aggregateRating (corporate cohort) ✓; HowTo ✓ (2026-07-09)
Corporate + Education: FAQPage schema (14 + 19 Q&As) ✓; Service type optional
Corporate + Education: operator HowTo (6-step setup) ✓ (SEO-018, 2026-07-10)
/about entity page ✓
Title/meta cleanup on core pages ✓
Demos: ItemList schema ✓ (2026-07-09)

Month 2 (Content architecture) — **pending:**

Launch blog/resources section (Astro content collections work perfectly for this)
Write first 3 corporate blog posts (The Knowing-Doing Gap + Scale Problem + Peer Role-Play)
Write first 3 education blog posts (NAAC Evidence + CO/PO Attainment + Employability Gap)
Begin LinkedIn posting: 3x/week minimum
Month 2–3 (Sub-pages):

Create /education/naac, /education/nba, /education/nep (highest-intent, lowest-competition queries)
Create /corporate/gccs, /corporate/sales-enablement
Create /case-studies/ashok-leyland (convert the proof points into a full narrative case study page)
/about — **shipped** (2026-06-23)
Core Web Vitals pass: LCP preload, image dimensions, lazy load, watermark CLS, education `content-visibility` — **shipped 2026-07-10** (SEO-003)

Month 3+ (Content at scale + knowledge graph):

Complete corporate + education sub-page set
Wikidata entry for A Degree Above (+ Altius product optional) — **done**
Google Business Profile (A Degree Above, service-area) — **done**
Begin PR outreach (ET L&D beat, YourStory, HR Katha) anchored on Ashok Leyland story
HTML sitemap at /sitemap
Phase 5B evaluation: per-scenario pages based on GSC data
Part 8: Measurement
What to track from Day 1:

Signal Tool Frequency
Organic impressions by query Google Search Console Weekly
Organic CTR by page GSC Weekly
Core Web Vitals per page GSC / Lighthouse CI Per deploy
Keyword ranking for target queries GSC Performance report Monthly
LinkedIn organic reach + profile visits LinkedIn Analytics Weekly
Brand search volume GSC (filter by brand queries) Monthly
AI citation check Manual Perplexity / ChatGPT query Monthly
30-day milestones:

All **5** core pages indexed (`/`, `/corporate`, `/education`, `/demos`, `/about`) with correct canonicals, OG tags, and schema (verify with Google Rich Results Test + LinkedIn/Facebook debugger)
/sitemap-index.xml submitted to GSC ✓
LinkedIn company page live + Altius showcase URL shared ✓
First 4 LinkedIn posts — **in progress**
90-day milestones:

3+ blog posts indexed and appearing for target queries
First sub-pages (NAAC, GCC) ranking for their primary target queries
Altius appearing in Perplexity answers for "AI conversation training tool India"
LinkedIn page: 200+ followers, 3–5 posts driving 500+ organic impressions each
6-month milestone:

15+ indexed content pages beyond the core 4
Consistent organic traffic to blog posts from L&D / HE audience
Ashok Leyland case study page ranking for "[OEM name] + training" queries
GSC data informing Phase 5B per-scenario page decision
What This Strategy Is Not
To be explicit about scope:

Not paid search. Nothing here requires ad spend. The entire strategy is organic.
Not hreflang. Single English site targeting India. No language variants needed.
Not LMS integration or SCORM. Those are product decisions, not SEO decisions.
Not white-hat link manipulation. Links should come from genuine PR, content sharing, and entity listings — not link exchanges.
The core reframe across everything above: the buyer's journey starts with their pain, not your product. The existing plan optimised for the last 10% of the journey (when someone is already searching for Altius or something like it). This strategy builds the entire funnel — catching the L&D head asking "why doesn't training change behaviour" just as much as the IQAC coordinator asking "NAAC experiential learning evidence tool." Every piece of content, every schema block, and every LinkedIn post is either pulling a new buyer into awareness or pushing an existing one toward a conversation.

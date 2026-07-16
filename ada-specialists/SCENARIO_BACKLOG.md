# Scenario content backlog

*Last updated: 2026-07-16. Ranked catalog of modules to build from the microsite gallery.*

**Execution status** (Pipeline, Active sprint, `done`): [BACKLOG.md](../../BACKLOG.md) — Content — Scenarios section.

**Source of truth for cards:** [landing-page/src/data/scenarios.ts](../../landing-page/src/data/scenarios.ts) (gallery copy + slugs) · **Live modules:** [registry.ts](./registry.ts)

---

## How to use this file

| Question | Where to look |
|----------|---------------|
| What slug, title, and gallery description? | [Catalog table](#catalog-all-gallery-scenarios) below |
| What order should we build in? | [Waves](#waves-ranked-build-order) + [Full priority stack](#full-priority-stack) |
| Why this order? | [Demand signals](#demand-signals-20252026) |
| How to build one module? | [SCENARIO_INTAKE.md](./SCENARIO_INTAKE.md) or [SCENARIO_INTAKE_EDUCATION.md](./SCENARIO_INTAKE_EDUCATION.md) → [SCENARIO_MODULE_GUIDE.md](./SCENARIO_MODULE_GUIDE.md) |
| What's in sprint / shipped? | [BACKLOG.md](../../BACKLOG.md) |

### Client-first override

Any **confirmed org or faculty request** (pattern: SCN-001 / Spacematrix) jumps to **V = 1** and the front of the queue regardless of wave. Update this file first, then sync Pipeline + table rows in `BACKLOG.md`.

### Live vs aspirational

| Bucket | Count | Slugs |
|--------|------:|-------|
| **Live (seeded)** | 12 | `vikram-sales`, `vikram-parts`, `vikram-service`, `flowbridge-discovery`, `design-build-gcc-norway`, `difficult-feedback`, `fmcg-distributor-negotiation`, `sales-product-knowledge`, `client-counselling-law`, `customer-success-renewals`, `divorce-custody-counselling`, `accountability-under-pressure` |
| **On `/demos` gallery** | 25 | All entries in `scenarios.ts` |
| **Gallery + playable** | 8 | `design-build-gcc-norway`, `difficult-feedback`, `fmcg-distributor-negotiation`, `sales-product-knowledge`, `client-counselling-law`, `customer-success-renewals`, `divorce-custody-counselling`, `accountability-under-pressure` |
| **Live but not on gallery** *(intentional)* | 4 | Vikram trilogy + Flowbridge — featured via homepage/corporate/education CTAs |
| **Aspirational (gallery only)** | 17 | Every other card in `scenarios.ts` |

**Unseeded funnel:** Aspirational CTAs still deep-link to `/pre-read/{slug}`. Missing or unpublished modules render [ScenarioUnavailable](../src/components/learner/scenario-unavailable.tsx) ("being re-engineered") plus live alternatives — not a 404.

**Gallery sync (SCN-GAL):** deferred by design — live slugs stay off the public gallery catalog. Track in [BACKLOG.md](../../BACKLOG.md) only.

---

## Demand signals (2025–2026)

Prioritization after Wave 1 uses published L&D and India market trends, not gallery row order.

| Signal | Implication for Altius |
|--------|------------------------|
| Management / supervisory training tops L&D budget increases (~30% of orgs); 90%+ report leadership skills gaps | Leadership & People scenarios rank high after Wave 1 |
| Soft skills / difficult conversations rising as AI handles technical work | `difficult-feedback` (Wave 1); extend to coaching, PIP |
| India corporate training ~$12B, 13% CAGR; sales + leadership fastest-growing lines | Sales, negotiation, leadership before vertical niches |
| India CX/BPO ~2M workers; market toward ~$9B by 2030 | Contact-centre service recovery is mass-market |
| BFSI: 83% prioritize digital transform, ~31% workforce ready; simulation-based learning cited | Loan advisory, trust-building |
| GCC India ~2M professionals; upskilling = primary retention lever | `gcc-escalation`, consulting pushback |
| Healthcare simulation: web-based fastest-growing; NMC India mandates sim labs | `patient-communication` (Wave 1) |
| Law: BCI 2020 rules + NEP 2020 mandate client counselling, clinical legal education | `client-counselling-law` (Wave 1); `divorce-custody-counselling` (Wave 1 sibling) |
| Hospitality: ~3.8L annual demand vs ~30k HM grads; soft-skills bottleneck | Strong content case; narrower institutional buyer base → Wave 4 |

References (external): [LMSPedia 2026 benchmarks](https://lmspedia.org/corporate-training-budgets-2026-benchmarks/) · [IMARC India corporate training](https://www.imarcgroup.com/india-corporate-training-market) · [Wisemonk CX market](https://www.wisemonk.io/blogs/india-cx-market) · [KNOLSKAPE BFSI 2025](https://www.webnewswire.com/2025/07/09/skill-and-leadership-gaps-are-slowing-digital-transformation-in-indias-bfsi-sector-says-knolskape-report-2025/) · [nasscom GCC Value Orbit](https://nasscom.in/knowledge-center/publications/gcc-value-orbit-delivery-engine-enterprise-nerve-centre) · [Kaam hospitality hiring](https://kaam.com/blog/hiring-and-training-hospitality-talent)

---

## Waves (ranked build order)

### Wave 1 — Broken marketing promises (highest V)

Homepage DemoTeaser and/or `/education` featured blocks link here today; sign-in without a seeded module fails.

| ID | Slug | Title | Intake |
|----|------|-------|--------|
| SCN-003 | `difficult-feedback` | Giving Difficult Feedback | Corporate |
| SCN-004 | `fmcg-distributor-negotiation` | The Promise | Corporate |
| SCN-005 | `patient-communication` | Patient Communication (Medical) | Education |
| SCN-006 | `client-counselling-law` | Eighteen Lakhs, No Reply | Education |

### Wave 2 — Tier A: highest volume + budget (India corporate)

Leadership continuation + sales core + CX at scale.

| ID | Slug | Title |
|----|------|-------|
| SCN-007 | `sales-objection-handling` | Sales: Objection Handling & Discovery |
| SCN-008 | `customer-support-contact-centre` | Customer Support & Service Recovery |
| SCN-009 | `coaching-direct-reports` | Coaching Direct Reports |
| SCN-010 | `performance-conversation` | Performance Conversations (PIP) |
| SCN-011 | `presales-solution-consulting` | Pre-sales & Solution Consulting |
| SCN-012 | `customer-success-renewals` | The Escalation |

### Wave 3 — Tier B: India strategic sectors (GCC + BFSI + negotiation)

| ID | Slug | Title |
|----|------|-------|
| SCN-013 | `gcc-escalation` | GCC: Escalation Management |
| SCN-014 | `banking-loan-advisory` | Retail Banking: Loan Advisory |
| SCN-015 | `negotiation-simulation` | Negotiation Simulation |
| SCN-016 | `procurement-negotiation` | Purchase & Procurement Negotiation |
| SCN-017 | `consulting-client-expectation` | Consulting: Managing Client Expectations |
| SCN-018 | `leading-through-change` | Leading Through Change |

### Wave 4 — Tier C: education breadth + hospitality skills gap

| ID | Slug | Title |
|----|------|-------|
| SCN-019 | `business-communication` | Business Communication Practice |
| SCN-020 | `hospitality-guest-dispute` | Hospitality: Guest Dispute Resolution |

### Wave 5 — Tier D: vertical niches (build on client pull)

| ID | Slug | Title |
|----|------|-------|
| SCN-021 | `sales-product-knowledge` | Sales: Product Knowledge in Context |
| SCN-022 | `telecom-enterprise-renewal` | Telecom: Enterprise Renewal |
| SCN-023 | `real-estate-objection` | Real Estate: High-Ticket Objection |
| SCN-024 | `quality-compliance` | Quality & Compliance Conversations |
| SCN-025 | `accountability-under-pressure` | Downstream (was Government & Regulatory) |

---

## Full priority stack

Single pick order after Wave 1 (unless client-first override):

1. `sales-objection-handling` (SCN-007)
2. `customer-support-contact-centre` (SCN-008)
3. `coaching-direct-reports` (SCN-009)
4. `performance-conversation` (SCN-010)
5. `presales-solution-consulting` (SCN-011)
6. `customer-success-renewals` (SCN-012)
7. `gcc-escalation` (SCN-013)
8. `banking-loan-advisory` (SCN-014)
9. `negotiation-simulation` (SCN-015)
10. `procurement-negotiation` (SCN-016)
11. `consulting-client-expectation` (SCN-017)
12. `leading-through-change` (SCN-018)
13. `business-communication` (SCN-019)
14. `hospitality-guest-dispute` (SCN-020)
15. `sales-product-knowledge` (SCN-021)
16. `telecom-enterprise-renewal` (SCN-022)
17. `real-estate-objection` (SCN-023)
18. `quality-compliance` (SCN-024)
19. `accountability-under-pressure` (SCN-025) — **done** (replaced gallery slug `government-regulatory`)

**Shipped:** SCN-001 → `design-build-gcc-norway` (Wave 0 client request; not from gallery queue).

---

## Catalog (all gallery scenarios)

Gallery rows from `scenarios.ts`. **Live** modules also include four slugs not listed here (Vikram trilogy, Flowbridge).

| ID | Slug | Gallery title | Function | Wave | Intake | Status |
|----|------|---------------|----------|------|--------|--------|
| SCN-001 | `design-build-gcc-norway` | The Nordic Standard | Revenue & Commercial | — | Corporate | **done** |
| SCN-003 | `difficult-feedback` | Giving Difficult Feedback | Leadership & People | 1 | Corporate | **done** |
| SCN-004 | `fmcg-distributor-negotiation` | The Promise | Revenue & Commercial | 1 | Corporate | **done** |
| SCN-005 | `patient-communication` | Patient Communication (Medical) | Academic | 1 | Education | **ready** (SME early next week) |
| SCN-006 | `client-counselling-law` | Eighteen Lakhs, No Reply | Academic | 1 | Education | **done** |
| SCN-026 | `divorce-custody-counselling` | What's Fair Here | Academic | 1 | Education | **done** |
| SCN-007 | `sales-objection-handling` | Sales: Objection Handling & Discovery | Revenue & Commercial | 2 | Corporate | backlog |
| SCN-008 | `customer-support-contact-centre` | Customer Support & Service Recovery | Operations & Support | 2 | Corporate | backlog |
| SCN-009 | `coaching-direct-reports` | Coaching Direct Reports | Leadership & People | 2 | Corporate | backlog |
| SCN-010 | `performance-conversation` | Performance Conversations (PIP) | Leadership & People | 2 | Corporate | backlog |
| SCN-011 | `presales-solution-consulting` | Pre-sales & Solution Consulting | Revenue & Commercial | 2 | Corporate | backlog |
| SCN-012 | `customer-success-renewals` | The Escalation | Revenue & Commercial | 2 | Corporate | **done** |
| SCN-013 | `gcc-escalation` | GCC: Escalation Management | Leadership & People | 3 | Corporate | backlog |
| SCN-014 | `banking-loan-advisory` | Retail Banking: Loan Advisory | Operations & Support | 3 | Corporate | backlog |
| SCN-015 | `negotiation-simulation` | Negotiation Simulation | Academic | 3 | Both | backlog |
| SCN-016 | `procurement-negotiation` | Purchase & Procurement Negotiation | Operations & Support | 3 | Both | backlog |
| SCN-017 | `consulting-client-expectation` | Consulting: Managing Client Expectations | Leadership & People | 3 | Both | backlog |
| SCN-018 | `leading-through-change` | Leading Through Change | Leadership & People | 3 | Corporate | backlog |
| SCN-019 | `business-communication` | Business Communication Practice | Academic | 4 | Education | backlog |
| SCN-020 | `hospitality-guest-dispute` | Hospitality: Guest Dispute Resolution | Operations & Support | 4 | Both | backlog |
| SCN-021 | `sales-product-knowledge` | Sales: Product Knowledge in Context | Revenue & Commercial | 5 | Corporate | done |
| SCN-022 | `telecom-enterprise-renewal` | Telecom: Enterprise Renewal | Revenue & Commercial | 5 | Corporate | backlog |
| SCN-023 | `real-estate-objection` | Real Estate: High-Ticket Objection | Revenue & Commercial | 5 | Corporate | backlog |
| SCN-024 | `quality-compliance` | Quality & Compliance Conversations | Operations & Support | 5 | Corporate | backlog |
| SCN-025 | `accountability-under-pressure` | Downstream | Academic | 5 | Education | **done** |

**Default effort per module:** **M** (3–5 eng/content days). Education modules with domain sensitivity (law, medical) may need SME calendar time — note in `BACKLOG.md` when scoped.

---

## Per-module acceptance (repeat for each SCN)

Same pattern as [SCN-001 in BACKLOG.md](../../BACKLOG.md#scn-001--spacematrix-client-solutions-gcc-setup):

- [ ] 20-min brief confirmed: situation, learning objectives, target audience, open field
- [ ] Claude builds full module: `persona.ts`, `index.ts`, `preread.md`, registry, all docs
- [ ] `npm run validate:scenarios` → `npm run seed:scenarios`
- [ ] Play-test: pre-read → sim → debrief (specialist scores on /internal/reports) — SME sign-off
- [ ] Gallery card copy confirmed in `scenarios.ts` — slug matches shipped `moduleId`

Expand item-specific detail blocks in this file (or in `BACKLOG.md`) only when an item moves to `ready`.

---

## Changelog

| Date | Change |
|------|--------|
| 2026-07-16 | SCN-025 shipped — `accountability-under-pressure` published (`Downstream`); replaced gallery slug `government-regulatory`; 12 live · 8 gallery-playable. |
| 2026-07-16 | SCN-026 shipped — `divorce-custody-counselling` published (`What's Fair Here`); 11 live · 7 gallery-playable. |
| 2026-07-16 | SCN-012 shipped — `customer-success-renewals` published (`The Escalation`); 10 live · 6 gallery-playable. |
| 2026-07-16 | SCN-006 shipped — `client-counselling-law` published (`Eighteen Lakhs, No Reply`); 9 live · 5 gallery-playable. |
| 2026-07-16 | SCN-021 shipped — `sales-product-knowledge` published (`Outcomes Not Specs`); 8 live · 4 gallery-playable. |
| 2026-07-10 | Documented unseeded funnel: `ScenarioUnavailable` re-engineering screen (not 404). |
| 2026-07-10 | SCN-004 SME sign-off complete. SCN-005/006 → ready (assigned; intake early next week). |
| 2026-06-21 | Initial catalog from microsite `scenarios.ts`. Waves 1–5 by market demand. SCN-GAL deferred (gallery sync intentional). |
| 2026-06-24 | SCN-003 `in_progress` — SME intake initiated. |
| 2026-07-09 | SCN-004 shipped — `fmcg-distributor-negotiation` published (`The Promise`); 7 live · 3 gallery-playable. |
| 2026-06-24 | SCN-003 shipped — `difficult-feedback` (`The Green RAG`); 6 live · 2 gallery-playable. |

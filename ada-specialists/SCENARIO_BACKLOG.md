# Scenario content backlog

*Last updated: 2026-07-22. Ranked catalog of modules to build from the microsite gallery.*

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
| **Live (seeded)** | 32 | Vikram trilogy + Flowbridge + all gallery modules except aspirational four — see [registry.ts](./registry.ts) |
| **On `/demos` gallery** | ~31 | All entries in `scenarios.ts` |
| **Gallery + playable** | ~28 | Every gallery card except `patient-communication`, `presales-solution-consulting`, `negotiation-simulation`, `business-communication` |
| **Live but not on gallery** *(intentional)* | 4 | Vikram trilogy + Flowbridge — featured via homepage/corporate/education CTAs |
| **Aspirational (gallery only)** | 4 | `patient-communication`, `presales-solution-consulting`, `negotiation-simulation`, `business-communication` |

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
| GCC India ~2M professionals; upskilling = primary retention lever | `gcc-cross-cultural-escalation-aaa`, consulting pushback |
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
| SCN-007 | `sales-objection-handling-spin` | Sales: Objection Handling & Discovery |
| SCN-008 | `customer-support-service-recovery-leap` | Customer Support & Service Recovery |
| SCN-009 | `coaching-direct-reports-grow` | Coaching Direct Reports |
| SCN-010 | `performance-conversation-pip-sbi` | Performance Conversations (PIP) |
| SCN-011 | `presales-solution-consulting` | Pre-sales & Solution Consulting |
| SCN-012 | `customer-success-renewals` | The Escalation |

### Wave 3 — Tier B: India strategic sectors (GCC + BFSI + negotiation)

| ID | Slug | Title |
|----|------|-------|
| SCN-013 | `gcc-cross-cultural-escalation-aaa` | GCC: Escalation Management |
| SCN-014 | `banking-loan-advisory-spin` | Retail Banking: Loan Advisory |
| SCN-015 | `negotiation-simulation` | Negotiation Simulation |
| SCN-016 | `procurement-negotiation-interests` | Purchase & Procurement Negotiation |
| SCN-017 | `consulting-client-pushback-state` | Consulting: Managing Client Expectations |
| SCN-018 | `change-communication-disagree-commit` | Leading Through Change |

### Wave 4 — Tier C: education breadth + hospitality skills gap

| ID | Slug | Title |
|----|------|-------|
| SCN-019 | `business-communication` | Business Communication Practice |
| SCN-020 | `hospitality-guest-dispute-leap` | Hospitality: Guest Dispute Resolution |

### Wave 5 — Tier D: vertical niches (build on client pull)

| ID | Slug | Title |
|----|------|-------|
| SCN-021 | `sales-product-knowledge` | Sales: Product Knowledge in Context |
| SCN-022 | `telecom-enterprise-renewal-challenger` | Telecom: Enterprise Renewal |
| SCN-023 | `real-estate-stall-commitment-spin` | Real Estate: High-Ticket Objection |
| SCN-024 | `quality-compliance-conversation-sbi` | Quality & Compliance Conversations |
| SCN-025 | `accountability-under-pressure` | Downstream (was Government & Regulatory) |
| SCN-027 | `quiet-exit-retention` | The Quiet Exit (OB/HRM retention · Positions vs Interests) |
| SCN-028 | `hospitality-fb-service-recovery-latte` | The Midnight Ticket |
| SCN-029 | `hospitality-loyalty-recognition-appreciative-inquiry` | The Fourteenth Stay |
| SCN-030 | `gov-regulatory-influence-without-authority` | The Right Desk |
| SCN-031 | `nursing-communication-adverse-candor` | The Ten Minutes |
| SCN-032 | `clinical-rapport-first-consult` | The First Session |
| SCN-033 | `hr-difficult-conversations` | The Unnamed Source |

---

## Full priority stack

Single pick order after Wave 1 (unless client-first override). **Done rows** marked — remaining backlog: SCN-005 (ready), SCN-011, SCN-015, SCN-019.

1. `sales-objection-handling-spin` (SCN-007) — **done**
2. `customer-support-service-recovery-leap` (SCN-008) — **done**
3. `coaching-direct-reports-grow` (SCN-009) — **done**
4. `performance-conversation-pip-sbi` (SCN-010) — **done**
5. `presales-solution-consulting` (SCN-011) — backlog
6. `customer-success-renewals` (SCN-012) — **done**
7. `gcc-cross-cultural-escalation-aaa` (SCN-013) — **done**
8. `banking-loan-advisory-spin` (SCN-014) — **done**
9. `negotiation-simulation` (SCN-015) — backlog
10. `procurement-negotiation-interests` (SCN-016) — **done**
11. `consulting-client-pushback-state` (SCN-017) — **done**
12. `change-communication-disagree-commit` (SCN-018) — **done**
13. `business-communication` (SCN-019) — backlog
14. `hospitality-guest-dispute-leap` (SCN-020) — **done**
15. `sales-product-knowledge` (SCN-021) — **done**
16. `telecom-enterprise-renewal-challenger` (SCN-022) — **done**
17. `real-estate-stall-commitment-spin` (SCN-023) — **done**
18. `quality-compliance-conversation-sbi` (SCN-024) — **done**
19. `accountability-under-pressure` (SCN-025) — **done** (replaced gallery slug `government-regulatory`)
20. `quiet-exit-retention` (SCN-027) — **done** (OB/HRM retention; Positions vs Interests)
21. `hospitality-fb-service-recovery-latte` (SCN-028) — **done** (*The Midnight Ticket*)
22. `hospitality-loyalty-recognition-appreciative-inquiry` (SCN-029) — **done** (*The Fourteenth Stay*)
23. `gov-regulatory-influence-without-authority` (SCN-030) — **done** (*The Right Desk*)
24. `nursing-communication-adverse-candor` (SCN-031) — **done** (*The Ten Minutes*)
25. `clinical-rapport-first-consult` (SCN-032) — **done** (*The First Session*)
26. `hr-difficult-conversations` (SCN-033) — **done** (*The Unnamed Source*)

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
| SCN-007 | `sales-objection-handling-spin` | Sales: Objection Handling & Discovery | Revenue & Commercial | 2 | Corporate | **done** |
| SCN-008 | `customer-support-service-recovery-leap` | Customer Support & Service Recovery | Operations & Support | 2 | Corporate | **done** |
| SCN-009 | `coaching-direct-reports-grow` | Coaching Direct Reports | Leadership & People | 2 | Corporate | **done** |
| SCN-010 | `performance-conversation-pip-sbi` | Performance Conversations (PIP) | Leadership & People | 2 | Corporate | **done** |
| SCN-011 | `presales-solution-consulting` | Pre-sales & Solution Consulting | Revenue & Commercial | 2 | Corporate | backlog |
| SCN-012 | `customer-success-renewals` | The Escalation | Revenue & Commercial | 2 | Corporate | **done** |
| SCN-013 | `gcc-cross-cultural-escalation-aaa` | GCC: Escalation Management | Leadership & People | 3 | Corporate | **done** |
| SCN-014 | `banking-loan-advisory-spin` | Retail Banking: Loan Advisory | Operations & Support | 3 | Corporate | **done** |
| SCN-015 | `negotiation-simulation` | Negotiation Simulation | Academic | 3 | Both | backlog |
| SCN-016 | `procurement-negotiation-interests` | Purchase & Procurement Negotiation | Operations & Support | 3 | Both | **done** |
| SCN-017 | `consulting-client-pushback-state` | Consulting: Managing Client Expectations | Leadership & People | 3 | Both | **done** |
| SCN-018 | `change-communication-disagree-commit` | Leading Through Change | Leadership & People | 3 | Corporate | **done** |
| SCN-019 | `business-communication` | Business Communication Practice | Academic | 4 | Education | backlog |
| SCN-020 | `hospitality-guest-dispute-leap` | Hospitality: Guest Dispute Resolution | Operations & Support | 4 | Both | **done** |
| SCN-021 | `sales-product-knowledge` | Sales: Product Knowledge in Context | Revenue & Commercial | 5 | Corporate | done |
| SCN-022 | `telecom-enterprise-renewal-challenger` | Telecom: Enterprise Renewal | Revenue & Commercial | 5 | Corporate | **done** |
| SCN-023 | `real-estate-stall-commitment-spin` | Real Estate: High-Ticket Objection | Revenue & Commercial | 5 | Corporate | **done** |
| SCN-024 | `quality-compliance-conversation-sbi` | Quality & Compliance Conversations | Operations & Support | 5 | Corporate | **done** |
| SCN-025 | `accountability-under-pressure` | Downstream | Academic | 5 | Education | **done** |
| SCN-027 | `quiet-exit-retention` | The Quiet Exit | Leadership & People | — | Education | **done** |
| SCN-028 | `hospitality-fb-service-recovery-latte` | The Midnight Ticket | Operations & Support | — | Education | **done** |
| SCN-029 | `hospitality-loyalty-recognition-appreciative-inquiry` | The Fourteenth Stay | Operations & Support | — | Education | **done** |
| SCN-030 | `gov-regulatory-influence-without-authority` | The Right Desk | Leadership & People | — | Corporate | **done** |
| SCN-031 | `nursing-communication-adverse-candor` | The Ten Minutes | Academic | — | Education | **done** |
| SCN-032 | `clinical-rapport-first-consult` | The First Session | Academic | — | Education | **done** |
| SCN-033 | `hr-difficult-conversations` | The Unnamed Source | Leadership & People | — | Education | **done** |

**Default effort per module:** **M** (3–5 eng/content days). Education modules with domain sensitivity (law, medical) may need SME calendar time — note in `BACKLOG.md` when scoped.

---

## Per-module acceptance (repeat for each SCN)

Same pattern as [SCN-001 in BACKLOG.md](../../BACKLOG.md#scn-001--spacematrix-client-solutions-gcc-setup):

- [ ] 20-min brief confirmed: situation, learning objectives, target audience, open field
- [ ] Claude builds full module: `persona.ts`, `index.ts`, `preread.md`, registry, all docs
- [ ] Build as `status: "draft"` until publish proposal passes
- [ ] **New module publish gate:** `npm run validate:scenarios` with **0 warnings** on this `moduleId` (not in `LEGACY_WARN_GRANDFATHER_IDS`) · `npm test` · probe row · staging seed
- [ ] Play-test: pre-read → sim → debrief (specialist scores on /internal/reports) — SME sign-off
- [ ] Flip `status: "published"` → prod seed
- [ ] Gallery card copy confirmed in `scenarios.ts` — slug matches shipped `moduleId`

**Existing live modules:** track hardening in [BACKLOG.md SCN-HARDEN](../../BACKLOG.md#content--scenario-hardening-live-modules) — grandfathered warnings until row is `done`.

Expand item-specific detail blocks in this file (or in `BACKLOG.md`) only when an item moves to `ready`.

---

## Changelog

| Date | Change |
|------|--------|
| 2026-07-22 | Intake build — 19 modules shipped with new slugs (SCN-007–010, SCN-013–014, SCN-016–018, SCN-020, SCN-022–024, SCN-028–033). Plan B gate green (audit 0 warn · probe 19/19 · seed). 32 live · 32 gallery · 4 aspirational (`patient-communication`, `presales-solution-consulting`, `negotiation-simulation`, `business-communication`). |
| 2026-07-16 | SCN-027 shipped — `quiet-exit-retention` published (`The Quiet Exit`); OB/HRM Positions vs Interests; 13 live · 9 gallery-playable. |
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

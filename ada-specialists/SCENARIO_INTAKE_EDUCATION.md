# Scenario Intake Guide — Higher Education

Use this checklist when gathering information from **faculty, programme directors, or accreditation leads** before building an Altius simulation for a college or university cohort.

For the shared intake fields (persona, scene, pre-read, rubric structure), start with [SCENARIO_INTAKE.md](./SCENARIO_INTAKE.md). This document adds **education-specific** questions that the corporate intake guide does not cover.

Each scenario is still **self-contained** — the same four artifacts apply:

1. **`persona.ts`** — who the counterpart is and how they speak  
2. **`scenarioPrompt`** — scene setup and scenario-specific behavior rules  
3. **`preread.md`** — student briefing (case-style, ends where chat begins)  
4. **`evalRubric`** — post-session scoring (maps to Course Outcomes)

---

## When to use this guide

| Audience | Use |
| :--- | :--- |
| **B-school / BBA / BMS** | Sales, negotiation, communication, HRM, entrepreneurship courses |
| **Law schools** | Client counselling, ADR, mediation prep |
| **Medical / nursing** | Patient communication, breaking bad news, history-taking |
| **Hotel management** | Guest relations, complaint handling, service recovery |
| **Engineering (soft skills)** | Professional communication, stakeholder management |

If the buyer is an **L&D or HR team inside a company**, use [SCENARIO_INTAKE.md](./SCENARIO_INTAKE.md) instead.

---

## 1. Pedagogical framing (Required)

Altius in higher education is designed as: **the simulation is the assignment; the debrief is the assessment.**

| Field | Question to ask | Example |
| :--- | :--- | :--- |
| **Course name & code** | Which course will use this simulation? | *Sales Management — MKT402* |
| **Programme level** | UG, PG, executive? | *MBA Year 1, core elective* |
| **Course Outcome(s) to assess** | Which stated COs should this simulation evidence? Copy verbatim from the syllabus. | *"CO3: Conduct a consultative sales conversation that uncovers customer needs and handles objections."* |
| **Assessment weight** | Formative practice only, or summative grade component? | *15% of course grade; one attempt; highest score counts* |
| **Prerequisite knowledge** | What should students have read or covered in class first? | *SPIN framework lecture; Gulab & Glow case discussion* |
| **What "good" looks like in this course** | How does the faculty member describe an A-grade performance? | *Probes before pitching; cites evidence from the briefing; respects time pressure* |

Collect **2–4 CO statements** even if only one simulation will run — rubric criteria should trace back to them (see [ACCREDITATION_EVIDENCE.md](../../ACCREDITATION_EVIDENCE.md)).

---

## 2. Cohort and faculty workflow (Required)

| Field | Question to ask | Notes |
| :--- | :--- | :--- |
| **Cohort size** | How many students in the batch? | Drives pilot vs full rollout planning |
| **Timeline** | When must students complete by? | Align with midterm, accreditation visit, placement season |
| **Faculty reviewer** | Who reads debriefs and cohort patterns? | Usually course coordinator or programme director |
| **Retakes** | One attempt or multiple? | Platform default: admin/specialist reset only — confirm policy |
| **Integration with LMS** | Must scores flow to Moodle/Canvas? | Not built-in today; plan manual export or CSV (Phase 2+) |
| **Language** | English only, or Hindi/regional later? | Set `locale` on module; non-English personas need explicit SME review |

Faculty often care about **bandwidth**: confirm they are not expected to run live role-plays — Altius replaces that with private, simultaneous practice.

---

## 3. Case-method bridge (Recommended)

Many HEI scenarios replace or extend a **case discussion**. Capture how this simulation relates:

| Field | Question to ask |
| :--- | :--- |
| **Existing case** | Is there a case study students already read? Can we reuse facts and names? |
| **What the case cannot do** | What pushback or adaptation does the case lack? |
| **Briefing style** | Traditional case PDF, or in-world memo (Slack thread, clinic handoff)? |
| **After the simulation** | Class debrief, written reflection, group presentation? |

**Reference:** [flowbridge-discovery](../modules/flowbridge-discovery/) — students read a B2B discovery briefing, then chat with a CPO mid-crisis. The briefing is the case; the conversation is where they apply it.

---

## 4. Domain-specific sensitivities

### Law — client counselling / ADR

| Topic | Collect from faculty |
| :--- | :--- |
| **Jurisdiction** | Which laws, courts, or procedural rules apply? Use fictionalised names if needed. |
| **Plain language** | What terms must the student translate for a lay client? |
| **Professional boundaries** | What must the student **not** promise (e.g. guaranteed outcome, "you won't go to jail")? |
| **Emotional baseline** | Anxious first-time client? Angry disputant? Mediation-neutral? |
| **Forbidden advice** | Specific legal conclusions the student must avoid stating prematurely |

**Evaluator lens example:** *"experienced clinical legal education supervisor"*

**Sample criteria:** rapport and trust; systematic fact-gathering; plain-language explanation; expectation management.

### Medical / nursing — patient communication

| Topic | Collect from faculty |
| :--- | :--- |
| **Clinical setting** | OPD, ward round, teleconsult, breaking bad news? |
| **Patient profile** | Literacy, denial, family present, language preference |
| **Honesty vs alarm** | How direct should the student be for this scenario? |
| **Scope of practice** | Student is intern / nurse / doctor — what can they say and do? |
| **Red lines** | No diagnosis beyond brief; escalate to senior; no treatment promises |

**Evaluator lens example:** *"experienced communication-skills facilitator in medical education"*

**Sample criteria:** empathy and rapport; clarity of explanation; checking understanding; appropriate next steps.

### Hospitality — guest relations

| Topic | Collect from faculty |
| :--- | :--- |
| **Touchpoint** | Front desk, F&B, duty manager, loyalty dispute |
| **Guest type** | Business traveller, family, loyalty member, social-media threat |
| **Recovery goal** | Retain guest, de-escalate, upsell, policy-bound refusal |
| **Brand voice** | Formal luxury vs practical mid-scale |

### B-school — discovery, negotiation, feedback

Use the standard [SCENARIO_INTAKE.md](./SCENARIO_INTAKE.md) sections. **Reference implementations:**

- **B2B discovery:** [flowbridge-discovery](../modules/flowbridge-discovery/)
- **High-stakes sales / retention:** [vikram-sales](../modules/vikram-sales/), [vikram-parts](../modules/vikram-parts/), [vikram-service](../modules/vikram-service/)

---

## 5. Evaluation rubric — CO alignment (Required)

For each **Course Outcome**, map at least one rubric criterion.

| CO (from syllabus) | Rubric criterion name | What "good" looks like (observable) |
| :--- | :--- | :--- |
| CO3: Consultative discovery | Needs Discovery | Probes root cause before solution |
| CO3: Consultative discovery | Crisis-Appropriate Tone | Acknowledges context; does not pitch prematurely |
| CO4: Professional communication | Stakeholder Awareness | Explores decision-makers and stakes |

Rules (same as corporate intake):

- **3–5 criteria**, each **1–5** scale  
- **Observable descriptions** — evaluator cites transcript moments  
- **`evaluatorRole`** matched to discipline (not generic "coach" unless appropriate)  
- **`overallGuidance`** bands aligned to total score (e.g. 16+/20)

Document the CO mapping in the intake handoff — it feeds accreditation reporting (see [ACCREDITATION_EVIDENCE.md](../../ACCREDITATION_EVIDENCE.md)).

---

## 6. Compliance and ethics (Required for HEI)

| Topic | Question to ask |
| :--- | :--- |
| **Real vs fictional** | Real company names, hospitals, or clients allowed? |
| **Student data** | Institution's data-processing agreement; who can see transcripts? |
| **Sensitive topics** | Topics to avoid (self-harm, communal, political) |
| **Accreditation visit** | Will artefacts from this cohort be shown to NAAC/NBA reviewers? |
| **IRB / ethics committee** | Required for patient or client scenarios at this institution? |

---

## 7. Education intake deliverables checklist

### What faculty must confirm before build starts

Confirm the four items in [SCENARIO_INTAKE.md §12](./SCENARIO_INTAKE.md#12-intake-deliverables-checklist) **plus**:

- [ ] Course name, code, and verbatim CO statement(s) to assess  
- [ ] Assessment weight and retake policy  
- [ ] Cohort size and completion deadline  
- [ ] Faculty reviewer contact  
- [ ] Domain sensitivity notes (law / medical / hospitality as applicable)  
- [ ] Publish status: `"draft"` for pilot, `"published"` for cohort

### What Claude handles (no faculty time)

- [ ] CO → rubric criterion mapping (derived from CO statements and learning objectives)
- [ ] Case-to-simulation bridge, pre-read prose, persona, all module code, all doc updates

---

## 8. Intake interview script — education (30 min)

1. **5 min** — Course, COs, and where this simulation fits in the syllabus  
2. **5 min** — Situation and learner role: what is the conflict, who is in the room  
3. **5 min** — Learning objectives mapped to COs: 2–4 observable behaviors  
4. **5 min** — Target audience: year, programme, prerequisite knowledge  
5. **5 min** — Domain sensitivities: red lines, scope of practice, jurisdiction  
6. **5 min** — Anything else to consider: non-obvious counterpart details, real failure patterns, case material to reference  

After the call: Claude builds the full module. Faculty next touchpoint is the play-test — not another intake call.

---

## Related documents

- [SCENARIO_INTAKE.md](./SCENARIO_INTAKE.md) — Shared intake (persona, scene, pre-read, rubric)  
- [SCENARIO_MODULE_GUIDE.md](./SCENARIO_MODULE_GUIDE.md) — Implementation and seeding  
- [ACCREDITATION_EVIDENCE.md](../../ACCREDITATION_EVIDENCE.md) — CO/PO attainment and export artefacts  
- [altius_education_explainer.md](../../altius_education_explainer.md) — External-facing HEI narrative  

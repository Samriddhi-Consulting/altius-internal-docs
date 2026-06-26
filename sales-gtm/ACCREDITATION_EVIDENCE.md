# Accreditation Evidence — How Altius Maps to CO/PO Frameworks

This document describes **what evidence Altius produces today**, how it maps to accreditation and outcome-based education (OBE) frameworks, and **what is automated vs manual**. Use it internally for delivery and with faculty/IQAC leads in follow-up conversations.

**Sales narrative:** [altius_education_explainer.md](./altius_education_explainer.md)  
**Shared GTM blocks:** [altius_shared_blocks.md](./altius_shared_blocks.md)  
**Scenario design:** [SCENARIO_INTAKE_EDUCATION.md](./app/scenarios/SCENARIO_INTAKE_EDUCATION.md)

---

## The core insight

Accreditation panels often receive two weak categories of evidence for interpersonal competence:

1. **Exams and presentations** — rigorous but test knowing, not doing under pressure  
2. **Internships and certificates** — realistic but uncontrolled and hard to standardise  

Altius creates a **third category**: standardised, scored, transcript-backed assessment of a realistic professional conversation — with timestamps and per-criterion behaviour scores.

---

## What Altius generates per student (automated today)

When a learner completes a simulation (`maxTurns` reached), the platform:

1. Stores the full **message transcript** (`messages` table, chronological, timestamped)  
2. Runs an **LLM coaching evaluation** against the scenario's **`evalRubric`**  
3. Persists one **evaluation** row per session  

| Field | Source | Use in accreditation narrative |
| :--- | :--- | :--- |
| **Transcript** | All user + persona messages | Evidence of *doing*; auditable interaction log |
| **Per-criterion scores** | `evaluations.scores` (JSON, keys = rubric criterion names) | Direct assessment of stated behaviours |
| **Overall score** | `evaluations.overallScore` | Summative grade input or attainment threshold |
| **Summary** | `evaluations.summary` | Qualitative assessment paragraph |
| **Strengths / improvements** | `evaluations.strengths`, `evaluations.improvements` | Formative feedback; moderation samples |
| **Session timestamps** | `sessions.startedAt`, `sessions.completedAt` | Experiential learning window |
| **Scenario metadata** | `scenarios.title`, `evalRubric` | Links activity to course and rubric design |

Learners see this on **`/debrief/[sessionId]`** immediately after completion.

---

## Mapping rubric criteria to Course Outcomes (COs)

Altius does **not** auto-import syllabus CO codes from an LMS. Mapping is a **design-time and reporting-time** step:

### At scenario design (recommended)

During intake, document a table like:

| Course Outcome (syllabus) | Rubric criterion | Scale |
| :--- | :--- | :--- |
| CO3: Conduct consultative discovery | Needs Discovery | 1–5 |
| CO3: Conduct consultative discovery | Probing Depth | 1–5 |
| CO4: Professional communication | Crisis-Appropriate Tone | 1–5 |

Store this in the project file or handoff doc — see [SCENARIO_INTAKE_EDUCATION.md](./app/scenarios/SCENARIO_INTAKE_EDUCATION.md) §5.

**Example (live scenario):** FlowBridge B2B discovery maps criteria such as *Crisis-Appropriate Tone*, *Needs Discovery*, *Probing Depth*, and *Stakeholder Awareness* to discovery-focused COs.

### At attainment calculation (manual or spreadsheet today)

Standard OBE formula:

```
CO attainment (%) = (Number of students scoring ≥ threshold on mapped criteria) / (Total students) × 100
```

Typical practice:

- **Threshold:** e.g. ≥3/5 on each mapped criterion, or ≥12/20 total  
- **Aggregation:** Per criterion or per CO (if one CO maps to multiple criteria, define rule: all must pass vs average)  
- **Evidence bundle:** Export scores + transcript samples for moderators  

**Not automated in app today:** CO code labels on criteria, threshold configuration UI, or auto-calculated attainment percentages.

---

## Framework mapping (what to tell accreditation leads)

Use framework names to signal fit; offer a **programme-specific walkthrough** for clause-level mapping.

| Framework | Relevant scope | What Altius provides | What you still document manually |
| :--- | :--- | :--- | :--- |
| **NAAC** | Criterion 2 (Teaching-Learning), Criterion 5 (Student Support) | Innovative pedagogy; experiential activity with outcomes; timestamped artefacts | Link to SSR sections; faculty moderation records |
| **NBA** | COs, POs, attainment | Per-student criterion scores; cohort-level export (Phase 2+) | CO–PO matrix; attainment worksheets |
| **NEP 2020** | OBE, experiential learning | Measurable practice of professional competence | Programme-level narrative |
| **AACSB** | Assurance of Learning (AoL) | Direct assessment of learning goals via simulation + rubric | Goal rubric crosswalk; sampling plan |
| **EQUIS** | Practical relevance | Industry-realistic scenarios | Industry advisory evidence |
| **AMBA** | Programme innovation | AI-powered practice simulation | Innovation narrative in self-study |

**Follow-up deliverable for meetings:** A one-page **CO/PO crosswalk** for the client's syllabus, plus a **sample anonymised debrief** and **sample cohort score summary**.

---

## Cohort and faculty views (roadmap vs today)

| Capability | Status | Notes |
| :--- | :--- | :--- |
| Individual learner debrief | **Live** | Learner-facing `/debrief/[sessionId]` |
| Individual transcript + scores for faculty | **Live** | `/internal/reports` (ada_specialist / super_admin) |
| "32/58 below 3 on Probing Depth" cohort view | **Phase 2 / 5** | Cohort analytics in [ada_platform_thinking.md](./ada_platform_thinking.md) |
| CSV export of all scores | **Phase 2** | Planned `/api/admin/export` |
| Executive / accreditation PDF summary | **Phase 5** | One-page cohort summary |
| CO attainment dashboard | **Not built** | Manual spreadsheet from CSV export |

When speaking to faculty, be explicit: **learners see completion + HR contact only**; **faculty/specialists see criterion scores on `/internal/reports`**; **programme-level aggregation and export remain on the roadmap** (Phase 2+).

---

## Suggested accreditation evidence bundle (per cohort)

Prepare this for IQAC or external review:

1. **Scenario brief** — `preread.md` or PDF export of pre-read  
2. **Assessment rubric** — criterion names, descriptions, scales (from `evalRubric`)  
3. **CO mapping table** — syllabus COs ↔ rubric criteria  
4. **Cohort completion list** — student identifier, completed date, overall score  
5. **Score detail export** — per-student per-criterion scores (CSV when Phase 2 live; SQL/report until then)  
6. **Sample transcripts** — 2–3 anonymised full conversations with debriefs  
7. **Moderation note** — faculty sign-off that rubric and thresholds were applied consistently  

---

## Data model reference

Relevant tables (see [ada_developer_build_guide.md](./ada_developer_build_guide.md) §6):

```
sessions     → user_id, scenario_id, status, started_at, completed_at
messages     → session_id, role, content, created_at
evaluations  → session_id, scores (JSON), summary, strengths, improvements, overall_score
scenarios    → title, eval_rubric (JSON), pre_read_markdown
orgs         → institution isolation via org_id on users
```

**Org isolation:** Enterprise and HEI clients should have learners under an `org_id` so only their administrators see their data.

---

## Evaluation engine (technical summary)

- Rubric is **per scenario** (`scenarios.eval_rubric` JSON), not global  
- After final chat turn, `evaluateSession()` sends transcript + rubric to Groq  
- Output: structured scores keyed by criterion **names** from the rubric  
- See `app/src/lib/llm/evaluation.ts` and [SCENARIO_MODULE_GUIDE.md](./app/scenarios/SCENARIO_MODULE_GUIDE.md) Step 4  

Changing criteria for a new course = new or updated scenario module + seed — not a code change to the evaluator.

---

## Related documents

- [SCENARIO_INTAKE_EDUCATION.md](./app/scenarios/SCENARIO_INTAKE_EDUCATION.md) — CO alignment at intake  
- [altius_education_explainer.md](./altius_education_explainer.md) — External HEI narrative  
- [altius_shared_blocks.md](./altius_shared_blocks.md) — Shared proof points  
- [ada_platform_thinking.md](./ada_platform_thinking.md) — Admin analytics phasing  
- [ada_developer_build_guide.md](./ada_developer_build_guide.md) — Phase 2 admin export, schema  
- [DOCS_INDEX.md](./DOCS_INDEX.md) — documentation map  

# Scenario Intake Guide

Use this checklist when gathering information from a subject-matter expert (SME), client stakeholder, or training designer before building an Altius simulation.

**Higher education (faculty, programme directors, IQAC):** also use [SCENARIO_INTAKE_EDUCATION.md](./SCENARIO_INTAKE_EDUCATION.md) for Course Outcome alignment, cohort workflow, and law/medical/hospitality sensitivities.

Each scenario is **self-contained**: the counterparty persona, the conversation setup, the pre-read briefing, and the evaluation rubric all belong to that one scenario. If the same customer appears in multiple scenarios, you may repeat persona details in each — that duplication is fine when scenarios need independent tuning.

The goal is to collect enough detail to produce four co-located artifacts:

1. **`persona.ts`** — who the customer is and how they speak  
2. **`scenarioPrompt`** (in `index.ts`) — scene setup and scenario-specific behavior rules  
3. **`preread.md`** — learner briefing  
4. **`evalRubric`** (in `index.ts`) — post-session scoring  

The minimum from the SME is the four items in **Two tiers of input** below — 20 minutes. Sections 1–9 are authoring reference for when you need to go deeper; not all of them require SME input.

---

## Two tiers of input

Every scenario has two distinct sources. Keeping them separate prevents SME overload and lets you deliver a fully drafted module for review — not a blank questionnaire.

| Tier | Source | What it covers |
|------|--------|----------------|
| **Tier 1 — Claude derives** | No SME time | Names, company names, `voicePrompt` prose, behavior rules, traps, `firstMessage`, pre-read narrative, rubric descriptions, scoring bands, metadata defaults, all module code, all doc updates |
| **Tier 2 — SME or client must provide** | 20 min | Situation concept, learning objectives, target audience, and anything else to consider (open field) |

### Minimum viable brief

Four items. Claude derives everything else — counterparty archetype, hidden layer, domain facts, failure modes, all prose, all code.

**1. The situation** (2–3 sentences): Who is the learner? Who is the counterparty? What is the conflict or pressure point?

**2. Learning objectives** (2–4 bullets): What observable behaviors should improve? State as actions — "name specific behaviors with dates and impact using SBI," not "understand how to give feedback."

**3. Target audience**: Role, years of experience, industry context. Calibrates difficulty and pre-read depth.

**4. Anything else to consider** (open — optional): Non-obvious counterparty details, real failure patterns the SME has seen in practice, org-specific policies or constraints, sensitivities to avoid. If there is nothing to add, leave blank. Claude researches and derives what is not here.

Once confirmed, the build begins. The SME's next touchpoint is the **play-test** — running 2–3 conversations on the finished module and reviewing the debrief scores. That is where specialist input has the highest impact per minute.

---

## 1. Scenario overview (Required)

| Field | Question to ask | Example |
|-------|-----------------|---------|
| **Working title** | What should learners see on the home screen? | *The Falcon Ultimatum* |
| **One-line pitch** | What is this simulation about in one sentence? | *Win a 50-truck fleet deal against a low-cost competitor.* |
| **Learner role** | What job title is the participant playing? | *Area Sales Manager* |
| **Industry / domain** | What business context applies? | *Commercial vehicle OEM, fleet sales* |
| **Learning objective(s)** | What should the learner be able to do after this exercise? List 2–4 observable behaviors. | *Reframe price to TCO; handle legacy trust issues; protect dealer service revenue.* |
| **Difficulty** | Introductory, intermediate, or advanced? | *Intermediate — requires math and multi-stakeholder trade-offs* |
| **Estimated duration** | How long should pre-read + conversation take? | *Pre-read 15–20 min; conversation 15–25 min* |
| **Target audience** | Who will take this? (role, tenure, region) | *Regional sales managers, 1–5 years experience, India corridor* |

---

## 2. The moment of truth (Reference depth)

Altius simulations open **in medias res** — the learner joins a live conversation that has already started. This material becomes **`scenarioPrompt`** (scene layer) and **`firstMessage`**.

| Field | Question to ask | Notes |
|-------|-----------------|-------|
| **Setting** | Where are the two people? | *Boardroom at Falcon Freight HQ, 9 AM* |
| **Stakes** | What happens if the learner fails? What if they succeed? | *Lose 50-unit order to Titan; hit quarterly target if won* |
| **Time pressure** | Is there a deadline or ticking clock? | *Decision by Friday; trucks grounded costing $2,400/day* |
| **Opening line** | What does the customer say first? (Verbatim or close) | *"I'm a busy man. I have fifty trucks to buy…"* |
| **What the learner already knows** | Summarize context they should have read in the briefing. | *2019 failures, Titan quote, dealer constraints, TCO data* |
| **What the learner must figure out in conversation** | What should **not** be spelled out in the opening message? | *Root cause of DPF failure; willingness to accept cost-share* |

The opening line becomes `firstMessage`. Claude drafts it from the situation concept — urgent, specific, and impossible to answer with a single generic sentence. The SME confirms or adjusts; they do not need to supply it verbatim.

---

## 3. Customer persona (Reference depth)

The AI plays one counterparty. All persona material for this scenario goes in that scenario's **`persona.ts`** — not in a shared library.

### Identity → `persona.name`, `persona.role`, `persona.company`

*Tier 2 (collect from SME): role/title, decision power, tenure concept. Tier 1 (Claude derives): name, company name — invented to keep scenarios fictional.*

| Field | Source | Question to ask |
|-------|--------|-----------------|
| **Name** | Claude | *(invented — confirm if the client has a preference)* |
| **Title / role** | SME | Job title and scope of authority |
| **Company** | Claude | *(invented — confirm if a fictional name already exists)* |
| **Tenure** | SME | How long in role; relevant history with the learner's firm |
| **Decision power** | SME | Can they sign today, or are they influencing someone else? |

### Voice and baseline behavior → `persona.voicePrompt`

*Tier 1 — Claude writes the full `voicePrompt` from the counterparty archetype and hidden layer. The fields below are optional enrichment prompts; the SME does not need to answer each one explicitly.*

| Field | Use if available |
|-------|-----------------|
| **Communication style** | Short and blunt? Formal? Warm but skeptical? |
| **Backstory** | What shaped how they think and decide? |
| **Sample phrases** | *(Claude derives — validate if specific idioms matter)* |
| **Emotional baseline** | Default tone before scenario-specific mood |
| **What earns their respect** | Data? Empathy? Directness? Follow-through? |
| **What triggers pushback** | Fluff, lectures, premature discounts, blame-shifting? |
| **Reply patterns** | *(Claude derives from character archetype)* |

`voicePrompt` is the **stable character layer** — who they are and how they generally behave. It should not include scene-specific stakes (those go in `scenarioPrompt`).

### Hidden layer (Tier 1 — Claude derives; SME may enrich via open field)

| Field | Question to ask | Maps to |
|-------|-----------------|---------|
| **Surface demand** | What do they say they want? | `scenarioPrompt` |
| **Real concerns** | What are they actually worried about? | `voicePrompt` + `scenarioPrompt` |
| **Non-negotiables** | What will they **not** accept? | `scenarioPrompt` |
| **Acceptable concessions** | What can they agree to if the learner makes a strong case? | `scenarioPrompt` |
| **Softening triggers** | What evidence makes them listen? | `scenarioPrompt` |

---

## 4. Scenario-specific behavior rules (Tier 1 — Claude derives)

These become the **`scenarioPrompt`** in `index.ts`. Claude constructs the full set of if/then behavior rules, emotional arc, traps, and scene anchor from the brief. The prompts below are optional diagnostic questions to use if the brief feels thin; the SME does not supply behavior rules directly.

| What Claude writes | Optional SME prompt if brief is thin |
|--------------------|--------------------------------------|
| **Scene anchor** | Where exactly does this take place? What is the physical setup? |
| **Default posture** | How is the counterparty feeling going into this moment? |
| **When learner is vague** | *(derived from character archetype)* |
| **When learner is specific** | *(derived from character archetype)* |
| **Forbidden outcomes** | What must never happen, no matter how the learner plays it? |
| **Acceptable end states** | What does a good-enough close look like in this scene? |

Global guardrails (never break character, never mention AI) are added at seed time via `buildSystemPrompt()` — do not repeat them in intake. Reply-length discipline (`normally under 180 words`) lives only in runtime `prompt-assembly.ts` REPLY_FOOTER; see [SCENARIO_MODULE_GUIDE.md](./SCENARIO_MODULE_GUIDE.md) layer ownership.

---

## 5. Facts, numbers, and reference data (Recommended)

For generic scenarios, Claude researches plausible domain benchmarks and invents specific fictional figures within realistic ranges. For org-specific builds — where a client wants the scenario to reflect their actual policies, thresholds, or product specs — those numbers must come from the client via the open field.

| Category | Examples to collect |
|----------|---------------------|
| **Pricing** | List prices, competitor quotes, discount floors, margin rules |
| **Operational data** | Fuel consumption, km/year, downtime cost/day, PM intervals |
| **Fleet / HR data** | Driver turnover %, replacement cost, headcount |
| **Product specs** | Engine options, features, upgrade costs, warranty terms |
| **Incident history** | Dates, counts, failure modes, disputed blame |
| **Policy constraints** | What corporate or dealer will/won't allow |
| **Correct answers (internal)** | Expected TCO comparison, ROI, or recommended proposal — for authors only |

Provide numbers in a consistent currency and unit system. If the scenario spans multiple options (A / B / C), label them clearly.

---

## 6. Stakeholder map (Recommended)

Many scenarios involve people who are **not** in the room but shape constraints. These belong in **`preread.md`**, not as chat personas.

| Stakeholder | Role | Position / pressure on the learner |
|-------------|------|-----------------------------------|
| *National Sales Director* | Internal boss | *No margin-destroying discounts; close by Friday* |
| *Dealer principal* | Channel partner | *Protect service bay revenue; no open API* |
| *Competitor* | Off-screen threat | *Titan 20% under list; unreliable but cheap upfront* |

---

## 7. Pre-read briefing content (Reference depth)

The pre-read is narrative non-fiction that Claude writes from the facts, numbers, stakeholder map, and hidden layer confirmed in §3–§6. The SME does not draft the pre-read. The table below shows what source material each section draws on.

| Section | Source material (from SME brief) |
|---------|----------------------------------|
| **Opening scene** | Counterparty archetype, setting concept |
| **Context / backstory** | Stakeholder map, incident history, domain context |
| **Learner's stake** | Learner role, objectives, internal pressures |
| **Data arsenal** | Domain facts and numbers (§5) |
| **Final scene** | Scene setup, time pressure — ends one beat before `firstMessage` |

Typical length: **1,800–2,500 words** (pass band). **Fail** if under 1,500 or over 2,800. **Warn** if 1,500–1,799 or 2,501–2,800. Multiple named sections. At least one dialogue exchange. Enough texture that the reader feels the world before they enter the room. Do not embed the system prompt or rubric in the pre-read.

**Writing craft**

The pre-read should read like a short story or a business school case, not a briefing document. It should grip a reader who does not yet know they care about the scenario.

**These human-voice rules apply everywhere the persona speaks** — `preread.md`, `persona.voicePrompt`, `firstMessage`, and LLM chat replies (via runtime `REPLY_FOOTER`). The persona should sound like the same person in the pre-read world: varied rhythm, plain words, concrete detail, no robotic LLM shapes.

Specific rules:

- **Vary the sentence rhythm.** Short sentences after long ones. Very short ones after even longer ones. It is the rhythm that makes prose feel human.
- **Prose over bullets for character.** Reveal who someone is through what they do and say, not through a list of traits. In chat: speak in sentences, not trait stacks.
- **Show stakes, don't state them.** Find the specific fact that makes the reader feel the consequence. "Kristian's name goes on the report to Oslo" does more work than "this is an important client."
- **No LLM sentence shapes.** The three to catch: (1) balanced pairs — "This is not X. It is Y." Merge or cut one half. (2) symmetrical triplets — three parallel sentences in a row with the same structure. Break the third or cut the pattern. (3) numbered strategy summaries at the end. These patterns appear in existing pre-reads (vikram-sales included); the standard is evolving and new modules should not repeat them — **including in `firstMessage` and live chat.**

**Pre-read document rules** (briefing shape — `preread.md` only):

- **No em-dashes in the pre-read.** Use a period, or restructure the sentence. An em-dash joins two thoughts that should either stand alone or be fused. Period.
- **Open on a scene, not context.** Start in a room, a specific moment, an event. Context follows. The reader needs something to see before they need something to understand.
- **Numbers in sentences, not tables (mostly).** One reference table is fine for data learners will look up mid-conversation. Everything else: weave it in. Running the calculation in prose lands harder than the same figures in a table cell.
- **No meta-language.** Do not write "the simulation begins here," "you are now ready," or "your goal in this conversation is to." End on the beat before the first message and stop.

**Discovery space**

The pre-read arms the learner to enter the room credibly. It does not hand them a playbook.

The test: if reading the pre-read tells you exactly which questions to ask and what each question will unlock, the pre-read has done too much. The learner should leave the pre-read knowing *who* and *what*, not *how*.

Specific rules:

- **Do not reveal what the counterparty is holding back.** If a character has a specific concern, story, or softening trigger that the learner must earn — keep it out of the pre-read. Let the simulation surface it.
- **Do not name the unlock.** "Nobody has asked him about the Oslo office yet" and "If you ask him this, he will answer at length" are coaching, not briefing. Cut them.
- **Hint at the unknown, do not map it.** If a gap (budget, workspace standard, decision process) is worth discovering, give the learner enough professional context to know the gap exists, not enough to skip past it.
- **Do not complete calculations for the learner.** Give them the data. Let them do the math in the room.

**Emotional-pull craft**

Pre-reads should read as scene-driven fiction, not briefing documents. See [SCENARIO_MODULE_GUIDE.md](./SCENARIO_MODULE_GUIDE.md) pre-read gates.

- **Counterparty humanizing beat:** When the counterparty is a person (not an institution), give one concrete, unresolved human moment before the confrontation — a habit, a scar from a past project, a small vulnerability. Gold: Kristian / Amsterdam in `design-build-gcc-norway`, Omar / 5am port in `difficult-feedback`.
- **Stakes shown, not stated:** Find the specific object or moment that makes consequence felt. Avoid survey-level exposition (engagement scores, benchmarking gaps as the main stakes frame). Caution: `quiet-exit-retention`.
- **Recurring time pressure:** If there is a clock, return to it at least once mid-document — not only in the final section.
- **No reference-number ledgers:** Do not dump facts in pipe tables or bolded `OPTION A/B/C` / `HIDDEN COST 1/2/3` stacks when the same numbers belong in prose through a scene. One lookup table max for genuine mid-conversation reference (benchmark bands, tariff grids) — not client-intake dumps or discovery checklists (`Question | Why it matters` is coaching meta-language; cut it).

**Company and client names**

Do not use real company names, client names, or brand names in scenario materials — pre-read, persona, or scenario prompt. This includes the name of the firm the learner works for. Use the learner's firm generically ("the firm," "your firm") and invent fictional names for all counterparty companies and individuals.

---

## 8. Evaluation rubric (Reference depth)

Altius scores each conversation against **3–5 criteria**, each on a **1–5** scale.

*Tier 2 (collect from SME): evaluator lens, criteria names, anti-patterns. Tier 1 (Claude derives): per-criterion descriptions, scoring bands, exemplar behaviors.*

| Field | Source | Question to ask |
|-------|--------|-----------------|
| **Evaluator lens** | SME | Who is judging? *"Expert sales coach," "senior banking people leader"* |
| **Criteria names (3–5)** | SME | What dimensions matter most? Names only — Claude writes the descriptions |
| **Per-criterion description** | Claude | *(derived from criteria names and learning objectives)* |
| **Scoring bands** | Claude | *(calculated from criteria count × scale — e.g. 21+/25 = Excellent)* |
| **Anti-patterns** | Claude | Derived from learning objectives and scenario context; SME may add examples from practice via the open field |
| **Exemplar behaviors** | Claude | *(derived from criteria and scenario context)* |

---

## 9. Metadata and publishing (Reference depth)

*Tier 2 (confirm with SME/client): publish status. Tier 1 (Claude derives): all other fields.*

| Field | Source | Notes | Altius field |
|-------|--------|-------|--------------|
| **Stable ID** | Claude | Kebab-case slug from title; never change after launch | `id` |
| **Display order** | Claude | Sort position on learner home | `displayOrder` |
| **Card subtitle** | Claude | Learner role + one-line hook | `subtitle` |
| **Locale** | Claude | Default `en` unless otherwise specified | `locale` |
| **Max turns** | Claude | Default 20; 24 for complex / high-difficulty | `maxTurns` |
| **Status** | SME | `"draft"` for pilot, `"published"` for learners | `status` |

---

## 10. Same persona in multiple scenarios (Optional)

Most scenarios have **one unique persona**. If the same customer appears in more than one scenario (e.g. Vikram in sales, parts, and service):

- Copy the persona into each scenario's **`persona.ts`** independently.
- Tune **`scenarioPrompt`** per scenario; keep **`voicePrompt`** aligned if you want consistent voice.
- Accept duplication — each module stays self-contained and can evolve on its own.

Do **not** maintain a shared `_personas/` library. Cross-scenario consistency is an authoring choice, not a platform requirement.

---

## 11. Compliance, sensitivity, and brand (Optional)

| Topic | Question to ask |
|-------|-----------------|
| **Anonymization** | Real company names allowed or must be fictional? |
| **Approved messaging** | Product names, program names, legal disclaimers |
| **Off-limits topics** | Politics, religion, personal attributes to avoid |
| **Regional adaptation** | Currency, units, names, cultural norms |

---

## 12. Intake deliverables checklist

### What the SME or client must confirm before build starts

- [ ] Situation concept: learner role, counterparty, conflict
- [ ] 2–4 observable learning objectives
- [ ] Target audience: role, tenure, industry
- [ ] Anything else to consider (open — optional; leave blank if nothing)
- [ ] Publish status: `"draft"` for pilot, `"published"` for learners
- [ ] SME contact for play-test

### What Claude handles — no SME time needed

- [ ] Persona name, company name, all `voicePrompt` prose
- [ ] Full `scenarioPrompt` (scene, if/then behavior rules, traps, emotional arc)
- [ ] `firstMessage`
- [ ] `preread.md` — all narrative prose and section structure
- [ ] Eval criteria descriptions, scoring bands, `overallGuidance`
- [ ] Module code (`persona.ts`, `index.ts`), registry, all doc updates
- [ ] Metadata: `id`, `displayOrder`, `maxTurns`, `subtitle`, `locale`

---

## 13. Intake interview script (optional)

20-minute flow. Three required items plus an open field. Claude handles everything else.

1. **5 min** — Situation and learner role: what is the conflict, who is in the room
2. **5 min** — Learning objectives: 2–4 observable behaviors to improve
3. **5 min** — Target audience: role, tenure, industry
4. **5 min** — Anything else to consider: non-obvious counterparty details, real failure patterns, org-specific constraints, sensitivities — or nothing if there is nothing

After the call: Claude builds the full module and shares it for review. The SME's next input is the play-test — not another intake call.

---

## Related documents

- [SCENARIO_INTAKE_EDUCATION.md](./SCENARIO_INTAKE_EDUCATION.md) — HEI-specific intake (COs, accreditation, domain sensitivities)
- [SCENARIO_MODULE_GUIDE.md](./SCENARIO_MODULE_GUIDE.md) — How intake maps to module files and seeding
- [ACCREDITATION_EVIDENCE.md](../../ACCREDITATION_EVIDENCE.md) — CO/PO attainment and export artefacts

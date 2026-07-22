# Altius Scenario Module Guide

How to turn intake information into a **ScenarioModule** that Altius can seed, publish, and run. Each scenario is a **self-contained folder** — persona, scene rules, pre-read, and rubric live together. No shared persona library.

### Intake path (choose one)

| Buyer | Intake doc | Typical reference module |
| :--- | :--- | :--- |
| **Corporate / L&D** | [SCENARIO_INTAKE.md](./SCENARIO_INTAKE.md) | `vikram-sales/`, `vikram-parts/`, `vikram-service/`, `design-build-gcc-norway/` |
| **Higher education** | [SCENARIO_INTAKE_EDUCATION.md](./SCENARIO_INTAKE_EDUCATION.md) (+ shared sections in SCENARIO_INTAKE) | `flowbridge-discovery/`, `client-counselling-law/`, `divorce-custody-counselling/` |

### Reference implementations

| Module | Use when |
| :--- | :--- |
| **`accountability-under-pressure/`** | B-school accountability under pressure, regulatory meeting, AAA (implicit), Truth vs Loyalty |
| **`quiet-exit-retention/`** | OB/HRM retention conversation, Positions vs Interests (Getting to Yes), first-line manager |
| **`client-counselling-law/`** | Law client counselling, expectation management, procedural justice (Voice/Respect/Trust/Reasoning) |
| **`divorce-custody-counselling/`** | Law client counselling, Binder & Price non-legal goals, matrimonial first consult (fairness as wrong first question) |
| **`design-build-gcc-norway/`** | B2B discovery, cross-cultural expectation-setting, GCC / workplace scoping |
| **`difficult-feedback/`** | Difficult feedback, banking governance, SBI, empathy without collusion, shared accountability |
| **`flowbridge-discovery/`** | B2B discovery, async/crisis context, education / MBA lead qualification |
| **`fmcg-distributor-negotiation/`** | FMCG trade sales, discovery before pitch, distributor economics |
| **`sales-product-knowledge/`** | SaaS AE value selling, SPIN, product knowledge in context |
| **`vikram-sales/`** | High-stakes B2B sales, objection handling, TCO |
| **`vikram-parts/`** | Aftermarket / negotiation, total cost of ownership |
| **`vikram-service/`** | Service recovery, empathy under pressure |
| **`customer-success-renewals/`** | CSM renewal escalation, LEAP service recovery, Indian employee benefits |

### Reference picker by failure mode

| If you are hardening against… | Start with |
| :--- | :--- |
| Counsel / role-swap (persona coaches the learner) | `client-counselling-law/`, `divorce-custody-counselling/` |
| Advice-leak (persona coaches deal structure or proposal) | `vikram-sales/`, `design-build-gcc-norway/` |
| Disclosure / earned unlock (pre-read recitation) | `flowbridge-discovery/`, `sales-product-knowledge/` |
| Accountability state machine (trigger-based arc) | `accountability-under-pressure/` |
| Pre-read craft (scene, humanizing beat, no ledgers) | `design-build-gcc-norway/`, `difficult-feedback/` |

**Older generation (do not copy preread shape):** `vikram-sales/`, `vikram-parts/`, `vikram-service/` — CHAPTER headers, mission checklists, or scripted playbooks. `flowbridge-discovery/` preread was hardened (SCN-HARDEN-001); use it for async crisis discovery craft. Prompt traps may be updated on other legacy modules; preread craft is separate follow-on debt.

---

## Authoring quality bar

Before file assembly, confirm the **pedagogy contract** and **layer ownership**. One coaching lens per scenario (see [CLIENT_SCENARIO_BRIEF.md](../../intake/CLIENT_SCENARIO_BRIEF.md) catalogue). Learning objectives are observable verbs. The pre-read arms the learner; the chat is where they practice.

### Layer ownership

```
scenarioPrompt (index.ts)  → scene, fixed facts, trigger rules, traps, disclosure gates, non-negotiables
persona.voicePrompt        → character voice, baseline reply patterns, sample phrases (human speech — see Voice scope below)
firstMessage               → in-character opener; same voice rules as live chat
buildSystemPrompt()        → character/AI lock only ("Do not break character…")
prompt-assembly.ts         → runtime role boundary + reply length ("normally under 180 words")
```

### Voice scope (human craft vs pre-read document rules)

**Human voice craft** from [SCENARIO_INTAKE §7 Writing craft](./SCENARIO_INTAKE.md#7-pre-read-briefing-content-required) applies to **all persona output**:

| Surface | Human voice applies |
|---------|---------------------|
| `preread.md` | Yes — case prose |
| `persona.voicePrompt` | Yes — shapes how the model speaks |
| `firstMessage` | Yes — first in-character turn; write it exactly as the persona would type or speak under scene pressure |
| **LLM chat replies** | Yes — runtime via `persona.voicePrompt` + `REPLY_FOOTER` in `prompt-assembly.ts` |

Shared craft (everywhere the persona writes or speaks):

- Vary sentence rhythm — short after long; one-sentence beats are fine.
- Plain words and concrete detail — SKUs, numbers, objects, not abstractions.
- Show through specifics, not trait lists or consultant summaries.
- **No LLM sentence shapes:** balanced pairs ("This is not X. It is Y."), symmetrical triplets, numbered strategy wrap-ups.
- Natural speech in dialogue — contractions, interruptions, stressed asides; a person under pressure does not sound like a briefing doc.

**Pre-read document rules** apply to `preread.md` only (structure, length, discovery space, meta-language ban, em-dash ban in written case prose, no coaching checklists). Those are briefing-shape constraints — not permission for robotic chat. Do not strip human voice from `firstMessage` or persona layers to satisfy pre-read lint.

Validation scans em-dashes and legacy briefing patterns on `preread.md` only. `firstMessage` and `persona.voicePrompt` are judged by the human craft bar above, not by pre-read document lint.

Do not paste deny-list regex from `chat-probe-denylist.ts` into module prompts. Use behavioral trap rules instead.

### Preread ↔ scenarioPrompt sync

If the pre-read or persona plants a **withheld** beat (fear, memo, hidden concern), `index.ts` needs a **reveal-trigger** — when and how it surfaces (see accountability's "Memo reveal"). If the persona must confirm, dispute, or reuse specific numbers, mirror only those figures into `scenarioPrompt` and add an explicit **do not invent** lock. Do not mirror every pre-read figure.

### Seed is the prod lever

Chat reads scenario config **from the database**, not module files at runtime. `npm run seed:scenarios` writes `systemPrompt`, `preReadMarkdown`, and `evalRubric`. **Deploy alone does not change scenario behavior.** Seed staging first; one prod seed when ready.

---

## How Altius uses a scenario

```mermaid
flowchart LR
  subgraph module [Scenario module folder]
    Persona[persona.ts]
    Index[index.ts]
    PreRead[preread.md]
    Index --> Persona
    Index --> PreRead
  end

  subgraph seed [Seed time]
    Registry[registry.ts]
    Build[buildSystemPrompt]
    DB[(scenarios table)]
    module --> Registry
    Registry --> Build
    Build --> DB
  end

  subgraph runtime [Learner runtime]
    Home[Learner home]
    PreReadPage[Pre-read page]
    Chat[Simulation chat]
    Eval[Evaluation debrief]
    Home --> PreReadPage
    PreReadPage --> Chat
    Chat --> Eval
  end

  DB --> Home
  DB --> PreReadPage
  DB --> Chat
  DB --> Eval
```

### Prompt assembly at seed time

The runtime system prompt is built from two layers plus global guardrails:

```
scenarioPrompt          ← scene setup + scenario-specific if/then rules (index.ts)
persona.voicePrompt     ← character voice and baseline behavior (persona.ts)
global guardrails       ← added by buildSystemPrompt() in build-prompt.ts
```

Stored in the database as `personaConfig.systemPrompt`.

| Module source | DB column / field | Runtime use |
|---------------|-------------------|-------------|
| `title`, `subtitle`, `displayOrder` | `title`, `personaConfig` | Learner home card |
| `getPreReadMarkdown()` | `preReadMarkdown` | `/pre-read/[scenarioId]` |
| `moduleId` (folder name) | `personaConfig.moduleId` | Public URL slug — `/pre-read/vikram-sales`, demo CTAs, `resolve.ts` lookup |
| `firstMessage` | `firstMessage` | First assistant message on session start |
| `persona` + `scenarioPrompt` → `buildSystemPrompt()` | `personaConfig.systemPrompt` | Every chat turn |
| `persona.name` | `personaName` | UI labels, evaluation context |
| `persona.role`, `persona.company` | `personaConfig.role`, `.company` | Fallback prompt assembly |
| `evalRubric` | `evalRubric` | Post-session LLM evaluation |

---

## Folder structure

```
scenarios/
├── types.ts                 # ScenarioModule, ScenarioPersona interfaces
├── build-prompt.ts          # buildSystemPrompt() — composes runtime prompt
├── registry.ts              # Active modules (seed + publish control)
├── seed.ts                  # DB upsert logic
├── validate.ts              # Module validation (used by validate:scenarios)
└── modules/
    └── your-scenario-id/
        ├── index.ts         # ScenarioModule export
        ├── persona.ts       # ScenarioPersona for this scenario
        └── preread.md       # Pre-read briefing
```

**Rules:**

- **`id`** — stable kebab-case slug (e.g. `vikram-sales`). Stored as `personaConfig.moduleId`. Do not rename after launch.
- **One folder per scenario.** Persona lives in that folder's `persona.ts`, not in a shared directory.
- **Register** every published scenario in `registry.ts`. Modules removed from the registry are archived on next seed.

### Public URLs (`moduleId`)

- Folder `id` (e.g. `vikram-sales`) is stored as `personaConfig.moduleId` at seed time.
- Learners and demo CTAs use `/pre-read/{moduleId}` — **not** the DB UUID (`scenarios.id`).
- Resolver: `src/lib/scenarios/resolve.ts` (`findScenarioByRouteId`, `getScenarioRouteId`).
- Microsite featured demos: keep `landing-page/src/data/featured.ts` in sync with `registry.ts`.

---

## Step 1: Create `persona.ts`

Define the counterparty for **this scenario only**.

```ts
// scenarios/modules/your-scenario-id/persona.ts
import type { ScenarioPersona } from "../../types";

export const persona: ScenarioPersona = {
  name: "Jordan Lee",
  role: "Procurement Director",
  company: "Acme Logistics",
  voicePrompt: `
Who Jordan is:
- [Backstory, tenure, what they care about]

Voice and behavior in every reply:
- [Communication style, sample phrases]
- [How they respond to strong data vs vague answers]
- [How they admit being wrong]`,
};
```

### What belongs in `voicePrompt`

| Include | Exclude |
|---------|---------|
| Backstory and personality | Today's specific scene (boardroom, grounded trucks) |
| Default communication style | Scenario-only demands (30% discount, 13L engines) |
| Sample phrases | Non-negotiables unique to this simulation |
| Baseline reply patterns (strong data / weak data / corrected) | Evaluation criteria |

### Same persona in multiple scenarios

Copy `persona.ts` into each module folder. Duplication is expected and preferred over shared imports — each scenario stays independently editable. Align `voicePrompt` text across copies only when you want consistent voice.

---

## Step 2: Create `index.ts`

```ts
// scenarios/modules/your-scenario-id/index.ts
import { readFileSync } from "fs";
import { dirname, join } from "path";
import { fileURLToPath } from "url";
import type { ScenarioModule } from "../../types";
import { persona } from "./persona";

const DIR = dirname(fileURLToPath(import.meta.url));

const SCENARIO_PROMPT = `You are ${persona.name}, ${persona.role} at ${persona.company}. [Scene: where you are, who the participant is, what you just said.]

[Scenario-id]-specific:
- [Default posture in this scene]
- [When learner is vague]
- [When learner brings strong data]
- [Non-negotiables]
- [Acceptable concessions]`;

const scenario: ScenarioModule = {
  id: "your-scenario-id",
  displayOrder: 3,
  title: "The Working Title",
  subtitle: "Learner Role — one-line hook",
  learnerRole: "Area Sales Manager",
  persona,
  scenarioPrompt: SCENARIO_PROMPT,
  firstMessage: "...",              // customer's opening line
  maxTurns: 20,
  status: "published",
  evalRubric: { ... },              // see Step 4
  getPreReadMarkdown: () =>
    readFileSync(join(DIR, "preread.md"), "utf-8"),
};

export default scenario;
```

### Brief → module mapping

Shows where each module property comes from. Most properties are Claude-derived from the four-item brief. For HEI scenarios, also complete [SCENARIO_INTAKE_EDUCATION.md](./SCENARIO_INTAKE_EDUCATION.md) §1, §2, §5 (CO alignment).

| Module property | Source | From brief item |
|----------------|--------|-----------------|
| `persona.name`, `.company` | Claude | Invented — keeps scenarios fictional |
| `persona.role`, `.company` context | Claude + brief | Learner role from brief item 1 |
| `persona.voicePrompt` | Claude | Derived from situation and learning objectives |
| `scenarioPrompt` | Claude | Derived from situation, objectives, and open field |
| `firstMessage` | Claude | Derived from situation |
| `title`, `subtitle`, `learnerRole` | Claude | Derived from situation and learner role |
| `id`, `displayOrder`, `maxTurns` | Claude | Defaults; `id` slugged from title |
| `status` | SME confirms | Draft for pilot, published for learners |
| `getPreReadMarkdown()` → `preread.md` | Claude | Written from situation, objectives, and open field |
| `evalRubric` criteria, descriptions, bands | Claude | Derived from learning objectives |
| `evalRubric` evaluatorRole | Claude | Derived from domain and learner role |

Global guardrails are **not** written in the module — `buildSystemPrompt()` appends them:

```ts
// scenarios/build-prompt.ts
export function buildSystemPrompt(mod: ScenarioModule): string {
  return [
    mod.scenarioPrompt.trim(),
    mod.persona.voicePrompt.trim(),
    "Do not break character or mention you are an AI.",
  ].join("\n\n");
}
```

Reply-length discipline (`normally under 180 words`, no lectures) is applied at **runtime** in `src/lib/llm/prompt-assembly.ts` — not in the seed footer.

**File split (Plan B convention):** traps, disclosure gates, and non-negotiables belong in `index.ts` `SCENARIO_PROMPT`; voice and length tone belong in `persona.ts` `voicePrompt`. Do not duplicate trap text in both files unless voice-specific. Advice-seeking traps ("what should I offer?") → state wants and constraints only; never coach the learner's deal structure, proposal, or escalation script.

### `scenarioPrompt` anatomy

Gold reference: [`accountability-under-pressure/index.ts`](./modules/accountability-under-pressure/index.ts). Use these blocks in order (adapt labels to the scene):

| Block | Purpose |
|-------|---------|
| **Scene anchor** | Who you are, where you are, who the participant is, what you just said |
| **Fixed facts** | Numbers and events the persona will stand on; only what must stay consistent |
| **Trigger-based behavior** | If/then rules keyed to learner moves (hedging, blame-shifting, strong data, etc.) |
| **Acceptable end states** | What a good-enough close looks like — earned, not gifted |
| **Forbidden end states** | Outcomes that must not happen regardless of charm |
| **Traps** | Advice-seeking, sympathy collusion, proposal-coaching, scripted playbooks — must fire when walked into |
| **Non-negotiables** | Character lock, do-not-invent, no coaching criteria as dialogue |

**Reveal-trigger:** Any withheld beat planted in pre-read or persona needs an explicit trigger in `scenarioPrompt` (e.g. memo reveal, unlock ladder). Law modules use "What you know that the participant does not (reveal only through the unlock ladder below)."

**Fixed-facts + do-not-invent lock:** Mirror into `scenarioPrompt` only numbers the persona must confirm, dispute, or reuse — not every pre-read figure. Add a line such as: `Do not invent new lab numbers. COD is 312 mg/L against 100 mg/L consented.`

**Disclosure cap:** When the learner earns depth, release at most **one concrete incident + one systemic pattern** per strong turn — not a full inventory dump.

**Escalation-cue (optional):** Async or time-boxed scenes may state when the persona must exit (e.g. return to a courier call if the chat drags). See `flowbridge-discovery/index.ts`.

**Advice-seeking trap (required for commercial modules):** If the learner asks what to offer, propose, or say to a third party → state wants and constraints only. Never coach deal structure, CFO deck, HQ script, legal next steps, or escalation wording.

---

## Step 3: Write `preread.md`

Claude writes the pre-read from the intake brief. The SME does not draft it. Loaded at seed time and rendered on the pre-read page.

**Template:**

```markdown
# [Title]

[Opening — no header. Third person on the counterparty or a scene from their world.
200–400 words. Ends on a beat that pulls the learner in before "you" enter.]

---

**[Section: context or backstory]**
[History that explains the conflict. Off-screen characters who matter.]

---

**[Section: learner's stake]**
[Why the learner is here. Their history with the counterparty. Where their own
accountability or complicity begins.]

---

**[Section: what the learner knows today]**
[Facts, numbers, documents, calls — the evidence they carry into the room.]

---

**[Section: entering the room]**
[Final scene. Physical setting. Ends one beat before `firstMessage`. No meta-language.]

**[END OF PRE-READ]**
```

Headers are **bold inline** (`**Section title**`), not `###`. No "CHAPTER N:" prefix. Sections are named for their content. The opening section has no header.

**Quality bar:**

- End on the **exact beat** before `firstMessage`.
- Include **actionable numbers** when learners must calculate TCO, ROI, or cost-share.
- Do **not** embed `voicePrompt`, `scenarioPrompt`, or rubric text.

**Writing craft and discovery rules (see [SCENARIO_INTAKE.md §7](./SCENARIO_INTAKE.md#7-pre-read-briefing-content-required) for full guidance):**

- Target **1,800–2,500 words** (pass band). **Fail** if `< 1,500` or `> 2,800`. **Warn** if `1,500–1,799` or `2,501–2,800`. Enforced in `preread-word-count.ts` → `validate.ts` + audit script.
- **Human voice craft** (rhythm, plain words, concrete detail, no LLM shapes) — see **Voice scope** above — applies to `preread.md`, `persona.voicePrompt`, `firstMessage`, and live chat replies.
- **Pre-read document only:** no em-dashes in case prose; open on a scene; no meta-language; discovery-space rules below.
- Prose for character; one reference table maximum for data learners will look up mid-conversation.
- Do not reveal what the counterparty is holding back or what question will unlock them.
- Do not script the winning play, pillar menu, or close — give constraints and data the learner must use in the room.
- Do not complete calculations for the learner. Give data; let them do the math in the room.
- Do not use real company or client names. Learner's firm is always "your firm" or "the firm." Counterparty companies and individuals are fictional.

**Pre-read gates (craft + sync):**

| Gate | Rule |
|------|------|
| **No reference-number ledgers** | No pipe-table dumps of facts already in prose; no bolded `HIDDEN COST 1/2/3` or `OPTION A/B/C` spec stacks. Numbers arrive through a scene moment and stay specific enough for an evaluator to cite. |
| **One lookup table max** | Only for genuine mid-conversation reference (benchmark bands, tariff grids) — not client-intake dumps. Gold: fit-out benchmarks in `design-build-gcc-norway/preread.md`. |
| **No coaching checklists** | Tables like `Question \| Why it matters` are meta-language (discovery scripting), not data — cut them. See `flowbridge-discovery/preread.md` Ch6. |
| **Counterparty humanizing beat** | One concrete, unresolved human moment before confrontation (when the counterparty is a person). Gold: Kristian / Amsterdam (`design-build-gcc-norway`), Omar / 5am port (`difficult-feedback`). |
| **Stakes shown, not stated** | Find the object or moment that makes consequence felt — not survey exposition. Caution: `quiet-exit-retention` (HR engagement data reads like a system report). |
| **Recurring time pressure** | Clock or deadline should return at least once mid-document, not only in the final section. |
| **Pre-read length** | **Fail** `< 1,500` or `> 2,800` words. **Warn** `1,500–1,799` or `2,501–2,800`. **Pass** `1,800–2,500`. See `preread-word-count.ts`. Enforced at seed/CI for non-grandfathered modules only. |
| **Withheld beats** | If pre-read hints at something held back, `index.ts` must have a matching reveal-trigger — do not orphan plants. |

Emotional-pull craft detail: [SCENARIO_INTAKE.md §7](./SCENARIO_INTAKE.md#emotional-pull-craft).

---

## Step 4: Define `evalRubric`

Translate intake [§8 Evaluation rubric](./SCENARIO_INTAKE.md#8-evaluation-rubric-required) (and for HEI, [§5 CO alignment](./SCENARIO_INTAKE_EDUCATION.md#5-evaluation-rubric--co-alignment-required)) into TypeScript:

```ts
evalRubric: {
  evaluatorRole: "expert sales coach",
  criteria: [
    {
      name: "Discovery & Diagnosis",
      description: "Did the learner uncover concerns beyond sticker price?",
      scale: 5,
    },
    // 3–5 criteria
  ],
  overallGuidance:
    "16+/20 = Excellent. 12-15 = Good. 8-11 = Needs Improvement. <8 = Significant gaps.",
},
```

| Rule | Reason |
|------|--------|
| 3–5 criteria | Readable debrief |
| `scale: 5` each | Consistent with existing modules |
| Observable descriptions | Evaluator cites transcript moments |
| Distinct `name` values | Become score keys in debrief |
| `overallGuidance` bands | Should match `criteria.length × scale` |
| Criteria align with coaching lens | SPIN, LEAP, AAA, procedural justice, etc. — names learners recognize in debrief |
| Rubric ↔ trap alignment | Descriptions must not reward outcomes the prompt forbids (e.g. "help build the CFO deck" when anti-proposal trap is active) |
| Immersion failure (counsel/manager roles) | Optional `overallGuidance` sentence when persona role-swap is a known failure mode — see law modules |

For HEI: map criteria to verbatim COs per [SCENARIO_INTAKE_EDUCATION.md](./SCENARIO_INTAKE_EDUCATION.md) §5. Woven numbers in pre-read should stay locatable for transcript-cited scoring (authoring practice; not a separate platform rule).

---

## Step 5: Register the module

```ts
// scenarios/registry.ts
import yourScenario from "./modules/your-scenario-id";

export const SCENARIO_MODULES: ScenarioModule[] = [
  // ...existing
  yourScenario,
];
```

- **Add** → seeded/updated on next `npm run seed:scenarios`
- **Remove** → existing DB row archived (matched by `personaConfig.moduleId`)

---

## Step 6: Seed to the database

Validate modules first, then seed from `v1/app/` with `DATABASE_URL` in `.env.local`:

```bash
npm run validate:scenarios
npm run seed:scenarios
```

`seed:scenarios` calls `assertReadyForSeed()` internally — errors always block; warnings block **published** modules not on the legacy grandfather list (see [Publish proposal](#publish-proposal-new-modules)).

Seed behavior:

- Upserts by **`personaConfig.moduleId`** (falls back to `title` for first-time migration)
- Composes `personaConfig.systemPrompt` via `buildSystemPrompt(mod)`
- Sets `personaName` from `mod.persona.name`
- Sets `personaConfig.role` / `.company` from `mod.persona`
- Archives published scenarios whose `moduleId` is no longer in the registry

**Deploy ≠ seed:** Pushing app code does not update stored prompts or pre-reads. Reseed after module edits.

---

## Step 7: Verify end-to-end

**Ship checklist:**

```bash
npm run validate:scenarios   # hard errors fail; warnings print for legacy style
npm run test                 # vitest (deny-list, prompt-assembly)
npm run probe:chat           # env-gated; add one smoke row per new module
npm run seed:scenarios       # staging first, then one prod seed
```

| Check | How |
|-------|-----|
| Home card | Title, subtitle, turn count, sort order |
| Pre-read | Scene-driven fiction; no ledgers; humanizing beat; ends before chat |
| Preread ↔ prompt sync | Withheld beats have reveal-triggers; persona numbers have do-not-invent lock |
| First message | Matches narrative; correct tone |
| Persona voice | Baseline patterns from `voicePrompt` hold across turns |
| Scenario rules | Traps fire on advice-seeking; disclosure caps hold |
| Evaluation | Debrief shows 3–5 scores with cited moments; rubric does not reward coached outcomes |
| Draft mode | `status: "draft"` hidden from learner home |

Pilot 2–3 conversations with the SME; tune `scenarioPrompt` first (most impact), then `voicePrompt` if character drifts.

### Probe contract (new modules)

Add one row to [`probe-chat-models.ts`](../scripts/probe-chat-models.ts) per new module: one advice-seeking or domain-specific role-swap user turn + appropriate `deny` list. See comment block above `PROBES` in that file. Probes are env-gated, not CI.

### Per-module acceptance

- [ ] Brief confirmed: situation, learning objectives, target audience, open field
- [ ] Full module: `persona.ts`, `index.ts`, `preread.md`, `registry.ts`
- [ ] `validate:scenarios` → `seed:scenarios`
- [ ] Play-test: pre-read → sim → debrief — SME sign-off
- [ ] Gallery slug matches `moduleId` if card exists in `landing-page/src/data/scenarios.ts`

### Publish proposal (new modules)

Every **new** module must pass strict validation before `status: "published"`. Do not add new `moduleId`s to `LEGACY_WARN_GRANDFATHER_IDS` in `validate.ts`.

| Phase | `status` | Learner-visible | Validation |
|-------|----------|-----------------|------------|
| Build | `draft` | No | Errors block seed; warnings allowed |
| Publish proposal | flip to `published` in PR | After seed | **0 errors, 0 warnings** for this module |
| Live | `published` + prod seed | Yes | CI + seed gate |

**Automated (blocking before publish PR merges):**

```bash
npm run validate:scenarios   # must pass with 0 warnings on the new moduleId
npm test
```

- Probe row in `probe-chat-models.ts` (advice-seeking or domain role-swap)
- Module **not** in `LEGACY_WARN_GRANDFATHER_IDS`

**Human (blocking before publish PR merges):**

- [ ] Intake brief complete (corporate or education doc)
- [ ] Built to Plan B bar — reference `accountability-under-pressure/`
- [ ] `scenarioPrompt` full anatomy (scene → fixed facts → triggers → acceptable/forbidden → traps → non-negotiables)
- [ ] Preread passes Step 3 gates (no CHAPTER, no mission checklists)
- [ ] Rubric aligns with coaching lens; no rewarded coached outcomes
- [ ] Staging seed + 2–3 play-test conversations; SME sign-off
- [ ] Specialist debrief spot-check on `/internal/reports`

Optional: `npx tsx ../debug/app/scripts/audit-module-guidelines.ts` — target 0 warns for the new module.

### Legacy hardening (existing live modules)

The 13 live modules have grandfathered warnings until [SCN-HARDEN rows in BACKLOG.md](../../BACKLOG.md) are `done`. When a hardening row completes:

1. Re-run audit — 0 warnings for that `moduleId`
2. Remove `moduleId` from `LEGACY_WARN_GRANDFATHER_IDS` in `validate.ts`
3. Mark SCN-HARDEN row `done`

---

## Complete minimal example

```
scenarios/modules/acme-renewal/
├── persona.ts
├── index.ts
└── preread.md
```

```ts
// persona.ts
import type { ScenarioPersona } from "../../types";

export const persona: ScenarioPersona = {
  name: "Jordan Lee",
  role: "Procurement Director",
  company: "Acme Logistics",
  voicePrompt: `
Who Jordan is:
- Fifteen years in procurement; evaluates vendors on total cost, not list price.

Voice and behavior in every reply:
- Direct and numbers-first. "Show me the impact in dollars."
- Softens when given quantified uptime or SLA recovery data; tests with one follow-up.`,
};
```

```ts
// index.ts
import { readFileSync } from "fs";
import { dirname, join } from "path";
import { fileURLToPath } from "url";
import type { ScenarioModule } from "../../types";
import { persona } from "./persona";

const DIR = dirname(fileURLToPath(import.meta.url));

const SCENARIO_PROMPT = `You are Jordan Lee, Procurement Director at Acme Logistics. The participant is your account manager. You are reviewing a renewal and just said you are leaning toward switching vendors unless they justify the 12% price increase.

Renewal-specific:
- Lead with total cost and SLA breaches last quarter.
- You will NOT accept a blanket discount without a service commitment.`;

const scenario: ScenarioModule = {
  id: "acme-renewal",
  displayOrder: 10,
  title: "The Renewal Crunch",
  subtitle: "Account Manager — defend renewal vs switch",
  learnerRole: "Account Manager",
  persona,
  scenarioPrompt: SCENARIO_PROMPT,
  firstMessage:
    "I've got three bids on my desk. Yours is twelve percent higher than last year. Give me one reason I shouldn't switch.",
  evalRubric: {
    evaluatorRole: "expert account management coach",
    criteria: [
      {
        name: "Discovery",
        description: "Did the learner uncover Jordan's real drivers beyond price?",
        scale: 5,
      },
      {
        name: "Value Proof",
        description: "Did the learner quantify value or SLA recovery with specifics?",
        scale: 5,
      },
      {
        name: "Commercial Creativity",
        description: "Did the learner propose a credible path without a blind discount?",
        scale: 5,
      },
    ],
    overallGuidance:
      "12+/15 = Excellent. 9-11 = Good. 6-8 = Needs Improvement. <6 = Significant gaps.",
  },
  getPreReadMarkdown: () =>
    readFileSync(join(DIR, "preread.md"), "utf-8"),
};

export default scenario;
```

---

## Anti-patterns

| Anti-pattern | Why it fails |
|--------------|--------------|
| Shared `_personas/` library | Scenarios become coupled; persona edits break unrelated modules |
| Scene-specific rules in `voicePrompt` | Persona layer bleeds into scenario layer; hard to tune independently |
| Putting guardrails in every module | Duplicated; use `buildSystemPrompt()` instead |
| Dual-write length (2–4 paragraphs in module + runtime footer) | Conflicts with `prompt-assembly.ts` REPLY_FOOTER |
| Pre-read that resolves the whole conversation | Nothing left to practice |
| Reference-number ledgers (pipe tables, OPTION A/B/C stacks) | Turns discovery into lookup; learner recites instead of thinks |
| Coaching checklists in pre-read (`Question \| Why it matters`) | Meta-language; scripts which questions unlock what |
| Advice-leak (persona coaches deal, proposal, HQ, legal steps) | Breaks immersion; learner gets answers instead of practice |
| Rubric rewards coached outcomes traps forbid | Eval fights prompt; debrief misleads |
| Withheld beat in pre-read, no reveal-trigger in `index.ts` | Chat cannot earn the beat consistently |
| Numbers in pre-read but no do-not-invent lock in prompt | Model drifts figures mid-session |
| Changing `id` after learners have sessions | Breaks registry archival and analytics |
| Forgetting `registry.ts` | Module never seeds |
| Editing DB manually | Code and database diverge |
| Expecting deploy to fix scenario content | Only `seed:scenarios` updates DB prompts and pre-reads |

---

## Type reference

```ts
interface ScenarioPersona {
  name: string;
  role: string;
  company: string;
  voicePrompt: string;
}

interface ScenarioModule {
  id: string;
  displayOrder: number;
  title: string;
  subtitle: string;
  learnerRole: string;
  persona: ScenarioPersona;
  scenarioPrompt: string;
  firstMessage: string;
  evalRubric: EvalRubric;
  maxTurns?: number;
  locale?: string;
  status?: "draft" | "published" | "archived";
  personaConfig?: Record<string, unknown>;
  getPreReadMarkdown: () => string;
}
```

Authoritative definitions: `scenarios/types.ts`.

---

## Related documents

- [SCENARIO_INTAKE.md](./SCENARIO_INTAKE.md) — Corporate / L&D intake checklist
- [SCENARIO_INTAKE_EDUCATION.md](./SCENARIO_INTAKE_EDUCATION.md) — Higher-ed intake (CO alignment, law/medical/hospitality)
- [ACCREDITATION_EVIDENCE.md](../../ACCREDITATION_EVIDENCE.md) — CO/PO mapping and accreditation artefacts
- `scenarios/build-prompt.ts` — Prompt composition
- `scenarios/validate.ts` — Module validation (`npm run validate:scenarios`)
- `src/lib/llm/evaluation.ts` — Post-session scoring
- `src/lib/llm/prompt-assembly.ts` — Runtime prompt read from DB

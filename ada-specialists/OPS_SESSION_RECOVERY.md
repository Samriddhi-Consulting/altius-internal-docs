# Session lockout recovery (ops runbook)

**Preferred:** use the admin or specialist **Reset session** UI on `/admin/reports/[sessionId]` or `/internal/reports/[sessionId]`:

- **`flag_unlock`** — active sessions only; clears lockout on the existing session
- **`full_redo`** — archives the session and creates a new active session

Who can reset: org-scoped **admin**, **ada_specialist** (assigned orgs), **super_admin**. Resend notifies specialists assigned to the learner's org (or `RESEND_FALLBACK_EMAIL` — see [PHASE0_SETUP.md](./PHASE0_SETUP.md) §4 Resend).

Manual SQL below remains for support edge cases.

**Learner contact:** connect@adegreeabove.org or their account manager.

---

## Symptom — Persona never replies (persist-failed session)

Learner sent messages but the persona never responded. Transcript may show only the opening line plus multiple learner messages in a row. `turn_count` stays **0** in admin reports.

**Cause (fixed 2026-06-29):** Cloudflare Workers did not reliably complete `streamText` + `onFinish` — Groq returned 200 with output tokens, but the client received an empty text stream and assistant rows were never saved. Chat now uses `generateText` with awaited persist before the HTTP response.

**Learner self-heal (post-fix):** Opening `/sim/{sessionId}` for a persist-failed session auto-completes the broken session and redirects to a fresh one (clean opening only).

**Ops — find affected sessions:**

```sql
SELECT s.id, u.email, sc.title, s.turn_count,
  SUM(CASE WHEN m.role = 'user' THEN 1 ELSE 0 END) AS user_msgs
FROM sessions s
JOIN users u ON u.id = s.user_id
JOIN scenarios sc ON sc.id = s.scenario_id
JOIN messages m ON m.session_id = s.id
WHERE s.status = 'active'
  AND s.turn_count = 0
GROUP BY s.id, u.email, sc.title, s.turn_count
HAVING SUM(CASE WHEN m.role = 'user' THEN 1 ELSE 0 END) > 0;
```

**Ops fix:** Ask learner to refresh or re-open the sim URL (auto-recovery), or use **Full redo** on the reset UI.

---

## Symptom — Session lockout

Learner reports the chat will not send messages. The UI shows:

> Too many flagged messages. Email connect@adegreeabove.org or your account manager to unlock your session.

---

## Information to collect from the learner

- Email used to sign in
- Scenario name (e.g. *The Digital Disconnect* / FlowBridge)
- Approximate time they started the simulation

---

## 1. Find the session

Run in Supabase SQL editor (or any Postgres client connected to the app DB):

```sql
SELECT
  s.id,
  s.status,
  s.flag_count,
  s.is_flagged,
  s.turn_count,
  s.started_at,
  u.email,
  sc.title
FROM sessions s
JOIN users u ON u.id = s.user_id
JOIN scenarios sc ON sc.id = s.scenario_id
WHERE u.email = 'learner@example.com'
  AND s.status = 'active'
ORDER BY s.started_at DESC
LIMIT 5;
```

Confirm `flag_count >= 5` on the session they are trying to use.

---

## 2. Unlock (most common fix)

Preserves transcript and turn count; learner continues the same session.

**Send lockout is governed by `flag_count` only** (`screenMessage` in the chat API). Clearing `flag_count` alone unblocks sending. Set both fields below to mirror the reset API and keep admin reports consistent:

```sql
UPDATE sessions
SET flag_count = 0,
    is_flagged = false
WHERE id = '<session_id>';
```

Ask the learner to **refresh the page** and continue the conversation.

---

## 3. Optional audit log

```sql
INSERT INTO session_resets (session_id, reset_by, reason, reset_type)
VALUES (
  '<session_id>',
  '<your_internal_user_uuid>',
  'Lockout recovery — learner contacted support',
  'flag_unlock'
);
```

For a manual **full redo**, also set `new_session_id` to the replacement session UUID if you create one outside the UI.

---

## 4. Diagnose flagged messages

Use after unlock or for post-cohort review:

```sql
SELECT flag_reason, LEFT(content, 120) AS content_preview, created_at
FROM messages
WHERE session_id = '<session_id>'
  AND is_flagged = true
ORDER BY created_at;
```

Post-cohort aggregate:

```sql
SELECT flag_reason, COUNT(*) AS n
FROM messages
WHERE is_flagged = true
  AND created_at > '<cohort_start_iso>'
GROUP BY flag_reason
ORDER BY n DESC;

SELECT COUNT(*) AS locked_sessions
FROM sessions
WHERE flag_count >= 5
  AND started_at > '<cohort_start_iso>';
```

---

## 5. Full restart (rare)

**Preferred:** use **Full redo** on the reset UI (`POST /api/admin/sessions/reset` with `resetType: "full_redo"`) — archives transcript and creates a new session with audit + specialist email.

Alternative (learner self-serve):

1. Mark the old session completed (or leave as-is for archive).
2. Learner starts the scenario again from the home page (creates a new session if no other active session exists for that scenario).

---

## Reply template

> Hi [Name],
>
> Your simulation session has been unlocked. Please refresh the page at altius-app.adegreeabove.org and continue your conversation with [Persona name].
>
> Tip: reply in your own words, as you would on Slack — short natural messages work best. Don't paste tables from the briefing.
>
> If anything still doesn't work, reply to this email.

---

## Strike rules (reference)

Only these block reasons increment `flag_count` toward lockout (5 strikes):

- `prompt_injection`
- `gibberish`
- `structured_data`

These block send but **do not** count toward lockout:

- `empty`
- `too_long`

See `src/lib/abuse-detection.ts` and `npm test`.

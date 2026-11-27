---
schema: open-questions.v1
run_id: run_01M8E1NVV8GFP3FDJ5XQMWQ952
ticket: HMS-34
workflow: brownfield-small@1.0.0
step: impl@1.4.0
created_at: '2026-11-27T08:52:51Z'
status: answered
---

# Open questions — HMS-34 (impl step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M8E1NVV8GFP3FDJ5XQMWQ952`

## Q-01 · blocking · ask: tech lead
**Question:** CO-2 needs a new file under db/migrations, which is a sensitive path (`**/migrations/**`). May the implement step add the migration <UTC timestamp>__appointment_stale.sql that replaces the appointment.status and appointment_event.event CHECK constraints to allow STALE?
**Why it matters:** Without the migration, every STALE update and STALE event insert fails the existing CHECK constraints, so S-1 to S-4 cannot work. The change limits forbid touching a sensitive path without a question.
**Evidence:** artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/design.md#item:CO-2, tsl.yaml#L37, context/conventions.md#database-changes
**Options:**
  A. Yes, add the migration as designed (proposed default)
  B. No, the tech lead writes the migration; implement does the rest
  C. Stop: change the design
**Proposed default:** Yes, add the migration as designed (proposed default)
answer: A. Yes, add the migration as designed (proposed default)
answered_by: eng-dffd
decision: approve

## Q-02 · blocking · ask: tech lead
**Question:** Unknown U-1 needs a decision
**Proposed default:** none
answer: Same as Q-01: A. Yes, add the migration as designed
answered_by: eng-dffd
decision: approve

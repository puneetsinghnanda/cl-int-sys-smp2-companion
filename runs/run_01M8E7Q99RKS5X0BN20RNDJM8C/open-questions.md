---
schema: open-questions.v1
run_id: run_01M8E7Q99RKS5X0BN20RNDJM8C
ticket: HMS-34
workflow: brownfield-small@1.0.0
step: impl@1.4.0
created_at: '2026-11-27T10:43:04Z'
status: answered
---

# Open questions — HMS-34 (impl step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M8E7Q99RKS5X0BN20RNDJM8C`

## Q-01 · blocking · ask: tech lead
**Question:** May this run add the migration db/migrations/<UTC timestamp>__add_stale_status.sql (sensitive path **/migrations/**) that drops and re-adds the appointment status CHECK and the appointment_event event CHECK with STALE added?
**Why it matters:** Without it, markStale() fails on the status CHECK and the STALE event CHECK, so REQ-2 and REQ-8 cannot work in a real database.
**Evidence:** tsl.yaml#L37, artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/design.md#item:CO-2, sample-system:db/migrations/20261016T090300__create_appointments.sql#L10, sample-system:db/migrations/20261016T090300__create_appointments.sql#L35
**Options:**
  A. Yes, add the migration as designed
  B. No, stop the run
**Proposed default:** Yes, add the migration as designed
answer: A. Yes, add the migration as designed
answered_by: eng-dffd
decision: approve

## Q-02 · non-blocking · ask: tech lead
**Question:** Who updates the companion's context/glossary.md (new Stale entry, updated Scheduled entry) for REQ-12, since this step edits only the product repo?
**Why it matters:** The design places the glossary change in the companion repo (CO-11), which the implement step may not edit.
**Evidence:** artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/design.md#item:CO-11
**Options:**
  A. Tech lead edits it when the PR is merged
  B. Next context refresh updates it
**Proposed default:** Tech lead edits it when the PR is merged
answer: A. Tech lead edits it when the PR is merged
answered_by: eng-dffd
decision: approve

## Q-03 · blocking · ask: tech lead
**Question:** Unknown U-1 needs a decision
**Proposed default:** none
answer: Same as Q-01: A. Yes, add the migration as designed
answered_by: eng-dffd
decision: approve

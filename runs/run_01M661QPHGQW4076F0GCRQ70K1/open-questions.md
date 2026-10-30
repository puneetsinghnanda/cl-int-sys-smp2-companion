---
schema: open-questions.v1
run_id: run_01M661QPHGQW4076F0GCRQ70K1
ticket: LIVE-1
workflow: brownfield-change@1.3.0
step: req@1.0.0
created_at: '2026-10-30T09:24:05Z'
status: open
---

# Open questions — LIVE-1 (req step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M661QPHGQW4076F0GCRQ70K1`

## Q-01 · non-blocking · ask: product owner
**Question:** Should the number count every matching patient, or only the patients on the current page?
**Why it matters:** The story and the acceptance criterion read differently when the list is paged.
**Evidence:** artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L13, artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L19
**Options:**
  A. Every matching patient
  B. Only the patients on the current page
**Proposed default:** Every matching patient, as the acceptance criterion says.
answer:
answered_by:
decision:

## Q-02 · non-blocking · ask: product owner
**Question:** What should the list say when exactly one patient matches?
**Why it matters:** The ticket shows only the plural form.
**Evidence:** artifacts/LIVE-1/run_01M661QPHGQW4076F0GCRQ70K1/ticket.md#L20
**Options:**
  A. 1 patient
  B. 1 patients
**Proposed default:** 1 patient
answer:
answered_by:
decision:

## Q-03 · non-blocking · ask: tech lead
**Question:** Should "patient list" be added to the glossary?
**Why it matters:** The ticket names the screen, but the glossary does not define it.
**Evidence:** context/glossary.md#glossary
**Options:**
  A. Add it
  B. Leave it out
**Proposed default:** Leave it out; the ticket's own wording is clear enough for this change.
answer:
answered_by:
decision:

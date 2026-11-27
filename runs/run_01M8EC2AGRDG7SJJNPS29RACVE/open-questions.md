---
schema: open-questions.v1
run_id: run_01M8EC2AGRDG7SJJNPS29RACVE
ticket: HMS-34
workflow: brownfield-small@1.0.0
step: req@1.6.0
created_at: '2026-11-27T11:32:26Z'
status: open
---

# Open questions — HMS-34 (req step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M8EC2AGRDG7SJJNPS29RACVE`

## Q-01 · non-blocking · ask: tech lead
**Question:** How often should the Stale background job run, or what is the longest allowed delay between the 24-hour window ending and the appointment showing as Stale?
**Proposed default:** none
answer:
answered_by:
decision:

## Q-05 · non-blocking · ask: tech lead
**Question:** Q-05: `Stale` is not in the glossary. Add an entry?
**Why it matters:** Later steps and staff need one agreed meaning; the glossary defines every other status.
**Evidence:** context/glossary.md#scheduled, context/glossary.md#no-show
**Options:**
  A. Add: A past Scheduled appointment neither completed nor rescheduled within 24 hours of its start time, set by the background job; it can still be rescheduled or closed.
  B. Leave the glossary unchanged.
**Proposed default:** Add: A past Scheduled appointment neither completed nor rescheduled within 24 hours of its start time, set by the background job; it can still be rescheduled or closed.
answer:
answered_by:
decision:

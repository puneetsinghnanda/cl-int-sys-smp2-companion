---
schema: open-questions.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
ticket: HMS-4
workflow: brownfield-change@1.3.0
step: split@1.0.1
created_at: '2026-10-30T13:04:02Z'
status: open
---

# Open questions — HMS-4 (split step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M66D77982JFTXDREQWB5QTJV`

## Q-01 · non-blocking · ask: product owner
**Question:** Which timestamp defines the Last visit date, the appointment start time or the time staff marked it Completed?
**Why it matters:** Staff may close an appointment after it took place, which can change the date shown; S-1 assumes start time (REQ-13, REQ-15).
**Evidence:** artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md#L115, context/glossary.md#completed
**Options:**
  A. appointment start time
  B. time marked Completed
**Proposed default:** appointment start time
answer:
answered_by:
decision:

## Q-02 · non-blocking · ask: product owner
**Question:** What date format should the Last visit column use?
**Why it matters:** S-1 AC4 assumes the Next appointment column's format (REQ-14).
**Evidence:** artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md#L122
**Options:**
  A. same format as the Next appointment column
  B. a new format
**Proposed default:** same format as the Next appointment column
answer:
answered_by:
decision:

## Q-03 · non-blocking · ask: product owner
**Question:** Should the Last visit column be sortable or searchable?
**Why it matters:** No story covers sorting or searching by Last visit; S-3 keeps the current search and sort order.
**Evidence:** artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md#L97
**Options:**
  A. display only
  B. sortable
  C. sortable and filterable
**Proposed default:** display only
answer:
answered_by:
decision:

## Q-04 · non-blocking · ask: tech lead
**Question:** Should Last visit, patient list and next appointment be added to the glossary?
**Why it matters:** Agreed definitions keep tests and later steps consistent.
**Evidence:** context/glossary.md#glossary
**Options:**
  A. add all three
  B. leave as is
**Proposed default:** add all three; Last visit as defined in REQ-2
answer:
answered_by:
decision:

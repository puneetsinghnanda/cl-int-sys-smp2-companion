---
schema: intent.v1
run_id: run_01M8BRYBC8NH9QF7E248WN3MCN
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - {path: artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/ticket.md, sha256: 1f1df232c2d426563c8caab6de588a04b5f1206f275885e69a107f1405231a55}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The settings route is in `api/src/routes/health.ts`.", cites: ["artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/ticket.md#L16"]}
  - {id: C2, text: "The settings response currently holds `pageSize` among its fields.", cites: ["artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/ticket.md#L16"]}
  - {id: C3, text: "The existing day helper is in `api/src/lib/time.ts`.", cites: ["artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/ticket.md#L20"]}
  - {id: C4, text: "The hospital time zone is `Asia/Kolkata`.", cites: ["context/intent-baseline.md#constraints"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4]
known: [REQ-1, REQ-2, REQ-3, REQ-4]
assumed: []
unknown: []
requirements:
  - {id: REQ-1, title: Settings returns today, type: functional, status: known, statement: "GET /settings also returns `today`, the current day in the hospital time zone, as `YYYY-MM-DD`.", cites: ["artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/ticket.md#L19"]}
  - {id: REQ-2, title: Use the existing day helper, type: constraint, status: known, statement: "`today` is computed with the existing day helper in `api/src/lib/time.ts`.", cites: ["artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/ticket.md#L20"]}
  - {id: REQ-3, title: Day boundary follows hospital zone, type: functional, status: known, statement: "When the hospital day and the UTC day differ, in either direction, `today` is the hospital day.", cites: ["artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/ticket.md#L21-L22"]}
  - {id: REQ-4, title: Existing behaviour unchanged, type: constraint, status: known, statement: "The existing settings fields and `GET /health` are unchanged.", cites: ["artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/ticket.md#L23"]}
unknowns: []
questions: []
answers: []
glossary: []
---
# Requirements review: HMS-40

## Summary
The ticket asks the settings endpoint to also return the current day in the hospital time zone as `today`. It is meant for web pages that default to "today". An existing helper already computes the hospital-local date. The main risk is the day boundary between the hospital time zone and UTC.

## Requirements
- REQ-1, Settings returns today, known: GET /settings also returns `today` in the hospital time zone as `YYYY-MM-DD`.
- REQ-2, Use the existing day helper, known: `today` is computed with the helper in `api/src/lib/time.ts`.
- REQ-3, Day boundary follows hospital zone, known: when the hospital day and the UTC day differ, `today` is the hospital day.
- REQ-4, Existing behaviour unchanged, known: existing settings fields and `GET /health` are unchanged.

## Known
- REQ-1: ticket acceptance criterion 1.
- REQ-2: ticket acceptance criterion 1.
- REQ-3: ticket acceptance criterion 2.
- REQ-4: ticket acceptance criterion 3.

## Assumed
None

## Unknown
None

## Open questions
None

## Out of scope
- Changing web pages to use `today`: the ticket gives this as the purpose, and its acceptance criteria cover only the endpoint.

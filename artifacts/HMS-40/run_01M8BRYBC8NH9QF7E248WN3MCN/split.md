---
schema: stories.v1
run_id: run_01M8BRYBC8NH9QF7E248WN3MCN
step: split
attempt: 1
skill: story-split@1.4.0
inputs:
  - {path: artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/req.md, sha256: 47ca1b8e9da969fec60ebc20f1098937b1760993517868ad4317e127dc147e4a}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "GET /settings is to also return `today`.", cites: ["artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/req.md#item:REQ-1"]}
  - {id: C2, text: "The value is computed with the helper in `api/src/lib/time.ts`.", cites: ["artifacts/HMS-40/run_01M8BRYBC8NH9QF7E248WN3MCN/req.md#item:REQ-2"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4]
known: [S-1]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: Settings tells the web developer the hospital's current day
    trace: [REQ-1, REQ-2, REQ-3, REQ-4]
    acceptance_criteria:
      - {id: AC1, text: "Given the hospital time zone is Asia/Kolkata (UTC+05:30) and the instant is 19:30 UTC on 9 March, which is 01:00 hospital time on 10 March, when GET /settings is called, then `today` is 10 March in `YYYY-MM-DD` form."}
      - {id: AC2, text: "Given a configured hospital time zone behind UTC and an instant just after midnight in UTC that is still the previous day in that zone, when GET /settings is called, then `today` is the previous day, the hospital's day."}
      - {id: AC3, text: "Given the settings endpoint with the change, when GET /settings is called, then `timeZone`, `currency` and `pageSize` are returned as before."}
      - {id: AC4, text: "Given the change is in place, when GET /health is called, then its response is the same as before."}
---
# Stories: HMS-40

## Stories
### S-1 Settings tells the web developer the hospital's current day
Traces REQ-1, REQ-2, REQ-3, REQ-4. `today` is computed with the existing day helper (REQ-2, a constraint on the build).
- AC1: Given the hospital time zone is Asia/Kolkata (UTC+05:30) and the instant is 19:30 UTC on 9 March (01:00 hospital time on 10 March), when GET /settings is called, then `today` is 10 March in `YYYY-MM-DD` form.
- AC2: Given a configured hospital time zone behind UTC and an instant just after midnight in UTC that is still the previous day there, when GET /settings is called, then `today` is that previous day.
- AC3: Given the change, when GET /settings is called, then `timeZone`, `currency` and `pageSize` are returned as before.
- AC4: Given the change, when GET /health is called, then its response is the same as before.

## Known
- S-1: built only from known requirements REQ-1 to REQ-4.

## Assumed
None

## Unknown
None

## Open questions
None

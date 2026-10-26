---
schema: ut-result.v1
run_id: run_01M5VBPY40KETZZR1EY7H5KK0V
step: ut
attempt: 2
skill: unit-test@1.0.1
inputs:
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/impl.md
    sha256: 9d94f1a05213257a0f4efc2a50ad1445847172044216294dbcd203326c1ca9dd
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/split.md
    sha256: 8193194ed9f5168ece5dab22f4c694ae9fe98e787bfd5dd0a55ae234091fed5e
  - path: context/test-strategy.md
    sha256: 1869d3aed8521e40123c46f16539c5ea10ae5ac8c9d872ebc8a1f48b704ce150
  - path: context/generated/test-layout.json
    sha256: bd0686fe34afae476a1ec3105bcf1039ce7e5c510941d1bcdc09ab2ca8e74f3c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
code:
  - repo: sample-system
    base_sha: 5f94becca5314bf1835a06e1f8bbb1b21f345b88
    head_sha: ac33d59e8898a324f482f7bff9f6689c0b54e293
    diff_sha256: 00bdcb600be813e9f5db515428210cb382afc099aef77d82c4ec6b86bbb2f29e
    files:
      - api/test/patients-next-appointment.test.ts
resolves:
  - {id: F-1, how: "The Completed and No-show tests now read the list with the default clock (Monday 08:00), when the closed 09:00 appointment is still in the future, and first check it is shown before closing; only the status filter can then hide it."}
  - {id: F-2, how: "Added an error-path test: GET /api/patients with an invalid status filter is still rejected with a 400 field error after the change."}
  - {id: F-3, how: "Every test in the file now starts with the requirement id it covers (HMS-1 REQ-n)."}
claims:
  - id: C1
    text: "The change adds `next_appointment` to each patient list item."
    cites: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/impl.md#L66"]
  - id: C2
    text: "Database tests run against a temporary `PostgreSQL` cluster and are skipped without one."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C3
    text: "Tests run with `npm test` at the repository root."
    cites: ["context/test-strategy.md#how-to-run"]
  - id: C4
    text: "Completed or `No-show` appointments must never show in the column."
    cites: ["artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/split.md#L148"]
  - id: C5
    text: "Every requirement has a test whose name starts with its `ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
trace: [REQ-2, REQ-6, REQ-7, REQ-9, REQ-10, REQ-11, REQ-13]
known: [S-1, S-2, S-4, S-5]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Should the project add a coverage script so the verifier can report coverage for this change?"
    why: "The verifier records coverage as skipped when the project has no coverage script, so coverage base and after cannot be reported."
    evidence: ["context/test-strategy.md#how-to-run"]
    options: ["Add a coverage script", "Keep coverage out of scope"]
    default: "Keep coverage out of scope for this ticket."
---
# Unit tests: HMS-1

## Summary
`api/test/patients-next-appointment.test.ts` now covers the whole HMS-1 list change with requirement-prefixed tests (C5): null with no appointment, the earliest Scheduled appointment with its start and doctor name, a Cancelled earlier one skipped, Completed and No-show ones skipped, an Inactive doctor's name kept, past appointments ignored, paging and search unchanged, newest-first order unchanged, and an invalid list filter still rejected. The Completed and No-show tests read the list while the closed appointment is still in the future, so they prove the status filter itself (C4). These are database tests run with `npm test` against a temporary PostgreSQL cluster in the verifier (C2, C3); an earlier verifier run of the same suite reported a failure inside these database tests, which shows they execute there rather than skip. Test counts and coverage come only from the verifier: tsl records them in checks.json when it runs H3.

## Tests
- `api/test/patients-next-appointment.test.ts` — REQ-7 null with no appointment (S-2); REQ-2 earliest Scheduled with start and doctor name, and past appointments ignored (S-1); REQ-6 Cancelled, Completed and No-show skipped (S-2); REQ-13 Inactive doctor's name kept (S-4); REQ-9, REQ-10 paging and search unchanged, and REQ-10 invalid filter rejected (S-5); REQ-11 newest-first order kept (S-5). The change adds `next_appointment` (C1).

## Known
- S-1: earliest pick, start, doctor name and the past-appointment edge are each asserted on the API body (C1).
- S-2: None, Cancelled, Completed and No-show cases are asserted with the default clock, so the status filter is what excludes closed appointments (C4).
- S-4: the doctor is set Inactive and the name is still asserted.
- S-5: paging, search, order and the error path for an invalid filter are asserted.

## Assumed
- None.

## Unknown
- U-1 (non-blocking): coverage cannot be reported because the project has no coverage script (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): add a coverage script so the verifier can report coverage? Default: keep coverage out of scope for this ticket.

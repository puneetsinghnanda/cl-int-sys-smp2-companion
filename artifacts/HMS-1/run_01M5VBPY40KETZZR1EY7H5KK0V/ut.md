---
schema: ut-result.v1
run_id: run_01M5VBPY40KETZZR1EY7H5KK0V
step: ut
attempt: 1
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
    head_sha: 7eca0cd0f4fc81049b58b2c3482d84936beb1098
    diff_sha256: 435523cc7b087d262f6561bcc189b5d3254783ad361f3095f738a52781b0e300
    files:
      - api/test/patients-next-appointment.test.ts
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
trace: [REQ-2, REQ-6, REQ-11]
known: [S-2, S-5]
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
The implement step already added `api/test/patients-next-appointment.test.ts` with the regression tests the design asked for (null with no appointment, earliest Scheduled with start and doctor name, a Cancelled earlier one skipped, an Inactive doctor's name kept, past appointments ignored, paging and search unchanged). This step adds the cases its review found missing: Completed and No-show appointments are never shown, and the list order stays newest first. They are database tests, run with `npm test` against a temporary PostgreSQL cluster in the verifier (C2, C3). Test counts and coverage come only from the verifier: tsl records them in checks.json when it runs H3, so they are not repeated here.

## Tests
- `api/test/patients-next-appointment.test.ts` (closed appointments and order): a Completed appointment is skipped and the next Scheduled one shown; a No-show appointment is skipped and the next Scheduled one shown (S-2 AC3, C4); the list stays newest first (S-5 AC3).

## Known
- S-2: the tests close a started appointment as Completed or No-show and check that the column shows the patient's next Scheduled one instead (C1, C4).
- S-5: the list order is checked to stay newest first with the new field present (C1).

## Assumed
- None.

## Unknown
- U-1 (non-blocking): coverage cannot be reported because the project has no coverage script (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): add a coverage script so the verifier can report coverage? Default: keep coverage out of scope for this ticket.

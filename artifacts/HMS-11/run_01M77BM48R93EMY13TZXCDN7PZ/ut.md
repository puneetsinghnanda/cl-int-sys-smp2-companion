---
schema: ut-result.v1
run_id: run_01M77BM48R93EMY13TZXCDN7PZ
step: ut
attempt: 1
skill: unit-test@1.1.0
inputs:
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impl.md
    sha256: 4a00b08471ecde2b4d9db5168ee234536a1669de0d906f565501f522bd4ec217
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/split.md
    sha256: d4dc5ed961e49e2c861c4e3256457f48726f4adc5af1ce2363cc97491f9a4d26
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every requirement's test name starts with its ID, for example `DOC-1`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "Without a test database the harness skips database tests, via `HMS_TEST_DATABASE_URL` or local binaries."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "The verifier skips coverage because the project has no coverage script: `coverage`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "Neighbouring API tests live in `api/test/appointments.test.ts` and its siblings under api/test."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/appointments.test.ts"]
  - id: C6
    text: "The change under test adds the route on `specializationsRouter`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impl.md#claim:C1"]
  - id: C7
    text: "Name ties compare ignoring case, with `doctor id as the final tie-break`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impl.md#claim:C10"]
  - id: C8
    text: "The window is `today to today+6`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/impl.md#claim:C9"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-17]
known: [S-1, S-2, S-3, S-4, S-5]
assumed: []
unknown: []
questions: []
code:
  - repo: sample-system
    base_sha: a87388be336f15edfb340b07b2ee3256ae80c4cb
    head_sha: 9bec1812cdb99a07506bc0dec462dd2d1d1085a0
    diff_sha256: af6f062030b6ef24ef8ea3974b53d513a51d039a74dc392a0abfe3937a586b23
    files:
      - api/test/specializations-earliest-slots.test.ts
---
# Unit tests: HMS-11

## Summary
One new test file covers `GET /api/specializations/:id/earliest-slots` (C6). It follows the strategy: Vitest and Supertest against a throwaway PostgreSQL with a fixed clock (C1), test names starting with their requirement id (C2), and invented fixture data from `api/test/helpers.ts`. It has 14 tests in two database-backed suites, each with its own fresh database:

- **Normal path:** the flat entry and its exact fields, the slot matching the day schedule's first FREE slot, and the unpaged `{ items }`.
- **Slot rules:** booked slots are skipped, Cancelled appointments free the slot again, and started slots are never returned.
- **Window:** a slot on today+6 is inside, and one on today+7 gives null (C8). Hospital-zone days are checked across UTC midnight.
- **Errors:** 404 for an unknown id, 400 for a malformed id, and 400 "specialization is Inactive". Inactive doctors are left out, giving `{ items: [] }`.
- **Data protection:** each entry's key set is exactly id, code, fullName, departmentName and slot. No doctor phone and no patient name or allergies appear in the response.
- **Ordering:** earliest slot first, then name ignoring case, then id, with no-slot entries last (C7).

No production code was changed. The run data has no verifier result yet, so tests and coverage are left out here. tsl records them in checks.json when it runs the verifier (H3). Coverage is skipped by the verifier in this project (C4), and the suites are skipped where no test database is available (C3).

## Tests
- `api/test/specializations-earliest-slots.test.ts`, suite "earliest free slot per specialization (HMS-11)":
  - REQ-1/REQ-3/REQ-4: Cardiology entry with the 09:00 slot (03:30Z).
  - REQ-6: matches the day schedule's first FREE slot.
  - REQ-10: `{ items }` only.
  - REQ-14/REQ-15: exact key set, no doctor phone.
  - REQ-6: a booked slot is skipped, a Cancelled one is free again, and no patient data appears.
  - REQ-7: a started slot is never returned.
  - REQ-2: today+6 is inside the window.
  - REQ-2/REQ-5: today+7 gives a null slot.
  - REQ-2: the window follows the hospital zone across UTC midnight.
  - REQ-12: 404 for an unknown id, and 400 for a malformed id.
  - REQ-1/REQ-11: Inactive doctors are left out, giving `{ items: [] }`.
  - REQ-13: 400 "specialization is Inactive".
- Same file, suite "earliest free slot ordering (HMS-11)":
  - REQ-8/REQ-9/REQ-17: Dr. Zed (08:30), then dr. alpha, Dr. Beta and Dr. Test Doctor One (all 09:00), then dr. amy, Dr. Bob, dr. kim and Dr. Kim with null slots. dr. kim comes before Dr. Kim by lower id.

## Known
- S-1: covered by the entry, day-schedule match, booked and cancelled tests.
- S-2: covered by the started-slot, today+6, today+7 and UTC-midnight tests.
- S-3: covered by the ordering suite.
- S-4: covered by the unpaged, 404, 400 and empty-items tests.
- S-5: covered by the key-set and no-personal-data tests.

## Assumed
None.

## Unknown
None.

## Open questions
None.

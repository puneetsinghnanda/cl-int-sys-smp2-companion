---
schema: ut-result.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
step: ut
attempt: 1
skill: unit-test@1.1.0
inputs:
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/impl.md
  sha256: 76ccc918ec49f6c63a2895b0ee2a9e14e10e11da16a4b7788892428ba3d7b0d4
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/split.md
  sha256: 1097c5a30f050a771deca3a06a8eb429ba26a1b31dd0aada1164d17b95a31518
- path: context/test-strategy.md
  sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
- path: context/generated/test-layout.json
  sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
- repo: sample-system
  base_sha: e9f0ab1fd26535e9607fa31ac78e756378fabf9b
  head_sha: fa5594d8811c51f108648f994f9eeae8076ae5e8
  diff_sha256: 0b8ae765b823aa374f03804e71f19d9495733d5878775ec8fa01d775ea203480
  files:
  - api/test/patients-last-visit.test.ts
  - web/src/__tests__/PatientsPage.test.tsx
claims:
- id: C1
  text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
  cites: ["context/test-strategy.md#layers"]
- id: C2
  text: "Web tests use Vitest with jsdom and `Testing Library`."
  cites: ["context/test-strategy.md#layers"]
- id: C3
  text: "Every requirement has a test whose name starts with its `ID`."
  cites: ["context/test-strategy.md#requirement-ids"]
- id: C4
  text: "Without a PostgreSQL binary or `HMS_TEST_DATABASE_URL`, database tests are skipped."
  cites: ["context/test-strategy.md#database-for-tests"]
- id: C5
  text: "The verifier skips `coverage`, so no run reports it."
  cites: ["context/test-strategy.md#gaps"]
- id: C6
  text: "The change gives each list item `lastVisitAt`, null when the patient has no Completed appointment."
  cites: ["artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/impl.md#claim:C7"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
assumed: []
unknown:
- {id: U-1, blocking: false}
questions: []
---
# Unit tests: HMS-4

## Summary
These tests extend the HMS-4 tests that implement added, covering the normal path, the edge and error paths, and the story acceptance criteria. They also close the gaps the implement review found: an exact time-zone check, the 23:45 case and a real 21-patient paging test.
- API tests use Vitest and Supertest with a fixed clock (C1), and their names start with the REQ id (C3).
- Web tests use Testing Library (C2).
- No production code was changed.

Test counts and coverage are not in this artifact. tsl records them in checks.json when it runs the verifier (H3), and the verifier reports no coverage for this project (C5). Database tests are skipped where no PostgreSQL is available (C4).

## Tests
- `api/test/patients-last-visit.test.ts`, first describe (the 2-patient fixture), extended:
  - REQ-13: a patient with medical history and a Completed visit still has no medical-history values in the list.
  - REQ-10 (error path): an invalid status filter is still rejected with a field error.
- `api/test/patients-last-visit.test.ts`, new describe "paging and search" (S-3 AC1 and AC2):
  - The set-up has 21 patients. The 1st and 20th rows (patients 21 and 2) each have two Completed, one Cancelled and one No-show appointment.
  - REQ-9, REQ-11: page one holds 20 rows, page two holds only the 21st, and every patient appears exactly once.
  - REQ-2, REQ-3: those rows show their latest Completed start, and a patient with no visit shows null.
  - REQ-10: a search returns each matching patient once, with its Last visit value.
  - REQ-11, REQ-15: the order stays newest first, not by Last visit.
- `web/src/__tests__/PatientsPage.test.tsx`, "last visit column (HMS-4)", extended:
  - REQ-5 (S-1 AC2): 19:30 UTC on 9 March shows "10 Mar 2026" and "01:00", never "09 Mar" or "19:30".
  - REQ-5 (S-1 AC3): 18:15 UTC on 10 March shows "10 Mar 2026" and "23:45".
  - REQ-5 (edge path): an invalid hospital zone in settings falls back to Asia/Kolkata, as Next appointment does.
  - REQ-14, REQ-15: the Last visit header has no button, no link and no aria-sort, so it offers no sort or filter.
- Already in place from implement, unchanged:
  - REQ-1: the column header.
  - REQ-6: the same format as Next appointment.
  - REQ-8: "-" for null.
  - REQ-7: Cancelled, No-show and Scheduled appointments are ignored.
  - REQ-12: the Next appointment cell; the HMS-2 tests read the second-to-last cell.

## Known
- REQ-1, REQ-6, REQ-8: web tests on the header, the shared format and the "-" (C6).
- REQ-2, REQ-3, REQ-4: API tests check the latest Completed start, whatever the closing order.
- REQ-5: exact 01:00 and 23:45 Asia/Kolkata checks, plus the invalid-zone fallback.
- REQ-7: API test with later Cancelled, No-show and Scheduled appointments.
- REQ-9, REQ-10, REQ-11: the 21-patient paging test, the search test and the order test.
- REQ-12: the HMS-2 Next appointment tests still pass against the same cell.
- REQ-13: the PAT-8 key-set test and the HMS-4 no-medical-history test.
- REQ-14, REQ-15: the header test (no control) and the order test.

## Assumed
None.

## Unknown
- U-1 (non-blocking): pass/fail results come only from the verifier (H3). The API tests run only where PostgreSQL 16 is available (C4).

## Open questions
None.

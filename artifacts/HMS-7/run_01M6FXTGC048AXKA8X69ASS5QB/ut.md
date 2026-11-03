---
schema: ut-result.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: ut
attempt: 2
skill: unit-test@1.0.1
inputs:
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impl.md
    sha256: 62586154589c341a2efe7ee5696ec741d288b17b601ec7084da8a07b98468299
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/split.md
    sha256: 4efeb373d4d22b7d74c2c0ec55e82009f5ddb45abe3a9bc94a490c762f071910
  - path: context/test-strategy.md
    sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
  - path: context/generated/test-layout.json
    sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: 676aad38a615151dc650b7c745d666fa6c6bce38
    head_sha: b9aec8427cf14195264c79d41a35f289ce047cc3
    diff_sha256: bfdf9cd27e9d613efe06af2e0248902f326560e0dedd3917dfd0db85652c3624
    files:
      - api/test/doctors-no-shows.test.ts
      - api/test/periods.test.ts
      - web/src/__tests__/DoctorDetailPage.test.tsx
claims:
  - id: C1
    text: "API tests use `Supertest` against a throwaway PostgreSQL with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use `jsdom` and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Without a test database, database tests are `skipped`."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "Test names start with the requirement `ID`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C5
    text: "The verifier skips `coverage`, so no run reports it."
    cites: ["context/test-strategy.md#gaps"]
  - id: C6
    text: "The change adds the shared rule `lastDays`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impl.md#summary"]
  - id: C7
    text: "The change adds the repository query `countByDoctor`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impl.md#summary"]
  - id: C8
    text: "The page builds the link with `noShowsQuery`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impl.md#summary"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [S-1, S-2, REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-7

## Summary
Retry for the H3 failure in attempt 1: four DoctorDetailPage tests failed because the test's fetch
stub answered the day-schedule request (`/api/doctors/:id/slots`) with the doctor object, so the page
crashed reading `slots`. The test was wrong, not the code: the stub now matches `/slots`. No production
code changed, and no other test changed.

Tests cover every changed function and the one changed endpoint, with a normal and an edge or error
path each, named with HMS-7 requirement ids as the test strategy asks:
- `lastDays` (pure, no database, always runs): the 30-day window matches the dashboard's 30d period, "today" is the hospital day, and 1 day equals the Today period.
- GET /api/doctors/:id with `countByDoctor` (Supertest on its own throwaway database): count 1 for doctor 1, Completed and Scheduled not counted, 0 for doctor 2, equal to the dashboard 30d count and window, the linked list total equals the count (also for 0), counted on day 30 and gone on day 31, unknown id still 404, a PATCH carrying the read-only count is ignored, and no patient contact or medical data in the new fields.
- DoctorDetailPage and `noShowsQuery` (jsdom): the labelled row sits in the details list, the exact link URL for 3 and for 0 (review finding F-1 on impl), the other sections still render, and the exact parameter names.

Results: tsl records the test run in checks.json when it runs the verifier (H3); no result is in this step's data, so tests and coverage are not reported here. The verifier does not measure coverage for this project. Database tests are skipped where no PostgreSQL is available; the verifier container provides one.

## Tests
- api/test/periods.test.ts — Shared No-show window rule `lastDays`: REQ-2/REQ-4 window, REQ-3 hospital day, REQ-2 1-day edge.
- api/test/doctors-no-shows.test.ts — DoctorService, AppointmentRepo.countByDoctor and GET /api/doctors/:id: REQ-1, REQ-2, REQ-3 edge, REQ-4, REQ-5, REQ-6/REQ-7 list total, REQ-8 404, PATCH and no personal data.
- web/src/__tests__/DoctorDetailPage.test.tsx — DoctorDetailPage and `noShowsQuery`: REQ-1 label, REQ-6 exact link, REQ-5/REQ-7 zero link, REQ-8 other sections. Fetch stub fixed to answer `/slots` with a day schedule.

## Known
- S-1: covered by the API count tests and the page label test.
- S-2: covered by the list-total test and the exact-link tests.
- REQ-1: label in the details list (web).
- REQ-2: count of NO_SHOW only; Completed and Scheduled excluded (API); window (pure).
- REQ-3: hospital-day "today" (pure) and the day-30/day-31 edge (API).
- REQ-4: equals the dashboard 30d count and window (API, pure).
- REQ-5: 0 for a doctor with none (API, web).
- REQ-6: exact link URL (web) and list total equals the count (API).
- REQ-7: 0 is still a link (web) and its list total is 0 (API).
- REQ-8: other sections still render (web); 404 and PATCH unchanged (API).

## Assumed
None.

## Unknown
None.

## Open questions
None.

---
schema: ut-result.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
step: ut
attempt: 2
skill: unit-test@1.1.0
inputs:
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/impl.md
    sha256: 64eabc5d66a4d1a05ab0755635b9faf8985e6940de2426991f73b10a90101aad
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/split.md
    sha256: 27a56fb6c5c77620388a68804cade76e555a471888785e875f98064fab7c3d95
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: 57a01551ebf3a7429a78830822e19e5384fc83b3f91c6b101bbcfb9cec22038f
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
code:
  - repo: sample-system
    base_sha: 2f3154df19709798a39553fc45513a3e929f65bc
    head_sha: 8b14d85d0f78212453bec45c2b9b83413d954c9c
    diff_sha256: 09df28a3ba40e2bb35dfe8ca68d03c82453cb78b8a2bb984ed07655c44c46983
    files:
      - api/test/appointments.test.ts
      - web/src/__tests__/AppointmentsPage.test.tsx
resolves:
  - id: F-1
    how: "A new API suite inserts one Scheduled, Completed, No-show and latest-starting Cancelled row for one doctor and day and asserts exactly the three non-cancelled ids."
  - id: F-2
    how: "A new API test asserts today's NOT_CANCELLED list equals the doctors list's today_count, including a 00:30 local appointment."
  - id: F-5
    how: "A second Clear All test starts with doctor, patient, status and dates all set."
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16; web tests use Vitest with jsdom and Testing Library."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Every requirement has a test whose name `starts with its ID`, so the new tests are named HMS-8 REQ-n."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C3
    text: "Without PostgreSQL binaries or `HMS_TEST_DATABASE_URL`, database tests are skipped."
    cites: ["context/test-strategy.md#database-for-tests"]
  - id: C4
    text: "The verifier skips coverage because the project has `no coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "The API test clock is fixed at `2026-10-19T02:30:00Z` (Monday 08:00 in Asia/Kolkata), so MONDAY is 'today' for the Today count."
    cites: ["sample-system:api/test/helpers.ts#L10"]
  - id: C6
    text: "Neighbouring web page tests stub `fetch` with vi.stubGlobal and render inside a MemoryRouter."
    cites: ["sample-system:web/src/__tests__/DoctorsPage.test.tsx#L27"]
  - id: C7
    text: "The test helper maps local times with a fixed offset, because Kolkata is `UTC+05:30` all year."
    cites: ["sample-system:api/test/helpers.ts#L13"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-14]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-14]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-8

## Summary
This step adds unit tests for every function the change touched, with a normal path and an error or edge path for each. No production code changed.
- **API:** `AppointmentService.list` and `AppointmentRepo.list` (CO-5, CO-6), tested with Supertest (C1).
- **Web:** AppointmentsPage (CO-1), tested with Testing Library in the style of the neighbouring page tests (C6).

The DoctorsPage Today link test (CO-3, CO-4) was already updated in the implement step.

Test names start with `HMS-8 REQ-n` (C2), and all test data is invented.

This retry fixes the review findings:
- **F-1:** A new API suite inserts one Scheduled, Completed, No-show and latest-starting Cancelled row for one doctor and day and asserts exactly the three non-cancelled ids.
- **F-2:** A new API test asserts today's NOT_CANCELLED list equals the doctors list's today_count, including a 00:30 local appointment.
- **F-5:** A second Clear All test starts with doctor, patient, status and dates all set.

It also fixes the two minor web findings:
- The mock now answers each `/api/doctors/:id` with its own doctor, and a test opens an Inactive doctor from a link.
- The "not the id" checks now look for the bare id text.

No verifier result was available yet, so tests and coverage are left out. tsl records them in checks.json when it runs the verifier (H3). Coverage is not measured in this project (C4), and the database tests skip without PostgreSQL (C3).

## Tests
**`api/test/appointments.test.ts`**, existing suite:
- "HMS-8 REQ-8: any other status value is a 400 on status". This is the error path.

**`api/test/appointments.test.ts`**, new suite "HMS-8 Any except Cancelled (status=NOT_CANCELLED)". It runs on its own fresh database, and its rows are inserted directly so closed statuses can be set up:
- "HMS-8 REQ-7, REQ-8: lists Scheduled, Completed and No-show but not Cancelled, even when Cancelled starts last". This covers S-4 AC1 and AC2:
  - For doctor 2 on one day, exactly the Scheduled, Completed and No-show ids come back, in start order.
  - The latest-starting Cancelled row is left out.
  - The same filter with any status returns 4.
- "HMS-8 REQ-9, REQ-10: today's NOT_CANCELLED list equals the doctors list's Today count, including 00:30 local". This covers S-5 AC1 and AC2. The clock is Monday (C5), and Kolkata is UTC+05:30 all year (C7):
  - 00:30 local on Monday is 19:00 UTC on the previous UTC date.
  - Doctor 1 has two Scheduled appointments today (00:30 and 09:00), one Cancelled appointment today, and one appointment at 23:45 on the evening before.
  - today_count is 2, and the NOT_CANCELLED list for today returns the same two ids.

**`web/src/__tests__/AppointmentsPage.test.tsx`**:
- REQ-1: a doctorId from the URL shows the doctor's label, and no bare id is shown.
- REQ-1, REQ-3: an Inactive doctor opened from a link shows its name (S-1 AC4).
- REQ-2: the doctor search box shows with no doctor filter, and Clear All is hidden.
- REQ-3: the chooser offers an Active and an Inactive doctor, and its searches send no status.
- REQ-4, REQ-5, REQ-14: "Change" clears only doctorId, and choosing another doctor sets doctorId=2. Both keep the patient, status and dates.
- REQ-6: Clear All with doctor, patient, status and dates set clears everything, and the button then hides.
- REQ-6: Clear All with only status and dates set also clears everything.
- REQ-7, REQ-8: choosing "Any except Cancelled" sends `status=NOT_CANCELLED` and keeps doctorId.
- REQ-8: a URL with `status=NOT_CANCELLED` opens with that option selected.

## Known
- REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-14: AppointmentsPage tests for the Picker and label.
- REQ-6: the two Clear All tests.
- REQ-7, REQ-8: the API NOT_CANCELLED and 400 tests, and the web status option tests.
- REQ-9, REQ-10: the API test comparing today's NOT_CANCELLED list with today_count. The Today href itself is pinned by the DoctorsPage test from the implement step.

## Assumed
None.

## Unknown
None.

## Open questions
None.

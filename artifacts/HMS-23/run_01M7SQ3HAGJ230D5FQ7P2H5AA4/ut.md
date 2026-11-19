---
schema: ut-result.v1
run_id: run_01M7SQ3HAGJ230D5FQ7P2H5AA4
step: ut
attempt: 1
skill: unit-test@1.3.0
inputs:
  - path: artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/impl.md
    sha256: 20681f0cdaf8c6dd6aa3000da161829ff3d487cbb97cc36ed2a30942e76c25ae
  - path: artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/split.md
    sha256: da8271707531844c5e53000e0ed06d5a8c0f9c7f3dcf626770f37cc163f6aae6
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: c0752ab8543e23547679dbe34f6381f656d36b0eb5506efb2a56f94dcb729cd6
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: c6df7c171d71c04d73c1155db0fa0979200f3357
    head_sha: 89b19ad293069577e5160fa869010f86b816e932
    diff_sha256: e0f2d177a2cfb5a8b4b195a3a05baba997b6c5d3f63de835e7438e4c10820ea6
    files:
      - api/test/dashboard-on-duty.test.ts
      - web/src/__tests__/DashboardPage.test.tsx
claims:
  - id: C1
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL with a fixed clock."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "Web tests use Vitest with jsdom and `Testing Library`."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "Every requirement has a test whose name starts with its ID, for example `DOC-1`."
    cites: ["context/test-strategy.md#requirement-ids"]
  - id: C4
    text: "The verifier skips coverage because the project has `no coverage script`."
    cites: ["context/test-strategy.md#gaps"]
  - id: C5
    text: "Tests run with a fixed now of `2026-10-19T02:30:00Z`, Monday 08:00 in Kolkata."
    cites: ["sample-system:api/test/helpers.ts#L10"]
  - id: C6
    text: "The at helper maps local HH:MM to a fixed UTC instant because Kolkata is `UTC+05:30` all year."
    cites: ["sample-system:api/test/helpers.ts#L13-L14"]
  - id: C7
    text: "The fixture's doctor 2 has `30-minute slots` on Monday 09:00 to 10:00 only."
    cites: ["sample-system:api/test/helpers.ts#L23"]
  - id: C8
    text: "The day schedule is served by the `/:id/slots` route, which the counts are compared against."
    cites: ["sample-system:api/src/routes/doctors.ts#L63"]
  - id: C9
    text: "The change adds an `onDuty` array to GET /api/dashboard."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/impl.md#claim:C1"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
---
# Unit tests: HMS-23

## Summary
These tests cover the `onDuty` change (C9) at both layers the test strategy names.
- **API:** Vitest and Supertest against a throwaway PostgreSQL with a fixed clock (C1). They use the shared fixture and clock helpers (C5, C6, C7), plus one invented doctor, "Dr. A Test Doctor Three", added to create a tie on free slots.
- **Web:** Vitest and Testing Library with a stubbed `fetch` (C2).
- **Names:** every test name starts with `HMS-23 REQ-n` (C3).

Each changed function gets a normal-path test and at least one edge test:
- `DoctorService.onDutyToday` and `DoctorRepo.activeOnWeekday`: through `GET /api/dashboard`.
- `DashboardService.get`: through `GET /api/dashboard`.
- `DashboardPage`: rendered directly.

No production code was edited. I didn't run the tests myself: tsl records the test results in `checks.json` when the verifier runs (H3). Coverage is not reported, because the project has no coverage script (C4).

## Tests
- `api/test/dashboard-on-duty.test.ts` (new):
  - **REQ-2, REQ-3, REQ-13:** the row's exact shape (name, department, booked, free, `nextFreeAt`, `nextFreeLabel`), with no phone or email.
  - **REQ-9:** sorts by free slots, most first, and ties go by name, not id ("Dr. A Test Doctor Three" comes before doctor 2).
  - **REQ-2:** at 00:30 Tuesday local time (19:00 UTC Monday), only Tuesday's doctors are listed.
  - **REQ-10:** Saturday gives an empty `onDuty`.
  - **REQ-12:** `period=7d` and `period=30d` give the same `onDuty`.
  - **REQ-5, REQ-6:** at exactly 09:30, the 09:30 slot is neither free nor the next free slot; the next is 09:45.
  - **REQ-7:** a doctor whose slots have all passed is still listed with 0 free and null next slot.
  - **REQ-9 edge:** with every doctor at 0 free, all are listed in name order.
  - **REQ-4:** with Completed, No-show and Cancelled appointments, booked and free equal the day schedule's BOOKED and FREE counts from `/:id/slots` (C8). They are 2 booked and 10 free, and the cancelled slot is free again.
  - **REQ-6:** the next free slot skips a slot that is now booked.
  - **REQ-11:** an Inactive doctor isn't listed, even though they have the most free slots.
  - **REQ-7** (separate database): a doctor whose remaining slots are all booked shows 0 free and a null next slot.
- `web/src/__tests__/DashboardPage.test.tsx` (extended):
  - **REQ-1, REQ-3, REQ-8, REQ-9:** the "On duty today" rows show in API order, each name links to `/doctors/:id`, and the next free label is shown.
  - **REQ-14:** the section sits between the tiles and "Next appointments".
  - **REQ-7:** a row with 0 free slots shows "Fully booked".
  - **REQ-10:** with no rows, the section shows "No doctors on duty today" and no table.

## Known
- **S-1:** covered by the REQ-2, REQ-3, REQ-10, REQ-11 and REQ-13 API tests and the REQ-1, REQ-3 and REQ-10 web tests.
- **S-2:** covered by the REQ-4, REQ-5 and REQ-6 API tests, including the slot that starts exactly now.
- **S-3:** covered by both REQ-7 API tests (all slots passed, and all slots booked) and the REQ-7 web test.
- **S-4:** covered by the REQ-9 API tests (sort order, ties by name, all at 0) and the web order test.
- **S-5:** covered by the web link test (`/doctors/3`).
- **S-6:** covered by the REQ-12 and REQ-13 API tests and the REQ-14 web test. The existing DSH tests in `api/test/dashboard.test.ts` still cover the tiles and Next appointments.

## Assumed
None.

## Unknown
None.

## Open questions
None.

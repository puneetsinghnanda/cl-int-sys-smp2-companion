---
schema: test-plan.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
step: tplan
attempt: 1
skill: test-plan@1.1.0
inputs:
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/split.md
    sha256: 27a56fb6c5c77620388a68804cade76e555a471888785e875f98064fab7c3d95
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/design.md
    sha256: 42358b2515450494074ee4e85508c7791b3c755e3e822e4ffc47d3c48b0c96a7
  - path: context/test-strategy.md
    sha256: f4bbc9d5d63dfc433c9fe466a7aa71a4fad0147baafe1f47db0c06e117ce6cba
  - path: context/generated/test-layout.json
    sha256: 57a01551ebf3a7429a78830822e19e5384fc83b3f91c6b101bbcfb9cec22038f
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "Web tests use Vitest with jsdom and `Testing Library`, so page behaviour is tested at unit level without a browser."
    cites: ["context/test-strategy.md#layers"]
  - id: C2
    text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock, which is the integration level here."
    cites: ["context/test-strategy.md#layers"]
  - id: C3
    text: "No page is tested `end to end` in a browser, so no e2e level is available."
    cites: ["context/test-strategy.md#gaps"]
  - id: C4
    text: "An existing dashboard test pins the `Appointments today` tile href."
    cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L26"]
  - id: C5
    text: "An existing doctor page test pins the No-shows href with `status=NO_SHOW`."
    cites: ["sample-system:web/src/__tests__/DoctorDetailPage.test.tsx#L49"]
  - id: C6
    text: "The existing APT-9 test checks `page_size` and start order, but not a 21st row."
    cites: ["sample-system:api/test/appointments.test.ts#L90"]
  - id: C7
    text: "Lists answer at `20 per page`."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/design.md#claim:C13"]
  - id: C8
    text: "The design's only new input value is `status=NOT_CANCELLED` on GET /api/appointments."
    cites: ["runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-04"]
  - id: C9
    text: "Hospital time is `Asia/Kolkata` (UTC+05:30), so the Today boundary is local midnight."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/split.md#claim:C1"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [TP-1, TP-2, TP-3, TP-4, TP-5, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16, TP-17, TP-18, TP-19, TP-20]
assumed: []
unknown: []
questions: []
tests:
  - {id: TP-1, title: "A doctor filter from a link shows the doctor's name and no bare id", covers: [S-1/AC1], level: unit}
  - {id: TP-2, title: "With no doctor filter the doctor chooser is shown with no doctor chosen", covers: [S-1/AC2], level: unit}
  - {id: TP-3, title: "The chooser offers an Active and an Inactive doctor and searches without a status", covers: [S-1/AC3], level: unit}
  - {id: TP-4, title: "An Inactive doctor opened from a link shows its name", covers: [S-1/AC4], level: unit}
  - {id: TP-5, title: "Choosing another doctor keeps patient, status and dates", covers: [S-2/AC1], level: unit}
  - {id: TP-6, title: "Clearing the doctor from the chooser keeps patient, status and dates", covers: [S-2/AC2], level: unit}
  - {id: TP-7, title: "Clearing a doctor-only filter leaves the list request with no filters", covers: [S-2/AC3], level: unit}
  - {id: TP-8, title: "Clear All with doctor, patient, status and dates set clears every filter", covers: [S-3/AC1], level: unit}
  - {id: TP-9, title: "Clear All with only status and dates set clears them too", covers: [S-3/AC2], level: unit}
  - {id: TP-10, title: "NOT_CANCELLED returns the Scheduled, Completed and No-show rows of a day, not the latest-starting Cancelled one", covers: [S-4/AC1, S-4/AC2], level: integration}
  - {id: TP-11, title: "Choosing Any except Cancelled puts status=NOT_CANCELLED in the URL and the list request", covers: [S-4/AC3], level: unit}
  - {id: TP-12, title: "A URL with status=NOT_CANCELLED opens with Any except Cancelled selected", covers: [S-4/AC4], level: unit}
  - {id: TP-13, title: "Negative: an unknown status value is a 400 with a field error on status", covers: [S-4/AC3], level: integration}
  - {id: TP-14, title: "The doctors list's Today link carries status=NOT_CANCELLED with doctorId and today's dates", covers: [S-5/AC1], level: unit}
  - {id: TP-15, title: "Today's NOT_CANCELLED list for a doctor equals today_count, including a 00:30 local appointment and excluding the evening before", covers: [S-5/AC1, S-5/AC2], level: integration}
  - {id: TP-16, title: "A doctor whose only appointment today is Cancelled has today_count 0 and an empty NOT_CANCELLED list", covers: [S-5/AC3], level: integration}
  - {id: TP-17, title: "Regression: the doctor page's No-shows link keeps its href", covers: [S-6/AC1], level: unit}
  - {id: TP-18, title: "Regression: the dashboard's Appointments today tile keeps its href", covers: [S-6/AC2], level: unit}
  - {id: TP-19, title: "Regression: with 21 matching appointments page 1 holds 20 and the 21st is on page 2", covers: [S-6/AC3], level: integration}
  - {id: TP-20, title: "Regression: an appointment booked first but starting later is listed after the earlier-starting one", covers: [S-6/AC4], level: integration}
---
# Test plan: HMS-8

## Scope
**In scope:**
- The appointment list page: doctor chooser, label, Clear All and the Any except Cancelled option.
- The doctors list's Today link.
- The `status=NOT_CANCELLED` value on GET /api/appointments (C8).
- Regressions in the other list links, page size and order.

**Levels:**
- Page behaviour is tested at unit level with Vitest, jsdom and Testing Library (C1).
- API behaviour is tested at integration level with Supertest against a throwaway PostgreSQL with a fixed clock (C2).
- No browser e2e exists in this project (C3), and no criterion needs one.

**Out of scope:** browser-level checks of the Picker's look, and load or performance.

## Tests
- TP-1 (unit, S-1/AC1): a doctor filter from a link shows the name and no bare id.
- TP-2 (unit, S-1/AC2): with no doctor filter, the chooser is shown with no doctor chosen.
- TP-3 (unit, S-1/AC3): the chooser offers an Active and an Inactive doctor and searches without a status.
- TP-4 (unit, S-1/AC4): an Inactive doctor opened from a link shows its name.
- TP-5 (unit, S-2/AC1): choosing another doctor keeps the patient, status and dates.
- TP-6 (unit, S-2/AC2): clearing the doctor keeps the patient, status and dates.
- TP-7 (unit, S-2/AC3): clearing a doctor-only filter leaves the list request with no filters.
- TP-8 (unit, S-3/AC1): Clear All with every filter set clears them all.
- TP-9 (unit, S-3/AC2): Clear All with only status and dates set clears them too.
- TP-10 (integration, S-4/AC1, S-4/AC2): NOT_CANCELLED returns the Scheduled, Completed and No-show rows of a day. The latest-starting Cancelled row is left out.
- TP-11 (unit, S-4/AC3): choosing Any except Cancelled puts `status=NOT_CANCELLED` in the URL and the list request.
- TP-12 (unit, S-4/AC4): a URL with `status=NOT_CANCELLED` opens with the option selected.
- TP-13 (integration, S-4/AC3): negative test for the new status value (C8). An unknown status is a 400 with a field error on status.
- TP-14 (unit, S-5/AC1): the Today link carries `status=NOT_CANCELLED` with doctorId and today's dates.
- TP-15 (integration, S-5/AC1, S-5/AC2): today's NOT_CANCELLED list equals today_count.
  - It includes 00:30 local. Asia/Kolkata is UTC+05:30, so that is 19:00 UTC on the previous UTC date (C9).
  - It excludes an appointment at 23:45 the evening before.
- TP-16 (integration, S-5/AC3): when a doctor's only appointment today is Cancelled, today_count is 0 and the list is empty.
- TP-17 (unit, S-6/AC1): regression. The No-shows href is unchanged, pinned by the existing doctor page test (C5).
- TP-18 (unit, S-6/AC2): regression. The Appointments today tile href is unchanged, pinned by the existing dashboard test (C4).
- TP-19 (integration, S-6/AC3): regression. With 21 matches, page 1 holds 20 (C7) and the 21st is on page 2. This extends the existing APT-9 check (C6).
- TP-20 (integration, S-6/AC4): regression. An appointment booked first but starting later is listed second.

Every criterion from S-1/AC1 to S-6/AC4 appears in at least one covers list.

## Known
- TP-1 to TP-9, TP-11, TP-12, TP-14: page behaviour at the unit level the strategy names (C1).
- TP-10, TP-13, TP-15, TP-16, TP-19, TP-20: API behaviour at the integration level the strategy names (C2).
- TP-17, TP-18: the existing regression tests are reused (C4, C5).

## Assumed
None.

## Unknown
None.

## Open questions
None.

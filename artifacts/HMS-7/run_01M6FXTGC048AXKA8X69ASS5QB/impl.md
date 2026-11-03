---
schema: change.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: impl
attempt: 1
skill: implement@1.0.1
inputs:
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md
    sha256: 215531f1ce96acf3cbf36175d23fe9f75a7d793b88397e524abe7591fb3b1056
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/split.md
    sha256: 4efeb373d4d22b7d74c2c0ec55e82009f5ddb45abe3a9bc94a490c762f071910
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: 6d88e2202ad87c8fe3eb3a342f5929d43fe12376
    head_sha: 167491d37c6fec518d430e15eab8b153d6c1cf5b
    diff_sha256: 09adfa997b64d7793ea00529f936ced0331f8b6807120b7c6bae2c1df60e3a40
    files:
      - api/src/lib/periods.ts
      - api/src/repositories/appointmentRepo.ts
      - api/src/services/dashboardService.ts
      - api/src/services/doctorService.ts
      - api/src/types.ts
      - web/src/api/types.ts
      - web/src/pages/DoctorDetailPage.tsx
claims:
  - id: C1
    text: "Shared helpers live in `api/src/lib`."
    cites: ["context/conventions.md#code-layout"]
  - id: C2
    text: "SQL lives in `api/src/repositories`."
    cites: ["context/conventions.md#code-layout"]
  - id: C3
    text: "Rules live in `api/src/services`."
    cites: ["context/conventions.md#code-layout"]
  - id: C4
    text: "The design adds the response field `noShowsLast30Days`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#overview"]
  - id: C5
    text: "The design adds the response field `noShowsWindow` with from and to as local days."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#overview"]
  - id: C6
    text: "The link filter name is `doctorId`, spelled exactly."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#overview"]
  - id: C7
    text: "The shared rule is the dashboard's `30d` window."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#overview"]
  - id: C8
    text: "The dashboard service is `DashboardService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C9
    text: "Doctor rules live in `DoctorService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C10
    text: "Appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C11
    text: "The doctor page is `DoctorDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage"]
  - id: C12
    text: "The edit form maps a doctor with `fromDoctor`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:fromDoctor"]
  - id: C13
    text: "The doctor page label is `No-shows (last 30 days)`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/split.md#stories"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [S-1, S-2, REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown: []
questions: []
summary: "Adds a read-only No-shows (last 30 days) count and its window to GET /api/doctors/:id, computed by a shared lastDays rule extracted unchanged from the dashboard's period logic, and shows it on the doctor page as a link to the filtered appointment list."
---
# Change: HMS-7

## Summary
- Shared No-show window rule (new): `api/src/lib/periods.ts` exports `lastDays(now, tz, days)`, the dashboard's period rule moved unchanged: to = today in the hospital zone, from = to − (days − 1), and [start of from, end of to) as UTC instants.
- DashboardService (changed): uses `lastDays` for its today/7d/30d period; the returned `today`, `period` and counts are computed exactly as before. Existing tests `api/test/dashboard.test.ts` and `api/test/dashboard-no-shows.test.ts` cover the 30d period and its list link.
- AppointmentRepo (changed): `countByDoctor(doctorId, status, from, to)` counts one doctor's appointments with that status starting in [from, to), with bind parameters only (threat T-3), served by the doctor/start index.
- DoctorService (changed): `get` adds `noShowsLast30Days` (NO_SHOW, `lastDays(…, 30)`, same as the dashboard's 30d) and `noShowsWindow { from, to }` as local days. create, update and replaceAvailability return `get`, so they carry the fields too. No patient fields are added (threat T-5).
- DoctorDetailPage (changed): a "No-shows (last 30 days)" row in the details list, always a link (0 included) to `/appointments?doctorId=…&status=NO_SHOW&from=…&to=…`, built with URLSearchParams by the exported `noShowsQuery` (threat T-9). Nothing else on the page changes.
- DoctorFormPage and PATCH /api/doctors/:id (existing, unchanged): checked in source. fromDoctor copies only named form fields, and DoctorService.read for PATCH reads only named fields, so the new read-only fields never reach the form or an update (design Q-03, threat T-10). No stripping code was needed.
- Tests: the design asks only that the dashboard stay guarded by its existing tests; no tests were added here. The count, the link URL and the window edges are left to the unit-test step.

## Changes
- api/src/lib/periods.ts — new: Shared No-show window rule (`lastDays`, `DayWindow`).
- api/src/services/dashboardService.ts — DashboardService: period window from `lastDays`.
- api/src/repositories/appointmentRepo.ts — AppointmentRepo: `countByDoctor`.
- api/src/services/doctorService.ts — DoctorService: `noShowsLast30Days`, `noShowsWindow` in the doctor detail.
- api/src/types.ts — DoctorDetail type gains the two read-only fields (GET /api/doctors/:id changed).
- web/src/api/types.ts — web DoctorDetail type gains the same fields (API client).
- web/src/pages/DoctorDetailPage.tsx — DoctorDetailPage: the labelled count and link, `noShowsQuery`.

## Known
- S-1: count shown in the details section via DoctorService and DoctorDetailPage.
- S-2: the count links to the appointment list with doctorId, NO_SHOW, from and to.
- REQ-1: label "No-shows (last 30 days)" in the doctor details list.
- REQ-2, REQ-3, REQ-4: NO_SHOW appointments starting in the dashboard's 30d window in the hospital zone, via the shared rule.
- REQ-5: an integer count; 0 when none.
- REQ-6, REQ-7: the count is always a link, including 0, with the same window the count used.
- REQ-8: only one row added to the page; other sections untouched.

## Assumed
None.

## Unknown
None. Design Q-03 was settled by reading fromDoctor and DoctorService.read (see Summary).

## Open questions
None.

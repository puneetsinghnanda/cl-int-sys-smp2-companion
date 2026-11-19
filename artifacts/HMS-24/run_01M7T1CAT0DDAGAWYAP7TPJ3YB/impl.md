---
schema: change.v1
run_id: run_01M7T1CAT0DDAGAWYAP7TPJ3YB
step: impl
attempt: 1
skill: implement@1.3.0
inputs:
  - path: artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/design.md
    sha256: ece73c2b935e0fb3e3f720a1e0892a85d673b03ceb7e0251564784a28052be5a
  - path: artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/split.md
    sha256: 7d2c8670cb2f4c61269fc2b9aaf0af2b59ba9a8d61e0be58f13a334769407a85
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: af548711151fad0648e2a4358f180c5798dc71e6
    diff_sha256: c3193b4ea3f3ecf447b5dba9ce1e89921b611e7ece144caee45515b1c8adfca9
    files:
      - api/src/repositories/patientRepo.ts
      - api/src/services/patientService.ts
      - api/src/types.ts
      - web/src/api/types.ts
      - web/src/components/VisitSummary.tsx
      - web/src/lib/format.ts
      - web/src/pages/PatientDetailPage.tsx
claims:
  - id: C1
    text: "`PatientRepo` in api/src/repositories/patientRepo.ts gains the visitSummary query (CO-1)."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C2
    text: "`PatientService` get() adds visitSummary to the patient detail (CO-2)."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C3
    text: "The API type `PatientDetail` gains visitSummary, typed by a new VisitSummary interface (CO-3)."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail"]
  - id: C4
    text: "The web mirror of `PatientDetail` gains the same field (CO-4)."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail"]
  - id: C5
    text: "The new localDate helper sits next to `dateTime` in web/src/lib/format.ts and uses its day, month and year style without the time (CO-5)."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
  - id: C6
    text: "`PatientDetailPage` renders the summary between the Appointments heading and the table (CO-7)."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]
  - id: C7
    text: "The last visit day is computed with the existing `localDay` helper in the hospital time zone."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]
  - id: C8
    text: "The summary UI is the new component file `web/src/components/VisitSummary.tsx` (CO-6)."
    cites: ["runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-06"]
  - id: C9
    text: "The response field is named `visitSummary` in camelCase."
    cites: ["runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-04"]
  - id: C10
    text: "lastVisit.date is sent as a hospital-local `YYYY-MM-DD` day."
    cites: ["runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-05"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7]
assumed: []
unknown: []
questions: []
summary: "GET /api/patients/:id now returns visitSummary: completed, noShow, cancelled and upcoming counts over all of the patient's appointments, plus the most recent Completed appointment as a hospital-local YYYY-MM-DD day, id and doctor name. A new VisitSummary component shows it directly above the patient page's Appointments table, or No visits yet when the patient has no appointments."
---
# Change: HMS-24

## Summary
This change builds the approved design for HMS-24 in 7 files, inside the change limits:
- CO-1 `PatientRepo.visitSummary` (C1) runs one `count(*) FILTER` query over all of the patient's
  appointments, with no row limit. Upcoming is `SCHEDULED AND starts_at > now`. A second query takes
  the latest COMPLETED appointment by `starts_at DESC, id DESC` and joins the doctor's name. Its day
  comes from `localDay` (C7) in the configured hospital time zone. Neither query selects `reason`,
  `cancellation_reason` or patient medical columns.
- CO-2 `PatientService.get` (C2) reads the clock once and returns `visitSummary` beside
  `appointments`. POST and PATCH go through `get`, so they return it too, as the design notes.
- CO-3 and CO-4 add the `VisitSummary` type and the `visitSummary` field (C9) on the API and web
  sides (C3, C4).
- CO-5 adds `localDate(day)` to format.ts (C5). It prints a `YYYY-MM-DD` day (C10) as
  "10 Mar 2026". It formats in UTC on purpose, because the day is already hospital-local.
- CO-6 adds the new `VisitSummary` component (C8). It shows the counts line
  "N completed · N no-show · N cancelled · N upcoming", always all four. Below it,
  "Last visit: <date link>, <doctor name>" links only the date, to `/appointments/:id`, or shows
  "Last visit: none yet". A patient with no appointments sees "No visits yet" instead.
- CO-7 `PatientDetailPage` (C6) renders the component after the Appointments heading and before the
  table. It passes `hasAppointments` from `appointments.length`. Nothing else on the page changes.

No tests are added: the design asks for no regression test. The unit-test step covers the stories.

## Changes
- api/src/repositories/patientRepo.ts: CO-1, new `visitSummary(id, now, timeZone)` method and `localDay` import.
- api/src/services/patientService.ts: CO-2, `get` returns `visitSummary`.
- api/src/types.ts: CO-3, new `VisitSummary` interface; `PatientDetail.visitSummary`.
- web/src/api/types.ts: CO-4, mirrors CO-3.
- web/src/lib/format.ts: CO-5, new `localDate(day)`.
- web/src/components/VisitSummary.tsx: CO-6, new component.
- web/src/pages/PatientDetailPage.tsx: CO-7, renders `VisitSummary` above `AppointmentTable`.

## Known
- CO-1 to CO-5 and CO-7: built exactly as the design describes, on the symbols the module map lists (C1 to C7).
- CO-6: new file at the location confirmed in the IDE (C8).

## Assumed
None.

## Unknown
None.

## Open questions
None.

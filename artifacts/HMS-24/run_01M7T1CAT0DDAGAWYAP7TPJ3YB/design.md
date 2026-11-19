---
schema: design.v1
run_id: run_01M7T1CAT0DDAGAWYAP7TPJ3YB
step: design
attempt: 1
skill: design@1.9.0
inputs:
  - path: artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/split.md
    sha256: 7d2c8670cb2f4c61269fc2b9aaf0af2b59ba9a8d61e0be58f13a334769407a85
  - path: artifacts/HMS-24/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/req.md
    sha256: 13b81d643b95fe75f614a4a6b02720f9fa3b75987ebe1a8b922ae11f0929beec
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "GET /api/patients/:id is served by the patients router in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id"]
  - id: C2
    text: "The API response type `PatientDetail` is declared in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail"]
  - id: C3
    text: "`PatientRepo` holds the patient SQL in api/src/repositories/patientRepo.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C4
    text: "`PatientService` holds the patient rules in api/src/services/patientService.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C5
    text: "`PatientDetailPage` renders the patient page in web/src/pages/PatientDetailPage.tsx."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]
  - id: C6
    text: "The web app mirrors the response type `PatientDetail` in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail"]
  - id: C7
    text: "The appointment table formats instants with `dateTime` from web/src/lib/format.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
  - id: C8
    text: "`localDay` in api/src/lib/time.ts turns an instant into a hospital-local day."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]
  - id: C9
    text: "The summary UI lives in a new component file `web/src/components/VisitSummary.tsx`, rendered by the patient page."
    cites: ["runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-06"]
  - id: C10
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C11
    text: "Instants are stored in UTC and \"today\" uses the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C12
    text: "`AppointmentTable` renders the patient page's appointment rows in web/src/components/AppointmentTable.tsx."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/AppointmentTable.tsx:AppointmentTable"]
  - id: C13
    text: "Each appointment has its own page at `/appointments/:id`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]
  - id: C14
    text: "The response field is named `visitSummary` in camelCase."
    cites: ["runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-04"]
  - id: C15
    text: "The last visit date is sent as a hospital-local `YYYY-MM-DD` day."
    cites: ["runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-05"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, CO-9, EP-1]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: "PatientRepo.visitSummary query"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-05"]
  - id: CO-2
    name: "PatientService.get: adds visitSummary to the PatientDetail it returns"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: CO-3
    name: "api/src/types.ts: new VisitSummary interface; PatientDetail gains visitSummary"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-04"]
  - id: CO-4
    name: "web/src/api/types.ts: mirror VisitSummary and PatientDetail.visitSummary"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-04"]
  - id: CO-5
    name: "format.ts localDate helper"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-01", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-05"]
  - id: CO-6
    name: "VisitSummary component"
    module: module:sample-system:web
    change: new
    cites: ["runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-06", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-02", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-03"]
  - id: CO-7
    name: "PatientDetailPage: renders VisitSummary between the Appointments heading and AppointmentTable"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]
  - id: CO-8
    name: "AppointmentTable: unchanged"
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/AppointmentTable.tsx:AppointmentTable"]
  - id: CO-9
    name: "web API client (api.patient): unchanged; it already returns PatientDetail"
    module: module:sample-system:web
    change: existing
    cites: ["context/architecture.md#components"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/patients/:id
    change: changed
    fields:
      - {name: visitSummary, type: object, nullable: false, format: "object with the five fields below; always present", example: "{\"completed\":4,\"noShow\":1,\"cancelled\":2,\"upcoming\":1,\"lastVisit\":{\"appointmentId\":42,\"date\":\"2026-11-05\",\"doctorName\":\"Dr A\"}}", status: known}
      - {name: visitSummary.completed, type: integer, nullable: false, format: "count of the patient's appointments with status COMPLETED, all doctors and dates", example: 4, status: known}
      - {name: visitSummary.noShow, type: integer, nullable: false, format: "count of the patient's appointments with status NO_SHOW, all doctors and dates", example: 1, status: known}
      - {name: visitSummary.cancelled, type: integer, nullable: false, format: "count of the patient's appointments with status CANCELLED, all doctors and dates, including ones whose start is after now", example: 2, status: known}
      - {name: visitSummary.upcoming, type: integer, nullable: false, format: "count of the patient's appointments with status SCHEDULED and starts_at strictly after the request time", example: 1, status: known}
      - {name: visitSummary.lastVisit, type: object, nullable: true, format: "the COMPLETED appointment with the latest starts_at (ties by highest id); null when the patient has none", example: "{\"appointmentId\":42,\"date\":\"2026-11-05\",\"doctorName\":\"Dr A\"}", status: known}
      - {name: visitSummary.lastVisit.appointmentId, type: integer, nullable: false, format: "appointment row id, used for the /appointments/:id link", example: 42, status: known}
      - {name: visitSummary.lastVisit.date, type: string, nullable: false, format: "YYYY-MM-DD, the hospital-local day of starts_at (HOSPITAL_TZ)", example: "2026-03-10", status: known}
      - {name: visitSummary.lastVisit.doctorName, type: string, nullable: false, format: "doctor full name as stored", example: "Dr A", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-04", "runs/run_01M7T1CAT0DDAGAWYAP7TPJ3YB/ide-answers.md#a-05"]
answers:
  - id: A-04
    kind: question
    about: Q-01
    answer: "visitSummary camelCase (proposed default)"
    decides: [EP-1, CO-3, CO-4, C14]
  - id: A-05
    kind: question
    about: Q-02
    answer: "Local YYYY-MM-DD (proposed default)"
    decides: [EP-1, CO-1, CO-5, C15]
  - id: A-06
    kind: assumption
    about: C9
    answer: "Yes, confirm"
    decides: [C9, CO-6]
---
# Design: HMS-24

## Overview
The patient detail response (EP-1, `GET /api/patients/:id`, C1) gains a `visitSummary` object
(A-04, C14). It holds four counts and the last Completed visit. A new `VisitSummary` component
(CO-6, C9) renders it on the patient page between the Appointments heading and the existing
`AppointmentTable` (CO-7, CO-8). No new endpoint, table, migration or infrastructure is added.

How it works:
- CO-1 adds `PatientRepo.visitSummary`. One `count(*) FILTER (WHERE ...)` query runs over all of
  the patient's appointments, with no row limit, so the counts cover every doctor and date (S-2).
  A second query takes the latest COMPLETED appointment by `starts_at DESC, id DESC` and joins the
  doctor's name (S-3 AC3, AC4). Both queries select only status, start, id and doctor name: never
  `reason`, `cancellation_reason` or patient medical columns (S-5 AC1, AC2).
- Upcoming means SCHEDULED with a start strictly after the request time (`now` from the service
  clock). A past, still-open Scheduled appointment and a future Cancelled one are not upcoming
  (S-1 AC3, AC5).
- The last visit's day is computed on the server with `localDay` (C8) in the hospital time zone
  (C11) and sent as `YYYY-MM-DD` (A-05, C15). The web app only reformats it with a new
  `localDate` helper (CO-5) to "10 Mar 2026". It shows no time (A-01), unlike `dateTime` (C7).
- The page shows "No visits yet" when `appointments` is empty. That field is already in the
  response and the page already uses it, so no new total field is needed. A patient whose only
  appointment is a past, unclosed Scheduled one still has appointments, so they see zero counts
  rather than "No visits yet".
- The doctor name is plain text and only the date links to `/appointments/:id` (C13, A-03).
  All four counts always show, zeros included (A-02).

Trade-offs:
- Server-side counts vs counting in the browser: counting the loaded rows would miss appointments
  beyond the patient page's row limit (S-2 AC2). One extra small query per page view is cheap.
- Server-side local day vs a UTC instant: the time-zone rule stays in one tested place on the
  server, and the UTC-previous-day case (S-3 AC2) can be asserted exactly on the API.
- Adding a field to `PatientDetail` vs a new endpoint: no extra round trip and no change to the
  client (CO-9, C10). POST and PATCH on patients also return `PatientDetail`, so they gain the
  same read-only field. The change only adds fields, so existing callers are unaffected (REQ-14).

Story coverage: S-1 by CO-1, CO-2, CO-3, CO-4, CO-6 and CO-7; S-2 by CO-1; S-3 by CO-1, CO-5 and
CO-6; S-4 by CO-6 and CO-7; S-5 by CO-1, CO-6 and CO-7, with CO-8 and CO-9 unchanged.

## Components
- CO-1 `PatientRepo.visitSummary` (api, changed): count query plus latest Completed query, with the day computed by `localDay`.
- CO-2 `PatientService.get` (api, changed): adds `visitSummary` to the returned PatientDetail.
- CO-3 api/src/types.ts (api, changed): new `VisitSummary` interface; `PatientDetail.visitSummary`.
- CO-4 web/src/api/types.ts (web, changed): mirrors CO-3.
- CO-5 web/src/lib/format.ts (web, changed): new `localDate(day)` helper, date only.
- CO-6 web/src/components/VisitSummary.tsx (web, new): counts line, Last visit line, empty states.
- CO-7 `PatientDetailPage` (web, changed): renders CO-6 directly above the Appointments table.
- CO-8 `AppointmentTable` (web, existing): unchanged.
- CO-9 web API client (web, existing): unchanged.
- EP-1 `GET /api/patients/:id` (changed). Fields:
  - `visitSummary` (object, not null): always present.
  - `visitSummary.completed` (integer): COMPLETED count, all doctors and dates, e.g. 4.
  - `visitSummary.noShow` (integer): NO_SHOW count, e.g. 1.
  - `visitSummary.cancelled` (integer): CANCELLED count, including future-dated ones, e.g. 2.
  - `visitSummary.upcoming` (integer): SCHEDULED with a start strictly after the request time, e.g. 1.
  - `visitSummary.lastVisit` (object, nullable): the latest COMPLETED appointment; null when there is none.
  - `visitSummary.lastVisit.appointmentId` (integer): e.g. 42.
  - `visitSummary.lastVisit.date` (string, `YYYY-MM-DD` hospital-local): e.g. "2026-03-10".
  - `visitSummary.lastVisit.doctorName` (string): e.g. "Dr A".

## Known
- CO-1, CO-2, CO-3, CO-4, CO-5, CO-7, CO-8: their files and symbols are module-map facts (C2 to C8, C12).
- CO-6: new file, location confirmed in the IDE (A-06, C9).
- CO-9: the client is the only path to the API (C10); unchanged.
- EP-1: route exists (C1); field names and date format decided in the IDE (A-04, A-05).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-04 (Q-01): field name and case? Answer: "visitSummary camelCase (proposed default)". Decides EP-1, CO-3, CO-4 and C14.
- A-05 (Q-02): format of the last visit date? Answer: "Local YYYY-MM-DD (proposed default)". Decides EP-1, CO-1, CO-5 and C15.
- A-06 (C9): is the summary a new component file `web/src/components/VisitSummary.tsx`? Answer: "Yes, confirm". Decides C9 and CO-6.

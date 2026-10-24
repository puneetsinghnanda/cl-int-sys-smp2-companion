---
schema: design.v1
run_id: run_01M5HF2TJRVWS776C5G07JE7NH
step: design
attempt: 1
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/impact.md
    sha256: 26f8e197dc27be22625f471d111541da33d8ae9fdb4e83ee60b515c24d333650
  - path: artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/split.md
    sha256: e56eb28249f0c18cd413a5fe48e8d5d272945a4d4e3976dedf148faf3690c637
  - path: artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/req.md
    sha256: b8f74a55a75818106966c7b4ca51081d509c09577bfc68cee5cea3f9ec42d7a4
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 31d9ca15805fc1b6488e8da89fdb0ee7714adbcdfda176d69581ab8f3d6482da
  - path: context/generated/api-inventory.json
    sha256: 8dfdbde2e1d64359cb29b8f710a2a467d5e19b383d90def7c856e7738d4ca104
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "The patient list is served by the existing route `/api/patients`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C2
    text: "The API is layered into routes, services and repositories; the web app calls it only through `client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C3
    text: "SQL lives in `api/src/repositories` and rules in `api/src/services`."
    cites: ["context/conventions.md#code-layout"]
  - id: C4
    text: "Lists answer items, page, page_size and total, `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C5
    text: "Instants are stored in `UTC`; display uses the hospital time zone."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C6
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C7
    text: "Medical history never appears in `lists`."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C8
    text: "Logs never carry `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C9
    text: "Patient list SQL lives in `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C10
    text: "Patient list rules live in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C11
    text: "Patient routes are defined in `patientsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter"]
  - id: C12
    text: "The web patient list page is `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C13
    text: "The web API client is the `api` symbol in client.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
  - id: C14
    text: "The web app has a date-time formatter `dateTime`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
  - id: C15
    text: "The appointment page is routed at `/appointments/:id`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]
  - id: C16
    text: "The appointment page component is `AppointmentDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: C17
    text: "The appointment table has the index `appointment_patient_start_idx` for per-patient lookups by start."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C18
    text: "Appointment rows are otherwise read through `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C19
    text: "Doctors can be deleted through the route `/api/doctors/:id`."
    cites: ["context/generated/api-inventory.json#http:DELETE:/api/doctors/:id"]
  - id: C20
    text: "The appointment page is backed by the route `/api/appointments/:id`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-1, S-2, S-3, S-5]
assumed: [S-4]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
  - {id: U-3, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Should the Next appointment column use the app's existing dateTime format in the hospital time zone?"
    why: "S-1 AC3 needs an exact expected string, and the intent leaves the format open."
    evidence: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/req.md#L124"]
    options: ["Reuse dateTime", "Specify a new format"]
    default: "Reuse dateTime with the hospital time zone."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "What does DELETE /api/doctors/:id do for a doctor who has Scheduled appointments: reject, set Inactive, or cascade?"
    why: "S-4 assumes the doctor's name is always available for the next appointment; a cascade or hard delete would change that."
    evidence: ["context/generated/api-inventory.json#http:DELETE:/api/doctors/:id", "artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/split.md#L152"]
    options: ["Rejects the delete", "Sets the doctor Inactive", "Cascades"]
    default: "Assume it rejects or sets Inactive, so the name stays readable; no change to the delete route in this ticket."
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Are the patient list's search and sort applied in the GET /api/patients query?"
    why: "The design adds the next appointment inside that query; search, sort and paging must keep their current results (S-5)."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "context/conventions.md#lists"]
    options: ["Yes, server side", "No, in PatientsPage"]
    default: "Assume server side and add regression tests on GET /api/patients."
components:
  - {name: patientsRouter, module: module:sample-system:api, change: existing}
  - {name: PatientService, module: module:sample-system:api, change: changed}
  - {name: PatientRepo, module: module:sample-system:api, change: changed}
  - {name: AppointmentRepo, module: module:sample-system:api, change: existing}
  - {name: api client (client.ts), module: module:sample-system:web, change: changed}
  - {name: PatientsPage, module: module:sample-system:web, change: changed}
  - {name: dateTime formatter, module: module:sample-system:web, change: existing}
  - {name: AppointmentDetailPage, module: module:sample-system:web, change: existing}
endpoints:
  - {method: GET, path: /api/patients, change: changed}
  - {method: GET, path: /api/appointments/:id, change: existing}
---
# Design: HMS-1

## Overview
The patient list gets its next appointment from the existing `GET /api/patients` route, so no new endpoint, table, migration or job is added. `PatientRepo` extends the list query: for each patient on the page it selects the earliest appointment with status Scheduled whose start is after the current instant, with that appointment's id, start and doctor name; this lookup uses the existing `appointment_patient_start_idx`. `PatientService` maps the result onto each list item (empty when there is none), and `patientsRouter` is unchanged because the path and parameters stay the same. In the web app the `api` client's patient list type gains the new fields, and `PatientsPage` adds a "Next appointment" column that shows the start in the hospital time zone (Asia/Kolkata) with the existing `dateTime` formatter plus the doctor's name, links to `/appointments/:id`, and shows "None" as plain text when there is no next appointment.

Trade-offs:
- Inside the list query, not a second call per patient: one round trip per page and an existing index, at the cost of a wider list query that must keep page size, search and sort unchanged.
- Widen the existing route, not a new endpoint: no new route to secure or document, but the list response shape grows for every caller of `GET /api/patients`.
- Compare against the current instant in UTC and format on the web: matches how instants are stored, so "after now" does not depend on time zone; only display uses the hospital zone.
- Keep `AppointmentRepo` unchanged: the lookup is a join inside the patient list SQL, so appointment code paths are untouched; the cost is that this one appointment query lives in `PatientRepo`.

## Components
- patientsRouter (module:sample-system:api) — existing: same path and parameters; passes the widened result through.
- PatientService (module:sample-system:api) — changed: adds the next appointment (id, start, doctor name) to each list item, or leaves it empty.
- PatientRepo (module:sample-system:api) — changed: the list query selects the earliest Scheduled appointment starting after now and its doctor's name; it never selects medical history columns.
- AppointmentRepo (module:sample-system:api) — existing: not used for this lookup.
- api client (client.ts) (module:sample-system:web) — changed: the patient list item type gains the next-appointment fields.
- PatientsPage (module:sample-system:web) — changed: new "Next appointment" column, link to the appointment page, "None" when empty.
- dateTime formatter (module:sample-system:web) — existing: reused to show the start in the hospital time zone.
- AppointmentDetailPage (module:sample-system:web) — existing: link target at `/appointments/:id`.
- GET /api/patients — changed: each list item gains the next appointment; still 20 per page with the same search and sort.
- GET /api/appointments/:id — existing: unchanged; backs the appointment page the column links to.

## Known
- S-1: served by PatientRepo, PatientService, the api client and PatientsPage; times are stored in UTC and shown in Asia/Kolkata with dateTime (C1, C5, C6, C9, C10, C12, C13, C14).
- S-2: the PatientRepo query filters to status Scheduled after now, so Cancelled, Completed and No-show are never picked; an empty result shows "None" (C9, C17).
- S-3: PatientsPage links the value to the appointment page at `/appointments/:id`, backed by GET /api/appointments/:id (C15, C16, C20).
- S-5: the route path, parameters and list shape stay the same, 20 per page; PatientRepo never selects medical history and logs never carry it (C4, C7, C8, C11).

## Assumed
- S-4: the doctor's name is read through the appointment's doctor whether or not the doctor is Inactive. Basis: Inactive doctors are kept for history (glossary, REQ-13). This holds only if deleting a doctor with Scheduled appointments does not remove the row (C19); see Q-02.

## Unknown
- U-1 (non-blocking): the exact display format of the start date and time (Q-01).
- U-2 (non-blocking): how DELETE /api/doctors/:id treats a doctor with Scheduled appointments (Q-02).
- U-3 (non-blocking): whether search and sort run in the GET /api/patients query (Q-03).

## Open questions
- Q-01 (non-blocking, product owner): reuse the existing dateTime format in the hospital time zone? Default: yes.
- Q-02 (non-blocking, tech lead): does deleting a doctor with Scheduled appointments reject, set Inactive or cascade? Default: assume it rejects or sets Inactive; no change to that route here.
- Q-03 (non-blocking, tech lead): are search and sort server side in GET /api/patients? Default: assume yes and add regression tests on that route.

---
schema: design.v1
run_id: run_01M7HYHSSG24BWQN7R2M4AZRF3
step: design
attempt: 2
skill: design@1.8.0
inputs:
  - path: artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/split.md
    sha256: 37e1c1b4a8f6927e7237a6d3526934d298e5ce3a870e6da718f067bf13f3507d
  - path: artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/req.md
    sha256: 3054686c1ee1ec1e6f05993d831044fb058e06c509ab0d9300f743d21b2ac834
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
resolves:
  - id: F-1
    how: "The list keeps its single lateral-join query per page and get runs the same shared SQL fragment for one patient, so no per-row calls are added."
  - id: F-2
    how: "CO-1 changes the repository get return type to omit both appointments and next_appointment."
claims:
  - id: C1
    text: "The patient page is rendered by `PatientDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]
  - id: C2
    text: "The patient detail endpoint `/api/patients/:id` exists (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id"]
  - id: C3
    text: "The API already declares the HMS-1 type `NextAppointment`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:NextAppointment"]
  - id: C4
    text: "The web app mirrors it as `NextAppointment`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:NextAppointment"]
  - id: C5
    text: "The API response type for the patient page is `PatientDetail`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail"]
  - id: C6
    text: "The web mirror of the patient page response is `PatientDetail`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail"]
  - id: C7
    text: "The HMS-1 list query picks the next appointment with status `'SCHEDULED'` and a start strictly after now."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L129"]
  - id: C8
    text: "The HMS-1 list query orders candidates by `a.starts_at, a.id` and keeps the first."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L130"]
  - id: C9
    text: "Patient SQL lives in `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C10
    text: "Patient rules live in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C11
    text: "The service's get builds the detail response using the injected clock `this.clock()`."
    cites: ["sample-system:api/src/services/patientService.ts#L42"]
  - id: C12
    text: "Create and update return the patient through `this.get`, so they return the same detail shape."
    cites: ["sample-system:api/src/services/patientService.ts#L58", "sample-system:api/src/services/patientService.ts#L69"]
  - id: C13
    text: "The create endpoint `/api/patients` exists (POST)."
    cites: ["context/generated/api-inventory.json#http:POST:/api/patients"]
  - id: C14
    text: "The update endpoint `/api/patients/:id` exists (PATCH)."
    cites: ["context/generated/api-inventory.json#http:PATCH:/api/patients/:id"]
  - id: C15
    text: "The Appointments table prints the When column with `dateTime`."
    cites: ["sample-system:web/src/components/AppointmentTable.tsx#L34"]
  - id: C16
    text: "The table takes the time zone from `useSettings`."
    cites: ["sample-system:web/src/components/AppointmentTable.tsx#L14"]
  - id: C17
    text: "Instants are stored in `UTC` and local times use the hospital time zone."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C18
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C19
    text: "The patient page renders the Appointments table with `AppointmentTable`."
    cites: ["sample-system:web/src/pages/PatientDetailPage.tsx#L76"]
  - id: C20
    text: "The patient page shows the `Book appointment` button in its page head."
    cites: ["sample-system:web/src/pages/PatientDetailPage.tsx#L42"]
  - id: C21
    text: "The next appointment comes from the API as a `next_appointment` field (`API field (proposed default)`)."
    cites: ["runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-03"]
  - id: C22
    text: "The date and time text is the link to the appointment detail page (`Date and time (proposed default)`)."
    cites: ["runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-04"]
  - id: C23
    text: "The appointment detail page route is `/appointments/:id`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]
  - id: C24
    text: "The HMS-1 list loads next appointments `in one query per page`."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L100"]
  - id: C25
    text: "The list joins the next appointment with a `LEFT JOIN LATERAL` subquery."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L125"]
  - id: C26
    text: "The repository get returns `Omit<PatientDetail, 'appointments'>`, which must also omit the new field."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L148"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, EP-1, EP-2, EP-3]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: "PatientRepo — shared next-appointment SQL fragment, used by list and nextAppointment(id, now)"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "sample-system:api/src/repositories/patientRepo.ts#L129", "sample-system:api/src/repositories/patientRepo.ts#L100", "sample-system:api/src/repositories/patientRepo.ts#L148", "runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-03"]
  - id: CO-2
    name: "PatientService.get — one now from this.clock() for appointments and next_appointment"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "sample-system:api/src/services/patientService.ts#L42", "runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-03"]
  - id: CO-3
    name: "API type PatientDetail — adds next_appointment: NextAppointment | null"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientDetail", "context/generated/module-map.json#sym:sample-system:api/src/types.ts:NextAppointment", "runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-03"]
  - id: CO-4
    name: "Web type PatientDetail — mirrors next_appointment: NextAppointment | null"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientDetail", "context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:NextAppointment", "runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-03"]
  - id: CO-5
    name: "PatientDetailPage — Next appointment line between the Appointments heading and AppointmentTable"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage", "sample-system:web/src/pages/PatientDetailPage.tsx#L76", "runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-04"]
  - id: CO-6
    name: "dateTime formatter (with useSettings timeZone) — reused unchanged for the line"
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "sample-system:web/src/components/AppointmentTable.tsx#L34"]
  - id: CO-7
    name: "AppointmentTable and the Book appointment button — unchanged"
    module: module:sample-system:web
    change: existing
    cites: ["sample-system:web/src/pages/PatientDetailPage.tsx#L76", "sample-system:web/src/pages/PatientDetailPage.tsx#L42"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/patients/:id
    change: changed
    fields:
      - {name: next_appointment, type: "NextAppointment object", nullable: true, format: "null when the patient has no Scheduled appointment starting strictly after the request instant", example: "{\"id\": 42, \"starts_at\": \"2027-03-15T04:30:00.000Z\", \"doctor_name\": \"<doctor full name>\"}", status: known}
      - {name: next_appointment.id, type: integer, nullable: false, format: "appointment id, used for /appointments/:id", example: "42", status: known}
      - {name: next_appointment.starts_at, type: string, nullable: false, format: "ISO 8601 UTC instant (Date.toISOString), shown in the hospital time zone by dateTime", example: "2027-03-15T04:30:00.000Z", status: known}
      - {name: next_appointment.doctor_name, type: string, nullable: false, format: "doctor full_name", example: "<doctor full name>", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id", "context/generated/module-map.json#sym:sample-system:api/src/types.ts:NextAppointment", "runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-03"]
  - id: EP-2
    method: POST
    path: /api/patients
    change: changed
    fields:
      - {name: next_appointment, type: "NextAppointment object", nullable: true, format: "same as EP-1; null for a new patient", example: "null", status: known}
    cites: ["context/generated/api-inventory.json#http:POST:/api/patients", "sample-system:api/src/services/patientService.ts#L58", "runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-03"]
  - id: EP-3
    method: PATCH
    path: /api/patients/:id
    change: changed
    fields:
      - {name: next_appointment, type: "NextAppointment object", nullable: true, format: "same as EP-1", example: "{\"id\": 42, \"starts_at\": \"2027-03-15T04:30:00.000Z\", \"doctor_name\": \"<doctor full name>\"}", status: known}
    cites: ["context/generated/api-inventory.json#http:PATCH:/api/patients/:id", "sample-system:api/src/services/patientService.ts#L69", "runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-03"]
answers:
  - id: A-03
    kind: question
    about: Q-01
    answer: "API field (proposed default)"
    decides: [CO-1, CO-2, CO-3, CO-4, EP-1, EP-2, EP-3, C21]
  - id: A-04
    kind: question
    about: Q-02
    answer: "Date and time (proposed default)"
    decides: [CO-5, C22]
---
# Design: HMS-15

## Overview
The API computes the next appointment and the web page shows it. `GET /api/patients/:id` gains a nullable `next_appointment` field of the existing HMS-1 type `NextAppointment` (`{id, starts_at, doctor_name}`), as the user chose in A-03.

The selection rule from HMS-1 becomes one shared SQL fragment in `PatientRepo`: status `'SCHEDULED'`, `a.starts_at > now`, ordered by `a.starts_at, a.id`, first row.
- `list` keeps using the fragment inside its existing single `LEFT JOIN LATERAL` query per page (C24, C25).
- A new `PatientRepo.nextAppointment(id, now)` runs the same fragment for one patient, and `get` uses it.

So the patients list and the patient page always agree. The list keeps its single lateral-join query per page and get runs the same shared SQL fragment for one patient, so no per-row calls are added.

`PatientService.get` reads `now = this.clock()` once and passes it to both `appointments` and `nextAppointment`. That instant is the request time, which is page-load time (REQ-10). The comparison is strictly after now, so an appointment that starts exactly at now, or has already started, is never next (REQ-7). Closed statuses are never matched (REQ-6). Instants stay in UTC (C17).

On the web, `PatientDetailPage` renders the line between the "Appointments" heading and `AppointmentTable`:
- with an appointment: "Next appointment:" followed by a `Link` to `/appointments/{id}` whose text is `dateTime(starts_at, timeZone)`, with `timeZone` from `useSettings`, exactly as the table does. Then the doctor's name follows as plain text (A-04).
- without one: "No upcoming appointment" as plain text.

`AppointmentTable` and the Book appointment button are not touched (REQ-8). Create and update return through `this.get`, so their responses gain the same field (EP-2, EP-3) without separate code.

Trade-offs:
- Server versus client: computing on the server reuses tested HMS-1 SQL and the server clock, so there is no browser clock skew. The cost is one extra indexed lookup per detail request.
- Shared fragment versus per-id calls: the list stays one query per page, at the cost of the SQL being spliced in two places.
- Naming: `next_appointment` and `starts_at` are snake_case inside a mostly camelCase `PatientDetail`. We reuse them unchanged so the list and the detail share one type, which is consistent but mixes cases.
- No database change: the existing columns and query cover everything, so no migration is needed.

## Components
- CO-1 `PatientRepo` (changed, api):
  - one shared next-appointment SQL fragment (status 'SCHEDULED', starts_at > now, ORDER BY starts_at, id, LIMIT 1), which `list` keeps using in its single `LEFT JOIN LATERAL` query per page;
  - new `nextAppointment(id, now)` that runs the same fragment for one patient;
  - CO-1 changes the repository get return type to omit both appointments and next_appointment (`Omit<PatientDetail, 'appointments' | 'next_appointment'>`).
- CO-2 `PatientService.get` (changed, api): `const now = this.clock()` once, passed to both `appointments` and `nextAppointment`; adds `next_appointment` to the response.
- CO-3 API `PatientDetail` (changed, api): add `next_appointment: NextAppointment | null`.
- CO-4 Web `PatientDetail` (changed, web): mirror `next_appointment: NextAppointment | null`.
- CO-5 `PatientDetailPage` (changed, web): the Next appointment line or "No upcoming appointment", placed between the Appointments heading and `AppointmentTable`; the date/time text links to `/appointments/:id`.
- CO-6 `dateTime` with the `useSettings` time zone (existing, web): reused, so the format matches the table.
- CO-7 `AppointmentTable` and the Book appointment button (existing, web): unchanged.
- EP-1 GET /api/patients/:id (changed), new fields:
  - `next_appointment`: NextAppointment object, nullable, null when no Scheduled appointment starts strictly after the request instant.
  - `next_appointment.id`: integer, the appointment id, used for /appointments/:id; example 42.
  - `next_appointment.starts_at`: string, ISO 8601 UTC instant; example 2027-03-15T04:30:00.000Z, shown via dateTime in the hospital time zone.
  - `next_appointment.doctor_name`: string, the doctor's full name.
- EP-2 POST /api/patients (changed): the same `next_appointment` field, null for a new patient.
- EP-3 PATCH /api/patients/:id (changed): the same `next_appointment` field as EP-1.

Story mapping:
- S-1 → CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, EP-1
- S-2 → CO-1, CO-5, EP-1
- S-3 → CO-5
- S-4 → CO-7

Resolved review findings:
- F-1: The list keeps its single lateral-join query per page and get runs the same shared SQL fragment for one patient, so no per-row calls are added.
- F-2: CO-1 changes the repository get return type to omit both appointments and next_appointment.

## Known
- CO-1: `PatientRepo`, the HMS-1 query, the one-query-per-page list and the repository `get` return type are all in the module map and the code (C7, C8, C9, C24, C25, C26); the shape was chosen in A-03.
- CO-2: `PatientService.get` uses `this.clock()` (C10, C11); the API field was chosen in A-03.
- CO-3, CO-4: both `PatientDetail` and `NextAppointment` symbols exist (C3, C4, C5, C6).
- CO-5: `PatientDetailPage` exists (C1, C19); the link element was chosen in A-04.
- CO-6: the table formats with `dateTime` and `useSettings` (C15, C16).
- CO-7: unchanged existing UI (C19, C20).
- EP-1, EP-2, EP-3: the routes exist in the api inventory (C2, C13, C14); create and update go through `get` (C12).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-03 (Q-01): where should the patient page get the next appointment? Answer: "API field (proposed default)". Decided CO-1, CO-2, CO-3, CO-4, EP-1, EP-2, EP-3 and C21. No mismatch.
- A-04 (Q-02): which part of the line links to /appointments/:id? Answer: "Date and time (proposed default)". Decided CO-5 and C22. No mismatch.

---
schema: design.v1
run_id: run_01M5VBPY40KETZZR1EY7H5KK0V
step: design
attempt: 1
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/impact.md
    sha256: d587878feb26610a74751e8c155cea2a8def3eb0d3c30499d0cf4de592776fba
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/split.md
    sha256: 8193194ed9f5168ece5dab22f4c694ae9fe98e787bfd5dd0a55ae234091fed5e
  - path: artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/req.md
    sha256: 168b14f2f9236bbbe97d4740cc4b53d5824259e81d856851c562ef78471ab680
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
    text: "Lists answer items, page, `page_size` and total, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C5
    text: "Instants are stored in `UTC`."
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
  - id: C21
    text: "An appointment's start is stored in the column `starts_at`."
    cites: ["context/generated/db-schema.json#column:appointment.starts_at"]
  - id: C22
    text: "An appointment's doctor is stored in the column `doctor_id`."
    cites: ["context/generated/db-schema.json#column:appointment.doctor_id"]
  - id: C23
    text: "Doctors and patients with appointments are made `Inactive`, never deleted."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C24
    text: "The web app reads settings through `useSettings`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings"]
  - id: C25
    text: "Settings are served by the route `/api/settings`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings"]
  - id: C26
    text: "The hospital time zone setting is `HOSPITAL_TZ`, default Asia/Kolkata."
    cites: ["context/architecture.md#time-and-slots"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-2, S-3, S-5]
assumed: [S-1, S-4]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
  - {id: U-3, blocking: false}
  - {id: U-4, blocking: false}
  - {id: U-5, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Should the Next appointment column use the app's existing dateTime format in the hospital time zone?"
    why: "S-1 AC3 needs an exact expected string, and the intent leaves the format open."
    evidence: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/req.md#L127"]
    options: ["Reuse dateTime", "Specify a new format"]
    default: "Reuse dateTime with the hospital time zone."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Does DELETE /api/doctors/:id enforce the baseline rule that a doctor with appointments is made Inactive, never deleted?"
    why: "S-4 relies on that rule so the doctor's name is always readable for a next appointment."
    evidence: ["context/intent-baseline.md#data-protection", "context/generated/api-inventory.json#http:DELETE:/api/doctors/:id", "artifacts/HMS-1/run_01M5VBPY40KETZZR1EY7H5KK0V/split.md#L157"]
    options: ["Yes, it enforces the rule", "No, it can delete such a doctor"]
    default: "Assume yes; no change to the delete route in this ticket."
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Are the patient list's search and sort applied in the GET /api/patients query?"
    why: "The design adds the next appointment inside that query; search, sort and paging must keep their current results (S-5)."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "context/conventions.md#lists"]
    options: ["Yes, server side", "No, in PatientsPage"]
    default: "Assume server side and add regression tests on GET /api/patients."
  - id: Q-04
    blocking: false
    ask: tech lead
    text: "Is next_appointment {id, starts_at, doctor_name} | null an acceptable addition to each GET /api/patients list item?"
    why: "The field names are not in any input; they follow the snake_case list and column names, and the client type and PatientsPage depend on them."
    evidence: ["context/conventions.md#lists", "context/generated/db-schema.json#column:appointment.starts_at"]
    options: ["Accept as proposed", "Rename the fields"]
    default: "Accept as proposed."
  - id: Q-05
    blocking: false
    ask: tech lead
    text: "Does GET /api/settings (read through useSettings) carry the hospital time zone HOSPITAL_TZ to the web app?"
    why: "S-1 shows the start in the hospital zone; if settings do not carry the zone, the web would need it added or would fall back to Asia/Kolkata."
    evidence: ["context/architecture.md#time-and-slots", "context/generated/api-inventory.json#http:GET:/api/settings", "context/generated/module-map.json#sym:sample-system:web/src/hooks/useSettings.tsx:useSettings"]
    options: ["Yes, settings carry the zone", "No, add it to settings", "No, use a fixed Asia/Kolkata"]
    default: "Assume yes and read the zone from useSettings."
components:
  - {name: patientsRouter, module: module:sample-system:api, change: existing}
  - {name: PatientService, module: module:sample-system:api, change: changed}
  - {name: PatientRepo, module: module:sample-system:api, change: changed}
  - {name: AppointmentRepo, module: module:sample-system:api, change: existing}
  - {name: api client (client.ts), module: module:sample-system:web, change: changed}
  - {name: PatientsPage, module: module:sample-system:web, change: changed}
  - {name: dateTime formatter, module: module:sample-system:web, change: existing}
  - {name: useSettings, module: module:sample-system:web, change: existing}
  - {name: AppointmentDetailPage, module: module:sample-system:web, change: existing}
endpoints:
  - {method: GET, path: /api/patients, change: changed}
  - {method: GET, path: /api/appointments/:id, change: existing}
  - {method: GET, path: /api/settings, change: existing}
---
# Design: HMS-1

## Overview
The patient list gets its next appointment from the existing `GET /api/patients` route, so no new endpoint, table, migration or job is added. `PatientRepo` extends the list query: for each patient on the page it selects the earliest appointment with status Scheduled whose start is after the current instant, with that appointment's id, start and doctor name; this lookup uses the existing `appointment_patient_start_idx`. `PatientService` maps the result onto each list item, and `patientsRouter` is unchanged because the path and parameters stay the same.

Response contract (each item in `items`, alongside the existing fields):
- `next_appointment`: `null` when the patient has no Scheduled appointment starting after now; otherwise an object with
  - `id`: the appointment id, used for the link to `/appointments/:id`;
  - `starts_at`: the start as a UTC instant in ISO-8601 form, the same value stored in the `starts_at` column;
  - `doctor_name`: the name of the appointment's doctor.
The field is always present, never omitted, so the client type is `{ id, starts_at, doctor_name } | null`. The names follow the snake_case list fields (`page_size`) and the column names (`starts_at`, `doctor_id`); they are not in any input, so they are assumed and confirmed by Q-04.

In the web app the `api` client's patient list item type gains `next_appointment`, and `PatientsPage` adds a "Next appointment" column. It formats `starts_at` with the existing `dateTime` formatter in the hospital time zone, assumed to be read from settings through `useSettings` and `GET /api/settings` (Q-05), with `HOSPITAL_TZ` defaulting to Asia/Kolkata. It shows `doctor_name` next to the time and links to `/appointments/:id`. When `next_appointment` is `null` it shows "None".

Trade-offs:
- Inside the list query, not a second call per patient: one round trip per page and an existing index, at the cost of a wider list query that must keep page size, search and sort unchanged.
- Widen the existing route, not a new endpoint: no new route to secure or document, but the list response shape grows for every caller of `GET /api/patients`.
- `null` rather than an omitted field: one shape for the client to handle, at the cost of a slightly larger response.
- Send the UTC instant and format on the web: matches how instants are stored, so "after now" does not depend on time zone; only display uses the hospital zone.
- Keep `AppointmentRepo` unchanged: the lookup is a join inside the patient list SQL, so appointment code paths are untouched; the cost is that this one appointment query lives in `PatientRepo`.

## Components
- patientsRouter (module:sample-system:api) — existing: same path and parameters; passes the widened result through.
- PatientService (module:sample-system:api) — changed: sets `next_appointment` on each list item, or `null`.
- PatientRepo (module:sample-system:api) — changed: the list query selects the earliest Scheduled appointment starting after now, its `starts_at` and its doctor's name; it never selects medical history columns.
- AppointmentRepo (module:sample-system:api) — existing: not used for this lookup.
- api client (client.ts) (module:sample-system:web) — changed: the patient list item type gains `next_appointment: { id, starts_at, doctor_name } | null`.
- PatientsPage (module:sample-system:web) — changed: new "Next appointment" column, link to the appointment page, "None" when `null`.
- dateTime formatter (module:sample-system:web) — existing: reused to show `starts_at` in the hospital time zone.
- useSettings (module:sample-system:web) — existing: assumed to supply the hospital time zone to the page (Q-05).
- AppointmentDetailPage (module:sample-system:web) — existing: link target at `/appointments/:id`.
- GET /api/patients — changed: each list item gains `next_appointment`; still 20 per page with the same search and sort.
- GET /api/appointments/:id — existing: unchanged; backs the appointment page the column links to.
- GET /api/settings — existing: unchanged; assumed source of the hospital time zone for the web app.

## Known
- S-2: the PatientRepo query filters to status Scheduled after now, so Cancelled, Completed and No-show are never picked, and a Cancelled appointment before a later Scheduled one is skipped; `null` shows "None" (C9, C17, C21).
- S-3: PatientsPage links `next_appointment.id` to the appointment page at `/appointments/:id`, backed by GET /api/appointments/:id (C15, C16, C20).
- S-5: the route path, parameters and list shape stay the same apart from the added field, 20 per page; PatientRepo never selects medical history and logs never carry it (C4, C7, C8, C11).

## Assumed
- S-1: the column, earliest Scheduled appointment, start time and doctor name are built in PatientRepo, PatientService, the api client and PatientsPage (C1, C9, C10, C12, C13). The hospital-zone display is assumed: `starts_at` is a UTC instant (C5) formatted with dateTime (C14) using the zone the web reads through useSettings from GET /api/settings (C24, C25), with HOSPITAL_TZ defaulting to Asia/Kolkata (C6, C26); confirmed by Q-05. The `next_appointment` field names and `null` representation are also assumed (C4, C21, C22; Q-04).
- S-4: the doctor's name is read through the appointment's `doctor_id` whether or not the doctor is Inactive. Basis: doctors with appointments are made Inactive, never deleted (C23), even though a delete route exists (C19); Q-02 checks the route enforces this.

## Unknown
- U-1 (non-blocking): the exact display format of the start date and time (Q-01).
- U-2 (non-blocking): whether DELETE /api/doctors/:id enforces the Inactive-not-deleted rule (Q-02).
- U-3 (non-blocking): whether search and sort run in the GET /api/patients query (Q-03).
- U-4 (non-blocking): whether the proposed `next_appointment` field names are accepted (Q-04).
- U-5 (non-blocking): whether GET /api/settings carries the hospital time zone to the web (Q-05).

## Open questions
- Q-01 (non-blocking, product owner): reuse the existing dateTime format in the hospital time zone? Default: yes.
- Q-02 (non-blocking, tech lead): does the doctor delete route enforce Inactive-not-deleted for doctors with appointments? Default: assume yes; no change to that route here.
- Q-03 (non-blocking, tech lead): are search and sort server side in GET /api/patients? Default: assume yes and add regression tests on that route.
- Q-04 (non-blocking, tech lead): accept `next_appointment: { id, starts_at, doctor_name } | null` on each list item? Default: accept as proposed.
- Q-05 (non-blocking, tech lead): do settings carry the hospital zone to the web? Default: assume yes and read it from useSettings.

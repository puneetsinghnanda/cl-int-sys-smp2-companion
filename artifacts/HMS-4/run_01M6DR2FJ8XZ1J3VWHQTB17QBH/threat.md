---
schema: threat-model.v1
run_id: run_01M6DR2FJ8XZ1J3VWHQTB17QBH
step: threat
attempt: 1
skill: threat-model@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/design.md
    sha256: 64ff400eb38b6193c9589148e368ad4face2dace7c50bac0764474c5723194f2
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: 9ba29278d8d66b7341defc3c67d6e7d68508ce25ee2991ba256be2b37194ec61
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "Every API request passes through `identify`, and routes call `requirePermission`."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "A patient row holds the basic `medical history`; it leaves the database only for the patient's own page."
    cites: ["context/data-model.md#patients"]
  - id: C4
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C5
    text: "Hospital-local dates use `HOSPITAL_TZ`, set in config."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C6
    text: "Lists answer 20 per page with `page_size`."
    cites: ["context/conventions.md#lists"]
  - id: C7
    text: "Logs carry method, route and error message only, never `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C8
    text: "Seed data is `invented`."
    cites: ["context/data-model.md#data-rules"]
  - id: C9
    text: "The design passes the zone as a `bound query parameter`, never as interpolated SQL."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/design.md#overview"]
  - id: C10
    text: "The design selects only the date, never the patient `medical columns`."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/design.md#overview"]
  - id: C11
    text: "The index `appointment_patient_start_idx` supports the per-patient lookup."
    cites: ["artifacts/HMS-4/run_01M6DR2FJ8XZ1J3VWHQTB17QBH/design.md#overview"]
  - id: C12
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C13
    text: "The list endpoint is `/api/patients` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
trace: [REQ-2, REQ-3, REQ-6, REQ-7, REQ-8, REQ-10]
known: [T-1, T-2, T-3, T-4, T-5, T-6, T-7, T-8, T-9, T-11, T-12, T-13, T-14]
assumed: [T-10]
unknown: []
questions: []
threats:
  - id: T-1
    component: patientsRouter (GET /api/patients handler)
    stride: spoofing
    threat: "Any caller who reaches the API is treated as the staff actor, so an outsider could read the patient list, which now includes visit dates."
    mitigation: "Existing control: every request passes identify and the route calls requirePermission (architecture#identity). In the MVP every request is staff, so this is an accepted, pre-existing risk that sign-in will close by replacing those two functions. No new control is needed for HMS-4."
  - id: T-2
    component: patientsRouter (GET /api/patients handler)
    stride: denial_of_service
    threat: "Large page requests on the list would multiply the new per-row last-visit subquery."
    mitigation: "Existing control: lists are fixed at 20 per page (conventions#lists). Implement must keep the existing page_size handling unchanged, so the subquery runs at most once per row of a bounded page."
  - id: T-3
    component: PatientService (list)
    stride: tampering
    threat: "If the time zone used for the date conversion could come from the request, a caller could shift the dates shown."
    mitigation: "New control the implement step must build: the service reads HOSPITAL_TZ from config only (architecture#time-and-slots) and never from query or body input."
  - id: T-4
    component: PatientRepo (list query)
    stride: tampering
    threat: "The new subquery and zone value could open an injection path into the list SQL if they are built by string concatenation."
    mitigation: "New control the implement step must build: the zone is passed as a bound query parameter (design overview). Existing search input stays parameterised; no request value is interpolated into SQL."
  - id: T-5
    component: PatientRepo (list query)
    stride: information_disclosure
    threat: "Widening the list query to join appointment data could also pull patient medical history columns, or appointment details beyond the date, into the list payload."
    mitigation: "Existing rule plus a design control. Medical history leaves the database only for the patient's own page (data-model#patients). The design selects only the computed date (design overview). Implement must keep the existing column list and add only last_visit; a test asserts no medical fields appear (S-4)."
  - id: T-6
    component: PatientRepo (list query)
    stride: denial_of_service
    threat: "A per-row subquery over appointment could become slow as appointment history grows, degrading the list and the booking picker."
    mitigation: "Existing control: appointment_patient_start_idx on (patient_id, starts_at) supports the lookup (design overview). New control: implement applies the subquery only to the paged rows and verifies latency in tests or review."
  - id: T-7
    component: Patient list row type (api, api/src/types.ts)
    stride: information_disclosure
    threat: "If the shared list row type is widened carelessly (for example by reusing the full patient type), medical fields could become part of the list contract."
    mitigation: "New control the implement step must build: add only `last_visit: string | null` to the list row type and keep it separate from the full patient type that carries medical history (data-model#patients)."
  - id: T-8
    component: Patient list row type (web, web/src/api/types.ts)
    stride: tampering
    threat: "The web type could drift from the api type, so the page renders a missing or wrongly typed value as if it were valid."
    mitigation: "New control: both types are changed in the same change set with the same `last_visit: string | null` field. A PatientsPage test checks both the null case and the date case."
  - id: T-9
    component: api client (web/src/api/client.ts)
    stride: information_disclosure
    threat: "The client is the only path from the web app to the API (architecture#components); the list response now carries visit dates."
    mitigation: "Existing control: the client is unchanged and only calls the API; no new storage or logging of responses is added. No new control is needed."
  - id: T-10
    component: PatientsPage
    stride: tampering
    threat: "Rendering an API value into the page could allow script injection if inserted as raw HTML."
    mitigation: "New control the implement step must build: render last_visit as plain text (or '-') through the normal component text path, never as raw HTML. This assumes the web module's standard escaping of text content, since it is written in `tsx` components."
  - id: T-11
    component: PatientsPage tests (web/src/__tests__/PatientsPage.test.tsx)
    stride: information_disclosure
    threat: "Test fixtures for the new column could include real patient names, contacts or medical data."
    mitigation: "Existing rule: seed and test data are invented (data-model#data-rules). New tests use invented patients only."
  - id: T-12
    component: BookAppointmentPage (patient picker)
    stride: information_disclosure
    threat: "The picker now receives each patient's last visit date, which the page does not need."
    mitigation: "Accepted: the field is a date only, not medical history, and the picker is used by the same staff actor (architecture#identity). The page ignores the field and does not display it. A regression check confirms nothing new is shown."
  - id: T-13
    component: BookAppointmentPage (patient picker)
    stride: denial_of_service
    threat: "Each picker search now runs the extra last-visit subquery, slowing typing-driven searches."
    mitigation: "Same control as T-6: indexed, page-bounded subquery. A regression and latency check of the picker is part of S-3."
  - id: T-14
    component: appointment table and appointment_patient_start_idx
    stride: repudiation
    threat: "A change to an appointment's status (for example to COMPLETED) changes the shown last visit with no trace of who did it."
    mitigation: "Existing control: appointment_event records every change with time and actor (data-model#appointments). The list only reads appointment data and adds no write path."
---
# Threat model: HMS-4

## Components
- **patientsRouter (GET /api/patients handler):** request input (search, paging) and the list response.
- **PatientService (list):** the hospital time zone from config, and the list rows.
- **PatientRepo (list query):** reads patient identity and contact columns and, newly, COMPLETED appointment start times. It must never select medical history.
- **Patient list row type (api, api/src/types.ts)** and **Patient list row type (web, web/src/api/types.ts):** the list row contract, including the new `last_visit`.
- **api client (web/src/api/client.ts):** carries list responses to the web app.
- **PatientsPage:** shows list rows, including the new Last visit column.
- **PatientsPage tests (web/src/__tests__/PatientsPage.test.tsx):** test fixtures for patients and appointments.
- **BookAppointmentPage (patient picker):** receives list rows, which now include `last_visit`.
- **appointment table and appointment_patient_start_idx:** appointment times and statuses, read only.

## Threats
- T-1 patientsRouter — spoofing: every caller is treated as staff in the MVP. Existing identify/requirePermission controls; this risk predates HMS-4.
- T-2 patientsRouter — denial of service: large pages multiply the subquery. The existing 20-per-page limit is kept.
- T-3 PatientService — tampering: the time zone must come from config only (new control).
- T-4 PatientRepo — tampering: SQL injection via the new subquery or zone. Bound parameters (new control).
- T-5 PatientRepo — information disclosure: medical history leaking into the list. Only the date is selected, the existing rule holds, and an S-4 test covers it.
- T-6 PatientRepo — denial of service: a slow per-row subquery. Existing index plus a page-bounded subquery.
- T-7 Patient list row type (api) — information disclosure: medical fields entering the list contract. Add only `last_visit` (new control).
- T-8 Patient list row type (web) — tampering: type drift. Change both types together and test the null and date cases.
- T-9 api client — information disclosure: no new storage or logging; unchanged.
- T-10 PatientsPage — tampering: script injection when rendering. Render as plain text (new control; relies on assumed text escaping).
- T-11 PatientsPage tests — information disclosure: real data in fixtures. Invented data only.
- T-12 BookAppointmentPage — information disclosure: an unneeded field reaches the picker. Accepted: date only, same staff actor, not displayed.
- T-13 BookAppointmentPage — denial of service: slower picker searches. Same control as T-6, plus a regression check.
- T-14 appointment table — repudiation: untracked status changes. The existing appointment_event audit covers them.

## Known
- T-1, T-2, T-9, T-14: rely on existing controls in architecture#identity, conventions#lists, architecture#components and data-model#appointments.
- T-3, T-4, T-5, T-6, T-7, T-8: rely on design decisions (config-only zone, bound parameter, date-only select, index) plus the cited data rules.
- T-11: relies on the invented-data rule (data-model#data-rules).
- T-12, T-13: the picker impact is stated in the design and accepted with a regression check.

## Assumed
- T-10: assumes the web module's components escape text content by default. The basis is that the web module is written in `tsx` components (module map languages). The pack does not state the UI framework's escaping, so implement must render the value as plain text and not as raw HTML.

## Unknown
None.

## Open questions
None.

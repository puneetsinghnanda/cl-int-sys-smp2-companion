---
schema: threat-model.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
step: threat
attempt: 1
skill: threat-model@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/design.md
    sha256: b31d420e0453c1145cb4585599d95a4f6cec5281f16b6f5300467e2fc94ed5d3
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
    text: "Every API request passes through `identify`, which sets the actor."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Routes call `requirePermission`."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C4
    text: "Medical history leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C5
    text: "`appointment_event` records every change to an appointment with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C6
    text: "Lists are paged at `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C7
    text: "Logs carry method, route and error message only, never `request bodies`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C8
    text: "SQL is kept in `api/src/repositories`."
    cites: ["context/conventions.md#code-layout"]
  - id: C9
    text: "The patient list endpoint is defined in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C10
    text: "Index `appointment_patient_start_idx` exists on the appointment table."
    cites: ["context/generated/db-schema.json#index:appointment_patient_start_idx"]
  - id: C11
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
trace: [REQ-1, REQ-2, REQ-4, REQ-5, REQ-6, REQ-8, REQ-12]
known: [T-3, T-5, T-6, T-8, T-9, T-10]
assumed: [T-1, T-2, T-4, T-7]
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: In the MVP every request is staff with every permission granted; is it accepted that the new visit history on the list has the same exposure?
    why: The Last visit column adds visit history to a list that has no real sign-in yet; spoofing and privilege threats rely on the future sign-in.
    evidence: ["context/architecture.md#identity"]
    options: [accept for MVP, block until sign-in exists]
    default: accept for MVP; the list already shows Next appointment with the same exposure
threats:
  - id: T-1
    component: PatientRepo list query
    stride: tampering
    threat: Request input (search, sort, page) reaching the new last-visit subquery could alter the SQL.
    mitigation: "New control for implement: the subquery uses no request input, and all list parameters stay bound parameters in the repository layer (api/src/repositories), as in the existing query."
  - id: T-2
    component: PatientRepo list query
    stride: information_disclosure
    threat: Widening the SELECT (for example selecting all patient columns) could put medical history into the list response.
    mitigation: "Existing rule that medical history leaves the database only for the patient's own page; new control for implement: keep an explicit column list and add a test that list items carry no medical history fields."
  - id: T-3
    component: PatientRepo list query
    stride: denial_of_service
    threat: A per-row subquery could make the list slow under many appointments.
    mitigation: "Existing controls: lists are paged at 20 per page, and the lookup uses the existing appointment_patient_start_idx (patient_id, starts_at)."
  - id: T-4
    component: PatientService list
    stride: information_disclosure
    threat: The service could pass through repository columns beyond the list fields.
    mitigation: "New control for implement: the service returns only the existing list fields plus the new last-visit field; covered by the same no-medical-history test as T-2."
  - id: T-5
    component: patientsRouter GET /api/patients
    stride: spoofing
    threat: In the MVP every request is treated as staff, so any caller that reaches the API can read last visit dates.
    mitigation: "Existing control: requests pass through identify and the route calls requirePermission, which sign-in will replace; no route change in this ticket. Risk accepted for MVP pending Q-01."
  - id: T-6
    component: patientsRouter GET /api/patients
    stride: repudiation
    threat: Reads of visit history are not attributed to a user.
    mitigation: "Accepted: the change is read-only; appointment_event already records every change with actor, and no new write is added."
  - id: T-7
    component: PatientsPage Last visit column
    stride: tampering
    threat: A value rendered as markup instead of text could inject content into the page.
    mitigation: "New control for implement: render the formatted date or '-' as plain text through the existing startsAt helper, never as raw HTML."
  - id: T-8
    component: PatientsPage Last visit column
    stride: information_disclosure
    threat: The list shows when each patient last visited to anyone who can see the list.
    mitigation: "Accepted: the same audience already sees the Next appointment column; no medical history is shown (REQ-12)."
  - id: T-9
    component: API client (web/src/api/client.ts)
    stride: information_disclosure
    threat: Client-side logging of list responses could expose visit history.
    mitigation: "Existing control: the web calls the API only through web/src/api/client.ts and logs carry method, route and error message only, never request bodies; no new logging is added."
  - id: T-10
    component: appointment table and appointment_patient_start_idx
    stride: elevation_of_privilege
    threat: The list path could gain write access to appointments.
    mitigation: "Existing control: the change only reads appointment; no write, migration or new permission is added, and appointment_event still records every change."
---
# Threat model: HMS-4

## Components
- **PatientRepo list query** reads patient list columns and, new in this change, the latest `starts_at` of COMPLETED appointments for each patient.
- **PatientService list** passes list rows to the route.
- **patientsRouter GET /api/patients** serves the list to the web app. Requests pass through `identify` and `requirePermission`.
- **PatientsPage Last visit column** shows the formatted date, or "-".
- **API client (web/src/api/client.ts)** is the web app's only path to the API.
- **appointment table and appointment_patient_start_idx** hold appointment instants and status. This change only reads them.

## Threats
- T-1, PatientRepo list query, tampering: request input could alter the new subquery's SQL. Mitigation: the subquery takes no request input and parameters stay bound. New control for implement.
- T-2, PatientRepo list query, information disclosure: a wider SELECT could put medical history into the list. Mitigation: the existing medical-history rule, plus an explicit column list and a test. New control for implement.
- T-3, PatientRepo list query, denial of service: the per-row subquery could be slow. Mitigation: 20 rows per page and the existing `appointment_patient_start_idx`.
- T-4, PatientService list, information disclosure: the service could pass extra columns through. Mitigation: return only the list fields plus last visit, covered by the T-2 test. New control for implement.
- T-5, patientsRouter GET /api/patients, spoofing: every MVP request acts as staff. Mitigation: the existing `identify` and `requirePermission`, which sign-in will replace. Risk accepted for the MVP, pending Q-01.
- T-6, patientsRouter GET /api/patients, repudiation: reads are not attributed to a user. Accepted, because the change is read-only and `appointment_event` already records every change.
- T-7, PatientsPage Last visit column, tampering: a value rendered as markup could inject content. Mitigation: render as plain text through `startsAt`. New control for implement.
- T-8, PatientsPage Last visit column, information disclosure: visit history is visible to everyone who can see the list. Accepted, because the same audience already sees Next appointment and no medical history is shown.
- T-9, API client, information disclosure: client-side logging could expose the list. Mitigation: the single client path and the existing logging rule (never request bodies). No new logging is added.
- T-10, appointment table and appointment_patient_start_idx, elevation of privilege: the list path could gain write access. Mitigation: the change only reads, and adds no migration or permission.

## Known
- T-3: the paging limit and the index are both existing, cited controls.
- T-5: `identify` and `requirePermission` are existing controls on every route.
- T-6: `appointment_event` already attributes every appointment change.
- T-8: the exposure matches the existing Next appointment column.
- T-9: the single client path and the logging rule already exist.
- T-10: the design reads the appointment table without changing it.

## Assumed
- T-1: assumes implement keeps every list parameter bound and the subquery free of request input. This is a new control.
- T-2: assumes implement keeps an explicit column list and adds the no-medical-history test. This is a new control.
- T-4: assumes the service returns only list fields. This is a new control.
- T-7: assumes the cell is rendered as text through `startsAt`. This is a new control.

## Unknown
- U-1 (non-blocking): whether the MVP's lack of sign-in is accepted for showing visit history on the list (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): Accept the MVP exposure for the new column? Default: accept, because the list already shows Next appointment with the same exposure.

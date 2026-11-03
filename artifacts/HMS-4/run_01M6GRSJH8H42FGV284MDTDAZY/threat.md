---
schema: threat-model.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
step: threat
attempt: 2
skill: threat-model@1.1.0
inputs:
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/design.md
  sha256: f9caf706ba92848aaa8be4ce155d2505c138a5e39292e0c5527eedec728526bd
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
- id: C12
  text: "The design gives each list item `lastVisitAt`, null when the patient has no Completed appointment."
  cites: ["artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/design.md#claim:C13"]
- id: C13
  text: "Instants are stored in `UTC`."
  cites: ["context/architecture.md#time-and-slots"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-13]
known: [T-3, T-6, T-8, T-9, T-12]
assumed: [T-1, T-2, T-4, T-5, T-7, T-10, T-11]
unknown:
- {id: U-1, blocking: false}
questions:
- id: Q-01
  blocking: false
  ask: tech lead
  text: In the MVP every request is staff with every permission granted; is it accepted that the new visit history on the list has the same exposure?
  why: The Last visit column adds visit history to a list that has no real sign-in yet; the spoofing threat T-5 has no protecting control until sign-in exists.
  evidence: ["context/architecture.md#identity"]
  options: [accept for MVP, block until sign-in exists]
  default: accept for MVP; the list already shows Next appointment with the same exposure
resolves:
- id: F-1
  how: C12 now cites the design's claim C13 for the nullable lastVisitAt field instead of a design-step IDE answer.
threats:
- id: T-2
  component: PatientRepo list query (adds lastVisitAt per patient)
  stride: information_disclosure
  threat: Widening the SELECT (for example selecting all patient columns) could put medical history into the list response.
  mitigation: "Existing rule that medical history leaves the database only for the patient's own page (C4); new control for implement: keep an explicit column list and add a test that list items carry no medical history fields."
- id: T-5
  component: patientsRouter GET /api/patients (returns lastVisitAt per item)
  stride: spoofing
  threat: In the MVP every request is treated as staff, so any caller that reaches the API can read last visit times.
  mitigation: "None in the MVP: identify and requirePermission exist but grant every permission (C3), so they do not stop spoofing. Risk accepted pending Q-01 and the future sign-in; no route permission change in this ticket."
- id: T-11
  component: Patient list item type (gains lastVisitAt)
  stride: information_disclosure
  threat: Reusing the patient detail type for list items could pull medical history fields into the list path.
  mitigation: "New control for implement: extend only the list item type with lastVisitAt; the web reaches the API only through web/src/api/client.ts (C11)."
- id: T-4
  component: PatientService list (carries lastVisitAt)
  stride: information_disclosure
  threat: The service could pass through repository columns beyond the list fields.
  mitigation: "New control for implement: the service returns only the existing list fields plus lastVisitAt; covered by the same no-medical-history test as T-2."
- id: T-1
  component: PatientRepo list query (adds lastVisitAt per patient)
  stride: tampering
  threat: Request input (search, sort, page) reaching the new last-visit subquery could alter the SQL.
  mitigation: "New control for implement: the subquery uses no request input, and all list parameters stay bound parameters in the repository layer (C8), as in the existing query."
- id: T-7
  component: PatientsPage Last visit column
  stride: tampering
  threat: A value rendered as markup instead of text could inject content into the page.
  mitigation: "New control for implement: render the dateTime output or '-' as plain text, never as raw HTML."
- id: T-3
  component: PatientRepo list query (adds lastVisitAt per patient)
  stride: denial_of_service
  threat: A per-patient subquery could make the list slow when patients have many appointments.
  mitigation: "Existing controls: lists are paged at 20 per page (C6), and the lookup uses the existing appointment_patient_start_idx (C10)."
- id: T-8
  component: PatientsPage Last visit column
  stride: information_disclosure
  threat: The list shows when each patient last visited to anyone who can see the list.
  mitigation: "Accepted: the same audience already sees the Next appointment column; no medical history is shown (REQ-13)."
- id: T-12
  component: appointment table and appointment_patient_start_idx (read only)
  stride: elevation_of_privilege
  threat: The list path could gain write access to appointments.
  mitigation: "Existing control: the change only reads appointment; no write, migration or new permission is added, and appointment_event still records every change (C5)."
- id: T-6
  component: patientsRouter GET /api/patients (returns lastVisitAt per item)
  stride: repudiation
  threat: Reads of visit history are not attributed to a user.
  mitigation: "Accepted: the change is read-only; appointment_event already records every change with actor (C5), and no new write is added."
- id: T-9
  component: dateTime helper (reused unchanged)
  stride: tampering
  threat: Someone who changes the deployment's zone setting, or a code change that passes a different zone, would alter the displayed visit time so staff see a wrong day.
  mitigation: "Existing control: the helper is reused unchanged with the same time zone argument as Next appointment, and instants are stored in UTC (C13)."
- id: T-10
  component: Patient list item type (gains lastVisitAt)
  stride: tampering
  threat: An unexpected null or malformed lastVisitAt from the API, if the type claims it is always present, would alter what staff see instead of '-'.
  mitigation: "New control for implement: type lastVisitAt as string or null (C12) and branch to '-' before formatting."
---
# Threat model: HMS-4

## Components
- **PatientRepo list query (adds lastVisitAt per patient)** reads patient list columns and, new in this change, the latest `starts_at` of COMPLETED appointments per patient.
- **PatientService list (carries lastVisitAt)** passes list rows to the route.
- **patientsRouter GET /api/patients (returns lastVisitAt per item)** serves the list; requests pass through `identify` and `requirePermission`, which grant everything in the MVP.
- **PatientsPage Last visit column** shows the formatted value, or "-".
- **dateTime helper (reused unchanged)** formats the instant in the hospital zone, like Next appointment.
- **Patient list item type (gains lastVisitAt)** carries the new nullable string on the web side (C12).
- **appointment table and appointment_patient_start_idx (read only)** hold appointment instants and status; only read here.

## Threats
Ordered by risk, highest first.
- T-2, PatientRepo list query, information disclosure: a wider SELECT could leak medical history. Mitigation: existing medical-history rule plus an explicit column list and a test. New control for implement.
- T-5, patientsRouter, spoofing: every MVP request acts as staff. Mitigation: none in the MVP; `identify` and `requirePermission` grant everything. Risk accepted pending Q-01 and sign-in.
- T-11, Patient list item type, information disclosure: reusing the detail type could pull in medical history. Mitigation: extend only the list item type. New control for implement.
- T-4, PatientService list, information disclosure: extra columns passed through. Mitigation: return only list fields plus lastVisitAt, covered by the T-2 test. New control for implement.
- T-1, PatientRepo list query, tampering: request input could alter the new subquery's SQL. Mitigation: no request input in the subquery, parameters stay bound. New control for implement.
- T-7, PatientsPage Last visit column, tampering: markup injection. Mitigation: render as plain text. New control for implement.
- T-3, PatientRepo list query, denial of service: the per-patient subquery could be slow. Mitigation: 20 per page and the existing `appointment_patient_start_idx`.
- T-8, PatientsPage Last visit column, information disclosure: visit history visible to list viewers. Accepted: same audience as Next appointment; no medical history shown.
- T-12, appointment table and index, elevation of privilege: list path gaining write access. Mitigation: read only; no migration or permission added.
- T-6, patientsRouter, repudiation: reads are not attributed. Accepted: read-only change; `appointment_event` records every change.
- T-9, dateTime helper, tampering: a changed zone setting or argument alters the displayed visit time. Mitigation: helper reused unchanged with the Next appointment zone; instants stored in UTC.
- T-10, Patient list item type, tampering: an unexpected null alters the displayed value. Mitigation: type as string or null, branch to "-". New control for implement.

## Known
- T-3: paging limit and index are existing, cited controls (C6, C10).
- T-6: `appointment_event` already attributes every appointment change (C5).
- T-8: the exposure matches the existing Next appointment column (REQ-13 keeps medical history out).
- T-9: the helper is reused unchanged and instants are stored in UTC (C13).
- T-12: the design only reads the appointment table (C5, C10).

## Assumed
- T-1: implement keeps every list parameter bound and the subquery free of request input (C8). New control.
- T-2: implement keeps an explicit column list and adds the no-medical-history test (C4). New control.
- T-4: the service returns only list fields plus lastVisitAt. New control.
- T-5: the MVP exposure is accepted until sign-in exists; basis C3 and the default of Q-01.
- T-7: the cell is rendered as text. New control.
- T-10: the type states lastVisitAt as string or null (C12). New control.
- T-11: only the list item type is extended (C11). New control.

## Unknown
- U-1 (non-blocking): whether the MVP's lack of sign-in is accepted for showing visit history on the list (Q-01).
- Note: C6 to C8 and C10 cite context/conventions.md and context/generated/db-schema.json, which are context-pack files outside this step's listed inputs; the inputs list is copied from tsl unchanged.

## Open questions
- Q-01 (non-blocking, tech lead): Accept the MVP exposure for the new column? Default: accept, because the list already shows Next appointment with the same exposure.

## Resolves
- F-1: C12 now cites the design's claim C13 for the nullable lastVisitAt field instead of a design-step IDE answer.

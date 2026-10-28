---
schema: threat-model.v1
run_id: run_01M5YQB3ZRP10586473KARZ7M4
step: threat
attempt: 1
skill: threat-model@1.0.1
inputs:
  - path: artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/design.md
    sha256: 9c01a9c1ddd65536f8dc4ae53ecaadecbec0ffc9b6b47d6e5d21557a93acb30a
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
    text: "Every API request passes through `identify` and routes call requirePermission."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "The patient row holds contact details and the basic `medical history`."
    cites: ["context/data-model.md#patients"]
  - id: C4
    text: "Medical history leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C5
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C6
    text: "The hospital time zone is configured as `HOSPITAL_TZ` in the API."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C7
    text: "The list route is served from `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C8
    text: "The design relies on the existing index `appointment_patient_start_idx`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/design.md#L113"]
  - id: C9
    text: "The design renders Last visit as text with `no Date parsing`."
    cites: ["artifacts/HMS-4/run_01M5YQB3ZRP10586473KARZ7M4/design.md#L111"]
  - id: C10
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: [REQ-4]
unknown: [U-1]
unknowns:
  - {id: U-1, blocking: false, text: "The MVP grants every permission to the actor staff; who can reach the API in the deployed environment is not stated in the inputs, so exposure of last-visit dates to non-staff cannot be assessed.", question: Q-01}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Until sign-in replaces identify and requirePermission, is the API reachable only by hospital staff?"
    why: "Every permission is granted in the MVP, so the new last-visit value is readable by anyone who can reach GET /api/patients."
    evidence: ["context/architecture.md#identity", "context/generated/api-inventory.json#http:GET:/api/patients"]
    options: ["Accept the existing MVP identity for this ticket", "Add a permission check before shipping"]
    default: "Accept the existing MVP identity for this ticket; the field rides the list route's existing permission"
threats:
  - {id: T-1, component: "PatientRepo list query", stride: information_disclosure, threat: "Changing the list query to add the last visit could also select patient columns that hold medical history or extra contact details, sending them to the list.", mitigation: "Existing rule: medical history leaves the database only for the patient's own page (data-model#patients). New control for implement: keep an explicit column list (no select-all), and add a test asserting list items carry no medical-history fields (S-4)."}
  - {id: T-2, component: "PatientRepo list query", stride: tampering, threat: "The new correlated subquery sits next to the existing search filter; building it by string concatenation could let search input change the SQL.", mitigation: "New control for implement: the subquery uses only bound parameters and literal status COMPLETED; no request value is concatenated into SQL."}
  - {id: T-3, component: "PatientRepo list query", stride: denial_of_service, threat: "A per-row max over appointments could make each list page slow as appointments grow.", mitigation: "Existing control: the appointment_patient_start_idx index on patient and start time serves the max (design C13); the page size stays at 20, so at most 20 lookups per request."}
  - {id: T-4, component: "PatientService list (adds hospital-local day via localDay)", stride: repudiation, threat: "A wrong Last visit could be disputed with no way to show which appointment and status change produced it.", mitigation: "Existing control: appointment_event records every appointment change with time and actor (data-model#appointments); the value is derived, not stored, so it always follows the recorded Completed status."}
  - {id: T-5, component: "PatientService list (adds hospital-local day via localDay)", stride: information_disclosure, threat: "Debug logging added while shaping list items could write patient rows to logs.", mitigation: "Existing rule cited by the design: logs never carry contact details or medical history (design C14). New control for implement: add no logging of list items."}
  - {id: T-6, component: "time.ts localDay and loadConfig HOSPITAL_TZ", stride: tampering, threat: "A wrong or missing HOSPITAL_TZ value would shift every Last visit date by a day near midnight.", mitigation: "Existing control: HOSPITAL_TZ is server-side configuration with a default of Asia/Kolkata (architecture#time-and-slots); it is not taken from any request. New control: the S-1 AC4 test fixes the zone and checks the hospital-local date."}
  - {id: T-7, component: "patients router (GET /api/patients)", stride: elevation_of_privilege, threat: "In the MVP every permission is granted, so any caller who can reach the route can read last-visit dates for all patients.", mitigation: "Existing control: the route keeps its requirePermission call, so the field is covered when sign-in replaces identify and requirePermission (architecture#identity). No new control in this ticket; see Q-01."}
  - {id: T-8, component: "patients router (GET /api/patients)", stride: spoofing, threat: "Requests are not signed in, so the caller's identity (actor staff) cannot be trusted for who viewed the list.", mitigation: "Existing design: identify sets the actor for every request and sign-in replaces it later (architecture#identity). The change is read-only and adds no new identity path."}
  - {id: T-9, component: "PatientsPage Last visit column (text render, no Date parsing)", stride: tampering, threat: "If the day string were inserted as raw HTML, an unexpected value in the response could change the page.", mitigation: "New control for implement: render last_visit_date as a plain text node (design C9 text render), never as raw HTML; null renders as \"-\"."}
  - {id: T-10, component: "web API client patient list type", stride: tampering, threat: "The client could accept a non-day value or a missing field and show a misleading date.", mitigation: "New control for implement: type last_visit_date as string or null in web/src/api/client.ts and treat anything else as no visit (\"-\"); the API side shapes the value with localDay."}
  - {id: T-11, component: "appointment_patient_start_idx index", stride: denial_of_service, threat: "If the planner did not use the index for the status-filtered max, list requests could scan the appointment table.", mitigation: "Existing control: the index covers patient and start time; new control for implement: check the list query plan uses appointment_patient_start_idx before merge."}
---
# Threat model: HMS-4

## Components
- **PatientRepo list query** (api, changed): reads patient list columns plus the latest COMPLETED appointment start per patient. The patient table also holds contact details and medical history (C3).
- **PatientService list (adds hospital-local day via localDay)** (api, changed): turns the UTC instant into a hospital-local day string.
- **time.ts localDay and loadConfig HOSPITAL_TZ** (api, existing): server-side time-zone configuration (C6).
- **patients router (GET /api/patients)** (api, existing): the list route in api/src/routes/patients.ts (C7), behind identify and requirePermission (C1, C2).
- **PatientsPage Last visit column (text render, no Date parsing)** (web, changed): shows the day string or "-" (C9).
- **web API client patient list type** (web, changed): the only path from web to API (C10).
- **appointment_patient_start_idx index** (db, existing): serves the per-patient max (C8).

## Threats
- T-1 · PatientRepo list query · information disclosure: medical history could leak into the list. Mitigation: explicit column list and a no-medical-history test (new); existing rule C4.
- T-2 · PatientRepo list query · tampering: the subquery could take search input as SQL. Mitigation: bound parameters only (new).
- T-3 · PatientRepo list query · denial of service: a slow per-row max. Mitigation: existing index (C8) and 20 per page.
- T-4 · PatientService list · repudiation: a disputed Last visit. Mitigation: appointment_event history (C5); the value is derived, never stored.
- T-5 · PatientService list · information disclosure: patient rows in logs. Mitigation: no logging of list items (new), existing logging rule.
- T-6 · time.ts localDay and loadConfig HOSPITAL_TZ · tampering: a wrong zone shifts dates. Mitigation: server-side config (C6) and the S-1 AC4 test.
- T-7 · patients router · elevation of privilege: every permission granted in the MVP (C2). Mitigation: keep requirePermission; see Q-01.
- T-8 · patients router · spoofing: no sign-in, so the actor is not trusted. Mitigation: existing identify; the change is read-only.
- T-9 · PatientsPage Last visit column · tampering: raw HTML insertion. Mitigation: plain text node only (new).
- T-10 · web API client patient list type · tampering: misleading non-day values. Mitigation: string-or-null type, otherwise "-" (new).
- T-11 · appointment_patient_start_idx index · denial of service: table scan. Mitigation: check the query plan uses the index (new).

## Known
- REQ-1, REQ-2, REQ-3, REQ-5 to REQ-13: covered by the design components modelled above; REQ-13 is the focus of T-1 and T-5.

## Assumed
- REQ-4: "most recent" orders by start time (design assumption); T-3 and T-11 rely on the index on patient and start time that this ordering uses.

## Unknown
- U-1 (non-blocking): who can reach the API while every permission is granted is not stated (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): until sign-in, is the API reachable only by hospital staff? Default: accept the existing MVP identity for this ticket.

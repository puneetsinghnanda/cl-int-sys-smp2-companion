---
schema: threat-model.v1
run_id: run_01M79P1N889PBTFM8J1EX89TBK
step: threat
attempt: 1
skill: threat-model@1.2.0
inputs:
  - {path: artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/design.md, sha256: 799df78b60dcfbc79dc1465cb608d0c935c43fb6ef1c5d88f90d28404b564347}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/data-model.md, sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55}
  - {path: context/generated/api-inventory.json, sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "In the MVP every request is the actor `staff` and every permission is granted.", cites: ["context/architecture.md#identity"]}
  - {id: C2, text: "Routes declare a permission with `requirePermission`, the seam sign-in will enforce.", cites: ["context/architecture.md#identity"]}
  - {id: C3, text: "The patient detail route declares the permission `patients:read`.", cites: ["sample-system:api/src/routes/patients.ts#L28"]}
  - {id: C4, text: "The patient detail route validates the path id with `parseId`.", cites: ["sample-system:api/src/routes/patients.ts#L30"]}
  - {id: C5, text: "Every appointment change is recorded in `appointment_event` with time and actor.", cites: ["context/data-model.md#appointments"]}
  - {id: C6, text: "The count reads appointment rows by patient through index `appointment_patient_start_idx`.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/design.md#claim:C18"]}
  - {id: C7, text: "Medical history leaves the database only for the `patient's own page`.", cites: ["context/data-model.md#patients"]}
  - {id: C8, text: "Logs never carry request bodies, `contact details` or medical history.", cites: ["context/conventions.md#logging-and-data"]}
  - {id: C9, text: "The web app talks to the API only through `web/src/api/client.ts`.", cites: ["context/architecture.md#components"]}
  - {id: C10, text: "The API also serves the built web app from `web/dist`.", cites: ["context/architecture.md#deployment-shape"]}
  - {id: C11, text: "The new field is `visitSummary { completed, noShow }`, two counts and nothing else.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/design.md#claim:C10"]}
  - {id: C12, text: "Create and update responses are built by `this.get`, so they also carry the counts.", cites: ["artifacts/HMS-5/run_01M79P1N889PBTFM8J1EX89TBK/design.md#claim:C12"]}
  - {id: C13, text: "Status values include COMPLETED and `NO_SHOW`.", cites: ["context/data-model.md#appointments"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [T-1, T-2, T-3, T-4, T-5, T-6, T-7, T-8, T-9, T-10, T-11, T-12]
assumed: []
unknown: []
questions: []
threats:
  - {id: T-1, component: "PatientRepo.visitSummary(id): counts the patient's COMPLETED and NO_SHOW appointments", stride: tampering, threat: "A crafted patient id or status value reaches the count SQL and changes the query.", mitigation: "Existing control: the route parses the id with parseId (C4). New control implement must build: the query passes the id as a bound parameter and the statuses COMPLETED and NO_SHOW (C13) as fixed literals, never concatenated input."}
  - {id: T-2, component: "PatientRepo.visitSummary(id): counts the patient's COMPLETED and NO_SHOW appointments", stride: information_disclosure, threat: "A count without the patient filter returns other patients' attendance figures on this patient's page.", mitigation: "New control implement must build: WHERE patient_id = the bound id; tested by S-1 AC5 (a second patient's appointments are not counted)."}
  - {id: T-3, component: "PatientRepo.visitSummary(id): counts the patient's COMPLETED and NO_SHOW appointments", stride: denial_of_service, threat: "An all-time count per page load scans many appointment rows and slows the patient page and the write endpoints that reuse get.", mitigation: "Existing control: the count is filtered by patient_id, which leads index appointment_patient_start_idx (C6); one aggregate query, no new table."}
  - {id: T-4, component: "PatientRepo.visitSummary(id): counts the patient's COMPLETED and NO_SHOW appointments", stride: repudiation, threat: "Staff dispute a count because a status change behind it cannot be traced.", mitigation: "Existing control: every status change is recorded in appointment_event with time and actor (C5); the counts are read-only and derived from those audited statuses."}
  - {id: T-5, component: "PatientService.get: adds visitSummary to the detail", stride: information_disclosure, threat: "The detail response, and the create and update responses built by the same get (C12), now expose a patient's attendance counts to every caller.", mitigation: "Existing control: the field holds only two integers (C11), no medical history or contact details; the same callers already receive the patient's full appointments list. Medical history stays on the patient's own page (C7)."}
  - {id: T-6, component: "PatientService.get: adds visitSummary to the detail", stride: elevation_of_privilege, threat: "Any user can read the counts because in the MVP every request is staff and every permission is granted (C1).", mitigation: "Existing control: the route declares patients:read through requirePermission (C2, C3), so sign-in enforces it later without touching this change; accepted MVP risk, unchanged by HMS-5."}
  - {id: T-7, component: "API type PatientDetail: gains visitSummary", stride: tampering, threat: "The database driver returns count(*) as text, so the response carries strings where the contract promises integers and the page shows wrong values.", mitigation: "New control implement must build: convert each count to a number (or cast in SQL) before it enters PatientDetail; a unit test asserts integer types."}
  - {id: T-8, component: "Web type PatientDetail: gains visitSummary", stride: tampering, threat: "The web type drifts from the API type, so the page reads a missing or renamed field and shows nothing or wrong numbers.", mitigation: "New control implement must build: both types declare the same visitSummary { completed, noShow } shape (C11), checked by the TypeScript build and the page test."}
  - {id: T-9, component: "PatientDetailPage: Visit summary block above the Appointments heading", stride: information_disclosure, threat: "The new block copies patient data into logs, storage or the URL, outside the patient's own page.", mitigation: "Existing control: logs never carry request bodies or medical history (C8). New control implement must build: render the two numbers as React text only, with no logging, storage or URL parameters."}
  - {id: T-10, component: "patientsRouter GET/POST/PATCH handlers", stride: spoofing, threat: "A caller poses as staff because the MVP has no sign-in and every request is the actor staff (C1).", mitigation: "Existing control: identify and requirePermission are the single seam that sign-in replaces (C2); HMS-5 adds no route and no bypass."}
  - {id: T-11, component: "web api client api.patient", stride: information_disclosure, threat: "The counts travel to a different origin or through a second client path.", mitigation: "Existing control: the web app calls the API only through web/src/api/client.ts (C9), and the API serves the web app itself from web/dist (C10); no new endpoint or origin."}
  - {id: T-12, component: "AppointmentTable on the patient page", stride: tampering, threat: "Changes for the Visit summary alter the appointments list, so staff see rows hidden or reordered.", mitigation: "Existing control: AppointmentTable is unchanged by the design; S-3 AC1 and AC2 check its rows and order stay as before."}
---
# Threat model: HMS-5

## Components
- PatientRepo.visitSummary(id): counts the patient's COMPLETED and NO_SHOW appointments. Handles the patient id and appointment statuses; reads the appointment table.
- PatientService.get: adds visitSummary to the detail. Handles the full patient detail, including medical history, as today; adds two integers.
- API type PatientDetail: gains visitSummary. The response contract.
- Web type PatientDetail: gains visitSummary. The web copy of the contract.
- PatientDetailPage: Visit summary block above the Appointments heading. Shows the two counts to staff.
- patientsRouter GET/POST/PATCH handlers. The entry point; identity and permission (unchanged).
- web api client api.patient. The only web path to the API (unchanged).
- AppointmentTable on the patient page. The appointments list (unchanged).

## Threats
- T-1 (PatientRepo.visitSummary, tampering): crafted input changes the count SQL. Mitigation: `parseId` (C4), plus a bound id parameter and fixed status literals that implement must build.
- T-2 (PatientRepo.visitSummary, information disclosure): an unscoped count leaks other patients' figures. Mitigation: the `patient_id` filter that implement must build, tested by S-1 AC5.
- T-3 (PatientRepo.visitSummary, denial of service): an all-time count is slow. Mitigation: index `appointment_patient_start_idx` (C6) and a single aggregate query.
- T-4 (PatientRepo.visitSummary, repudiation): a disputed count can't be traced. Mitigation: `appointment_event` audits every status change (C5).
- T-5 (PatientService.get, information disclosure): more data in the detail, create and update responses. Mitigation: only two integers (C11), and medical history stays on the patient's own page (C7).
- T-6 (PatientService.get, elevation of privilege): in the MVP every user has every permission. Mitigation: `patients:read` through `requirePermission` (C2, C3). This is an accepted MVP risk.
- T-7 (API type PatientDetail, tampering): counts arrive as text, not integers. Mitigation: implement must convert them to numbers, checked by a unit test.
- T-8 (Web type PatientDetail, tampering): the web and API types drift apart. Mitigation: the same shape in both (C11), checked by the TypeScript build and the page test.
- T-9 (PatientDetailPage, information disclosure): the counts copied into logs, storage or the URL. Mitigation: no logging of bodies or medical history (C8), and the numbers render as plain text only.
- T-10 (patientsRouter, spoofing): there is no sign-in in the MVP. Mitigation: the existing identity seam (C1, C2), unchanged.
- T-11 (web api client, information disclosure): a new origin or client path. Mitigation: a single client (C9), served from the same origin (C10).
- T-12 (AppointmentTable, tampering): the list is altered by mistake. Mitigation: the table is unchanged, checked by S-3 AC1 and AC2.

## Known
- T-1 to T-12: each threat's control is cited (C1 to C13), or is named as a new control the implement step must build (T-1, T-2, T-7, T-8, T-9).

## Assumed
None.

## Unknown
None.

## Open questions
None.

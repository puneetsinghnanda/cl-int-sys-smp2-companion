---
schema: threat-model.v1
run_id: run_01M60VHZHR79G15R3AKPB0VYDK
step: threat
attempt: 1
skill: threat-model@1.0.1
inputs:
  - path: artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md
    sha256: 8deafa866532fab9ef893732597031376c69447af56a8d73760a0458cf6e701a
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
    text: "Routes declare a permission with `requirePermission`."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C4
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C5
    text: "Medical history leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C6
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C7
    text: "One patient is read through `/api/patients/:id` with GET."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients/:id"]
  - id: C8
    text: "A patient is updated through `PATCH` on /api/patients/:id."
    cites: ["context/generated/api-inventory.json#http:PATCH:/api/patients/:id"]
  - id: C9
    text: "The design adds a `visit_summary` object with the two counts to the patient response."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md#overview"]
  - id: C10
    text: "The design counts appointments with status `COMPLETED` and `NO_SHOW` in `AppointmentRepo`."
    cites: ["artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md#overview"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: [REQ-2, REQ-3, REQ-9]
unknown: []
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Is `appointment.patient_id` indexed, so the new per-patient count query stays cheap as appointments grow (T-8)?"
    why: The design adds one aggregate query to every patient read. The inputs do not list indexes.
    evidence: ["context/data-model.md#appointments", "artifacts/HMS-5/run_01M60VHZHR79G15R3AKPB0VYDK/design.md#overview"]
    options: ["An index already covers patient_id", "Add an index in a new migration"]
    default: "Check the existing constraints. Add an index only if no existing one covers patient_id."
threats:
  - id: T-1
    component: PatientDetailPage
    stride: information_disclosure
    threat: "Visit counts become visible to anyone who can open the patient page."
    mitigation: "Existing control: the page loads data only through the API (C4), where every request passes `identify` and `requirePermission` (C1, C2). The counts are less sensitive than the medical history already shown on this page (C5)."
  - id: T-2
    component: PatientDetailPage
    stride: tampering
    threat: "Values in the response are rendered as markup, so page content could be injected."
    mitigation: "New control (implement): render the counts as plain numeric text only, never as raw HTML."
  - id: T-3
    component: PatientFormPage
    stride: tampering
    threat: "The edit form sends the read-only `visit_summary` back in the PATCH body (C8). The API could then accept or store client-supplied counts."
    mitigation: "New control (implement): `fromPatient` copies only the editable fields, and the PATCH path ignores or rejects `visit_summary`. The counts are always derived on read and never stored (C9)."
  - id: T-4
    component: api client (web/src/api/client.ts)
    stride: information_disclosure
    threat: "The response type change exposes more patient data to every caller of the client than the page needs."
    mitigation: "Existing control: the client is the single channel to the API (C4). New control (implement): add only the two integer counts to the type, with no appointment details."
  - id: T-5
    component: patientsRouter
    stride: spoofing
    threat: "A caller reads another patient's visit summary without a verified identity."
    mitigation: "Existing control: `identify` and `requirePermission` on every route (C1, C2). In the MVP every request acts as `staff` (C3), and sign-in will replace only these two functions."
  - id: T-6
    component: patientsRouter
    stride: elevation_of_privilege
    threat: "When sign-in is added, the counts stay reachable by roles that should not see patient pages."
    mitigation: "Existing control: the route keeps its declared permission through `requirePermission` (C2). No new route is added, so the new data inherits the patient read permission (C7)."
  - id: T-7
    component: PatientService
    stride: information_disclosure
    threat: "Counts for one patient are attached to another patient's response, through a wrong id passed to the count query."
    mitigation: "New control (implement and unit-test): pass the same patient id used for the patient read to the count query, and test it with two patients."
  - id: T-8
    component: PatientService
    stride: denial_of_service
    threat: "The extra aggregate query on every patient read slows the patient page as appointments grow."
    mitigation: "New control (implement): one grouped count filtered by patient id and the two statuses. Index coverage of patient_id is confirmed via Q-01."
  - id: T-9
    component: PatientService
    stride: repudiation
    threat: "Staff dispute how a count arose, because counts change when appointments are closed."
    mitigation: "Existing control: every appointment change is recorded in `appointment_event` with time and actor (C6). The new code only reads."
  - id: T-10
    component: AppointmentRepo
    stride: tampering
    threat: "The new count query builds SQL from request input and lets input change the query."
    mitigation: "New control (implement): pass the patient id as a bound parameter. The two status values are constants in code, not taken from the request (C10)."
  - id: T-11
    component: AppointmentRepo
    stride: information_disclosure
    threat: "The count query returns appointment rows or columns beyond the two counts."
    mitigation: "New control (implement): the query returns only counts per status. No row data leaves the repository for this feature."
  - id: T-12
    component: PatientRepo
    stride: information_disclosure
    threat: "Changes around the patient read widen where medical history is sent."
    mitigation: "Existing control: medical history leaves the database only for the patient's own page (C5). The design leaves `PatientRepo` unchanged."
---
# Threat model: HMS-5

## Components
- PatientDetailPage (web, changed): shows the patient record, medical history and the new Visit summary counts.
- PatientFormPage (web, existing): reads the patient response and sends edits through PATCH (C8).
- api client (web/src/api/client.ts) (web, existing): the only channel from the web app to the API (C4). Its patient type gains `visit_summary`.
- patientsRouter (api, existing): serves `GET /api/patients/:id` (C7) behind `identify` and `requirePermission` (C1, C2).
- PatientService (api, changed): adds `visit_summary` (C9) to the patient read.
- AppointmentRepo (api, changed): the new count query over appointment status (C10).
- PatientRepo (api, existing): reads patient rows, including medical history (C5).

## Threats
- T-1 PatientDetailPage — information disclosure: counts visible to anyone who can open the page. Mitigated by the existing API identity and permission checks.
- T-2 PatientDetailPage — tampering: values rendered as markup. New control: render the counts as plain numbers.
- T-3 PatientFormPage — tampering: read-only counts echoed back in PATCH. New control: copy only editable fields and ignore or reject `visit_summary`.
- T-4 api client (web/src/api/client.ts) — information disclosure: the type exposes more than needed. New control: add only the two integer counts.
- T-5 patientsRouter — spoofing: an unverified caller reads counts. Mitigated by the existing `identify` and `requirePermission`.
- T-6 patientsRouter — elevation of privilege: wrong roles reach counts after sign-in. Mitigated by the existing route permission, and no new route is added.
- T-7 PatientService — information disclosure: counts attached to the wrong patient. New control: use the same id for both queries, with a unit test.
- T-8 PatientService — denial of service: extra query cost per read. New control: one grouped count, with the index checked via Q-01.
- T-9 PatientService — repudiation: count history disputed. Mitigated by the existing `appointment_event` audit.
- T-10 AppointmentRepo — tampering: SQL built from input. New control: a bound patient id and constant statuses.
- T-11 AppointmentRepo — information disclosure: extra rows or columns returned. New control: the query returns counts only.
- T-12 PatientRepo — information disclosure: medical history sent more widely. Mitigated by the existing data rule, and `PatientRepo` is unchanged.

## Known
- REQ-1, REQ-7, REQ-8: the page placement and the unchanged list and medical history are covered by T-1, T-2 and T-12.
- REQ-4, REQ-5, REQ-6: the status filter and zero defaults sit in the count query, covered by T-10 and T-11.

## Assumed
- REQ-2, REQ-3: the counts travel as `visit_summary` on the patient response (design assumption). T-3, T-4 and T-7 depend on this shape.
- REQ-9: the count has no date filter (design assumption). This raises the query cost, so T-8 and Q-01 apply.

## Unknown
No threat is unassessed. The one gap, index coverage for the count query, is carried as non-blocking question Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): is `appointment.patient_id` indexed? Default: check the existing constraints, and add an index in a new migration only if none covers it.

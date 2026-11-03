---
schema: threat-model.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: threat
attempt: 1
skill: threat-model@1.0.1
inputs:
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md
    sha256: 215531f1ce96acf3cbf36175d23fe9f75a7d793b88397e524abe7591fb3b1056
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
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "Routes declare a permission with `requirePermission`; sign-in replaces identify and requirePermission only."
    cites: ["context/architecture.md#identity"]
  - id: C4
    text: "\"Today\" uses the hospital time zone setting `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C5
    text: "`Medical history` leaves the database only for the patient's own page."
    cites: ["context/data-model.md#patients"]
  - id: C6
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C7
    text: "Appointment status values include `NO_SHOW`."
    cites: ["context/data-model.md#appointments"]
  - id: C8
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C9
    text: "The doctor detail route is `/api/doctors/:id`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id"]
  - id: C10
    text: "Doctor updates use `PATCH` on /api/doctors/:id."
    cites: ["context/generated/api-inventory.json#http:PATCH:/api/doctors/:id"]
  - id: C11
    text: "The count query is served by the existing index `appointment_doctor_start_idx`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#overview"]
  - id: C12
    text: "GET /api/doctors/:id adds the read-only field `noShowsLast30Days`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#overview"]
  - id: C13
    text: "The link's `doctorId` must be spelled exactly, because the list ignores unknown query keys."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#overview"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does DoctorFormPage.fromDoctor map only named editable fields, and does PATCH /api/doctors/:id reject or ignore unknown body fields?"
    why: "T-10 depends on it: the new read-only fields must not be sent back in an update. The design leaves this to implement (its Q-03)."
    evidence: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/design.md#open-questions", "context/generated/api-inventory.json#http:PATCH:/api/doctors/:id"]
    options: ["Both keep unknown fields out; no new control", "Either passes them through; strip the fields explicitly and test it"]
    default: "Implement checks both and, if needed, strips noShowsLast30Days and noShowsWindow explicitly, with a test."
threats:
  - id: T-1
    component: Shared No-show window rule (Last 30 days)
    stride: tampering
    threat: "A wrong time zone or clock source shifts the window, so the doctor count and the dashboard tile disagree or include the wrong days."
    mitigation: "Existing: the rule uses the configured hospital time zone (HOSPITAL_TZ) and the service's clock, as the dashboard does today. New (implement): unit tests at the window edges and around midnight in the hospital time zone."
  - id: T-2
    component: DashboardService
    stride: tampering
    threat: "Extracting the 30d rule changes the dashboard's existing counts by accident (integrity of a figure staff rely on)."
    mitigation: "New (implement): move the rule without changing its logic and keep or add dashboard tests that assert the 30d No-shows count and period before and after the change."
  - id: T-3
    component: AppointmentRepo
    stride: tampering
    threat: "The doctor id or window bounds reach the count query as text, letting malformed input alter the SQL."
    mitigation: "New (implement): pass the doctor id and bounds only as bind parameters, never by string building; the id is the integer already parsed for GET /api/doctors/:id."
  - id: T-4
    component: AppointmentRepo
    stride: denial_of_service
    threat: "Every doctor page view runs an extra count; a slow query could load the database."
    mitigation: "Existing: the existing index appointment_doctor_start_idx on (doctor_id, starts_at) serves the count; one count per view, no scan of other doctors."
  - id: T-5
    component: DoctorService
    stride: information_disclosure
    threat: "The doctor detail response could start carrying patient details from the counted appointments."
    mitigation: "New (implement): return only the integer and the two local days; no patient fields. Existing rule: medical history leaves the database only for the patient's own page."
  - id: T-6
    component: doctorsRouter
    stride: spoofing
    threat: "Anyone who can reach the API sees the count, because the MVP has no sign-in."
    mitigation: "Existing, accepted for the MVP: every request passes identify and is the actor staff; the route keeps its requirePermission, so adding sign-in covers this field with no extra work."
  - id: T-7
    component: doctorsRouter
    stride: elevation_of_privilege
    threat: "The count is exposed under the doctor read permission even if later roles should not see attendance figures."
    mitigation: "Existing: the field rides on the existing doctor read route and its permission; no new permission is introduced. A separate permission is out of scope for HMS-7."
  - id: T-8
    component: time helpers (api/src/lib/time.ts)
    stride: tampering
    threat: "A mistaken HOSPITAL_TZ setting makes 'today' and the window wrong for every count."
    mitigation: "Existing: the time zone comes from configuration (HOSPITAL_TZ, default Asia/Kolkata) shared with slots and the dashboard, so one setting governs all. No new control."
  - id: T-9
    component: DoctorDetailPage
    stride: information_disclosure
    threat: "A wrongly named or unencoded link parameter drops the doctor filter, so the list opens on every doctor's No-shows and misleads staff."
    mitigation: "New (implement): build the link with URL encoding from the API's noShowsWindow and the doctor id, use the exact name doctorId, and test the exact URL."
  - id: T-10
    component: DoctorFormPage
    stride: tampering
    threat: "The read-only fields are copied into the edit form and sent back in PATCH, where a client could try to set them."
    mitigation: "New (implement, see Q-01): confirm fromDoctor maps only editable fields and PATCH ignores or rejects unknown fields; otherwise strip the two fields explicitly and test it."
  - id: T-11
    component: AppointmentsPage
    stride: information_disclosure
    threat: "Opening the list from the count shows appointments to whoever can open the page."
    mitigation: "Existing: the list is unchanged and shows what it shows today; medical history never appears in lists. Access follows the same MVP identity as T-6."
  - id: T-12
    component: API client (web/src/api/client.ts)
    stride: information_disclosure
    threat: "Extra fields in the doctor response reach places that render the whole object."
    mitigation: "Existing: all calls go through the single API client, and the new fields are an aggregate count and two dates with no personal data."
  - id: T-13
    component: appointment table and appointment_doctor_start_idx
    stride: repudiation
    threat: "A change in No-show status could alter the count with no trace."
    mitigation: "Existing: the feature only reads; status changes are already recorded in appointment_event with time and actor, and closed appointments never change."
---
# Threat model: HMS-7

## Components
- Shared No-show window rule (Last 30 days): computes the 30d window from the clock and the hospital time zone; no personal data.
- DashboardService: dashboard counts; aggregate numbers only.
- AppointmentRepo: reads appointment rows by doctor, status `NO_SHOW` and start time; returns a count.
- DoctorService: doctor detail plus the new count and window.
- doctorsRouter: GET /api/doctors/:id, behind identify and requirePermission.
- time helpers (api/src/lib/time.ts): day and time-zone arithmetic.
- DoctorDetailPage: shows the count and builds the list link.
- DoctorFormPage: edit form fed by the doctor detail response.
- AppointmentsPage: the list opened by the link.
- API client (web/src/api/client.ts): the only path from web to API.
- appointment table and appointment_doctor_start_idx: stored appointments; read only here.

## Threats
- T-1 Shared No-show window rule — tampering: wrong zone or clock shifts the window. Mitigation: configured HOSPITAL_TZ and service clock; new edge and midnight tests.
- T-2 DashboardService — tampering: extraction changes dashboard counts. Mitigation: move logic unchanged; dashboard regression tests (new).
- T-3 AppointmentRepo — tampering: input altering the SQL. Mitigation: bind parameters only (new).
- T-4 AppointmentRepo — denial of service: extra count per view. Mitigation: existing index on doctor and start time.
- T-5 DoctorService — information disclosure: patient details leaking into the response. Mitigation: return only the count and dates (new); existing medical-history rule.
- T-6 doctorsRouter — spoofing: no sign-in in the MVP. Mitigation: existing identify/requirePermission; accepted for the MVP.
- T-7 doctorsRouter — elevation of privilege: count under the doctor read permission. Mitigation: existing permission, no new one; separate permission out of scope.
- T-8 time helpers — tampering: wrong HOSPITAL_TZ. Mitigation: one shared configuration setting.
- T-9 DoctorDetailPage — information disclosure: wrong link parameter widens the list. Mitigation: encoded link with exact doctorId and a URL test (new).
- T-10 DoctorFormPage — tampering: read-only fields sent back in PATCH. Mitigation: check fromDoctor and PATCH; strip and test if needed (new, Q-01).
- T-11 AppointmentsPage — information disclosure: list visible to page users. Mitigation: list unchanged; no medical history in lists.
- T-12 API client — information disclosure: extra fields rendered elsewhere. Mitigation: single client; fields hold no personal data.
- T-13 appointment table and index — repudiation: untraced status changes. Mitigation: read only; appointment_event already records changes.

New controls for implement: T-1, T-2, T-3, T-5, T-9, T-10.

## Known
- REQ-1 to REQ-8: every component that serves them has at least one threat row with an existing or new control.

## Assumed
None.

## Unknown
- U-1 (non-blocking): whether fromDoctor and PATCH keep unknown fields out, which decides T-10's control — Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): do fromDoctor and PATCH /api/doctors/:id keep the new read-only fields out? Default: implement checks both and strips the fields with a test if needed.

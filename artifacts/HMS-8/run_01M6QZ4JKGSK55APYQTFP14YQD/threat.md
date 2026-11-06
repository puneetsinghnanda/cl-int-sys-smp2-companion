---
schema: threat-model.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
step: threat
attempt: 1
skill: threat-model@1.1.0
inputs:
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/design.md
    sha256: 42358b2515450494074ee4e85508c7791b3c755e3e822e4ffc47d3c48b0c96a7
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: dd1254b0a78f85d6da3dd3722fd41947c0cec5177fe5b46028e49af681dc4c85
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "The appointment list route declares `requirePermission('appointments:read')`."
    cites: ["sample-system:api/src/routes/appointments.ts#L12"]
  - id: C2
    text: "The doctors list route declares `requirePermission('doctors:read')`."
    cites: ["sample-system:api/src/routes/doctors.ts#L14"]
  - id: C3
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C4
    text: "The appointment repository binds filter values as parameters with `params.push(value)`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L65"]
  - id: C5
    text: "The appointment service accepts status only from the list `APPOINTMENT_STATUSES`; the design extends that list with NOT_CANCELLED only."
    cites: ["sample-system:api/src/services/appointmentService.ts#L35"]
  - id: C6
    text: "The doctor name search is a bound `ILIKE` value with wildcard characters escaped."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L85"]
  - id: C7
    text: "Lists are capped by `pageSize: 20`."
    cites: ["sample-system:api/src/config.ts#L41"]
  - id: C8
    text: "Medical history leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C9
    text: "Logs carry method, route and error message only, `never request bodies`, contact details or medical history."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C10
    text: "Invalid input is a 400 with one message per field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C11
    text: "The Today count excludes Cancelled with `status <> 'CANCELLED'`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L129"]
  - id: C12
    text: "The design's Picker label is `${d.fullName} (${d.code}) · ${d.specialization.name}`: name, display code and specialization only."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/design.md#claim:C4"]
  - id: C13
    text: "The web client builds query strings with `export function query`."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/design.md#claim:C10"]
  - id: C14
    text: "The impact found an existing `DoctorDetailPage` test that guards the No-shows href."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/impact.md#claim:C23"]
  - id: C15
    text: "Every appointment change is recorded in `appointment_event`; this change only reads appointments."
    cites: ["context/data-model.md#appointments"]
  - id: C16
    text: "The impact found the existing `DoctorsPage` test asserts the Today link href."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/impact.md#claim:C21"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [T-1, T-2, T-3, T-4, T-5, T-6, T-7, T-8, T-9, T-10, T-11, T-12, T-13, T-14]
assumed: []
unknown: []
questions: []
threats:
  - id: T-1
    component: AppointmentsPage (doctor Picker, doctor label, Any except Cancelled option, Clear All)
    stride: information_disclosure
    threat: "The new doctor label or the list could show more data than the list shows today, such as a patient's medical history."
    mitigation: "Existing control: the label holds only name, display code and specialization (C12), and medical history never leaves the database for lists (C8). Implement adds no other field."
  - id: T-2
    component: AppointmentsPage (doctor Picker, doctor label, Any except Cancelled option, Clear All)
    stride: tampering
    threat: "A crafted list URL could carry unexpected doctorId or status values, or label text that is rendered as markup."
    mitigation: "Existing control: the API validates doctorId and status and answers a 400 ValidationError (C5, C10). New control for implement: render the doctor label as plain text only, never as HTML."
  - id: T-3
    component: AppointmentsPage (doctor Picker, doctor label, Any except Cancelled option, Clear All)
    stride: spoofing
    threat: "Without sign-in, anyone who reaches the app acts as staff and can use the new filters."
    mitigation: "Accepted MVP risk, unchanged by this ticket: every request is the actor staff (C3), and sign-in replaces identify and requirePermission only."
  - id: T-4
    component: Picker component
    stride: denial_of_service
    threat: "Search-as-you-type on the list page sends a request per keystroke to GET /api/doctors."
    mitigation: "Existing control: each response is capped at 20 per page (C7), and the search value is a bound, escaped parameter (C6)."
  - id: T-5
    component: Picker component
    stride: information_disclosure
    threat: "The chooser now offers Inactive doctors, which the booking picker hides."
    mitigation: "Accepted by A-01. Only name, code and specialization are shown (C12), and the route keeps requirePermission('doctors:read') (C2)."
  - id: T-6
    component: DoctorsPage Today link
    stride: tampering
    threat: "The Today link could be built by string concatenation and carry a malformed or wrong status."
    mitigation: "Existing control: the link is built with the client's query helper (C13). Implement passes the fixed literal NOT_CANCELLED, never a value taken from input."
  - id: T-7
    component: DoctorsPage test (HMS-6 Today link href)
    stride: tampering
    threat: "Editing the test could weaken it so that a Today link missing the status still passes."
    mitigation: "New control for implement: the test asserts the exact href, including status=NOT_CANCELLED (C16)."
  - id: T-8
    component: AppointmentService.list status validation
    stride: tampering
    threat: "Widening the status check could let arbitrary status strings reach the repository."
    mitigation: "Existing control: status is checked against a fixed allowed list (C5). Implement adds only NOT_CANCELLED to it, and other values stay a 400 ValidationError (C10)."
  - id: T-9
    component: AppointmentService.list status validation
    stride: elevation_of_privilege
    threat: "The changed list path could bypass the route's permission check."
    mitigation: "Existing control: GET /api/appointments keeps requirePermission('appointments:read') (C1). Implement does not touch the route."
  - id: T-10
    component: AppointmentFilter and AppointmentRepo.list status condition
    stride: tampering
    threat: "The new branch for NOT_CANCELLED adds SQL that, if built from the request value, would let input change the query."
    mitigation: "New control for implement: the NOT_CANCELLED branch adds the fixed condition from the Today count (C11) and takes nothing from the request. Every other value stays a bound parameter (C4)."
  - id: T-11
    component: AppointmentFilter and AppointmentRepo.list status condition
    stride: repudiation
    threat: "A change in the list path could alter appointments without an event record."
    mitigation: "Existing control: the change only reads appointments, and every change is still recorded in appointment_event (C15)."
  - id: T-12
    component: AppointmentFilter and AppointmentRepo.list status condition
    stride: denial_of_service
    threat: "The not-equal condition could make the list query return or scan many rows."
    mitigation: "Existing control: results stay capped at 20 per page (C7), and doctor and date conditions still apply."
  - id: T-13
    component: Web API client (api.appointments, api.doctors, api.doctor, query)
    stride: information_disclosure
    threat: "Filter values in the query string could end up in server logs."
    mitigation: "Existing control: logs carry method, route and error message only, never request bodies (C9). The filters hold ids and a status, not personal data."
  - id: T-14
    component: DoctorDetailPage and DashboardPage links to the list
    stride: tampering
    threat: "The change could unintentionally alter the other links into the list, so staff see the wrong appointments."
    mitigation: "Existing control: the DoctorDetailPage test guards the No-shows href (C14). Implement leaves these links untouched."
---
# Threat model: HMS-8

## Components
- AppointmentsPage (doctor Picker, doctor label, Any except Cancelled option, Clear All): reads URL filters (doctorId, patientId, status, dates) and shows doctor names and appointment summaries.
- Picker component: sends a search text to GET /api/doctors and shows doctor names, codes and specializations.
- DoctorsPage Today link: builds a list URL from a doctor id, today's date and the fixed NOT_CANCELLED status.
- DoctorsPage test (HMS-6 Today link href): test code only.
- AppointmentService.list status validation: validates the query parameters.
- AppointmentFilter and AppointmentRepo.list status condition: reads appointment rows.
- Web API client (api.appointments, api.doctors, api.doctor, query): query strings to the API.
- DoctorDetailPage and DashboardPage links to the list: existing link URLs, unchanged.

## Threats
- T-1 AppointmentsPage, information disclosure: the label or list could expose more data. The label is only name, code and specialization (C12), and medical history is never in lists (C8).
- T-2 AppointmentsPage, tampering: a crafted URL or label markup. The API validates and answers 400 (C5, C10). Implement renders the label as plain text.
- T-3 AppointmentsPage, spoofing: no sign-in, so every request is staff (C3). This is an accepted MVP risk, unchanged.
- T-4 Picker component, denial of service: a request per keystroke. Each response is capped at 20 (C7), and the search value is a bound, escaped parameter (C6).
- T-5 Picker component, information disclosure: Inactive doctors are offered. This was accepted by A-01. Only the label fields are shown (C12), and the permission check stays (C2).
- T-6 DoctorsPage Today link, tampering: a malformed link. It is built with the query helper and a fixed literal status (C13).
- T-7 DoctorsPage test, tampering: a weakened assertion. Implement asserts the exact href with status=NOT_CANCELLED (C16).
- T-8 AppointmentService.list status validation, tampering: arbitrary statuses getting through. The allowed list gains only NOT_CANCELLED, and anything else is a 400 (C5, C10).
- T-9 AppointmentService.list status validation, elevation of privilege: a permission bypass. The route keeps requirePermission('appointments:read') (C1).
- T-10 AppointmentFilter and AppointmentRepo.list status condition, tampering: SQL built from input. The NOT_CANCELLED branch uses the fixed condition (C11), and other values stay bound parameters (C4).
- T-11 AppointmentFilter and AppointmentRepo.list status condition, repudiation: an unrecorded change. The change is read-only, and appointment_event is unchanged (C15).
- T-12 AppointmentFilter and AppointmentRepo.list status condition, denial of service: large results. Still capped at 20 per page (C7).
- T-13 Web API client, information disclosure: filters in logs. Logs carry the route only, and the filters hold no personal data (C9).
- T-14 DoctorDetailPage and DashboardPage links, tampering: unintended link changes. The existing test guards the No-shows href (C14), and implement leaves these links untouched.

## Known
- T-1 to T-14: each rests on a cited control in the code or context (C1–C16), or on a control the implement step must build (T-2, T-6, T-7, T-10).

## Assumed
None.

## Unknown
None.

## Open questions
None.

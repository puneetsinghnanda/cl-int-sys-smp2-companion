---
schema: threat-model.v1
run_id: run_01M7Q1DS00W5S7BFVX9Y70NPZG
step: threat
attempt: 2
skill: threat-model@1.3.0
inputs:
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/design.md
    sha256: 67fec0fca1d7cdefdee99ee1eeed978a3cefdfdb2b3ca102126f07401d052b77
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
resolves:
  - id: F-1
    how: "C13 now covers seed data only, and invented test fixtures are a new control for implement in T-15 and T-16."
  - id: F-2
    how: "The started-list no-PII unit test is now a new control for implement in T-7 and is in the list of new controls."
claims:
  - id: C1
    text: "Every API request passes through `identify`, which sets the actor; routes call requirePermission."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "The appointment list route declares `requirePermission('appointments:read')`."
    cites: ["sample-system:api/src/routes/appointments.ts#L12"]
  - id: C4
    text: "The dashboard route declares `requirePermission('dashboard:read')`."
    cites: ["sample-system:api/src/routes/dashboard.ts#L10"]
  - id: C5
    text: "A patient row holds identity, `contact details` and the basic medical history."
    cites: ["context/data-model.md#patients"]
  - id: C6
    text: "Medical history leaves the database only for the `patient's own page`."
    cites: ["context/data-model.md#patients"]
  - id: C7
    text: "Appointment list rows name the patient by `p.full_name AS patient_name`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L22"]
  - id: C8
    text: "Logs carry method, route and error message only, never request bodies, `contact details` or medical history."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C9
    text: "Invalid input is 400 with one message per field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C10
    text: "A value outside the allowed set fails with the field message `must be one of`."
    cites: ["sample-system:api/src/lib/validate.ts#L65"]
  - id: C11
    text: "Lists answer items, page, page_size and total, `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C12
    text: "`appointment_event` records every change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C13
    text: "`Seed data is invented`; the data rules say nothing about test fixtures."
    cites: ["context/data-model.md#data-rules"]
  - id: C14
    text: "The `started` filter compares with the server's now, decided as `New filter, server now`."
    cites: ["runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01"]
  - id: C15
    text: "The list filter is the query parameter `started=true`."
    cites: ["runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-03"]
  - id: C16
    text: "The appointments route forwards the raw query to the service: `services.appointments.list(req.query`."
    cites: ["sample-system:api/src/routes/appointments.ts#L14"]
  - id: C17
    text: "AppointmentService validates the status filter with `f.oneOf('status', APPOINTMENT_STATUSES)`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L35"]
  - id: C18
    text: "An existing API test checks `DSH-9: carries no contact details or medical history`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/dashboard.test.ts"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
questions: []
threats:
  - id: T-1
    component: DashboardService (awaitingOutcome count)
    stride: information_disclosure
    threat: "The new count query could be written to return or join patient columns, putting contact details or medical history on the dashboard (REQ-12, REQ-13)."
    mitigation: "The count is an aggregate over the appointment table only and returns a number; no patient columns are selected (C5, C6). Existing control: the DSH-9 API test asserts the dashboard carries no contact details or medical history (C18)."
  - id: T-2
    component: DashboardService (awaitingOutcome count)
    stride: denial_of_service
    threat: "The count has no period window, so it scans every Scheduled appointment that ever started; a large backlog makes each dashboard load slower."
    mitigation: "Accepted risk: it is one aggregate per request and the backlog shrinks as outcomes are recorded. No new control; revisit if dashboard latency grows."
  - id: T-3
    component: Dashboard type (API, totals.awaitingOutcome)
    stride: information_disclosure
    threat: "Widening the type could later invite non-count fields into totals."
    mitigation: "The field is a non-null integer count (design EP-1); type checks keep totals numeric."
  - id: T-4
    component: AppointmentService list (validates started, sets server now)
    stride: tampering
    threat: "A caller sends an unexpected started value (or several) to change the filter's meaning or reach the SQL."
    mitigation: "Existing control: the service validates query fields with the same validator it uses for status (C17); any value other than true is 400 ValidationError on started (C9, C10). New control for implement: the bound is the server clock, never taken from the request (C14)."
  - id: T-5
    component: AppointmentService list (validates started, sets server now)
    stride: spoofing
    threat: "Any caller can reach the list; in the MVP there is no sign-in, so the actor cannot be told apart."
    mitigation: "Existing control: identify sets the actor and the route declares appointments:read (C1, C3). Known MVP gap: every request is staff and every permission is granted (C2); sign-in replaces these functions without changes here."
  - id: T-6
    component: AppointmentFilter and AppointmentRepo list query (start-before condition)
    stride: tampering
    threat: "The start-before condition is built by string concatenation, letting input alter the SQL."
    mitigation: "New control for implement: add the condition with a bound parameter like the existing filters; the value is a server Date, never request text (C14)."
  - id: T-7
    component: AppointmentFilter and AppointmentRepo list query (start-before condition)
    stride: information_disclosure
    threat: "The filtered list could expose patient contact details or medical history (REQ-12, REQ-13)."
    mitigation: "Existing control: list rows carry the patient name only (C7), and the change adds no columns. New control for implement: a unit test asserting that rows of the started list carry no contact details or medical history."
  - id: T-8
    component: AppointmentFilter and AppointmentRepo list query (start-before condition)
    stride: denial_of_service
    threat: "A started list over a large backlog returns many rows."
    mitigation: "Existing control: lists are paged at 20 per page (C11); the filter does not change paging."
  - id: T-9
    component: Appointments and dashboard routes
    stride: elevation_of_privilege
    threat: "A new parameter or field could be served without a permission check."
    mitigation: "Existing control: both routes already declare appointments:read and dashboard:read (C3, C4) and are not changed; the routes forward the query to the services (C16)."
  - id: T-10
    component: Appointments and dashboard routes
    stride: repudiation
    threat: "Reads of the started list are not recorded, so who viewed which appointments cannot be shown later."
    mitigation: "Accepted for the MVP: request logs carry method and route but no actor (C8), and every request is staff (C2), so no read audit is possible until sign-in exists. Changes are still recorded in appointment_event (C12). No new control."
  - id: T-11
    component: Dashboard type (web, totals.awaitingOutcome)
    stride: tampering
    threat: "A response missing the field would render as undefined or NaN instead of a count."
    mitigation: "The type declares a required number (design EP-1); the API always returns it, and the web test's typed fixture enforces the shape."
  - id: T-12
    component: DashboardPage (Awaiting outcome tile and link)
    stride: information_disclosure
    threat: "The tile could show patient-level detail rather than a count."
    mitigation: "The tile renders only totals.awaitingOutcome and a fixed link to status=SCHEDULED and started=true (C15); no patient data is fetched for it."
  - id: T-13
    component: AppointmentsPage (passes started from the URL)
    stride: tampering
    threat: "A user edits the URL to send an arbitrary started value to the API."
    mitigation: "Existing control: the page builds the query with the client helper, and the server rejects any value other than true with a 400 (C9, C10); the page shows the error instead of a wrong list."
  - id: T-14
    component: API client query helper
    stride: tampering
    threat: "A query value is not encoded, so a crafted value changes other parameters."
    mitigation: "The helper is existing and unchanged; the only new value it carries is the fixed literal true (C15)."
  - id: T-15
    component: API dashboard test (totals toEqual)
    stride: information_disclosure
    threat: "New test fixtures could include real patient data."
    mitigation: "New control for implement: new or changed fixtures use invented identities only, following the seed-data rule (C13); the totals fixture holds counts only."
  - id: T-16
    component: Web DashboardPage test (typed fixture and tile assertions)
    stride: information_disclosure
    threat: "New test fixtures could include real patient data."
    mitigation: "New control for implement: new or changed fixtures use invented identities only, following the seed-data rule (C13); the fixture holds counts only."
---
# Threat model: HMS-21

## Resolves
- F-1: C13 now covers seed data only, and invented test fixtures are a new control for implement in T-15 and T-16.
- F-2: The started-list no-PII unit test is now a new control for implement in T-7 and is in the list of new controls.

## Components
- DashboardService (awaitingOutcome count): it reads appointment status and start times and returns a count.
- Dashboard type (API, totals.awaitingOutcome): a numeric field in the response shape.
- AppointmentService list (validates started, sets server now): it reads the `started` query value and sets a server-time bound.
- AppointmentFilter and AppointmentRepo list query (start-before condition): SQL over appointments. Rows name the patient only (C7).
- Appointments and dashboard routes: unchanged, each behind its permission (C3, C4).
- Dashboard type (web, totals.awaitingOutcome): the web copy of the count field.
- DashboardPage (Awaiting outcome tile and link): it shows the count and a fixed link.
- AppointmentsPage (passes started from the URL): it forwards `started` from the URL.
- API client query helper: it builds query strings and is unchanged.
- API dashboard test (totals toEqual) and Web DashboardPage test (typed fixture and tile assertions): test fixtures. The data rules cover seed data only (C13).

Patient rows hold `contact details` and medical history (C5). Medical history leaves the database only for the patient's own page (C6). The change does not read either.

## Threats
New controls the implement step must build:
- T-4: the bound always comes from the server clock.
- T-6: the start-before condition uses a bound parameter.
- T-7: a unit test checks that the started list carries no contact details or medical history.
- T-15 and T-16: test fixtures use invented identities only.

By risk, the highest first:
- T-7 AppointmentRepo list query — information disclosure. Mitigation: rows carry the patient name only (C7), and no columns are added. New: a unit test checks the started list has no contact details or medical history.
- T-1 DashboardService — information disclosure. The count could expose patient columns. Mitigation: an aggregate over appointments only, plus the existing DSH-9 test (C18).
- T-6 AppointmentRepo list query — tampering. SQL built by concatenation. New control for implement: use a bound parameter whose value is a server Date.
- T-4 AppointmentService list — tampering. A caller sends an unexpected `started` value. Mitigation: the existing validator rejects anything other than `true` with a 400 (C9, C10, C17). New: the bound is always the server clock (C14).
- T-5 AppointmentService list — spoofing. Mitigation: `identify` and `appointments:read` (C1, C3). The MVP grants every permission to `staff` (C2), a known gap that sign-in closes.
- T-9 Routes — elevation of privilege. Mitigation: the routes are unchanged and keep their permissions (C3, C4, C16).
- T-13 AppointmentsPage — tampering. A user edits `started` in the URL. Mitigation: the server returns a 400 for any value other than `true` (C9, C10).
- T-12 DashboardPage — information disclosure. Mitigation: the tile shows the count only, with a fixed link (C15).
- T-15 API dashboard test — information disclosure. New control: invented identities only, following the seed-data rule (C13).
- T-16 Web DashboardPage test — information disclosure. New control: invented identities only, following the seed-data rule (C13).
- T-10 Routes — repudiation. Reads are not recorded. Accepted for the MVP: logs carry no actor (C8), and every request is staff (C2). Changes go to `appointment_event` (C12).
- T-2 DashboardService — denial of service. The count has no period window. Accepted risk: it is one aggregate query.
- T-8 AppointmentRepo list query — denial of service. Mitigation: paging at `20 per page` (C11).
- T-14 API client query helper — tampering. Mitigation: the helper is unchanged and carries only the fixed literal `true` (C15).
- T-11 Dashboard type (web) — tampering. A missing field would render wrongly. Mitigation: the field is a required number, and the typed test fixture enforces it.
- T-3 Dashboard type (API) — information disclosure. Mitigation: the field is a non-null integer count.

## Known
- REQ-1 to REQ-11: the threats to the count, the filter and the tile are covered by T-1 to T-14, using the cited controls.
- REQ-12, REQ-13: no medical history or contact details. T-1, T-7 and T-12 cover them (C5, C6, C7, C8, C18).

## Assumed
None.

## Unknown
None.

## Open questions
None.

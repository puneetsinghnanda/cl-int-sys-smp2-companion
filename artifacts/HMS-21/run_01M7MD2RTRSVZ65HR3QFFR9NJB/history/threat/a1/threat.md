---
schema: threat-model.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
step: threat
attempt: 1
skill: threat-model@1.2.0
inputs:
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md, sha256: 7af71278d91675313b29a7418b6bbc21ac67eec00434e34a677eb231824f5a7d}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/data-model.md, sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55}
  - {path: context/generated/api-inventory.json, sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "Every API request passes through `identify`, which sets the actor; routes call requirePermission.", cites: ["context/architecture.md#identity"]}
  - {id: C2, text: "The dashboard route declares `requirePermission('dashboard:read')`.", cites: ["sample-system:api/src/routes/dashboard.ts#L10"]}
  - {id: C3, text: "The appointment list route declares `requirePermission('appointments:read')`.", cites: ["sample-system:api/src/routes/appointments.ts#L12"]}
  - {id: C4, text: "The repository binds every filter value as a numbered parameter with `params.push(value)`.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L65"]}
  - {id: C5, text: "The Fields validator's `oneOf` rejects values outside its allowed list.", cites: ["sample-system:api/src/lib/validate.ts#L62"]}
  - {id: C6, text: "Validation failures are collected and thrown as one `ValidationError`.", cites: ["sample-system:api/src/lib/validate.ts#L114"]}
  - {id: C7, text: "The app installs a shared `errorHandler` for all routes.", cites: ["sample-system:api/src/app.ts#L46"]}
  - {id: C8, text: "The appointment list is paged with the configured `pageSize`.", cites: ["sample-system:api/src/services/appointmentService.ts#L41"]}
  - {id: C9, text: "The configured page size is `pageSize: 20`.", cites: ["sample-system:api/src/config.ts#L41"]}
  - {id: C10, text: "Medical history `leaves the database only` for the patient's own page.", cites: ["context/data-model.md#patients"]}
  - {id: C11, text: "List rows carry only the patient's name, `p.full_name AS patient_name`.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L22"]}
  - {id: C12, text: "The dashboard returns counts only, with no contact details or medical history (`DSH-9`).", cites: ["sample-system:api/src/services/dashboardService.ts#L28"]}
  - {id: C13, text: "`appointment_event` records every appointment change with time and actor.", cites: ["context/data-model.md#appointments"]}
  - {id: C14, text: "The started filter compares with the server clock: `started=true, server now`.", cites: ["runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-03"]}
  - {id: C15, text: "The dashboard totals already run in one query using `count(*)` sub-selects.", cites: ["sample-system:api/src/services/dashboardService.ts#L52"]}
  - {id: C16, text: "Index `appointment_start_idx` exists on appointment start time.", cites: ["context/generated/db-schema.json#index:appointment_start_idx"]}
  - {id: C17, text: "The web app talks to the API only through `web/src/api/client.ts`.", cites: ["context/architecture.md#components"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [T-1, T-2, T-3, T-4, T-5, T-6, T-7, T-8, T-9, T-10, T-11, T-12, T-13, T-14]
assumed: []
unknown: []
questions: []
threats:
  - {id: T-1, component: "DashboardService.get: awaitingOutcome count", stride: information_disclosure, threat: "The new count query could return or log patient data beyond a number.", mitigation: "Existing control: the service returns counts only (C12). Implement adds only a count(*) sub-select with no patient columns."}
  - {id: T-2, component: "DashboardService.get: awaitingOutcome count", stride: denial_of_service, threat: "The new count, which has no period bound, scans more rows than the period-bounded counts as appointments grow.", mitigation: "Existing controls: one query with count sub-selects (C15) and the starts_at index (C16). No new control."}
  - {id: T-3, component: "Dashboard type (API)", stride: information_disclosure, threat: "Widening the response type could invite adding non-count fields to the dashboard payload.", mitigation: "New control for implement: add only the integer awaitingOutcome to totals; the dashboard test's toEqual on totals pins the exact shape (C12)."}
  - {id: T-4, component: "AppointmentService.list: started filter", stride: tampering, threat: "A crafted started value could alter the query or the filter's meaning.", mitigation: "Existing controls: oneOf allows only listed values (C5), errors are collected as one ValidationError (C6), and the value never reaches SQL as text (C4)."}
  - {id: T-5, component: "AppointmentService.list: started filter", stride: spoofing, threat: "A caller could reach the filtered list without being identified.", mitigation: "Existing control: identify sets the actor (C1) and the list route requires appointments:read (C3). The MVP grants every permission to staff, unchanged here."}
  - {id: T-6, component: "AppointmentService.list: started filter", stride: tampering, threat: "A client could try to supply its own instant to move the before-now boundary.", mitigation: "Existing design control: the boundary is the server clock, not a client value (C14). Implement must not read an instant from the query."}
  - {id: T-7, component: "AppointmentFilter and AppointmentRepo.list: startedBefore bound", stride: tampering, threat: "A new SQL fragment built by string concatenation could allow injection.", mitigation: "Existing control: the add helper binds the value as a numbered parameter (C4). Implement must add startedBefore through add()."}
  - {id: T-8, component: "AppointmentFilter and AppointmentRepo.list: startedBefore bound", stride: denial_of_service, threat: "Started appointments with no period bound could form a very large result.", mitigation: "Existing control: the list is paged with pageSize 20 (C8, C9)."}
  - {id: T-9, component: "Dashboard type (web)", stride: information_disclosure, threat: "The web type could expose a field the API never sends, leading the page to render undefined data.", mitigation: "New control for implement: mirror only awaitingOutcome: number; the web fixture typed Dashboard catches drift."}
  - {id: T-10, component: "DashboardPage: Awaiting outcome tile", stride: information_disclosure, threat: "The tile could show patient details instead of a count.", mitigation: "Existing control: tiles render a value and a label only, and the API sends only counts (C12)."}
  - {id: T-11, component: "AppointmentsPage: pass started through", stride: information_disclosure, threat: "The list opened from the tile could show contact details or medical history.", mitigation: "Existing controls: list rows carry the patient's name only (C11), and medical history leaves the database only for the patient's own page (C10)."}
  - {id: T-12, component: "AppointmentsPage: pass started through", stride: tampering, threat: "A user could edit the URL's started value.", mitigation: "Existing control: the API validates it (C5) and the web app reaches the API only through the client (C17), so an invalid value gives a 400 field error, not a wider list."}
  - {id: T-13, component: "Dashboard tests (api and web): add awaitingOutcome", stride: repudiation, threat: "Weakening the tests could hide an unreviewed change to the dashboard shape.", mitigation: "New control for implement: keep the exact toEqual on totals and add awaitingOutcome to it, rather than loosening the match."}
  - {id: T-14, component: "appointment table and appointment_start_idx", stride: repudiation, threat: "Read-only queries leave no audit trail of who looked at the list.", mitigation: "Accepted: no new write. Changes are already recorded in appointment_event (C13); reads are not audited today and HMS-21 adds none."}
---
# Threat model: HMS-21

## Components
- DashboardService.get: awaitingOutcome count. Handles appointment status and start times; returns an integer.
- Dashboard type (API). Shapes the dashboard response: counts only.
- AppointmentService.list: started filter. Handles one query value from the URL.
- AppointmentFilter and AppointmentRepo.list: startedBefore bound. Builds SQL with a server-side instant.
- Dashboard type (web). Mirrors the response in the browser.
- DashboardPage: Awaiting outcome tile. Shows the number and a link.
- AppointmentsPage: pass started through. Shows appointment rows: names, times, status.
- Dashboard tests (api and web): add awaitingOutcome. Tests only.
- appointment table and appointment_start_idx. Read only.

## Threats
- T-1 DashboardService.get: awaitingOutcome count, information disclosure. The count query must return a number only (C12).
- T-2 DashboardService.get: awaitingOutcome count, denial of service. It is one more count with no period bound; the query is already a single round trip (C15) and `starts_at` is indexed (C16).
- T-3 Dashboard type (API), information disclosure. New control: totals gain only `awaitingOutcome`, pinned by the exact totals test.
- T-4 AppointmentService.list: started filter, tampering. `oneOf` validation (C5, C6); the value never becomes SQL text (C4).
- T-5 AppointmentService.list: started filter, spoofing. `identify` and `appointments:read` (C1, C3).
- T-6 AppointmentService.list: started filter, tampering. The boundary is the server clock, never a client instant (C14).
- T-7 AppointmentFilter and AppointmentRepo.list: startedBefore bound, tampering. Bound through the parameterised `add` helper (C4).
- T-8 AppointmentFilter and AppointmentRepo.list: startedBefore bound, denial of service. Paged at 20 rows (C8, C9).
- T-9 Dashboard type (web), information disclosure. New control: mirror only the new count.
- T-10 DashboardPage: Awaiting outcome tile, information disclosure. Tiles show a number and a label (C12).
- T-11 AppointmentsPage: pass started through, information disclosure. Rows carry the name only (C11); medical history stays on the patient page (C10).
- T-12 AppointmentsPage: pass started through, tampering. The API validates the URL value (C5, C17).
- T-13 Dashboard tests (api and web): add awaitingOutcome, repudiation. New control: keep the exact match and add the field.
- T-14 appointment table and appointment_start_idx, repudiation. Accepted: reads are not audited; changes are already in `appointment_event` (C13).

## Known
- T-1 to T-14: each rests on a cited control or names a control implement must build (T-3, T-6, T-7, T-9, T-13).

## Assumed
None.

## Unknown
None.

## Open questions
None.

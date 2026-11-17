---
schema: threat-model.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
step: threat
attempt: 2
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
  - {id: C14, text: "The design's list filter is `started=true` compared with the server clock.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/design.md#claim:C22"]}
  - {id: C15, text: "The dashboard totals already run in one query using `count(*)` sub-selects.", cites: ["sample-system:api/src/services/dashboardService.ts#L52"]}
  - {id: C16, text: "Index `appointment_start_idx` exists on appointment start time.", cites: ["context/generated/db-schema.json#index:appointment_start_idx"]}
  - {id: C17, text: "The web app talks to the API only through `web/src/api/client.ts`.", cites: ["context/architecture.md#components"]}
  - {id: C18, text: "The list also runs an unpaged total, `SELECT count(*) AS n FROM appointment a`, over the same filter.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L75"]}
  - {id: C19, text: "In the MVP every request is the actor `staff` and every permission is granted.", cites: ["context/architecture.md#identity"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [T-1, T-2, T-3, T-4, T-5, T-6, T-7, T-8, T-9, T-10, T-11, T-12, T-13, T-14]
assumed: []
unknown: []
questions: []
resolves:
  - {id: F-1, how: "C14 now cites the design claim C22 instead of the IDE answer."}
threats:
  - {id: T-1, component: "DashboardService.get: awaitingOutcome count", stride: information_disclosure, threat: "The new count query could return or log patient data beyond a number.", mitigation: "Existing control: the service returns counts only (C12). Implement adds only a count(*) sub-select with no patient columns."}
  - {id: T-2, component: "DashboardService.get: awaitingOutcome count", stride: denial_of_service, threat: "The new count has no lower time bound, so its scan grows with the number of past Scheduled appointments.", mitigation: "Accepted risk: the count runs inside the existing single query (C15), as the unbounded Upcoming count already does; the starts_at index (C16) helps but does not cap the scan. No new control in HMS-21."}
  - {id: T-3, component: "Dashboard type (API)", stride: information_disclosure, threat: "Widening the response type could invite adding non-count fields to the dashboard payload.", mitigation: "New control for implement: add only the integer awaitingOutcome to totals; the dashboard test's exact match on totals pins the shape (C12)."}
  - {id: T-4, component: "AppointmentService.list: started filter", stride: tampering, threat: "A crafted started value could alter the query or the filter's meaning.", mitigation: "Existing controls: oneOf allows only listed values (C5), errors are collected as one ValidationError (C6), and the value never reaches SQL as text (C4)."}
  - {id: T-5, component: "AppointmentService.list: started filter", stride: spoofing, threat: "A caller could reach the filtered list without real authentication.", mitigation: "Accepted: pre-existing MVP identity, unchanged by HMS-21. identify and appointments:read are in place (C1, C3), but every request is staff with every permission (C19)."}
  - {id: T-6, component: "AppointmentService.list: started filter", stride: tampering, threat: "A client could try to supply its own instant to move the before-now boundary.", mitigation: "Existing design control with an implement constraint: the boundary is the server clock (C14); implement must not read an instant from the query."}
  - {id: T-7, component: "AppointmentFilter and AppointmentRepo.list: startedBefore bound", stride: tampering, threat: "A new SQL fragment built by string concatenation could allow injection.", mitigation: "Existing control with an implement constraint: the add helper binds values as numbered parameters (C4); implement must add startedBefore through add()."}
  - {id: T-8, component: "AppointmentFilter and AppointmentRepo.list: startedBefore bound", stride: denial_of_service, threat: "Started appointments with no period bound could form a very large result and a large total-count scan.", mitigation: "Existing control limits the response size: pages of 20 rows (C8, C9). Accepted risk: the unpaged total count (C18) still scans every match, as it does for the existing filters."}
  - {id: T-9, component: "Dashboard type (web)", stride: tampering, threat: "The web type could drift from the API contract and the page would render a field the API does not send.", mitigation: "New control for implement: mirror only awaitingOutcome: number; the web test fixture typed Dashboard catches drift at compile time."}
  - {id: T-10, component: "DashboardPage: Awaiting outcome tile", stride: information_disclosure, threat: "The tile could show patient details instead of a count.", mitigation: "Existing control: tiles render a value and a label only, and the API sends only counts (C12)."}
  - {id: T-11, component: "AppointmentsPage: pass started through", stride: information_disclosure, threat: "The list opened from the tile could show contact details or medical history.", mitigation: "Existing controls: list rows carry the patient's name only (C11), and medical history leaves the database only for the patient's own page (C10)."}
  - {id: T-12, component: "AppointmentsPage: pass started through", stride: tampering, threat: "A user could edit the URL's started value.", mitigation: "Existing control: the API validates it (C5) and the web app reaches the API only through the client (C17), so an invalid value gives a 400 field error, not a wider list."}
  - {id: T-13, component: "Dashboard tests (api and web): add awaitingOutcome", stride: tampering, threat: "Loosening the tests could let an unreviewed change to the dashboard contract pass.", mitigation: "New control for implement: keep the exact match on totals and add awaitingOutcome to it, rather than loosening the match."}
  - {id: T-14, component: "appointment table and appointment_start_idx", stride: repudiation, threat: "Read-only queries leave no audit trail of who looked at the list.", mitigation: "Accepted: no new write. Changes are already recorded in appointment_event (C13); reads are not audited today and HMS-21 adds none."}
---
# Threat model: HMS-21

## Components
- DashboardService.get: awaitingOutcome count. Reads appointment status and start times and returns an integer.
- Dashboard type (API). Shapes the dashboard response: counts only.
- AppointmentService.list: started filter. Reads one query value from the URL.
- AppointmentFilter and AppointmentRepo.list: startedBefore bound. Builds SQL with a server-side instant.
- Dashboard type (web). Mirrors the response in the browser.
- DashboardPage: Awaiting outcome tile. Shows the number and a link.
- AppointmentsPage: pass started through. Shows appointment rows: names, times and status.
- Dashboard tests (api and web): add awaitingOutcome. Tests only.
- appointment table and appointment_start_idx. Read only.

## Threats
Ordered by risk: information disclosure and tampering first, then denial of service, spoofing and repudiation.
- T-11 AppointmentsPage: pass started through, information disclosure. Mitigated by an existing control: rows carry the name only (C11), and medical history stays on the patient page (C10).
- T-1 DashboardService.get: awaitingOutcome count, information disclosure. Mitigated by an existing control: the service returns a number only (C12).
- T-10 DashboardPage: Awaiting outcome tile, information disclosure. Mitigated by an existing control: tiles show a number and a label (C12).
- T-3 Dashboard type (API), information disclosure. New control for implement: totals gain only `awaitingOutcome`, pinned by the exact totals test.
- T-7 AppointmentFilter and AppointmentRepo.list: startedBefore bound, tampering. Existing control: values are bound through the parameterised `add` helper (C4). Implement constraint: add `startedBefore` through `add()`.
- T-6 AppointmentService.list: started filter, tampering. Existing design control: the boundary is the server clock (C14). Implement constraint: never read an instant from the query.
- T-4 AppointmentService.list: started filter, tampering. Mitigated by existing controls: `oneOf` validation (C5, C6), and the value never becomes SQL text (C4).
- T-12 AppointmentsPage: pass started through, tampering. Mitigated by an existing control: the API validates the URL value (C5, C17).
- T-9 Dashboard type (web), tampering. New control for implement: mirror only the new count, so contract drift fails the typed fixture.
- T-13 Dashboard tests (api and web): add awaitingOutcome, tampering. New control for implement: keep the exact match and add the field.
- T-2 DashboardService.get: awaitingOutcome count, denial of service. Accepted risk, like the existing Upcoming count (C15). The index (C16) helps but does not cap the scan.
- T-8 AppointmentFilter and AppointmentRepo.list: startedBefore bound, denial of service. Pages of 20 rows limit the response size (C8, C9). Accepted risk: the total count still scans every match (C18).
- T-5 AppointmentService.list: started filter, spoofing. Accepted: pre-existing MVP identity, unchanged by HMS-21 (C1, C3, C19).
- T-14 appointment table and appointment_start_idx, repudiation. Accepted: reads are not audited; changes are already recorded in `appointment_event` (C13).

## Known
- T-1, T-4, T-10, T-11, T-12: existing cited controls.
- T-6, T-7: existing controls plus a constraint that implement must keep.
- T-3, T-9, T-13: new controls that implement must build.
- T-2, T-5, T-8, T-14: accepted risks, each stated with its basis.

Retry fixes:
- F-1: C14 now cites the design claim C22 instead of the IDE answer.
- The minor findings are also addressed. T-2 and T-8 are now labelled as accepted risks (C18). T-5 is accepted as the pre-existing MVP identity (C19). T-9 and T-13 are recategorised as tampering. The Known list now matches the mitigation labels, and the Threats section is ordered by risk.

## Assumed
None.

## Unknown
None.

## Open questions
None.

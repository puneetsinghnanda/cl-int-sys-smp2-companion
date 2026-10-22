---
schema: design.v1
run_id: run_01M5FDD5N0FKAMMHFBXMBBP8TS
step: design
attempt: 2
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/impact.md
    sha256: c78d2ded17a727cf3726349a45d02caf9d7455d406eb85664f93c24ec8e99adb
  - path: artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/split.md
    sha256: dbdbaf19a649ec1588c827e93ede9f3b99fd6dd8dd61a44153b5bd4579a7bcc7
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 31d9ca15805fc1b6488e8da89fdb0ee7714adbcdfda176d69581ab8f3d6482da
  - path: context/generated/api-inventory.json
    sha256: 8dfdbde2e1d64359cb29b8f710a2a467d5e19b383d90def7c856e7738d4ca104
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
resolves:
  - id: F-1
    how: "C4 now states only what the architecture says (instants stored in UTC; HOSPITAL_TZ, default Asia/Kolkata, is the hospital zone). PatientsPage formats the start explicitly in Asia/Kolkata instead of relying on dateTime's default; whether dateTime accepts a zone is raised as U-4/Q-04."
  - id: F-2
    how: "The known and assumed lists now match the requirement statuses copied from req.md (REQ-1 to REQ-11 known, REQ-12 assumed). The design's own mechanism assumptions are listed separately under Assumed as design assumptions."
claims:
  - id: C1
    text: "The patient list endpoint is `/api/patients`, declared in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C2
    text: "API lists answer `items`, `page`, `page_size` and `total`, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C3
    text: "The web app reaches the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C4
    text: "Instants are stored in `UTC`; the hospital time zone is `HOSPITAL_TZ`, default `Asia/Kolkata`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C5
    text: "SQL lives in `api/src/repositories` and rules in `api/src/services`."
    cites: ["context/conventions.md#code-layout"]
  - id: C6
    text: "The web app already has a date-time formatter, `dateTime`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
  - id: C7
    text: "Patient SQL is in `PatientRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C8
    text: "Patient rules are in `PatientService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C9
    text: "The patient list page is `PatientsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C10
    text: "The appointment page UI route is `/appointments/:id`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]
  - id: C11
    text: "An appointment has `starts_at` in UTC and a status such as `SCHEDULED`."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/impact.md#L39"]
  - id: C12
    text: "The index `appointment_patient_start_idx` covers appointments by patient and start."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/impact.md#L45"]
  - id: C13
    text: "Medical history appears only on the patient's own page, `never in lists`."
    cites: ["context/intent-baseline.md#data-protection"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
requirements:
  - id: REQ-1
    title: Patient list has a Next appointment column
    type: functional
    status: known
    statement: "The patient list has a column titled \"Next appointment\"."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L47"]
  - id: REQ-2
    title: Column picks the earliest upcoming Scheduled appointment
    type: functional
    status: known
    statement: "For each patient, the column shows the patient's earliest appointment with status Scheduled whose start is after the current time."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L53"]
  - id: REQ-3
    title: Start date and time shown in hospital time zone
    type: functional
    status: known
    statement: "The column shows the selected appointment's start date and time in the hospital time zone (Asia/Kolkata)."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L59"]
  - id: REQ-4
    title: Doctor's name shown
    type: functional
    status: known
    statement: "The column shows the name of the doctor of the selected appointment."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L65"]
  - id: REQ-5
    title: Closed appointments never shown
    type: functional
    status: known
    statement: "Appointments with status Cancelled, Completed or No-show are never shown in the Next appointment column."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L71"]
  - id: REQ-6
    title: None shown when nothing is upcoming
    type: functional
    status: known
    statement: "A patient with no upcoming Scheduled appointment shows \"None\" in the column."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L77"]
  - id: REQ-7
    title: Value links to the appointment's page
    type: functional
    status: known
    statement: "The value in the column links to the page of the selected appointment."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L83"]
  - id: REQ-8
    title: List stays 20 per page
    type: constraint
    status: known
    statement: "The patient list still shows 20 patients per page."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L89"]
  - id: REQ-9
    title: Current search keeps working
    type: constraint
    status: known
    statement: "The patient list's current search behaviour is unchanged."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L95"]
  - id: REQ-10
    title: Current sort order is kept
    type: constraint
    status: known
    statement: "The patient list's current sort order is unchanged."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L101"]
  - id: REQ-11
    title: No medical history in the list
    type: constraint
    status: known
    statement: "No medical history appears in the patient list."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L107"]
  - id: REQ-12
    title: None is plain text, not a link
    type: functional
    status: assumed
    statement: "When the column shows \"None\", it is not a link."
    cites: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/req.md#L113"]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: [REQ-12]
unknown: [U-1, U-2, U-3, U-4]
unknowns:
  - id: U-1
    text: "Other web pages may call GET /api/patients; the facts do not show them (impact U-1)."
    question: Q-01
  - id: U-2
    text: "Whether an Inactive doctor or patient changes what the column shows is not decided (impact U-2)."
    question: Q-02
  - id: U-3
    text: "The display format is not fixed; the design reuses the existing dateTime formatter (impact U-3)."
    question: Q-03
  - id: U-4
    text: "The facts do not show whether dateTime accepts a time zone or formats in the browser's local zone."
    question: Q-04
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "May GET /api/patients list items gain a next_appointment field, given other pages may call the route?"
    why: "The design adds one field to each list item; callers that ignore unknown fields are unaffected."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/patients", "context/conventions.md#lists"]
    options: ["Add the field to the existing route", "Add a separate route for next appointments"]
    default: "Add the field to the existing route"
  - id: Q-02
    blocking: false
    ask: product owner
    text: "Should an upcoming Scheduled appointment with an Inactive doctor, or for an Inactive patient, still be shown?"
    why: "The design selects only by status Scheduled and start time; hiding Inactive cases needs one more filter."
    evidence: ["artifacts/HMS-1/run_01M5FDD5N0FKAMMHFBXMBBP8TS/impact.md#L39"]
    options: ["Show it", "Hide it"]
    default: "Show it"
  - id: Q-03
    blocking: false
    ask: product owner
    text: "Is the app's existing dateTime format acceptable for the Next appointment column?"
    why: "Reusing the formatter keeps the list consistent with other pages and avoids a new format."
    evidence: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "context/architecture.md#time-and-slots"]
    options: ["Reuse dateTime", "A new format chosen by the product owner"]
    default: "Reuse dateTime"
  - id: Q-04
    blocking: false
    ask: tech lead
    text: "Does web/src/lib/format.ts dateTime take a time zone argument, or does it format in the browser's local zone?"
    why: "REQ-3 needs the start shown in Asia/Kolkata on every browser; the design passes the hospital zone explicitly and needs to know how."
    evidence: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "context/architecture.md#time-and-slots"]
    options: ["Pass Asia/Kolkata to dateTime", "Add a zone parameter to dateTime", "Format with an explicit Asia/Kolkata zone in PatientsPage"]
    default: "Format the start explicitly in Asia/Kolkata, using dateTime if it accepts a zone"
components:
  - name: PatientRepo list query
    module: module:sample-system:api
    change: changed
  - name: PatientService list
    module: module:sample-system:api
    change: changed
  - name: patientsRouter GET /api/patients
    module: module:sample-system:api
    change: existing
  - name: PatientsPage
    module: module:sample-system:web
    change: changed
  - name: dateTime formatter
    module: module:sample-system:web
    change: existing
  - name: API client
    module: module:sample-system:web
    change: existing
  - name: appointment table and appointment_patient_start_idx
    module: module:sample-system:db
    change: existing
endpoints:
  - method: GET
    path: /api/patients
    change: changed
  - method: GET
    path: /api/appointments/:id
    change: existing
---
# Design: HMS-1

## Overview
The patient list route GET /api/patients (C1) keeps its list shape `{ items, page, page_size, total }` with 20 per page (C2), and each item gains one field, `next_appointment`: either null or `{ id, starts_at, doctor_name }`. PatientRepo (C7) computes it in the existing list query with a per-row lookup on the appointment table: status `SCHEDULED`, `starts_at` after the current time, earliest first, at most one row, joined to the doctor for the name (C11). The lookup runs only for the page's rows, after paging, search and sort are applied, so those stay unchanged; the index `appointment_patient_start_idx` (C12) serves it. PatientService (C8) passes the field through. On the web side, PatientsPage (C9) adds a "Next appointment" column that shows the start, which arrives as a UTC instant (C4), formatted explicitly in the hospital time zone Asia/Kolkata (C4) with the existing `dateTime` formatter (C6) if it accepts a zone (U-4), plus the doctor name. The value links to `/appointments/:id` (C10), and plain-text "None" is shown when the field is null. The web app gets the field through the existing API client (C3); no new endpoint, table, migration or job is needed.

Trade-offs:
- Extending the existing route instead of adding a new one keeps one request per page; the cost is a slightly larger list response for every caller (U-1).
- A per-row lookup limited to the page's 20 rows keeps the cost bounded; a single grouped query over all patients would be simpler SQL but would scan more rows.
- Formatting with an explicit Asia/Kolkata zone makes REQ-3 hold on every browser; relying on the formatter's default would follow the browser's zone (U-4).
- Reusing `dateTime` keeps one date format across the app; a new format would need a product decision (Q-03).
- The list query keeps selecting no medical history columns, because medical history is never shown in lists (C13).

Note on the requirements list: the front matter's `requirements` (REQ-1 to REQ-12) copies the statements, titles and statuses of the intent artifact `req.md`, re-cited to its lines. It exists only because the brownfield workflow does not give this step the intent as an input, so the trace check could not otherwise find the REQ ids. It adds no new requirement; compare it with `req.md`.

## Components
- PatientRepo list query (module:sample-system:api), **changed**: adds the next-appointment lookup and returns `next_appointment` per item (C7, C11, C12).
- PatientService list (module:sample-system:api), **changed**: passes `next_appointment` through unchanged (C8).
- patientsRouter GET /api/patients (module:sample-system:api), **existing**: route, paging and parameters unchanged (C1).
- PatientsPage (module:sample-system:web), **changed**: new "Next appointment" column formatted in Asia/Kolkata, "None" text, link to the appointment page (C4, C9, C10).
- dateTime formatter (module:sample-system:web), **existing**: reused for the date-time text if it accepts a zone (C6, U-4).
- API client (module:sample-system:web), **existing**: carries the response; no change (C3).
- appointment table and appointment_patient_start_idx (module:sample-system:db), **existing**: read only; no migration (C11, C12).
- GET /api/patients, **changed**: each item gains `next_appointment` (C1, C2).
- GET /api/appointments/:id, **existing**: backs the linked appointment page (C10).

Story mapping: S-1 is served by PatientRepo list query, PatientService list, PatientsPage and the dateTime formatter; S-2 by PatientsPage ("None" when the field is null, as plain text); S-3 by PatientsPage and GET /api/appointments/:id; S-4 by leaving paging, search and sort in the existing route and query; S-5 by keeping medical history columns out of the list query.

## Known
- REQ-1: PatientsPage is the patient list page that gets the column (C9).
- REQ-2: served by the PatientRepo lookup on status `SCHEDULED` and `starts_at` (C11); see design assumption D-1.
- REQ-3: served by formatting the UTC start explicitly in Asia/Kolkata (C4); see design assumption D-2.
- REQ-4: served by the doctor join in the same lookup (C11); see design assumption D-3.
- REQ-5: served by selecting only status `SCHEDULED`, which excludes Cancelled, Completed and No-show (C11).
- REQ-6: served by showing "None" when `next_appointment` is null.
- REQ-7: the link target `/appointments/:id` already exists (C10).
- REQ-8: lists already answer 20 per page, and paging is unchanged (C2).
- REQ-9: search stays in the existing list query and route (C1).
- REQ-10: sort stays in the existing list query and route (C1).
- REQ-11: medical history is never shown in lists, and the list query adds no medical columns (C13).

## Assumed
- REQ-12: "None" is rendered as plain text, not a link, following the intent's assumption.

Design assumptions (about the mechanism, not the requirement status):
- D-1: GET /api/patients carries the data, as the impact analysis assumes, and the SQL lives in PatientRepo per the conventions (C1, C5, C7).
- D-2: PatientsPage can format the start in an explicit zone; whether `dateTime` accepts one is open (U-4, Q-04).
- D-3: the doctor's name is read from the doctor row joined to the appointment; the facts given to this step do not name the column, so implement confirms it.

## Unknown
- U-1 (non-blocking): other callers of GET /api/patients are not visible in the facts. Raises Q-01.
- U-2 (non-blocking): the effect of Inactive doctors or patients is not decided. Raises Q-02.
- U-3 (non-blocking): the display format is not fixed; the design reuses dateTime. Raises Q-03.
- U-4 (non-blocking): whether dateTime accepts a time zone is not shown in the facts. Raises Q-04.

## Open questions
- Q-01 (non-blocking, tech lead): may GET /api/patients list items gain a next_appointment field? Proposed default: add the field to the existing route.
- Q-02 (non-blocking, product owner): should appointments with an Inactive doctor, or for an Inactive patient, still be shown? Proposed default: show them.
- Q-03 (non-blocking, product owner): is the existing dateTime format acceptable for the column? Proposed default: reuse dateTime.
- Q-04 (non-blocking, tech lead): does dateTime take a time zone argument? Proposed default: format the start explicitly in Asia/Kolkata, using dateTime if it accepts a zone.

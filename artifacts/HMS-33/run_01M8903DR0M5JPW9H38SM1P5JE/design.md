---
schema: design.v1
run_id: run_01M8903DR0M5JPW9H38SM1P5JE
step: design
attempt: 1
skill: design@1.10.0
inputs:
  - path: artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/split.md
    sha256: f961428452c46b09a4d423a90e9dbdaea0571b036cd7e36f169c73ceccd5fca5
  - path: artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/req.md
    sha256: be606b63b4e120ec769701f53e8d5a8a4fc7eafc2f0faf17caffd96007153b30
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The list endpoint is served from `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C2
    text: "The list query lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C3
    text: "List rows are built from the shared `SUMMARY_SELECT`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:SUMMARY_SELECT"]
  - id: C4
    text: "The API's list item type today is `AppointmentSummary` in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentSummary"]
  - id: C5
    text: "The web's list item type today is `AppointmentSummary` in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentSummary"]
  - id: C6
    text: "The web calls the API through the `api` object of the client."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
  - id: C7
    text: "All appointment tables render through the shared `AppointmentTable` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/AppointmentTable.tsx:AppointmentTable"]
  - id: C8
    text: "The table's inputs are declared in its `Props` interface."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/AppointmentTable.tsx:Props"]
  - id: C9
    text: "The appointment list is the `AppointmentsPage` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"]
  - id: C10
    text: "The dashboard is the `DashboardPage` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]
  - id: C11
    text: "The doctor page is the `DoctorDetailPage` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage"]
  - id: C12
    text: "The patient page is the `PatientDetailPage` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]
  - id: C13
    text: "The appointment's own page is the `AppointmentDetailPage` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage"]
  - id: C14
    text: "The detail type `AppointmentDetail` serves the appointment's own page."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail"]
  - id: C15
    text: "The reason is stored in the column `cancellation_reason`."
    cites: ["context/generated/db-schema.json#column:appointment.cancellation_reason"]
  - id: C16
    text: "List items carry `cancellationReason, full` text; the web table shortens it."
    cites: ["runs/run_01M8903DR0M5JPW9H38SM1P5JE/ide-answers.md#a-02"]
  - id: C17
    text: "Long reasons are cut at `60` characters on the list."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/req.md#item:REQ-4"]
  - id: C18
    text: "The user chose to `Verify existing` code and change only what is missing."
    cites: ["artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/req.md#item:REQ-16"]
  - id: C19
    text: "The web talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: AppointmentRepo.list returns the cancellation reason
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo", "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:SUMMARY_SELECT", "runs/run_01M8903DR0M5JPW9H38SM1P5JE/ide-answers.md#a-02"]
  - id: CO-2
    name: AppointmentListItem type (API), AppointmentSummary plus cancellationReason
    module: module:sample-system:api
    change: new
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentSummary", "runs/run_01M8903DR0M5JPW9H38SM1P5JE/ide-answers.md#a-02"]
  - id: CO-3
    name: AppointmentListItem type (web), AppointmentSummary plus cancellationReason
    module: module:sample-system:web
    change: new
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentSummary", "runs/run_01M8903DR0M5JPW9H38SM1P5JE/ide-answers.md#a-02"]
  - id: CO-4
    name: api client appointments() returns a page of AppointmentListItem
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api", "context/architecture.md#components"]
  - id: CO-5
    name: AppointmentTable gets an opt-in Cancellation reason column
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/AppointmentTable.tsx:AppointmentTable", "context/generated/module-map.json#sym:sample-system:web/src/components/AppointmentTable.tsx:Props", "artifacts/HMS-33/run_01M8903DR0M5JPW9H38SM1P5JE/req.md#item:REQ-4"]
  - id: CO-6
    name: AppointmentsPage turns the column on
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"]
  - id: CO-7
    name: DashboardPage keeps the table without the column
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]
  - id: CO-8
    name: DoctorDetailPage keeps the table without the column
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage"]
  - id: CO-9
    name: PatientDetailPage keeps the table without the column
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientDetailPage.tsx:PatientDetailPage"]
  - id: CO-10
    name: AppointmentDetailPage keeps showing the full reason
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentDetailPage.tsx:AppointmentDetailPage", "context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:AppointmentDetail"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - name: items[].cancellationReason
        type: string
        nullable: true
        format: "plain text, the full reason as stored in appointment.cancellation_reason, not shortened; null unless status is CANCELLED"
        example: "Doctor on leave"
        status: known
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/db-schema.json#column:appointment.cancellation_reason", "runs/run_01M8903DR0M5JPW9H38SM1P5JE/ide-answers.md#a-02"]
  - id: EP-2
    method: GET
    path: /api/appointments/:id
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
  - id: EP-3
    method: POST
    path: /api/appointments/:id/cancel
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel"]
  - id: EP-4
    method: POST
    path: /api/appointments
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments"]
answers:
  - id: A-02
    kind: question
    about: Q-01
    answer: "cancellationReason, full (proposed default)"
    decides: [EP-1, CO-1, CO-2, CO-3, C16]
---
# Design: HMS-33

## Overview
The appointment list gains a "Cancellation reason" column; nothing else changes. The list endpoint EP-1 already reads from the `appointment` table, whose `cancellation_reason` column holds the reason (C15). CO-1 adds that column to each list item as `cancellationReason` (A-02), CO-2 and CO-3 give the list item its own type so the dashboard, doctor and patient payloads that reuse `AppointmentSummary` (C4, C5) stay byte-for-byte the same, and CO-4 types the client call. On the web, the shared `AppointmentTable` (C7) gets an opt-in prop that adds the column after Status (CO-5); only `AppointmentsPage` (C9) sets it (CO-6), so the dashboard, doctor page and patient page (CO-7 to CO-9) render exactly as before. Per C18, implement first checks the product code at the pinned SHA and changes only what these components still lack.

Trade-offs:
- Separate list item type instead of adding the field to `AppointmentSummary`: one more type, but no new field leaks into the dashboard, doctor or patient payloads (S-4).
- Full reason in the API, shortened in the web (A-02): a few more bytes per row, but one source of truth and the detail page keeps the full text (S-2 AC3).
- Opt-in prop defaulting to off instead of a new table component: one shared table to maintain; the default keeps every existing caller unchanged.
- Shortening counts characters (code points), not UTF-16 units, so the cut at `60` characters (C17) never splits an emoji or other character outside the basic plane.

## Components
- CO-1 AppointmentRepo.list returns the cancellation reason — changed (api). The list query selects `cancellation_reason` and maps it to `cancellationReason` on each item; filters, ordering, `LIMIT`/`OFFSET` and the count query stay as they are, so the column follows status, date range and paging (S-3).
- CO-2 AppointmentListItem type (API) — new, next to `AppointmentSummary` in api/src/types.ts: `AppointmentSummary & { cancellationReason: string | null }`.
- CO-3 AppointmentListItem type (web) — new, next to `AppointmentSummary` in web/src/api/types.ts, same shape as CO-2.
- CO-4 api client appointments() — changed (web). Returns a page of `AppointmentListItem` instead of `AppointmentSummary`.
- CO-5 AppointmentTable — changed (web). New optional prop `showCancellationReason` (default false). When true, a header "Cancellation reason" is added after Status, and each row shows: the reason for a CANCELLED appointment, its first 60 characters followed by "…" when it is longer than 60 characters, and "-" for every other status or when the reason is null. When false, the table is identical to today.
- CO-6 AppointmentsPage — changed (web). Passes `showCancellationReason` to the table. Filters and pagination are untouched.
- CO-7 DashboardPage — existing. Does not pass the prop; no column (S-4 AC1, AC2).
- CO-8 DoctorDetailPage — existing. Does not pass the prop; no column (S-4 AC3).
- CO-9 PatientDetailPage — existing. Does not pass the prop; no column (S-4 AC4).
- CO-10 AppointmentDetailPage — existing. Keeps showing the full reason from `AppointmentDetail` (S-2 AC3).
- EP-1 GET /api/appointments — changed. New field on each item:
  - `items[].cancellationReason`: string, nullable; plain text, the full reason as stored, not shortened; null unless status is CANCELLED; example "Doctor on leave"; known (C16).
- EP-2 GET /api/appointments/:id — existing, unchanged.
- EP-3 POST /api/appointments/:id/cancel — existing, unchanged (S-5 AC2).
- EP-4 POST /api/appointments — existing, unchanged (S-5 AC3).

No medical history or contact details are added to any list payload or column (S-6): EP-1 gains only `cancellationReason`.

Story map:
- S-1: CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, EP-1.
- S-2: CO-5, CO-10, EP-2.
- S-3: CO-1, CO-6, EP-1.
- S-4: CO-5 (prop off by default), CO-7, CO-8, CO-9.
- S-5: CO-10, EP-2, EP-3, EP-4.
- S-6: EP-1, CO-5.

## Known
- S-1: served by CO-1 to CO-6 and EP-1; every file and type is proven by a module-map fact (C2 to C9).
- S-2: served by CO-5 and CO-10; the cut is at 60 characters (C17).
- S-3: served by CO-1 and CO-6; existing filters and paging are reused.
- S-4: served by the prop defaulting to off and CO-7 to CO-9 (C10 to C12).
- S-5: served by the existing EP-2 to EP-4 and CO-10 (C13).
- S-6: EP-1 adds only `cancellationReason`.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-02 (Q-01): which contract should list items use for the reason? Answer: "cancellationReason, full (proposed default)". Decides EP-1, CO-1, CO-2, CO-3 and C16. No mismatch with the inputs.

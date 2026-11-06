---
schema: design.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
step: design
attempt: 1
skill: design@1.7.0
inputs:
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/impact.md
    sha256: 1affd116521dda7653d29e21cc46a16bb001666371bfd9f13bc60abf453e1d1b
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/split.md
    sha256: 27a56fb6c5c77620388a68804cade76e555a471888785e875f98064fab7c3d95
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/req.md
    sha256: 21ea23abcc5278cf34af72ae9fd62e5fb3032e9b40fc280a88a84a26953f2670
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 07674e27e969e7fd56aba8a2a1419c1f597c1b4a9c490b574a56b82ff6571114
  - path: context/generated/api-inventory.json
    sha256: dd1254b0a78f85d6da3dd3722fd41947c0cec5177fe5b46028e49af681dc4c85
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "The existing Picker component already offers a clear action that calls `onChange(undefined, '')`."
    cites: ["sample-system:web/src/components/Picker.tsx#L28"]
  - id: C2
    text: "The booking page's doctor Picker searches with `api.doctors({ q, status: 'ACTIVE' })`; the list page reuses it without the status, so Inactive doctors are offered (A-01)."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L77"]
  - id: C3
    text: "The booking page loads the label of a doctor id from the URL with `api.doctor(doctorId)`."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L32"]
  - id: C4
    text: "The booking page's doctor label is `${d.fullName} (${d.code}) · ${d.specialization.name}`; the list page uses the same label."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L36"]
  - id: C5
    text: "The appointment service validates status with `APPOINTMENT_STATUSES`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L35"]
  - id: C6
    text: "The repository filter declares `status?: AppointmentStatus`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L43"]
  - id: C7
    text: "The repository adds the status condition as `a.status = ?`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L70"]
  - id: C8
    text: "The Today count excludes Cancelled with `status <> 'CANCELLED'`; the new filter uses the same condition."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L129"]
  - id: C9
    text: "The Today link is built from `doctorId: d.id, from: today, to: today`."
    cites: ["sample-system:web/src/pages/DoctorsPage.tsx#L86"]
  - id: C10
    text: "The web client builds query strings with `export function query`."
    cites: ["sample-system:web/src/api/client.ts#L40"]
  - id: C11
    text: "The impact found that the existing `DoctorsPage` test asserts the old Today href."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/impact.md#claim:C21"]
  - id: C12
    text: "Invalid input is a 400 with one message per field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C13
    text: "Lists answer with `{ items, page, page_size, total }` at 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C14
    text: "The run's default for when Clear All shows is `Only while at least one filter is set`."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/split.md#item:Q-01"]
  - id: C15
    text: "The status select builds its options from `STATUS_LABEL`."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L34"]
  - id: C16
    text: "The list page's clear button today is `Clear doctor / patient filter`."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L40"]
  - id: C17
    text: "The list page's set helper copies the current parameters with `new URLSearchParams(params)`, so changing one filter keeps the others."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L18"]
  - id: C18
    text: "The list is ordered `ORDER BY a.starts_at`, then id."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L79"]
  - id: C19
    text: "GET /api/doctors filters status only when given, from `RECORD_STATUSES`."
    cites: ["sample-system:api/src/services/doctorService.ts#L42"]
  - id: C20
    text: "The list's from and to dates are turned into hospital-local day bounds with `dayBounds`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L46"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: AppointmentsPage (doctor Picker, doctor label, Any except Cancelled option, Clear All)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "sample-system:web/src/pages/AppointmentsPage.tsx#L34", "sample-system:web/src/pages/AppointmentsPage.tsx#L40", "runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-01", "runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-02", "runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-03", "runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-04"]
  - id: CO-2
    name: Picker component
    module: module:sample-system:web
    change: existing
    cites: ["sample-system:web/src/components/Picker.tsx#L28"]
  - id: CO-3
    name: DoctorsPage Today link
    module: module:sample-system:web
    change: changed
    cites: ["sample-system:web/src/pages/DoctorsPage.tsx#L86", "runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-04"]
  - id: CO-4
    name: DoctorsPage test (HMS-6 Today link href)
    module: module:sample-system:web
    change: changed
    cites: ["sample-system:web/src/__tests__/DoctorsPage.test.tsx#L62"]
  - id: CO-5
    name: AppointmentService.list status validation
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "sample-system:api/src/services/appointmentService.ts#L35", "runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-04"]
  - id: CO-6
    name: AppointmentFilter and AppointmentRepo.list status condition
    module: module:sample-system:api
    change: changed
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L43", "sample-system:api/src/repositories/appointmentRepo.ts#L70"]
  - id: CO-7
    name: Web API client (api.appointments, api.doctors, api.doctor, query)
    module: module:sample-system:web
    change: existing
    cites: ["sample-system:web/src/api/client.ts#L40"]
  - id: CO-8
    name: DoctorDetailPage and DashboardPage links to the list
    module: module:sample-system:web
    change: existing
    cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L72", "sample-system:web/src/pages/DashboardPage.tsx#L43"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - name: status
        type: string (query parameter)
        nullable: true
        format: "one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, NOT_CANCELLED; absent or empty means any status; any other value is a 400 ValidationError on status"
        example: NOT_CANCELLED
        status: known
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-04", "sample-system:api/src/services/appointmentService.ts#L35", "context/conventions.md#errors"]
  - id: EP-2
    method: GET
    path: /api/doctors
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors", "sample-system:api/src/services/doctorService.ts#L42"]
  - id: EP-3
    method: GET
    path: /api/doctors/:id
    change: existing
    fields: []
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id"]
answers: []
---
# Design: HMS-8

## Overview
This change touches only the existing web and API modules. There is no new endpoint, table, store
or job.

**Web (CO-1, CO-3).**
- The appointment list page gets the booking page's doctor Picker (C1, C2), which is always shown (A-02).
  - It searches GET /api/doctors with no status, so Inactive doctors are offered (A-01, C19).
  - It loads the label of a doctor id in the URL with GET /api/doctors/:id (C3).
  - Picking or clearing a doctor changes only `doctorId` through the existing set helper. That keeps the patient, status and dates and resets the page (C17).
- The status select gets an "Any except Cancelled" option with value `NOT_CANCELLED` (A-04, C15).
- The old clear button becomes Clear All, which removes every filter (A-03, C16). It is shown only while at least one filter is set (C14).
- The doctors list's Today link adds `status: 'NOT_CANCELLED'` (C9), and its test changes with it (C11).

**API (CO-5, CO-6).**
- The service accepts NOT_CANCELLED next to `APPOINTMENT_STATUSES` (C5).
- The repository filter widens to `AppointmentStatus | 'NOT_CANCELLED'` (C6).
- For NOT_CANCELLED, the repository adds `a.status <> 'CANCELLED'` instead of `a.status = ?` (C7). This is the same condition the Today count uses (C8), so over the same hospital-local day (C20) the listed number equals the count.
- Order and page size are untouched (C13, C18).

**Trade-offs:**
- Reusing Picker instead of a dropdown: the doctors API is paged at 20 (C13), so a dropdown could not list every doctor. The cost is that staff type to search.
- Picker label same as the booking page (C4): name, code and specialization tell apart doctors with the same name. The label shows the display code, not the raw id.
- NOT_CANCELLED is written inline in the service's allowed list rather than as a new shared type: the change stays small, but the web keeps the string as a literal.
- Clear All only while a filter is set (C14): it avoids a button that does nothing, but it is one more condition to test.

## Components
- CO-1 AppointmentsPage: changed.
  - It adds the doctor Picker and the doctor label.
  - It adds the `NOT_CANCELLED` option labelled "Any except Cancelled".
  - "Clear doctor / patient filter" is replaced by "Clear All", shown while any of doctorId, patientId, status, from or to is set.
- CO-2 Picker: existing. Its clear action serves REQ-14 (C1).
- CO-3 DoctorsPage Today link: changed. The link becomes `query({ doctorId: d.id, status: 'NOT_CANCELLED', from: today, to: today })`.
- CO-4 DoctorsPage test: changed. It expects the Today href to carry `status=NOT_CANCELLED`.
- CO-5 AppointmentService.list: changed. Status is one of the four stored statuses or NOT_CANCELLED. Anything else is a 400 (C12).
- CO-6 AppointmentFilter / AppointmentRepo.list: changed.
  - `status` widens to `AppointmentStatus | 'NOT_CANCELLED'`.
  - NOT_CANCELLED adds `a.status <> 'CANCELLED'` with no parameter. Other values keep `a.status = ?`.
- CO-7 Web API client: existing. It passes the parameters through `query` (C10).
- CO-8 DoctorDetailPage and DashboardPage links: existing and unchanged (REQ-11).
- EP-1 GET /api/appointments: changed. The response shape is unchanged (C13).
  - Field `status` (query parameter): a string, optional (absent or empty means any status).
  - Its format is one of SCHEDULED, COMPLETED, CANCELLED, NO_SHOW or NOT_CANCELLED. Any other value is a 400 ValidationError on status.
  - Example `NOT_CANCELLED`. Status known (A-04).
- EP-2 GET /api/doctors: existing. It is called with `q` and no status (C19).
- EP-3 GET /api/doctors/:id: existing. It gives the label of a doctor id from the URL (C3).

Stories served:
- S-1: CO-1, CO-2, EP-2, EP-3.
- S-2: CO-1, CO-2.
- S-3: CO-1.
- S-4: CO-1, CO-5, CO-6, EP-1.
- S-5: CO-3, CO-4, CO-5, CO-6.
- S-6: CO-7, CO-8, plus the order and paging left as they are in CO-6.

## Known
- S-1: Picker search and label lookup reuse existing calls (C1–C4), with A-01 and A-02.
- S-2: the set helper keeps the other parameters (C17), and Picker clears (C1).
- S-3: Clear All per A-03, shown under the run's default (C14).
- S-4: the NOT_CANCELLED value (A-04) is accepted by CO-5 and filtered by CO-6 (C5–C7).
- S-5: the Today link carries NOT_CANCELLED (C9), and the filter matches the Today count rule (C8, C20).
- S-6: the other links, order and page size are untouched (C13, C18).

## Assumed
None.

## Unknown
None.

## Open questions
None open. Nothing new was asked in the IDE at this step. The design uses A-01 (CO-1 search without status), A-02 (CO-1 Picker always shown), A-03 (CO-1 Clear All) and A-04 (EP-1, CO-3, CO-5) from the requirements step. Clear All visibility follows the run's default for the story-split question Q-01 (C14).

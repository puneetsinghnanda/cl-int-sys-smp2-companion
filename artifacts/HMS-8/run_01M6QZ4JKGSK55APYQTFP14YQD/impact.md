---
schema: impact.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
step: impact
attempt: 2
skill: impact-analysis@1.3.0
inputs:
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/req.md
    sha256: 21ea23abcc5278cf34af72ae9fd62e5fb3032e9b40fc280a88a84a26953f2670
  - path: context/generated/module-map.json
    sha256: 07674e27e969e7fd56aba8a2a1419c1f597c1b4a9c490b574a56b82ff6571114
  - path: context/generated/api-inventory.json
    sha256: dd1254b0a78f85d6da3dd3722fd41947c0cec5177fe5b46028e49af681dc4c85
  - path: context/generated/db-schema.json
    sha256: d0487757e73c20b1eaced48f38b101f87348f97927b76d4b7bd46fc6f5f985ff
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C2
    text: "GET /api/appointments accepts status only as one of `APPOINTMENT_STATUSES`, so the new NOT_CANCELLED value is rejected today and the service must change (REQ-8)."
    cites: ["sample-system:api/src/services/appointmentService.ts#L35"]
  - id: C3
    text: "The appointment repository filters status by a single equality, `a.status = ?`, so excluding one status needs a new condition."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L70"]
  - id: C4
    text: "The doctors list's Today count leaves out Cancelled with `status <> 'CANCELLED'`; the new filter must use the same rule for REQ-10 to hold."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L129"]
  - id: C5
    text: "The doctors list's Today link is built from `doctorId: d.id, from: today, to: today` and sets no status (REQ-9 changes it)."
    cites: ["sample-system:web/src/pages/DoctorsPage.tsx#L86"]
  - id: C6
    text: "The appointment list page has the `Clear doctor / patient filter` button that A-03 turns into Clear All."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L40"]
  - id: C7
    text: "The status select builds its options from `STATUS_LABEL`; Any except Cancelled is a new option outside that map."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L34"]
  - id: C8
    text: "Each appointment summary already carries the doctor's `fullName`."
    cites: ["sample-system:api/src/types.ts#L53"]
  - id: C9
    text: "GET /api/doctors is paged by the configured `pageSize`."
    cites: ["sample-system:api/src/services/doctorService.ts#L45"]
  - id: C10
    text: "The configured page size is `pageSize: 20`, so a single unfiltered GET /api/doctors call cannot return more than 20 doctors for the chooser."
    cites: ["sample-system:api/src/config.ts#L41"]
  - id: C11
    text: "GET /api/doctors filters status only when it is given, from `RECORD_STATUSES`; without it both Active and Inactive doctors are returned (REQ-3, A-01)."
    cites: ["sample-system:api/src/services/doctorService.ts#L42"]
  - id: C12
    text: "The booking page already has a doctor search picker that calls `api.doctors({ q, status: 'ACTIVE' })`."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L77"]
  - id: C13
    text: "The booking page loads a preset doctor's name by id with `api.doctor(doctorId)`, a pattern REQ-1 can reuse."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L32"]
  - id: C14
    text: "The doctor page's No-shows link is built by `noShowsQuery` and must not change (REQ-11)."
    cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L72"]
  - id: C15
    text: "The dashboard's `Appointments today` tile links to the appointment list and must not change (REQ-11)."
    cites: ["sample-system:web/src/pages/DashboardPage.tsx#L43"]
  - id: C16
    text: "The appointment list is ordered `ORDER BY a.starts_at`; REQ-13 keeps it."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L79"]
  - id: C17
    text: "Appointment status is stored as SCHEDULED, COMPLETED, `CANCELLED` or NO_SHOW. NOT_CANCELLED is a filter value, not a stored status, so no migration is needed."
    cites: ["context/data-model.md#appointments"]
  - id: C18
    text: "`No background jobs exist yet`, so no job reads the changed filter."
    cites: ["context/architecture.md#open-points"]
  - id: C19
    text: "The web package is the module `web`."
    cites: ["context/generated/module-map.json#module:sample-system:web"]
  - id: C20
    text: "The API package is the module `api`."
    cites: ["context/generated/module-map.json#module:sample-system:api"]
  - id: C21
    text: "The existing DoctorsPage test asserts the Today link href `/appointments?doctorId=1&from=${today}&to=${today}` with no status. REQ-9 breaks it, so the test must change to expect status=NOT_CANCELLED."
    cites: ["sample-system:web/src/__tests__/DoctorsPage.test.tsx#L62"]
  - id: C22
    text: "The repository filter declares `status?: AppointmentStatus`, so the filter type must widen to carry the NOT_CANCELLED filter value."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L43"]
  - id: C23
    text: "The DoctorDetailPage test asserts the No-shows href `/appointments?doctorId=1&status=NO_SHOW&from=2026-09-20&to=2026-10-19`. It is an existing guard for REQ-11 and must keep passing unchanged."
    cites: ["sample-system:web/src/__tests__/DoctorDetailPage.test.tsx#L49"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
assumed: []
unknown: []
questions: []
modules:
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "sample-system:web/src/pages/AppointmentsPage.tsx#L40", "sample-system:web/src/pages/DoctorsPage.tsx#L86", "sample-system:web/src/__tests__/DoctorsPage.test.tsx#L62"]
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "sample-system:api/src/services/appointmentService.ts#L35", "sample-system:api/src/repositories/appointmentRepo.ts#L70", "sample-system:api/src/repositories/appointmentRepo.ts#L43"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "context/data-model.md#appointments"]
apis:
  - id: http:GET:/api/appointments
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "sample-system:api/src/services/appointmentService.ts#L35", "runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-04"]
  - id: http:GET:/api/doctors
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors", "sample-system:api/src/services/doctorService.ts#L42", "runs/run_01M6QZ4JKGSK55APYQTFP14YQD/ide-answers.md#a-01"]
  - id: http:GET:/api/doctors/:id
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id", "sample-system:web/src/pages/BookAppointmentPage.tsx#L32"]
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/data-model.md#appointments"]
  - id: table:doctor
    change: existing
    cites: ["context/generated/db-schema.json#table:doctor"]
answers: []
---
# Impact analysis: HMS-8

## Summary
HMS-8 changes the web app's appointment list and the doctors list's Today link
(module:sample-system:web). It also changes the API's appointment list filter
(module:sample-system:api), so GET /api/appointments accepts `status=NOT_CANCELLED` (C2, C3, C22).
Nothing changes in the database: NOT_CANCELLED is a filter value, not a stored status (C17), and no
job reads the filter (C18).

The main risks:
- The new filter must exclude Cancelled exactly as the Today count does (C4), or REQ-10 breaks.
- The existing DoctorsPage test asserts the old Today href and must change with REQ-9 (C21).
- GET /api/doctors is paged at 20 (C9, C10), so the "all doctors" chooser (A-01) cannot be one
  unpaged dropdown. A search picker like the booking page's, without the Active filter, fits (C12).
- The No-shows link and the dashboard tile must keep their current queries (C14, C15, C23).

## Impacted modules
- module:sample-system:web: changed.
  - AppointmentsPage gets the doctor chooser, the doctor's name, Clear All and the Any except Cancelled option (C6, C7, C13).
  - DoctorsPage's Today link adds `status=NOT_CANCELLED` (C5).
  - The DoctorsPage test (`web/src/__tests__/DoctorsPage.test.tsx`, the HMS-6 Today link test) changes to expect the new href (C21).
  - The DoctorDetailPage and DashboardPage links and their tests stay as they are (C14, C15, C23).
- module:sample-system:api: changed.
  - The appointment service accepts NOT_CANCELLED (C2).
  - The repository filter type widens (C22) and adds an exclude-Cancelled condition. The order stays by start time (C3, C16).
- module:sample-system:db: existing. No migration (C17).
- http:GET:/api/appointments: changed. It accepts `status=NOT_CANCELLED` (A-04, C2).
- http:GET:/api/doctors: existing. The chooser reads it with no status filter, so Inactive doctors are included (C11, A-01). It is paged at 20 (C10).
- http:GET:/api/doctors/:id: existing. It gives the name of a doctor set by a link (C13). The summaries also carry `fullName` (C8).
- table:appointment: existing. It is read with the new filter.
- table:doctor: existing. It is read for the chooser and the name.

## Known
- REQ-1, REQ-2, REQ-3, REQ-14: web chooser and name, using existing GET /api/doctors and GET /api/doctors/:id (C11, C12, C13).
- REQ-4, REQ-5: web only. The chooser changes one URL parameter and leaves the others alone.
- REQ-6: web only. The existing clear button becomes Clear All (C6, A-03).
- REQ-7, REQ-8: web status option, plus the API service, filter type and repository change (C2, C3, C7, C22, A-04).
- REQ-9, REQ-10: web Today link (C5) and its existing test (C21), plus the API exclusion matching the Today count rule (C4).
- REQ-11: the No-shows and dashboard links are read and must not change (C14, C15, C23).
- REQ-12, REQ-13: page size and order are read and must not change (C10, C16).

## Assumed
None.

## Unknown
None.

## Open questions
None open. This step asked nothing new in the IDE. It builds on A-01 to A-04 from the requirements step: A-01 and A-04 are cited on GET /api/doctors and GET /api/appointments, and A-03 on the clear button (C6).

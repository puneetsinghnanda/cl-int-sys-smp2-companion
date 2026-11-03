---
schema: impact.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: impact
attempt: 3
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/req.md
    sha256: 8cacd7572d8146137cd6151565f9b5e880d72194da94af616afa60fd16406ef8
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
  - path: context/generated/api-inventory.json
    sha256: 9ba29278d8d66b7341defc3c67d6e7d68508ce25ee2991ba256be2b37194ec61
  - path: context/generated/db-schema.json
    sha256: f047583e5b452649edd65cf48083feaad4c664531202fba78652d06b415749f5
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
resolves:
  - id: F-1
    how: "Listed the edit form (/doctors/:id/edit, fromDoctor) and PATCH /api/doctors/:id as existing readers; the count stays read-only and out of both; Q-03 asks where it is served."
claims:
  - id: C1
    text: "The doctor detail API route is `/api/doctors/:id` (GET), served by `api/src/routes/doctors.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id"]
  - id: C2
    text: "The appointment list API route is `/api/appointments` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C3
    text: "The dashboard API route is `/api/dashboard` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C4
    text: "The dashboard service defines a `PERIODS` symbol in `dashboardService.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:PERIODS"]
  - id: C5
    text: "The doctor page UI route is `/doctors/:id`."
    cites: ["context/generated/api-inventory.json#ui:/doctors/:id"]
  - id: C6
    text: "The doctor page is rendered by `DoctorDetailPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage"]
  - id: C7
    text: "The appointment list UI route is `/appointments`, rendered by `AppointmentsPage`."
    cites: ["context/generated/api-inventory.json#ui:/appointments", "context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"]
  - id: C8
    text: "Appointment status values include `NO_SHOW`; times are stored as [starts_at, ends_at) in UTC."
    cites: ["context/data-model.md#appointments"]
  - id: C9
    text: "\"Today\" uses the hospital time zone setting `HOSPITAL_TZ`, default Asia/Kolkata; instants are stored in UTC."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C10
    text: "Index `appointment_doctor_start_idx` on appointment covers doctor_id and starts_at."
    cites: ["context/generated/db-schema.json#index:appointment_doctor_start_idx"]
  - id: C11
    text: "The web app calls the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C12
    text: "The appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C13
    text: "Doctor rules live in `DoctorService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C14
    text: "The dashboard counts live in `DashboardService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C15
    text: "No background `jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
  - id: C16
    text: "The doctor edit UI route is `/doctors/:id/edit`."
    cites: ["context/generated/api-inventory.json#ui:/doctors/:id/edit"]
  - id: C17
    text: "The doctor edit form `DoctorFormPage` maps a loaded doctor with `fromDoctor`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:DoctorFormPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:fromDoctor"]
  - id: C18
    text: "Doctor updates go through `/api/doctors/:id` (PATCH)."
    cites: ["context/generated/api-inventory.json#http:PATCH:/api/doctors/:id"]
  - id: C19
    text: "The API has shared time helpers `dayBounds`, `localDay` and `zonedToUtc` in `api/src/lib/time.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:dayBounds", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:zonedToUtc"]
  - id: C20
    text: "The web app has a `todayIn` helper in `web/src/lib/format.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
  - {id: U-3, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does GET /api/appointments (and the /appointments page) already accept filters for doctor, status and a start-date window, so the count can link to it unchanged?"
    why: "REQ-6 and REQ-7 need a list filtered to doctor, No-show and the 30-day window; the generated facts list the route but not its query parameters."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/api-inventory.json#ui:/appointments"]
    options: ["Filters exist; link only", "Some filters missing; extend the route and page"]
    default: "Design reads api/src/routes/appointments.ts and AppointmentsPage and extends only the missing filters."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Does the dashboard's PERIODS include a Last 30 days period with a No-shows count, and can its window rule be reused per doctor?"
    why: "REQ-4 (answer A-01) reuses the tile's window and rule; the facts show a PERIODS symbol but not its values."
    evidence: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:PERIODS"]
    options: ["Yes; extract and share the window rule", "No such period; ask the product owner"]
    default: "Design reads dashboardService.ts and shares its window computation with the doctor count."
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Should the 30-day No-show count be added to the GET /api/doctors/:id response (also read by the /doctors/:id/edit form) or served by a separate read used only by the doctor page?"
    why: "GET /api/doctors/:id is also read by DoctorFormPage (fromDoctor), whose data goes back through PATCH /api/doctors/:id; a new read-only field there must not leak into the edit form or the PATCH body."
    evidence: ["context/generated/api-inventory.json#ui:/doctors/:id/edit", "context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:fromDoctor", "context/generated/api-inventory.json#http:PATCH:/api/doctors/:id"]
    options: ["Add to GET /api/doctors/:id; edit form and PATCH ignore it", "Separate read for the doctor page; doctor detail response unchanged"]
    default: "Add to GET /api/doctors/:id as a read-only field; fromDoctor does not map it and PATCH does not accept it; design confirms against current source."
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService", "context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:dayBounds"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:fromDoctor", "context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#index:appointment_doctor_start_idx"]
apis:
  - id: http:GET:/api/doctors/:id
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id"]
  - id: http:GET:/api/dashboard
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: http:GET:/api/appointments
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: http:PATCH:/api/doctors/:id
    change: existing
    cites: ["context/generated/api-inventory.json#http:PATCH:/api/doctors/:id"]
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/data-model.md#appointments"]
  - id: table:doctor
    change: existing
    cites: ["context/generated/db-schema.json#table:doctor"]
---
# Impact analysis: HMS-7

## Summary
HMS-7 adds a read-only count of a doctor's No-show appointments to the doctor page. The API (doctor
detail read, its service and repository, possibly a shared window rule extracted from
DashboardService) and the web doctor page change; the appointment and doctor tables are only read,
and the existing `appointment_doctor_start_idx` supports the query, so no migration is expected.
The main risks are drift from the dashboard No-shows tile (REQ-4, Q-02), whether the appointment list
supports the filters the link needs (Q-01), and the other readers of GET /api/doctors/:id: the edit
form at `/doctors/:id/edit` and the PATCH round-trip must not pick up the new field (Q-03). The run
started with a warning that the context pack lags the source, so design should confirm touched files
against current code. No background jobs exist.

## Impacted modules

Modules (highest risk first):
- module:sample-system:api — changed: the doctor detail read (or a separate read, Q-03) gains the 30-day No-show count via DoctorService and AppointmentRepo; if the Q-02 default is taken, DashboardService is refactored to share its window rule, and the dashboard No-shows tile needs a regression check. The window reuses the existing time helpers in api/src/lib/time.ts (dayBounds, localDay, zonedToUtc).
- module:sample-system:web — changed: DoctorDetailPage (`/doctors/:id`) shows the field and links to `/appointments` with filters; AppointmentsPage changes only if a filter is missing (Q-01). DoctorFormPage (`/doctors/:id/edit`, fromDoctor) is an existing reader of the doctor response and must not map the count. The link's date filter may reuse todayIn in web/src/lib/format.ts.
- module:sample-system:db — existing: no schema change; the doctor/start index already exists.

APIs (highest risk first):
- http:GET:/api/doctors/:id — changed: carries the read-only count (unless Q-03 picks a separate read). Other readers: the doctor edit form.
- http:GET:/api/dashboard — existing: source of the Last 30 days rule; its response must not change, and it needs a regression check if its window code is shared.
- http:GET:/api/appointments — existing: the link target; changed only if filters are missing (Q-01).
- http:PATCH:/api/doctors/:id — existing: must not accept or require the count; the edit form's round-trip stays as today.

Tables:
- table:appointment — existing: read by doctor_id, status NO_SHOW and starts_at window.
- table:doctor — existing: read for the doctor page as today.

## Known
- REQ-1, REQ-5, REQ-8: doctor page `/doctors/:id` rendered by DoctorDetailPage; web module changes; the edit form and PATCH stay unchanged (REQ-8).
- REQ-2, REQ-3: appointment table holds status NO_SHOW and starts_at in UTC; "today" uses HOSPITAL_TZ via existing time helpers.
- REQ-4: dashboard counts live in DashboardService with a PERIODS symbol (details under Q-02).
- REQ-6, REQ-7: link target is the `/appointments` page backed by GET /api/appointments (filters under Q-01).

## Assumed
None.

## Unknown
- U-1 (non-blocking): which filters GET /api/appointments and the `/appointments` page accept — Q-01.
- U-2 (non-blocking): the dashboard PERIODS values and whether the Last 30 days window rule is reusable per doctor — Q-02.
- U-3 (non-blocking): whether the count rides on GET /api/doctors/:id (shared with the edit form) or a separate read — Q-03.

## Open questions
- Q-01 (non-blocking, tech lead): do the appointment list route and page already support doctor, status and start-date window filters? Default: design checks and extends only what is missing.
- Q-02 (non-blocking, tech lead): does PERIODS include Last 30 days, and can its window rule be shared? Default: design reads dashboardService.ts and shares the window computation, with a dashboard regression check.
- Q-03 (non-blocking, tech lead): add the count to GET /api/doctors/:id or serve it separately? Default: add it as a read-only field that fromDoctor does not map and PATCH does not accept.

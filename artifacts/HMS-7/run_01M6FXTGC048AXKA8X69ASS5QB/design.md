---
schema: design.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: design
attempt: 1
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impact.md
    sha256: d7a4bf86331ed0aaaab21344aecf10a31ef2298ee8f21767fdbe7f83e94f0cc7
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/split.md
    sha256: 4efeb373d4d22b7d74c2c0ec55e82009f5ddb45abe3a9bc94a490c762f071910
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/req.md
    sha256: 8cacd7572d8146137cd6151565f9b5e880d72194da94af616afa60fd16406ef8
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
  - path: context/generated/api-inventory.json
    sha256: 9ba29278d8d66b7341defc3c67d6e7d68508ce25ee2991ba256be2b37194ec61
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The API is layered: routes parse input, services hold the rules, repositories hold the SQL; the web app uses `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C2
    text: "Shared helpers live in `api/src/lib`."
    cites: ["context/conventions.md#code-layout"]
  - id: C3
    text: "\"Today\" uses the hospital time zone `HOSPITAL_TZ`, default Asia/Kolkata; instants are stored in UTC."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C4
    text: "The doctor detail route is `/api/doctors/:id` (GET) in `api/src/routes/doctors.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id"]
  - id: C5
    text: "Doctor updates go through `/api/doctors/:id` (PATCH)."
    cites: ["context/generated/api-inventory.json#http:PATCH:/api/doctors/:id"]
  - id: C6
    text: "The appointment list route is `/api/appointments` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C7
    text: "The dashboard route is `/api/dashboard` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C8
    text: "Dashboard counts live in `DashboardService`, which defines `PERIODS`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:PERIODS"]
  - id: C9
    text: "Doctor rules live in `DoctorService`; appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService", "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C10
    text: "The doctor page is `DoctorDetailPage`; the edit form `DoctorFormPage` maps a doctor with `fromDoctor`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:fromDoctor"]
  - id: C11
    text: "The appointment list page is `AppointmentsPage` at `/appointments`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage", "context/generated/api-inventory.json#ui:/appointments"]
  - id: C12
    text: "API time helpers include `dayBounds` and `zonedToUtc` in `api/src/lib/time.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:dayBounds", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:zonedToUtc"]
  - id: C13
    text: "Index `appointment_doctor_start_idx` covers doctor_id and starts_at, so no migration is needed."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impact.md#summary"]
  - id: C14
    text: "The count reuses the window and rule of the `dashboard` No-shows tile for Last 30 days."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/req.md#requirements"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [S-1, S-2, REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
  - {id: U-3, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does GET /api/appointments and the /appointments page already accept doctor, status and start-date window filters?"
    why: "S-2 links the count to the list filtered the same way; the generated facts list the route but not its query parameters."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/api-inventory.json#ui:/appointments"]
    options: ["Filters exist; the link only builds the query", "Some filters missing; extend the route, AppointmentRepo list query and AppointmentsPage"]
    default: "Implement reads appointments.ts and AppointmentsPage first and extends only the missing filters, keeping existing parameters unchanged."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Does DashboardService PERIODS hold a Last 30 days period whose window computation can be extracted to a shared helper without changing dashboard output?"
    why: "REQ-4 requires the doctor count to use exactly the dashboard tile's window and rule."
    evidence: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:PERIODS"]
    options: ["Yes; extract to api/src/lib and use from both", "No Last 30 days period; ask the product owner before implementing"]
    default: "Extract the existing window computation unchanged into api/src/lib; DashboardService calls the helper; dashboard tests guard regressions."
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Besides DoctorFormPage, does any other web page read GET /api/doctors/:id and pass its whole body on (for example into PATCH)?"
    why: "The new read-only field must not reach PATCH /api/doctors/:id; the facts do not show every caller of the doctor detail read."
    evidence: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:fromDoctor", "context/generated/api-inventory.json#http:PATCH:/api/doctors/:id"]
    options: ["Only the doctor page and edit form; no change elsewhere", "Other callers exist; check each maps fields explicitly"]
    default: "Implement lists every caller of the doctor detail read through web/src/api/client.ts and confirms none forwards the field."
components:
  - {name: Shared No-show window rule (Last 30 days), module: module:sample-system:api, change: new}
  - {name: DashboardService, module: module:sample-system:api, change: changed}
  - {name: AppointmentRepo, module: module:sample-system:api, change: changed}
  - {name: DoctorService, module: module:sample-system:api, change: changed}
  - {name: doctorsRouter, module: module:sample-system:api, change: existing}
  - {name: time helpers (api/src/lib/time.ts), module: module:sample-system:api, change: existing}
  - {name: DoctorDetailPage, module: module:sample-system:web, change: changed}
  - {name: DoctorFormPage, module: module:sample-system:web, change: existing}
  - {name: AppointmentsPage, module: module:sample-system:web, change: existing}
  - {name: API client (web/src/api/client.ts), module: module:sample-system:web, change: existing}
  - {name: appointment table and appointment_doctor_start_idx, module: module:sample-system:db, change: existing}
endpoints:
  - {method: GET, path: /api/doctors/:id, change: changed}
  - {method: PATCH, path: /api/doctors/:id, change: existing}
  - {method: GET, path: /api/appointments, change: existing}
  - {method: GET, path: /api/dashboard, change: existing}
---
# Design: HMS-7

## Overview
The doctor detail read gains one read-only number: the doctor's No-show appointments in the dashboard's
Last 30 days window. To guarantee REQ-4 (same window and rule as the dashboard tile), the window
computation that DashboardService uses today is moved, unchanged, into a new shared helper in
`api/src/lib`; both DashboardService and DoctorService call it. AppointmentRepo gets one count query
(doctor_id, status NO_SHOW, starts_at in [window start, window end)), which the existing
`appointment_doctor_start_idx` serves, so there is no migration. DoctorService adds the count to the
GET /api/doctors/:id response; DoctorDetailPage shows it in the details section labelled
"No-shows (last 30 days)" and renders it as a link (including 0) to `/appointments` with doctor,
No-show and the same window as query parameters. No new infrastructure, store or endpoint.

Trade-offs:
- Count on GET /api/doctors/:id instead of a new endpoint: one request and no new route, at the cost of a field the edit form must ignore (DoctorFormPage maps fields via fromDoctor; PATCH /api/doctors/:id does not accept it).
- Extracting the dashboard window rule instead of re-implementing it: one source of truth for REQ-4, at the cost of touching DashboardService, guarded by its existing tests.
- Link window from the API, not the browser clock: the response carries the window bounds used for the count so the link filters exactly what was counted, avoiding drift around midnight in HOSPITAL_TZ.
- Count is computed on each doctor page read, not cached: one indexed count query per view; no cache to invalidate when appointments close.

## Components
- Shared No-show window rule (Last 30 days) — new, module:sample-system:api, in `api/src/lib`: returns the window [start, end) in UTC for "last 30 days up to end of today" in HOSPITAL_TZ, built on the existing time helpers.
- DashboardService — changed: its Last 30 days window uses the shared rule; output unchanged.
- AppointmentRepo — changed: adds a count of a doctor's NO_SHOW appointments whose starts_at falls in a window.
- DoctorService — changed: the doctor detail result gains the read-only count and the window bounds.
- doctorsRouter — existing: GET /api/doctors/:id handler unchanged; returns the service result; unknown id stays 404.
- time helpers (api/src/lib/time.ts) — existing: used by the shared rule.
- DoctorDetailPage — changed: shows "No-shows (last 30 days)" in the details section as a link to `/appointments?…` with doctor, No-show and window; 0 is still a link; the rest of the page is unchanged.
- DoctorFormPage — existing: fromDoctor keeps mapping only editable fields, so the count never enters the form or PATCH body.
- AppointmentsPage — existing: target of the link; changes only if Q-01 finds a filter missing.
- API client (web/src/api/client.ts) — existing: carries the extra field.
- appointment table and appointment_doctor_start_idx — existing: no migration.
- GET /api/doctors/:id — changed: adds the count and window bounds to the response.
- PATCH /api/doctors/:id — existing: does not accept the count.
- GET /api/appointments — existing: link target (see Q-01).
- GET /api/dashboard — existing: response unchanged; regression-tested after the extraction.

Story coverage:
- S-1 (see the count): shared window rule, DashboardService, AppointmentRepo, DoctorService, GET /api/doctors/:id, DoctorDetailPage.
- S-2 (open the list): DoctorDetailPage link, GET /api/appointments, AppointmentsPage.

## Known
- S-1, S-2: both stories are served by the components above.
- REQ-1, REQ-5, REQ-8: DoctorDetailPage shows the labelled field in the details section, 0 when none, rest unchanged.
- REQ-2, REQ-3, REQ-4: one shared window rule in HOSPITAL_TZ used by both the dashboard and the doctor count; AppointmentRepo counts NO_SHOW by starts_at.
- REQ-6, REQ-7: the count is always a link with the same filters and window, including 0.

## Assumed
None.

## Unknown
- U-1 (non-blocking): which filters GET /api/appointments and AppointmentsPage accept — Q-01.
- U-2 (non-blocking): whether PERIODS holds a Last 30 days window that can be extracted unchanged — Q-02.
- U-3 (non-blocking): whether any caller other than DoctorFormPage forwards the doctor detail body — Q-03.

## Open questions
- Q-01 (non-blocking, tech lead): do the appointment list route and page accept doctor, status and window filters? Default: implement checks and extends only missing filters.
- Q-02 (non-blocking, tech lead): can the dashboard's Last 30 days window be extracted to `api/src/lib` unchanged? Default: extract unchanged; dashboard tests guard it.
- Q-03 (non-blocking, tech lead): any other caller of the doctor detail read that forwards its body? Default: implement lists callers via the API client and confirms none does.

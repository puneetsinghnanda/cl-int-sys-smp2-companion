---
schema: design.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: design
attempt: 3
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
    text: "The doctor page is `DoctorDetailPage`; the edit form `DoctorFormPage` has a `fromDoctor` function."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorDetailPage.tsx:DoctorDetailPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:fromDoctor"]
  - id: C11
    text: "The appointment list page is `AppointmentsPage` at `/appointments`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage", "context/generated/api-inventory.json#ui:/appointments"]
  - id: C12
    text: "API time helpers include `addDays`, `localDay` and `dayBounds` in `api/src/lib/time.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:addDays", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:dayBounds"]
  - id: C13
    text: "Index `appointment_doctor_start_idx` covers doctor_id and starts_at, so no migration is needed."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impact.md#summary"]
  - id: C14
    text: "The count reuses the window and rule of the `dashboard` No-shows tile for Last 30 days."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/req.md#requirements"]
  - id: C15
    text: "GET /api/appointments accepts `doctorId`, patientId, status, from, to and page; from/to are hospital-local days."
    cites: ["runs/run_01M6FXTGC048AXKA8X69ASS5QB/open-questions.md#q-01"]
  - id: C16
    text: "Unknown query keys are ignored, so a wrong name such as `doctor_id` would silently drop the filter."
    cites: ["runs/run_01M6FXTGC048AXKA8X69ASS5QB/open-questions.md#q-01"]
  - id: C17
    text: "PERIODS includes `30d`, which counts NO_SHOW from the start of today minus 29 days to the end of today in config.timeZone."
    cites: ["runs/run_01M6FXTGC048AXKA8X69ASS5QB/open-questions.md#q-02"]
  - id: C18
    text: "The dashboard No-shows tile links to `/appointments?status=NO_SHOW&from=period.from&to=period.to`, selecting the same set it counts."
    cites: ["runs/run_01M6FXTGC048AXKA8X69ASS5QB/open-questions.md#q-02"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [S-1, S-2, REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown:
  - {id: U-3, blocking: false}
questions:
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Does DoctorFormPage.fromDoctor map only named editable fields, and does PATCH /api/doctors/:id reject or ignore unknown body fields? Does any other caller forward the doctor detail body?"
    why: "The new read-only fields must not reach the edit form or PATCH body; the context pack does not show fromDoctor's mapping or PATCH validation, and it lags the source."
    evidence: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:fromDoctor", "context/generated/api-inventory.json#http:PATCH:/api/doctors/:id"]
    options: ["fromDoctor maps named fields and PATCH validates a fixed field set; no change", "Either forwards unknown fields; make fromDoctor explicit and/or have PATCH drop the new fields"]
    default: "Implement reads fromDoctor and the PATCH validation before changing anything; if either passes unknown fields through, it strips the new fields explicitly and adds a test."
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
The doctor detail read gains a read-only No-show count for the dashboard's Last 30 days (`30d`)
window, plus that window as two hospital-local days. The `30d` window computation in
DashboardService moves, unchanged, into a new shared helper in `api/src/lib`: with tz the configured
hospital time zone and today = localDay(now, tz), from = addDays(today, -29); appointments count when
starts_at >= dayBounds(from, tz).start and starts_at < dayBounds(today, tz).end. DashboardService and
DoctorService both call it, so REQ-2, REQ-3 and REQ-4 rest on one rule. AppointmentRepo adds the same
NO_SHOW count filtered by doctor_id, served by the existing `appointment_doctor_start_idx`, so there is
no migration. DoctorDetailPage shows the count in the details section labelled
"No-shows (last 30 days)" and always renders it as a link, including 0, in the same shape as the
dashboard No-shows tile. No new infrastructure, store or endpoint; GET /api/appointments and
AppointmentsPage already accept every filter needed (open-questions Q-01).

Interface:
- GET /api/doctors/:id adds `noShowsLast30Days`: integer ≥ 0, and `noShowsWindow`: { `from`, `to` }, hospital-local days (YYYY-MM-DD) from the shared rule, the same form as the dashboard's `period.from`/`period.to`. camelCase follows the existing response fields (feePaise, slotMinutes, noShows).
- Link: `/appointments?doctorId=<id>&status=NO_SHOW&from=<noShowsWindow.from>&to=<noShowsWindow.to>`. The list turns from/to into [dayBounds(from).start, dayBounds(to).end), the same bounds the count uses, so the list `total` equals the count (S-2 AC2). Lists are paged at 20; the link opens on page 1 and AC2 compares `total`, not the page length.
- `doctorId` must be spelled exactly: the list ignores unknown query keys, so a misspelt name would silently drop the doctor filter. The web test for the link asserts the exact URL.

Trade-offs:
- Count on GET /api/doctors/:id instead of a new endpoint: one request and no new route, at the cost of fields the edit form and PATCH must not pick up (checked under Q-03).
- Extracting the dashboard `30d` rule instead of re-implementing it: one source of truth for REQ-4, at the cost of touching DashboardService, guarded by its tests.
- Window returned by the API as local days, not computed in the browser: the link matches the count exactly and avoids drift around midnight in the hospital time zone.
- Count computed per doctor page read, not cached: one indexed count query per view; no cache to invalidate when appointments close.

## Components
Components:
- Shared No-show window rule (Last 30 days) — new, module:sample-system:api, in `api/src/lib`: returns { from, to } as local days and their UTC bounds for the `30d` rule, using addDays, localDay and dayBounds.
- DashboardService — changed: its `30d` window comes from the shared rule; dashboard output unchanged.
- AppointmentRepo — changed: adds a count of one doctor's NO_SHOW appointments with starts_at inside the given bounds.
- DoctorService — changed: the doctor detail result gains `noShowsLast30Days` and `noShowsWindow`.
- doctorsRouter — existing: GET /api/doctors/:id handler returns the service result; unknown id stays 404.
- time helpers (api/src/lib/time.ts) — existing: used by the shared rule.
- DoctorDetailPage — changed: shows the labelled count in the details section as a link (0 included) built from `noShowsWindow`; rest of the page unchanged.
- DoctorFormPage — existing: behaviour with the new fields is checked under Q-03 before implementing.
- AppointmentsPage — existing: already reads doctorId, status, from, to and page from the URL; no change.
- API client (web/src/api/client.ts) — existing: carries the extra fields.
- appointment table and appointment_doctor_start_idx — existing: no migration.

Endpoints:
- GET /api/doctors/:id — changed: adds `noShowsLast30Days` and `noShowsWindow`.
- PATCH /api/doctors/:id — existing: must not accept the new fields (checked under Q-03).
- GET /api/appointments — existing: link target with doctorId, status, from, to; no change.
- GET /api/dashboard — existing: response unchanged; regression-tested after the extraction.

Story coverage:
- S-1 (see the count): shared window rule, DashboardService, AppointmentRepo, DoctorService, GET /api/doctors/:id, DoctorDetailPage.
- S-2 (open the list): DoctorDetailPage link, GET /api/appointments, AppointmentsPage.

## Known
- S-1, S-2: both stories are served by the components above.
- REQ-1, REQ-5, REQ-8: DoctorDetailPage shows the labelled field in the details section, 0 when none, rest unchanged.
- REQ-2, REQ-3, REQ-4: the dashboard `30d` rule, extracted unchanged and filtered by doctor_id, in the hospital time zone.
- REQ-6, REQ-7: the count is always a link to the list with doctorId, NO_SHOW and the same local-day window, including 0; the list total equals the count.

## Assumed
None.

## Unknown
- U-3 (non-blocking): whether fromDoctor, PATCH validation or another caller could carry the new fields into an update — Q-03.

## Open questions
- Q-03 (non-blocking, tech lead): do fromDoctor and PATCH keep unknown fields out? Default: implement reads both first and strips the new fields explicitly if needed, with a test.
- Answered by the tech lead (eng-3c51) in open-questions.md: Q-01 (filters exist; link only) and Q-02 (`30d` exists; extract and share). Q-04 is dropped: the names follow the code, as the approver asked at gate G-escalate-design-a2.

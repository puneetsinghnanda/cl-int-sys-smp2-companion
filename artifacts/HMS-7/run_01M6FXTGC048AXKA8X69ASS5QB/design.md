---
schema: design.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: design
attempt: 2
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
resolves:
  - id: F-1
    how: "Proposed exact response fields, types and link query parameters; names stay provisional under Q-04 and, for the list, Q-01."
  - id: F-2
    how: "Dropped the uncited fromDoctor/PATCH claims; the edit-form and PATCH behaviour is now unknown U-3 with a check required in Q-03."
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
    text: "API time helpers include `dayBounds` and `zonedToUtc` in `api/src/lib/time.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:dayBounds", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:zonedToUtc"]
  - id: C13
    text: "Index `appointment_doctor_start_idx` covers doctor_id and starts_at, so no migration is needed."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impact.md#summary"]
  - id: C14
    text: "The count reuses the window and rule of the `dashboard` No-shows tile for Last 30 days."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/req.md#requirements"]
  - id: C15
    text: "API JSON keys are snake_case, for example `page_size`; lists answer items, page, page_size and total, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C16
    text: "Appointment status values include `NO_SHOW`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/impact.md#known"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [S-1, S-2, REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
assumed: []
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
  - {id: U-3, blocking: false}
  - {id: U-4, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does GET /api/appointments and the /appointments page already accept doctor, status and start-date window filters, and under which parameter names?"
    why: "S-2 links the count to the list filtered the same way; the generated facts list the route but not its query parameters."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/api-inventory.json#ui:/appointments"]
    options: ["Filters exist; the link uses their existing names", "Some filters missing; add them with the names proposed in Q-04"]
    default: "Implement reads appointments.ts and AppointmentsPage first, uses existing parameter names where present, and adds only missing ones with the Q-04 names."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Does DashboardService PERIODS hold a Last 30 days period whose window computation can be extracted to a shared helper without changing dashboard output?"
    why: "REQ-4 requires the doctor count to use exactly the dashboard tile's window and rule."
    evidence: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:PERIODS"]
    options: ["Yes; extract to api/src/lib and use from both", "No Last 30 days period; stop and ask the product owner before implementing"]
    default: "Extract the existing window computation unchanged into api/src/lib; DashboardService calls the helper; dashboard tests guard regressions."
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Does DoctorFormPage.fromDoctor map only named editable fields, and does PATCH /api/doctors/:id reject or ignore unknown body fields? Does any other caller forward the doctor detail body?"
    why: "The new read-only fields must not reach the edit form or PATCH body; the context pack does not show fromDoctor's mapping or PATCH validation, and it lags the source."
    evidence: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorFormPage.tsx:fromDoctor", "context/generated/api-inventory.json#http:PATCH:/api/doctors/:id"]
    options: ["fromDoctor maps named fields and PATCH validates a fixed field set; no change", "Either forwards unknown fields; make fromDoctor explicit and/or have PATCH drop the new fields"]
    default: "Implement reads fromDoctor and the PATCH validation before changing anything; if either passes unknown fields through, it strips the new fields explicitly and adds a test."
  - id: Q-04
    blocking: false
    ask: tech lead
    text: "Confirm the proposed names: response fields no_shows_last_30_days (integer) and no_shows_window {from, to} (ISO 8601 UTC strings, half-open), and link query parameters doctor_id, status=NO_SHOW, from, to, page=1."
    why: "The interface needs exact names; the context pack gives the snake_case convention but no names for these new values."
    evidence: ["context/conventions.md#lists", "context/generated/api-inventory.json#http:GET:/api/doctors/:id"]
    options: ["Use the proposed names", "Use other names given by the tech lead"]
    default: "Use the proposed names, except list parameters that already exist under another name (Q-01)."
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
The doctor detail read gains a read-only No-show count for the dashboard's Last 30 days window, plus
the window it used. To meet REQ-4, the window computation DashboardService uses today is moved,
unchanged, into a new shared helper in `api/src/lib`; both DashboardService and DoctorService call it,
and REQ-2/REQ-3 are checked against whatever that rule returns rather than restated here.
AppointmentRepo gets one count query (doctor_id, status NO_SHOW, starts_at inside the window), served
by the existing `appointment_doctor_start_idx`, so there is no migration. DoctorDetailPage shows the
count in the details section labelled "No-shows (last 30 days)" and always renders it as a link,
including 0, to `/appointments` filtered to that doctor, NO_SHOW and the returned window, opening on
page 1. No new infrastructure, store or endpoint.

Interface (names provisional until Q-04; list parameter names until Q-01):
- GET /api/doctors/:id adds `no_shows_last_30_days`: integer ≥ 0, and `no_shows_window`: { `from`, `to` }, ISO 8601 UTC strings, the window exactly as the shared rule returns it.
- Link: `/appointments?doctor_id=<id>&status=NO_SHOW&from=<no_shows_window.from>&to=<no_shows_window.to>&page=1`.
- S-2 AC2 is checked against the list response's `total`, not the page length, because lists are paged at 20.

Trade-offs:
- Count on GET /api/doctors/:id instead of a new endpoint: one request and no new route, at the cost of fields the edit form and PATCH must not pick up (verified under Q-03).
- Extracting the dashboard window rule instead of re-implementing it: one source of truth for REQ-4, at the cost of touching DashboardService, guarded by its tests.
- Window bounds returned by the API, not computed in the browser: the link filters exactly what was counted, avoiding drift around midnight in HOSPITAL_TZ.
- Count computed per doctor page read, not cached: one indexed count query per view; no cache to invalidate when appointments close.

## Components
Components:
- Shared No-show window rule (Last 30 days) — new, module:sample-system:api, in `api/src/lib`: returns the Last 30 days window exactly as DashboardService computes it today (start and end in UTC), using the existing time helpers.
- DashboardService — changed: calls the shared rule; dashboard output unchanged.
- AppointmentRepo — changed: adds a count of one doctor's NO_SHOW appointments whose starts_at falls in a given window.
- DoctorService — changed: the doctor detail result gains `no_shows_last_30_days` and `no_shows_window`.
- doctorsRouter — existing: GET /api/doctors/:id handler returns the service result; unknown id stays 404.
- time helpers (api/src/lib/time.ts) — existing: used by the shared rule.
- DoctorDetailPage — changed: shows the labelled count in the details section as a link (0 included); rest of the page unchanged.
- DoctorFormPage — existing: behaviour with the new fields is checked under Q-03 before implementing.
- AppointmentsPage — existing: link target; changes only if Q-01 finds a filter missing.
- API client (web/src/api/client.ts) — existing: carries the extra fields.
- appointment table and appointment_doctor_start_idx — existing: no migration.

Endpoints:
- GET /api/doctors/:id — changed: adds `no_shows_last_30_days` and `no_shows_window`.
- PATCH /api/doctors/:id — existing: must not accept the new fields (checked under Q-03).
- GET /api/appointments — existing: link target; `total` must equal the count (see Q-01).
- GET /api/dashboard — existing: response unchanged; regression-tested after the extraction.

Story coverage:
- S-1 (see the count): shared window rule, DashboardService, AppointmentRepo, DoctorService, GET /api/doctors/:id, DoctorDetailPage.
- S-2 (open the list): DoctorDetailPage link, GET /api/appointments, AppointmentsPage.

## Known
- S-1, S-2: both stories are served by the components above.
- REQ-1, REQ-5, REQ-8: DoctorDetailPage shows the labelled field in the details section, 0 when none, rest unchanged.
- REQ-2, REQ-3, REQ-4: one shared window rule, extracted unchanged from the dashboard, used for the doctor count; AppointmentRepo counts NO_SHOW by starts_at.
- REQ-6, REQ-7: the count is always a link with the same doctor, status and window, including 0; the list total equals the count.

## Assumed
None.

## Unknown
- U-1 (non-blocking): which filters and parameter names GET /api/appointments and AppointmentsPage accept — Q-01.
- U-2 (non-blocking): whether PERIODS holds a Last 30 days window that can be extracted unchanged — Q-02.
- U-3 (non-blocking): whether fromDoctor, PATCH validation or another caller could carry the new fields into an update — Q-03.
- U-4 (non-blocking): final names of the new response fields and link parameters — Q-04.

## Open questions
- Q-01 (non-blocking, tech lead): which filters does the appointment list accept, under which names? Default: reuse existing names, add only what is missing.
- Q-02 (non-blocking, tech lead): can the dashboard's Last 30 days window be extracted to `api/src/lib` unchanged? Default: extract unchanged; if no such period exists, stop and ask the product owner.
- Q-03 (non-blocking, tech lead): do fromDoctor and PATCH keep unknown fields out? Default: implement reads both first and strips the new fields explicitly if needed, with a test.
- Q-04 (non-blocking, tech lead): confirm `no_shows_last_30_days`, `no_shows_window {from, to}` and link parameters doctor_id, status, from, to, page. Default: use them.

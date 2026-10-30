---
schema: design.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
step: design
attempt: 2
skill: design@1.4.1
inputs:
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/impact.md
    sha256: affb90a6bdd83c43e210eb3195ae462b1dc743d84272556f92675ff4c041c53f
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/split.md
    sha256: 0347e867d613f325eceea3249daca63249ed02bafd70ff50bf50e83989fa7c76
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md
    sha256: e04911f106dc8dc1dd0ed9c0dfdc17bb61a93e72095ba3a354e1a994ab185ffb
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
    how: "Named and typed the interface: with_today flag, today_count field, DoctorListRow type, link query keys (U-5/Q-05)."
  - id: F-2
    how: "Q-02 default now shares the dashboard condition only if it excludes only CANCELLED; otherwise REQ-3 wins."
claims:
  - id: C1
    text: "The doctors list route is `/api/doctors` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors"]
  - id: C2
    text: "The doctor detail route is `/api/doctors/:id` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id"]
  - id: C3
    text: "The appointment list route is `/api/appointments` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C4
    text: "The dashboard route is `/api/dashboard` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C5
    text: "A settings route `/api/settings` (GET) exists."
    cites: ["context/generated/api-inventory.json#http:GET:/api/settings"]
  - id: C6
    text: "Routes live in `api/src/routes`, rules in services, SQL in `api/src/repositories`, shared helpers in api/src/lib."
    cites: ["context/conventions.md#code-layout"]
  - id: C7
    text: "Lists answer `{ items, page, page_size, total }`, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C8
    text: "\"Today\" uses the hospital time zone `HOSPITAL_TZ`, default Asia/Kolkata; instants are stored in UTC."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C9
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C10
    text: "Routes declare a permission with `requirePermission`; in the MVP every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C11
    text: "Logs never carry request bodies, contact details or `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C12
    text: "Doctor routes are defined by `doctorsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/doctors.ts:doctorsRouter"]
  - id: C13
    text: "Doctor rules live in `DoctorService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C14
    text: "Doctor SQL lives in `DoctorRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: C15
    text: "Appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C16
    text: "The API day-boundary helper is `dayBounds`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:dayBounds"]
  - id: C17
    text: "Dashboard counts are computed in `DashboardService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C18
    text: "The doctors list screen is `DoctorsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorsPage.tsx:DoctorsPage"]
  - id: C19
    text: "The appointment list screen is `AppointmentsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"]
  - id: C20
    text: "The booking screen component `BookAppointmentPage` exists."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/BookAppointmentPage.tsx:BookAppointmentPage"]
  - id: C21
    text: "The web API client object is `api`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
  - id: C22
    text: "The web helper for today's date in a time zone is `todayIn`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn"]
  - id: C23
    text: "List paging in the web app uses the `Pagination` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/Pagination.tsx:Pagination"]
  - id: C24
    text: "The appointment table identifies the doctor by `doctor_id`."
    cites: ["context/generated/db-schema.json#column:appointment.doctor_id"]
  - id: C25
    text: "Impact analysis records the booking picker as a second caller of the doctors list route (`BookAppointmentPage`)."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/impact.md#L93"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-8, REQ-9, REQ-10]
assumed: [REQ-4, REQ-6, REQ-7, REQ-11]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
  - {id: U-3, blocking: false}
  - {id: U-4, blocking: false}
  - {id: U-5, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Do GET /api/appointments and AppointmentsPage already accept a doctor filter and a single-day filter (read from the URL)? If not, may this change add them?"
    why: "S-2 links the count to the appointment list filtered to that doctor and today."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"]
    options: ["Filters exist; link to them", "Add the missing filter(s) in this change"]
    default: "Add the missing filter(s) in this change"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Does DashboardService's 'Appointments today' count exclude only CANCELLED? If so, may its status condition be shared with the new per-doctor count?"
    why: "S-1 AC3 (exclude only Cancelled) must hold even if the tile counts differently; S-1 AC4 needs the column and tile to agree."
    evidence: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L62"]
    options: ["Share one status condition only if the tile excludes only CANCELLED; otherwise follow the ticket and raise a separate fix", "Keep separate conditions"]
    default: "Share one status condition only if the tile excludes only CANCELLED; otherwise follow the ticket and raise a separate fix"
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Where does the web app get the hospital time zone for the link's day (for example from GET /api/settings)?"
    why: "S-2 AC2 filters the linked list to today in Asia/Kolkata; the browser's own zone may differ."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/settings", "context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn"]
    options: ["Read it from GET /api/settings", "Have the API return the day with the list"]
    default: "Read it from GET /api/settings"
  - id: Q-04
    blocking: false
    ask: tech lead
    text: "The doctor response types are not in the generated facts. Confirm the Today count goes on a list-only row type, leaving the shared doctor type and GET /api/doctors/:id unchanged."
    why: "Adding the count to the shared doctor type would also change the detail route and the booking picker."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id", "artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/impact.md#L93"]
    options: ["List-only row type", "Add to the shared doctor type"]
    default: "List-only row type"
  - id: Q-05
    blocking: false
    ask: tech lead
    text: "The new names (with_today, today_count, DoctorListRow) and the link keys (doctor_id, date) are proposals, not taken from the facts. Keep them, or match names the existing list filters already use?"
    why: "The facts do not show the existing query keys of GET /api/doctors or GET /api/appointments."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/doctors", "context/generated/api-inventory.json#http:GET:/api/appointments"]
    options: ["Keep the proposed names unless an existing key already means the same", "Rename all to a team convention"]
    default: "Keep the proposed names unless an existing key already means the same"
components:
  - {name: doctorsRouter (GET /api/doctors), module: "module:sample-system:api", change: changed}
  - {name: DoctorService (list with today counts), module: "module:sample-system:api", change: changed}
  - {name: AppointmentRepo (count today per doctor), module: "module:sample-system:api", change: changed}
  - {name: DoctorListRow type (api), module: "module:sample-system:api", change: new}
  - {name: DoctorRepo (list query), module: "module:sample-system:api", change: existing}
  - {name: dayBounds (lib/time), module: "module:sample-system:api", change: existing}
  - {name: DashboardService, module: "module:sample-system:api", change: existing}
  - {name: api client (web/src/api/client.ts), module: "module:sample-system:web", change: changed}
  - {name: DoctorListRow type (web), module: "module:sample-system:web", change: new}
  - {name: DoctorsPage, module: "module:sample-system:web", change: changed}
  - {name: todayIn (lib/format), module: "module:sample-system:web", change: existing}
  - {name: Pagination, module: "module:sample-system:web", change: existing}
  - {name: AppointmentsPage, module: "module:sample-system:web", change: existing}
  - {name: BookAppointmentPage, module: "module:sample-system:web", change: existing}
  - {name: appointment table and appointment_doctor_start_idx, module: "module:sample-system:db", change: existing}
endpoints:
  - {method: GET, path: /api/doctors, change: changed}
  - {method: GET, path: /api/doctors/:id, change: existing}
  - {method: GET, path: /api/appointments, change: existing}
  - {method: GET, path: /api/dashboard, change: existing}
  - {method: GET, path: /api/settings, change: existing}
---
# Design: HMS-6

## Overview
`GET /api/doctors` gains an optional boolean query parameter, `with_today`. When `with_today=true`, each item in `items` is a `DoctorListRow`: the existing doctor summary fields plus `today_count`, an integer of 0 or more. `today_count` is the number of that doctor's appointments that start today in `HOSPITAL_TZ` and are not CANCELLED. Without the parameter, the response is exactly as today, so the booking picker is unaffected.

The flow is:
1. `DoctorService` lists the page through the unchanged `DoctorRepo` query, so filters, search, sort by name and 20 per page stay as they are.
2. It takes today's UTC bounds from `dayBounds`.
3. It calls one new `AppointmentRepo` method, `countTodayByDoctor(doctorIds, from, to)`. The method returns a map from doctor id to count, using `starts_at >= from AND starts_at < to AND status <> 'CANCELLED'` grouped by `doctor_id`, and the existing (doctor_id, starts_at) index.
4. Doctors missing from the map get `today_count = 0`.

On the web side, `api.doctors({ ..., with_today: true })` returns `Page<DoctorListRow>`. `DoctorsPage` renders a "Today" column after the existing columns. Each count, including 0, links to `/appointments?doctor_id=<id>&date=<YYYY-MM-DD>`, where the date is `todayIn(<hospital time zone>)`.

No migration, new store or new route is needed.

Trade-offs:
- Separate count query vs. a join in the list query: the separate query keeps the list query untouched (REQ-8 to REQ-10), at the cost of one extra indexed query per page.
- Opt-in `with_today` vs. always on: opt-in keeps the picker's cost and response unchanged, at the cost of one more parameter.
- List-only `DoctorListRow` vs. the shared doctor type: the list-only type leaves `GET /api/doctors/:id` unchanged (Q-04), at the cost of a second type in api and web.
- Status rule: the count excludes only CANCELLED, as the ticket says (REQ-3). It shares `DashboardService`'s condition only if that condition is the same; otherwise the tile gets a separate fix (Q-02). This protects REQ-3, at the cost of a possible temporary mismatch with the tile.

## Components
- doctorsRouter, GET /api/doctors (api), **changed**: parses `with_today` (boolean, default false) next to the existing filters and keeps its existing `requirePermission`.
- DoctorService (api), **changed**: when `with_today` is true, adds `today_count` to each row of the page; 0 for doctors without appointments.
- AppointmentRepo (api), **changed**: new `countTodayByDoctor(doctorIds, from, to)`, grouped by `doctor_id`, excluding CANCELLED.
- DoctorListRow type (api), **new**: the doctor summary plus `today_count: number`; it lives with the existing API types.
- DoctorRepo (api), **existing**: list query, filters, search, sort and paging unchanged.
- dayBounds (api lib/time), **existing**: today's UTC range in the hospital zone.
- DashboardService (api), **existing**: becomes changed only if Q-02 confirms its condition matches and is shared; its tile value must not change.
- api client (web), **changed**: `api.doctors` accepts `with_today` and returns `Page<DoctorListRow>` when it is set.
- DoctorListRow type (web), **new**: mirrors the API type in the web API types.
- DoctorsPage (web), **changed**: Today column plus a link per row to `/appointments?doctor_id=<id>&date=<YYYY-MM-DD>`.
- todayIn (web lib/format), **existing**: today's date for the link.
- Pagination (web), **existing**: unchanged paging.
- AppointmentsPage (web), **existing**: reads `doctor_id` and `date` from the URL; becomes changed only if Q-01 finds those filters missing.
- BookAppointmentPage (web), **existing**: does not pass `with_today`, so nothing changes for it.
- appointment table and appointment_doctor_start_idx (db), **existing**: read only, no migration.
- GET /api/doctors, **changed**: optional `with_today`; the list shape `{ items, page, page_size, total }` is kept.
- GET /api/doctors/:id, **existing**: unchanged.
- GET /api/appointments, **existing**: link target (see Q-01).
- GET /api/dashboard, **existing**: unchanged.
- GET /api/settings, **existing**: assumed source of the hospital time zone for the web link (Q-03).
- Story map: S-1 → doctorsRouter, DoctorService, AppointmentRepo, DoctorListRow, dayBounds, api client, DoctorsPage. S-2 → DoctorsPage, todayIn, AppointmentsPage, GET /api/appointments. S-3 → DoctorRepo, Pagination, DoctorsPage (existing columns kept), GET /api/doctors list shape.

## Known
- REQ-1, REQ-5: DoctorsPage renders the Today column from `today_count`, which is 0 when the doctor has none.
- REQ-2: the count uses dayBounds in HOSPITAL_TZ over starts_at stored in UTC.
- REQ-3: countTodayByDoctor excludes CANCELLED and nothing else.
- REQ-8, REQ-9, REQ-10: the DoctorRepo list query, 20 per page, sort by name, filters and search are not touched.

## Assumed
- REQ-4: assumed the dashboard tile also excludes only CANCELLED, so the column and the tile agree. Basis: DashboardService computes the dashboard counts; if it differs, REQ-3 wins and the tile is fixed separately (Q-02).
- REQ-6, REQ-7: assumed `/appointments` can be linked with `doctor_id` and `date` filters. Basis: the page and GET /api/appointments exist, and appointment rows carry `doctor_id` (Q-01). The key names are proposals (Q-05).
- REQ-11: assumed the existing columns stay and Today is added after them. Basis: the intent's assumed REQ-11.
- The booking picker calling GET /api/doctors is taken from the impact analysis (C25), not from a call fact in the module map.

## Unknown
- U-1 (non-blocking): whether the appointment list supports doctor and day filters → Q-01.
- U-2 (non-blocking): whether the dashboard's count excludes only CANCELLED → Q-02.
- U-3 (non-blocking): where the web gets the hospital time zone for the link → Q-03.
- U-4 (non-blocking): the doctor response types are not in the facts; the count goes on a list-only row type → Q-04.
- U-5 (non-blocking): the new names and link keys are proposals not found in the facts → Q-05.

## Open questions
- Q-01 (non-blocking, tech lead): do the appointment list API and page accept doctor and day filters? Default: add the missing filter(s) in this change.
- Q-02 (non-blocking, tech lead): does the tile exclude only CANCELLED, and may its condition be shared? Default: share it only if it excludes only CANCELLED; otherwise follow the ticket and raise a separate fix.
- Q-03 (non-blocking, tech lead): where does the web get the hospital time zone? Default: read it from GET /api/settings.
- Q-04 (non-blocking, tech lead): list-only row type for the count? Default: list-only row type.
- Q-05 (non-blocking, tech lead): keep the proposed names with_today, today_count, DoctorListRow, doctor_id and date? Default: keep them unless an existing key already means the same.

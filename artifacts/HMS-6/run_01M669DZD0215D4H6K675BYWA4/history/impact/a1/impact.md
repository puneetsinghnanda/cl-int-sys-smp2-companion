---
schema: impact.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
step: impact
attempt: 1
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md
    sha256: e04911f106dc8dc1dd0ed9c0dfdc17bb61a93e72095ba3a354e1a994ab185ffb
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
claims:
  - id: C1
    text: "The doctors list is served by the route `/api/doctors` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors"]
  - id: C2
    text: "The appointment list is served by the route `/api/appointments` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C3
    text: "The dashboard data comes from the route `/api/dashboard` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C4
    text: "The web app has a doctors list screen at UI route `/doctors`."
    cites: ["context/generated/api-inventory.json#ui:/doctors"]
  - id: C5
    text: "The web app has an appointment list screen at UI route `/appointments`."
    cites: ["context/generated/api-inventory.json#ui:/appointments"]
  - id: C6
    text: "The doctors list screen is the component `DoctorsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorsPage.tsx:DoctorsPage"]
  - id: C7
    text: "The appointment list screen is the component `AppointmentsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"]
  - id: C8
    text: "Dashboard tiles are rendered by the component `Tile` on the dashboard page."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:Tile"]
  - id: C9
    text: "Dashboard counts are computed in the service `DashboardService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C10
    text: "Doctor routes are defined by `doctorsRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/doctors.ts:doctorsRouter"]
  - id: C11
    text: "Doctor rules live in `DoctorService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C12
    text: "Doctor SQL lives in `DoctorRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: C13
    text: "Appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C14
    text: "The web app calls the API only through `client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C15
    text: "\"Today\" is computed in the hospital time zone `HOSPITAL_TZ`, default Asia/Kolkata."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C16
    text: "Appointment status is one of SCHEDULED, COMPLETED, `CANCELLED` or NO_SHOW."
    cites: ["context/data-model.md#appointments"]
  - id: C17
    text: "The appointment table has a `status` column."
    cites: ["context/generated/db-schema.json#column:appointment.status"]
  - id: C18
    text: "The appointment table has a `starts_at` column stored in UTC."
    cites: ["context/generated/db-schema.json#column:appointment.starts_at", "context/data-model.md#appointments"]
  - id: C19
    text: "The appointment table has a `doctor_id` column referencing the doctor table."
    cites: ["context/generated/db-schema.json#column:appointment.doctor_id"]
  - id: C20
    text: "The index `appointment_doctor_start_idx` covers doctor_id and starts_at of appointment."
    cites: ["context/generated/db-schema.json#index:appointment_doctor_start_idx"]
  - id: C21
    text: "The index `doctor_name_idx` covers the doctor's full name."
    cites: ["context/generated/db-schema.json#index:doctor_name_idx"]
  - id: C22
    text: "No `background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: [REQ-4, REQ-6, REQ-7]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Do GET /api/appointments and AppointmentsPage already accept a doctor filter and a single-day filter? If not, may this change add them?"
    why: "REQ-6 and REQ-7 need a link target filtered to one doctor and today; if the filters are missing, GET /api/appointments and AppointmentsPage become changed instead of existing."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"]
    options: ["Filters exist; link to them", "Add the missing filter(s) in this change"]
    default: "Add the missing filter(s) in this change"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Where does DashboardService compute the 'Appointments today' count, and does it exclude only CANCELLED? May the doctors count reuse that same predicate?"
    why: "REQ-4 needs the doctors count and the tile to agree; the facts show DashboardService exists but not its counting rule."
    evidence: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "context/generated/api-inventory.json#http:GET:/api/dashboard"]
    options: ["Reuse the dashboard's predicate, shared between both", "Write a separate query for the doctors count"]
    default: "Reuse the dashboard's predicate, shared between both"
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/routes/doctors.ts:doctorsRouter", "context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService", "context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorsPage.tsx:DoctorsPage"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#index:appointment_doctor_start_idx"]
apis:
  - id: http:GET:/api/doctors
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors"]
  - id: http:GET:/api/appointments
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: http:GET:/api/dashboard
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/generated/db-schema.json#column:appointment.status", "context/generated/db-schema.json#column:appointment.starts_at"]
  - id: table:doctor
    change: existing
    cites: ["context/generated/db-schema.json#table:doctor", "context/generated/db-schema.json#index:doctor_name_idx"]
---
# Impact analysis: HMS-6

## Summary
The change adds a per-doctor count of today's appointments, excluding CANCELLED, to the doctors list. On the API side, `GET /api/doctors` (doctorsRouter, DoctorService, DoctorRepo) changes so each doctor row carries the count. On the web side, `DoctorsPage` at `/doctors` shows the Today column and links to `/appointments`. No table or migration changes: the `appointment` table already has `doctor_id`, `starts_at` (UTC) and `status`, and the index `appointment_doctor_start_idx` covers the lookup; "today" must use `HOSPITAL_TZ`. Main risks: the facts do not show whether the appointment list can filter by doctor and day (U-1), or how DashboardService counts "Appointments today" (U-2).

## Impacted modules
- module:sample-system:api, **changed**: the doctors list route, service and repository return a Today count per doctor.
- module:sample-system:web, **changed**: DoctorsPage adds the Today column and the link; calls go through client.ts.
- module:sample-system:db, **existing**: no migration; the existing index on appointment (doctor_id, starts_at) serves the count.
- http:GET:/api/doctors, **changed**: response gains a Today count per doctor; paging, sort by name, filters and search unchanged.
- http:GET:/api/appointments, **existing**: link target; becomes changed if Q-01 says the doctor/day filters must be added.
- http:GET:/api/dashboard, **existing**: its "Appointments today" rule is the reference for REQ-4; not changed.
- table:appointment, **existing**: read for the count (doctor_id, starts_at, status).
- table:doctor, **existing**: read as today; sort by name uses doctor_name_idx.
- Indirect effects: UI route `/doctors` shows the changed data; UI route `/appointments` is the link target. No background jobs exist, so none are affected.

## Known
- REQ-1, REQ-5: the Today column and its 0 value sit in DoctorsPage at `/doctors`.
- REQ-2: count by appointment.doctor_id and starts_at, with "today" in HOSPITAL_TZ (architecture time-and-slots).
- REQ-3: appointment.status holds CANCELLED, so the exclusion can be expressed on existing data.
- REQ-8, REQ-9, REQ-10: the existing GET /api/doctors paging, sort by name (doctor_name_idx), filters and search are kept; only the response grows.
- REQ-11: the existing doctor columns stay; table:doctor is unchanged.

## Assumed
- REQ-4: assumed the count can share DashboardService's rule; basis: DashboardService computes the dashboard counts behind GET /api/dashboard, but the facts do not show the rule (see U-2).
- REQ-6, REQ-7: assumed the link targets the existing `/appointments` UI route and GET /api/appointments; basis: both exist in the API inventory, but their filters are not in the facts (see U-1).

## Unknown
- U-1 (non-blocking): whether GET /api/appointments and AppointmentsPage support doctor and single-day filters → Q-01.
- U-2 (non-blocking): how DashboardService counts "Appointments today", and whether it excludes only CANCELLED → Q-02.

## Open questions
- Q-01 (non-blocking, tech lead): do the appointment list API and page accept doctor and day filters? Default: add the missing filter(s) in this change.
- Q-02 (non-blocking, tech lead): where and how does DashboardService count "Appointments today", and may the doctors count reuse it? Default: reuse the dashboard's predicate, shared between both.

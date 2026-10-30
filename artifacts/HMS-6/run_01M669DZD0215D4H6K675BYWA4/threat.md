---
schema: threat-model.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
step: threat
attempt: 1
skill: threat-model@1.0.1
inputs:
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/design.md
    sha256: 9b1662a069745e77e899b72506170a589549e1d2603b91da482f174e575ab746
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: 9ba29278d8d66b7341defc3c67d6e7d68508ce25ee2991ba256be2b37194ec61
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "Every API request passes through `identify`, which sets the actor; routes call requirePermission."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "In the MVP every request is the actor `staff` and every permission is granted; sign-in replaces only identify and requirePermission."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "The API is layered: routes parse input, services hold the rules, `repositories` hold the SQL."
    cites: ["context/architecture.md#components"]
  - id: C4
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C5
    text: "Instants are stored in UTC; \"today\" uses `HOSPITAL_TZ`, default Asia/Kolkata."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C6
    text: "Medical history leaves the database only for the patient's own page (`medical history`)."
    cites: ["context/data-model.md#patients"]
  - id: C7
    text: "An appointment links one doctor and one patient; status is SCHEDULED, COMPLETED, `CANCELLED` or NO_SHOW."
    cites: ["context/data-model.md#appointments"]
  - id: C8
    text: "`appointment_event` records every change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C9
    text: "The doctors list route is `/api/doctors` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors"]
  - id: C10
    text: "The appointment list route is `/api/appointments` (GET)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C11
    text: "The design adds the opt-in parameter `with_today` and the per-row field today_count to GET /api/doctors."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/design.md#L201"]
  - id: C12
    text: "The design's count method is `countTodayByDoctor(doctorIds, from, to)` in AppointmentRepo."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/design.md#L203"]
  - id: C13
    text: "No `background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [T-1, T-4, T-5, T-7, T-9, T-11, T-12, T-17, T-18, T-19]
assumed: [T-2, T-3, T-6, T-8, T-10, T-13, T-14, T-15, T-16, T-20]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "When sign-in replaces identify and requirePermission, should the Today count need the same permission as the doctors list, or an appointment-read permission?"
    why: "The count summarises appointment data on a doctor route; with real roles it could expose appointment volume to users who cannot see appointments."
    evidence: ["context/architecture.md#identity"]
    options: ["Same permission as the doctors list", "Require appointment-read permission for with_today"]
    default: "Same permission as the doctors list"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Does the API have any request-rate limit today? The inputs do not mention one."
    why: "with_today adds a database query per request; without a limit, repeated calls cost more than before."
    evidence: ["context/architecture.md#deployment-shape"]
    options: ["No limit; accept the bounded per-page cost", "Add a limit in a separate ticket"]
    default: "No limit; accept the bounded per-page cost"
threats:
  - id: T-1
    component: doctorsRouter (GET /api/doctors)
    stride: spoofing
    threat: "There is no real sign-in in the MVP, so any caller of GET /api/doctors acts as staff and can request today's counts."
    mitigation: "Existing control: the route keeps its requirePermission call behind identify, so the future sign-in covers the new parameter without route changes (C1, C2). The permission choice for counts is Q-01."
  - id: T-2
    component: doctorsRouter (GET /api/doctors)
    stride: tampering
    threat: "A malformed with_today value could be read as true or break parsing of the other list filters."
    mitigation: "New control for implement: parse with_today strictly as a boolean, default false, and answer 400 for any other value, like the route's other input checks (C3)."
  - id: T-3
    component: doctorsRouter (GET /api/doctors)
    stride: denial_of_service
    threat: "Callers set with_today=true on every request, adding a count query per request."
    mitigation: "The extra query is bounded by the existing page size and runs on the existing (doctor_id, starts_at) index (design). The rate-limit question is Q-02."
  - id: T-4
    component: DoctorService (list with today counts)
    stride: information_disclosure
    threat: "Adding counts could leak patient or appointment details into the doctors list."
    mitigation: "Existing rule: only one integer per doctor is added; no patient fields are read or returned, and medical history never leaves the database except on the patient's own page (C6)."
  - id: T-5
    component: AppointmentRepo (count today per doctor)
    stride: tampering
    threat: "Doctor ids or time bounds built into the SQL text could change the query."
    mitigation: "Existing pattern: SQL lives only in repositories (C3). New control for implement: pass doctorIds, from and to as bound parameters and never concatenate them."
  - id: T-6
    component: AppointmentRepo (count today per doctor)
    stride: denial_of_service
    threat: "An unbounded doctor id list would make the count query scan widely."
    mitigation: "New control for implement: call countTodayByDoctor only with the ids of the current page and return early for an empty list."
  - id: T-7
    component: DoctorListRow type (api)
    stride: information_disclosure
    threat: "A widened row type could carry extra doctor or appointment fields into the list response."
    mitigation: "The type is the existing doctor summary plus today_count only (C11); implement must not spread raw rows into it."
  - id: T-8
    component: DoctorRepo (list query)
    stride: tampering
    threat: "Changes near the list query could alter filters, search or sort and expose other doctors' data."
    mitigation: "The design leaves the list query unchanged; existing list tests must still pass (REQ-8 to REQ-10)."
  - id: T-9
    component: dayBounds (lib/time)
    stride: tampering
    threat: "A wrong time zone would shift the day window and count the wrong appointments."
    mitigation: "Existing control: today comes from HOSPITAL_TZ with default Asia/Kolkata over UTC instants (C5). Implement adds tests around local midnight."
  - id: T-10
    component: DashboardService
    stride: tampering
    threat: "Sharing its status condition could silently change the dashboard tile value."
    mitigation: "Change it only if Q-02 of the design confirms the condition matches; add a regression test that the tile value is unchanged."
  - id: T-11
    component: api client (web/src/api/client.ts)
    stride: tampering
    threat: "A client-side query string built by hand could mix or inject parameters."
    mitigation: "Existing control: all calls go through client.ts (C4); with_today goes through its existing query builder, not string concatenation."
  - id: T-12
    component: DoctorListRow type (web)
    stride: information_disclosure
    threat: "The web type could expect fields the API does not send, so the page renders stale or undefined values."
    mitigation: "The web type mirrors the API type exactly (summary plus today_count); a type check in the build catches drift."
  - id: T-13
    component: DoctorsPage
    stride: tampering
    threat: "The link and count are built from response data; a bad value could produce a misleading link."
    mitigation: "New control for implement: build the link from the numeric doctor id and a YYYY-MM-DD date with URL encoding, and render the count as plain text."
  - id: T-14
    component: todayIn (lib/format)
    stride: tampering
    threat: "Using the browser's time zone or clock instead of the hospital zone links to the wrong day."
    mitigation: "Call todayIn with the hospital time zone, not the browser's zone (design Q-03). A wrong browser clock affects only the link's day, not data."
  - id: T-15
    component: Pagination
    stride: denial_of_service
    threat: "Paging through every page with with_today=true multiplies count queries."
    mitigation: "Unchanged paging; each page runs one bounded, indexed count (see T-3, T-6)."
  - id: T-16
    component: AppointmentsPage
    stride: tampering
    threat: "doctor_id and date read from the URL are user-controlled."
    mitigation: "New control for implement, if the filters are added (design Q-01): validate doctor_id as an integer and date as YYYY-MM-DD in the page, and validate again in GET /api/appointments."
  - id: T-17
    component: BookAppointmentPage
    stride: denial_of_service
    threat: "If the count were always on, every picker search would pay for it."
    mitigation: "The picker does not send with_today, so its requests and response stay as before (C11)."
  - id: T-18
    component: appointment table and appointment_doctor_start_idx
    stride: information_disclosure
    threat: "The new read could pull patient-linked rows beyond what the count needs."
    mitigation: "The count reads only doctor_id, starts_at and status and returns aggregates; no patient columns are selected (C7)."
  - id: T-19
    component: doctorsRouter (GET /api/doctors)
    stride: repudiation
    threat: "Staff could deny actions taken based on the count."
    mitigation: "The feature is read-only; all appointment changes are still recorded with actor and time in appointment_event (C8)."
  - id: T-20
    component: doctorsRouter (GET /api/doctors)
    stride: elevation_of_privilege
    threat: "Once roles exist, a user allowed to list doctors but not appointments could read appointment volume through with_today."
    mitigation: "Keep the count behind the route's requirePermission now; decide the permission in Q-01 before roles are added."
---
# Threat model: HMS-6

## Components
- doctorsRouter (GET /api/doctors): parses list filters and the new with_today flag; returns doctor summaries and counts.
- DoctorService (list with today counts): joins the page of doctors with their counts.
- AppointmentRepo (count today per doctor): reads appointment doctor_id, starts_at and status; returns integer counts.
- DoctorListRow type (api): doctor summary plus today_count.
- DoctorRepo (list query): unchanged doctor list SQL.
- dayBounds (lib/time): today's UTC range in HOSPITAL_TZ.
- DashboardService: dashboard counts; touched only if its status condition is shared.
- api client (web/src/api/client.ts): sends with_today and types the response.
- DoctorListRow type (web): mirror of the API type.
- DoctorsPage: renders the Today column and links.
- todayIn (lib/format): today's date for the link.
- Pagination: unchanged paging.
- AppointmentsPage: link target; reads doctor_id and date from the URL.
- BookAppointmentPage: unchanged picker; does not send with_today.
- appointment table and appointment_doctor_start_idx: read only; appointment rows link a doctor and a patient.
- No background jobs exist, so none are modelled (C13).

## Threats
- T-1 doctorsRouter, spoofing: no real sign-in in the MVP → keep requirePermission behind identify (existing).
- T-2 doctorsRouter, tampering: malformed with_today → strict boolean parse, 400 otherwise (new).
- T-3 doctorsRouter, denial of service: extra query per request → bounded by page size and index; rate limit is Q-02.
- T-4 DoctorService, information disclosure: patient data leaking → one integer per doctor only (existing rule).
- T-5 AppointmentRepo, tampering: SQL built from input → bound parameters only (new).
- T-6 AppointmentRepo, denial of service: unbounded id list → only the current page's ids, early return when empty (new).
- T-7 DoctorListRow type (api), information disclosure: extra fields → summary plus today_count only.
- T-8 DoctorRepo, tampering: list behaviour drift → query unchanged; list tests must pass.
- T-9 dayBounds, tampering: wrong day window → HOSPITAL_TZ over UTC (existing); midnight tests.
- T-10 DashboardService, tampering: tile value changes → change only if confirmed; regression test.
- T-11 api client, tampering: hand-built query strings → existing client.ts query builder.
- T-12 DoctorListRow type (web), information disclosure: type drift → mirror the API type; build type check.
- T-13 DoctorsPage, tampering: misleading link → numeric id plus YYYY-MM-DD date, URL-encoded; count as text (new).
- T-14 todayIn, tampering: wrong day from the browser zone → use the hospital zone.
- T-15 Pagination, denial of service: many pages → one bounded count per page.
- T-16 AppointmentsPage, tampering: user-controlled URL filters → validate in the page and the API (new, if filters are added).
- T-17 BookAppointmentPage, denial of service: picker paying for counts → it does not send with_today.
- T-18 appointment table and appointment_doctor_start_idx, information disclosure: reading patient-linked rows → only doctor_id, starts_at, status, aggregated.
- T-19 doctorsRouter, repudiation: read-only feature; changes are still logged in appointment_event (existing).
- T-20 doctorsRouter, elevation of privilege: counts readable without appointment permission once roles exist → decide in Q-01.

## Known
- T-1, T-19: rely on the existing identify/requirePermission and appointment_event controls.
- T-4, T-7, T-18: rely on the existing rule that patient and medical data stay out of lists, plus the design's narrow row type.
- T-5: relies on the existing repository layering, plus bound parameters.
- T-9: relies on the existing HOSPITAL_TZ handling.
- T-11: relies on the existing single API client.
- T-12: relies on the build's type check.
- T-17: relies on the design's opt-in flag.

## Assumed
- T-2, T-6, T-13, T-16: assumed implement adds the stated new controls; basis: they follow the existing layering (routes parse input, repositories hold SQL).
- T-3, T-15: assumed the per-page cost is acceptable; basis: the existing page size and index (design).
- T-8: assumed the list query stays unchanged; basis: the design marks DoctorRepo existing.
- T-10: assumed DashboardService changes only after confirmation; basis: design Q-02.
- T-14: assumed the hospital zone reaches the web; basis: design Q-03.
- T-20: assumed the MVP's single staff actor remains until sign-in; basis: architecture identity.

## Unknown
- U-1 (non-blocking): which permission should guard the counts once sign-in exists → Q-01.
- U-2 (non-blocking): whether any request-rate limit exists → Q-02.

## Open questions
- Q-01 (non-blocking, tech lead): permission for counts after sign-in? Default: same permission as the doctors list.
- Q-02 (non-blocking, tech lead): is there a rate limit? Default: no limit; accept the bounded per-page cost.

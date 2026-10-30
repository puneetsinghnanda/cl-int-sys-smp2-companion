---
schema: change.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
step: impl
attempt: 1
skill: implement@1.0.1
inputs:
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/design.md
    sha256: 9b1662a069745e77e899b72506170a589549e1d2603b91da482f174e575ab746
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/split.md
    sha256: 0347e867d613f325eceea3249daca63249ed02bafd70ff50bf50e83989fa7c76
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 04f84ff09b4ad6ffaba59ec846716d25f03c8aae22292958a47ebb9247632345
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
code:
  - repo: sample-system
    base_sha: 6358971fc7b27e2198f87a516dad8c5a58cc38a6
    head_sha: 220122879e4301223c5a3f2941a66582c4a6dd10
    diff_sha256: 2b6994cc3810138a726b528bdffc0a72f9f48c73a11f4f4e35a796853595cf6d
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/services/doctorService.ts
      - api/src/types.ts
      - web/src/api/client.ts
      - web/src/api/types.ts
      - web/src/pages/DoctorsPage.tsx
claims:
  - id: C1
    text: "SQL lives in `api/src/repositories`; rules in services; shared helpers in api/src/lib."
    cites: ["context/conventions.md#code-layout"]
  - id: C2
    text: "Lists answer `{ items, page, page_size, total }`, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C3
    text: "Invalid input is 400 with one message per field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C4
    text: "Logs never carry request bodies, contact details or `medical history`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C5
    text: "Appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C6
    text: "Doctor rules live in `DoctorService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C7
    text: "The doctors list screen is `DoctorsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorsPage.tsx:DoctorsPage"]
  - id: C8
    text: "The web API client object is `api`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
  - id: C9
    text: "The web helper for today's date in a time zone is `todayIn`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:todayIn"]
  - id: C10
    text: "The API day-boundary helper is `dayBounds`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:dayBounds"]
  - id: C11
    text: "The API helper for the local calendar day of an instant is `localDay`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localDay"]
  - id: C12
    text: "The design adds the opt-in parameter `with_today` to GET /api/doctors."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/design.md#L181"]
  - id: C13
    text: "The design names the count method `countTodayByDoctor`."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/design.md#L186"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: [S-1, S-2]
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "The design's names with_today and today_count are snake_case, while other doctor fields and filters are camelCase (fullName, departmentId). Keep them as designed or rename to withToday/todayCount?"
    why: "Naming is the only visible style difference in the API response; renaming later would break the web client."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/design.md#L181", "context/conventions.md#lists"]
    options: ["Keep with_today and today_count as designed", "Rename to withToday and todayCount"]
    default: "Keep with_today and today_count as designed"
summary: "GET /api/doctors?with_today=true adds today_count (appointments starting today in the hospital zone, not CANCELLED) per row; DoctorsPage shows it as a Today column linking to the doctor's appointments today."
---
# Change: HMS-6

## Summary
- **AppointmentRepo** (changed): new `countTodayByDoctor(doctorIds, from, to)`. It counts, per doctor id, the appointments with `starts_at` in [from, to) and status other than CANCELLED, grouped by `doctor_id`. All three values are bound parameters, and an empty id list returns early without a query.
- **DoctorService** (changed): `list` parses `with_today` with the existing `Fields.oneOf` (`true`/`false`; anything else is a 400 `ValidationError`). When it is true, the service takes today's UTC bounds from `localDay` + `dayBounds` in the configured hospital time zone. It counts only the ids on the current page and returns each row as a `DoctorListRow` with `today_count`, using 0 when a doctor has no appointments. The list query, filters, search, sort by name and page size are untouched.
- **DoctorListRow** (new, api and web types): `DoctorSummary` plus `today_count: number`. The shared `DoctorSummary`, `DoctorDetail` and `GET /api/doctors/:id` are unchanged.
- **api client** (changed): new `api.doctorsWithToday(params)` adds `with_today=true` and returns `Page<DoctorListRow>`. `api.doctors` is unchanged, so the booking picker sends nothing new.
- **DoctorsPage** (changed): calls `doctorsWithToday` with the same filters and adds a "Today" column after Status. Each count, including 0, is a `Link` to `/appointments?doctorId=<id>&from=<today>&to=<today>`. The query string is built by the client's existing `query()` (URL-encoded), today comes from `todayIn(timeZone)` with the time zone from `useSettings`, and the count renders as text.
- **Not edited:**
  - doctorsRouter already passes `req.query` to `DoctorService.list`, where every other list filter is parsed, so no route change was needed.
  - DashboardService already counts "Appointments today" as `starts_at` in today's bounds and `status <> 'CANCELLED'`, which is the same rule (design Q-02), so it was left unchanged and the tile cannot shift.
  - AppointmentsPage and GET /api/appointments already filter by `doctorId`, `from` and `to` (design Q-01), so the link reuses those keys and neither changed.
  - No migration.

## Changes
- api/src/repositories/appointmentRepo.ts: AppointmentRepo (count today per doctor), adds `countTodayByDoctor`.
- api/src/services/doctorService.ts: DoctorService (list with today counts), adds the `with_today` parse and `today_count`.
- api/src/types.ts: DoctorListRow type (api).
- web/src/api/types.ts: DoctorListRow type (web).
- web/src/api/client.ts: api client, adds `doctorsWithToday`.
- web/src/pages/DoctorsPage.tsx: DoctorsPage, adds the Today column and link.
- Tests: none added. The design names no regression test, and no existing test pins the doctors list; the unit-test step covers REQ-1 to REQ-11.

## Known
- REQ-1: DoctorsPage has a column labelled "Today".
- REQ-2: the count uses `localDay` + `dayBounds` in the configured hospital time zone over `starts_at`.
- REQ-3: the count excludes `CANCELLED` and nothing else.
- REQ-4: the count's predicate is the same as the dashboard's `appointmentsToday` query (today's bounds, `status <> 'CANCELLED'`).
- REQ-5: doctors missing from the count map get `today_count = 0`, shown as 0.
- REQ-6, REQ-7: each count links to `/appointments` with `doctorId`, `from` and `to` set to that doctor and today, keys the appointment list already reads.
- REQ-8, REQ-9, REQ-10: DoctorRepo.list, page size, sort by name, filters and search are unchanged; `with_today` is only an extra field in the same query object.
- REQ-11: the existing columns stay and Today is added after Status.

## Assumed
- S-1: assumed `api.doctorsWithToday` as a separate client call is acceptable in place of overloading `api.doctors`. Basis: the design wants the picker unchanged (design L181), and a separate call guarantees that.
- S-2: assumed the link keys follow the existing `doctorId`/`from`/`to` filters rather than the design's proposed `doctor_id`/`date`. Basis: the design's Q-05 default is "keep the proposed names unless an existing key already means the same", and these keys already mean the same.

## Unknown
- U-1 (non-blocking): snake_case `with_today`/`today_count` vs the camelCase style of other fields → Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): keep `with_today`/`today_count` as designed, or rename to camelCase? Default: keep as designed.

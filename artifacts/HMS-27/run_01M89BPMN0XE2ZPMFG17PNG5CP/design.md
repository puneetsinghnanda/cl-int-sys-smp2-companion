---
schema: design.v1
run_id: run_01M89BPMN0XE2ZPMFG17PNG5CP
step: design
attempt: 2
skill: design@1.9.0
inputs:
  - path: artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/split.md
    sha256: 3f92b6f319ce0bc8d22bd48258fa93ea3a73480096486aaa8646237ad43331b4
  - path: artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/req.md
    sha256: 447d72cf313d2a06ab87a4c709878bfd9f923c28907519db3dd6997a343ec267
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Patient SQL lives in `PatientRepo` in api/src/repositories/patientRepo.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo"]
  - id: C2
    text: "The patient list row type is `ListRow` in api/src/repositories/patientRepo.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:ListRow"]
  - id: C3
    text: "Patient rules live in `PatientService` in api/src/services/patientService.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService"]
  - id: C4
    text: "GET /api/patients is declared in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C5
    text: "The API's patient list item type is `PatientListItem` in api/src/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientListItem"]
  - id: C6
    text: "The web app's patient list item type is `PatientListItem` in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientListItem"]
  - id: C7
    text: "The patient list page component is `PatientsPage` in web/src/pages/PatientsPage.tsx."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage"]
  - id: C8
    text: "The API has a local-calendar helper `localParts` in api/src/lib/time.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localParts"]
  - id: C9
    text: "The API has a helper `zonedToUtc` that turns a local wall-clock time into a UTC instant."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:zonedToUtc"]
  - id: C10
    text: "\"Today\" uses the hospital time zone `HOSPITAL_TZ`, default Asia/Kolkata."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C11
    text: "Instants are stored in `UTC`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C12
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C13
    text: "Web formatting helpers, including `dateTime`, live in web/src/lib/format.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
  - id: C14
    text: "The web patient list request goes through `api` in web/src/api/client.ts, which builds query strings with `query`; the new filter flag is added there."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-08"]
  - id: C15
    text: "Lists answer `{ items, page, page_size, total }`, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C16
    text: "Medical history appears only on the patient's own page, `never in lists`."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C17
    text: "A last visit is the patient's latest appointment with status `COMPLETED`."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/req.md#item:REQ-2"]
  - id: C18
    text: "Invalid input is 400 with one message per field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C19
    text: "The API types already declare `NextAppointment` for the Next appointment column."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:NextAppointment"]
  - id: C20
    text: "Routes parse input, services hold the rules and `repositories hold the SQL`."
    cites: ["context/architecture.md#components"]
  - id: C21
    text: "The GET /api/patients route passes the whole query to the service: `services.patients.list(req.query`."
    cites: ["sample-system:api/src/routes/patients.ts#L14"]
  - id: C22
    text: "A boolean query flag is parsed in the service with `oneOf`, as with_today is in DoctorService."
    cites: ["sample-system:api/src/services/doctorService.ts#L36"]
  - id: C23
    text: "Query validation uses the `Fields` helper in api/src/lib/validate.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/validate.ts:Fields"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [S-1, S-2, S-3, S-4, S-5, S-6, CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, EP-1]
assumed: []
unknown: []
questions: []
resolves:
  - id: F-1
    how: "The service parses and validates not_seen with Fields oneOf, as with_today is parsed, and the route stays unchanged."
components:
  - id: CO-1
    name: PatientRepo list query (and ListRow)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:PatientRepo", "context/generated/module-map.json#sym:sample-system:api/src/repositories/patientRepo.ts:ListRow"]
  - id: CO-2
    name: PatientService list (not_seen parsing, cutoff and last-visit date)
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/patientService.ts:PatientService", "context/generated/module-map.json#sym:sample-system:api/src/lib/validate.ts:Fields", "sample-system:api/src/services/doctorService.ts#L36", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:localParts", "context/generated/module-map.json#sym:sample-system:api/src/lib/time.ts:zonedToUtc", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-06"]
  - id: CO-3
    name: patientsRouter GET /api/patients (passes req.query through)
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/patients.ts:patientsRouter", "sample-system:api/src/routes/patients.ts#L14"]
  - id: CO-4
    name: PatientListItem plus new LastVisit type in api/src/types.ts
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:PatientListItem", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-05"]
  - id: CO-5
    name: PatientListItem plus new LastVisit type in web/src/api/types.ts
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:PatientListItem", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-05"]
  - id: CO-6
    name: api client patient list call (not_seen flag)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-06", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-08"]
  - id: CO-7
    name: PatientsPage (Last visit column and Not seen for a year filter)
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/PatientsPage.tsx:PatientsPage", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-07"]
  - id: CO-8
    name: date-only formatter in web/src/lib/format.ts
    module: module:sample-system:web
    change: new
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-07"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/patients
    change: changed
    fields:
      - {name: "query.not_seen", type: boolean, nullable: false, format: "optional; \"true\" turns the filter on, absent or \"false\" leaves it off, any other value is 400 ValidationError", example: "true", status: known}
      - {name: "items[].last_visit", type: object, nullable: true, format: "{ date, doctor_name }; null when the patient has no COMPLETED appointment (shown as Never)", example: "{\"date\": \"2025-11-25\", \"doctor_name\": \"Dr A\"}", status: known}
      - {name: "items[].last_visit.date", type: string, nullable: false, format: "YYYY-MM-DD, the hospital-local date (Asia/Kolkata) of the latest COMPLETED appointment's starts_at", example: "2025-11-25", status: known}
      - {name: "items[].last_visit.doctor_name", type: string, nullable: false, format: "the name of that appointment's doctor, as stored on the doctor row", example: "Dr A", status: known}
      - {name: "total", type: integer, nullable: false, format: "count of patients matching search, status and not_seen together", example: "21", status: known}
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-05", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-06"]
answers:
  - id: A-05
    kind: question
    about: Q-01
    answer: "Nested object (proposed default)"
    decides: [EP-1, CO-4, CO-5]
  - id: A-06
    kind: question
    about: Q-02
    answer: "not_seen=true (proposed default)"
    decides: [EP-1, CO-2, CO-6]
  - id: A-07
    kind: question
    about: Q-03
    answer: "New date-only helper (proposed default)"
    decides: [CO-7, CO-8]
  - id: A-08
    kind: assumption
    about: C14
    answer: "Yes, confirm"
    decides: [C14]
---
# Design: HMS-27

## Overview
The existing GET /api/patients list (C4) gains a `last_visit` object per row (A-05) and an optional `not_seen=true` filter (A-06). The web patient list (C7) shows the new column and a checkbox filter. The route already passes the whole query to the service (C21), so it stays unchanged. The service (C3) parses `not_seen` and owns the time rules. The repository (C1) adds the SQL (C20).

The last visit is the latest COMPLETED appointment (C17). It is found with a `LEFT JOIN LATERAL` subquery per patient (latest `starts_at` with status COMPLETED, joined to its doctor for the name). The not-seen filter is a `WHERE lv.starts_at < $cutoff` on that join, so never-visited patients (null) drop out automatically (REQ-8). The same WHERE feeds both the page query and the count query, so paging and `total` reflect the filter (S-3 AC3, AC4).

Parsing: the service validates `not_seen` with the `Fields` helper (C23) using `oneOf` over "true" and "false", exactly as DoctorService parses with_today (C22). An absent flag means off, and any other value is a 400 ValidationError (C18). F-1: The service parses and validates not_seen with Fields oneOf, as with_today is parsed, and the route stays unchanged.

Time rules (S-2): instants are UTC (C11) and "today" is hospital-local (C10). The service takes today's local date in HOSPITAL_TZ from the request clock (`localParts`, C8). It subtracts 12 calendar months, keeping the day of the month and clamping to the month's last day when that day does not exist (29 Feb 2028 → 28 Feb 2027). It then converts local midnight of that cutoff date to a UTC instant with `zonedToUtc` (C9). A visit is older than 12 months exactly when its UTC `starts_at` is before that instant, which is the same as its local date being before the cutoff date (S-2 AC2, AC3, AC4). The service also turns each row's `starts_at` into a `YYYY-MM-DD` local date with the same zone, so the page never converts zones (S-1 AC3).

Trade-offs:
- Lateral subquery rather than a stored `last_visit` column: no migration and no new write paths. The cost is a per-patient lookup, which is acceptable at MVP data sizes. An index on appointment (patient_id, status, starts_at) can come later if the list slows down.
- Cutoff computed in the service rather than in SQL `AT TIME ZONE`: it reuses the existing time helpers and keeps the rule unit-testable with a pinned clock. The cost is one extra parameter in the query.
- Clamping to the month's last day: the result stays a valid date. The cost is that, in a leap year, the 29 February cutoff is one day earlier than a roll-forward to 1 March would give.
- Parsing `not_seen` in the service, like with_today: it keeps one validation path for list queries. The cost is that the route signature does not show the flag.
- Toggling the filter on the page resets to page 1, as changing the search or status filter would. This avoids landing on an empty page.

Medical history (S-4): the list query and ListRow (C2) select no medical-history columns, and `last_visit` carries only a date and a doctor name, so lists stay clean (C16).

## Components
- CO-1 PatientRepo list query and ListRow — changed. Adds the lateral join for the latest COMPLETED appointment and its doctor name, plus an optional `lastVisitBefore` cutoff (a UTC instant) applied to both the page query and the count query. It still selects no medical-history columns.
- CO-2 PatientService list — changed. Parses `not_seen` with Fields `oneOf` over "true" and "false" (absent means off; any other value is a 400 ValidationError). It computes the cutoff (local today in HOSPITAL_TZ minus 12 calendar months, local midnight converted to UTC) and maps `starts_at` to `last_visit.date` (`YYYY-MM-DD` local).
- CO-3 patientsRouter GET /api/patients — existing. It already passes `req.query` and the page to the service (C21), so the new flag needs no route change.
- CO-4 PatientListItem in api/src/types.ts — changed. Adds `last_visit: LastVisit | null`; `LastVisit` (`{ date: string; doctor_name: string }`) is a new interface in the same file, next to `NextAppointment` (C19).
- CO-5 PatientListItem in web/src/api/types.ts — changed. It mirrors CO-4.
- CO-6 api client patient list call in web/src/api/client.ts — changed. Sends `not_seen=true` through `query` when the filter is on (C14).
- CO-7 PatientsPage — changed. Adds a "Last visit" header directly after "Next appointment". Each cell shows the CO-8 date and the doctor name, or "Never" when `last_visit` is null. A "Not seen for a year" checkbox sits next to the status filter, and toggling it resets to page 1.
- CO-8 date-only formatter in web/src/lib/format.ts — new. Formats a `YYYY-MM-DD` string as a readable date (for example 10 Mar 2026) from its parts, with no Date time-zone conversion. `dateTime` is not used because it prints a time (A-07).
- EP-1 GET /api/patients — changed. Fields:
  - `query.not_seen`: boolean, optional; "true" filters, while absent or "false" does not, and any other value is a 400; example `true`.
  - `items[].last_visit`: object or null (null means Never); example `{"date": "2025-11-25", "doctor_name": "Dr A"}`.
  - `items[].last_visit.date`: string `YYYY-MM-DD`, the hospital-local date (Asia/Kolkata) of the latest COMPLETED starts_at; example `2025-11-25`.
  - `items[].last_visit.doctor_name`: string, that appointment's doctor; example `Dr A`.
  - `total`: integer, counting the patients that match search, status and not_seen together; example `21`.

Story coverage:
- S-1 is served by CO-1, CO-2, CO-4, CO-5, CO-7, CO-8 and EP-1.
- S-2 is served by CO-2, CO-6, CO-7 and EP-1.
- S-3 is served by CO-1, CO-2 and CO-7.
- S-4 is served by CO-1 and CO-7.
- S-5 is served by CO-1 (the COMPLETED-only join).
- S-6 is served by CO-1, CO-2 and CO-3: with `not_seen` absent, the query is the existing one plus the left join, which filters nothing.

## Known
- S-1 to S-6: every story is served by the components above.
- CO-1, CO-2: the repository and service are proven by module-map symbols (C1, C2, C3). Their parsing follows the existing with_today pattern (C22, C23).
- CO-3: the existing route already forwards the query (C4, C21).
- CO-4, CO-5: the type locations are proven by module-map symbols (C5, C6); the shape is decided by A-05.
- CO-6: the client location is confirmed by A-08 (C14).
- CO-7: the page is proven by its module-map symbol (C7).
- CO-8: new, in the existing format.ts (C13); decided by A-07.
- EP-1: an existing endpoint (C4), changed. A-05 decides its fields and A-06 decides its query flag.

## Assumed
None. C14 was confirmed in A-08; implement still names the exact client method when it edits the file.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-05 (Q-01): the shape of the last visit in GET /api/patients → "Nested object (proposed default)". Decides EP-1, CO-4 and CO-5. No mismatch with the inputs.
- A-06 (Q-02): the query parameter for the filter → "not_seen=true (proposed default)". Decides EP-1, CO-2 and CO-6. No mismatch.
- A-07 (Q-03): how the web page displays the date → "New date-only helper (proposed default)". Decides CO-7 and CO-8. No mismatch.
- A-08 (C14 assumption): the patient list request goes through `api` in web/src/api/client.ts → "Yes, confirm". Decides C14. No mismatch.

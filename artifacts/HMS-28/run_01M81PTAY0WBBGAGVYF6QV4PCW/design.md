---
schema: design.v1
run_id: run_01M81PTAY0WBBGAGVYF6QV4PCW
step: design
attempt: 2
skill: design@1.9.0
inputs:
  - path: artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/split.md
    sha256: 2a67d6bac48a0f331a38dd9686cc75735f7f339ab7c2b0be8ed789c19e99fa98
  - path: artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/req.md
    sha256: 35b81b83af2e9a6dfa955d70288cccd1ced2f5904e2cd3b03a6218dd1e9703c7
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
resolves:
  - id: F-1
    how: "CO-3 now reads weekday with Fields.oneOf over the strings 1 to 7, as the status filter does, then converts it with Number."
  - id: F-3
    how: "The weekday parameter now follows from the stored coding (C14) and the existing filter keys; A-05 only confirms that default."
claims:
  - id: C1
    text: "The doctor list filter type `DoctorFilter` is declared in the doctor repository."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorFilter"]
  - id: C2
    text: "The SQL for doctors lives in the `DoctorRepo` class."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: C3
    text: "The doctor list rules live in the `DoctorService` class."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService"]
  - id: C4
    text: "The API list row type `DoctorListRow` is declared in the API types."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:DoctorListRow"]
  - id: C5
    text: "The web list row type `DoctorListRow` is declared in the web API types."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:DoctorListRow"]
  - id: C6
    text: "The doctor list screen is the `DoctorsPage` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorsPage.tsx:DoctorsPage"]
  - id: C7
    text: "The web format helpers already hold the full weekday names in `WEEKDAYS`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:WEEKDAYS"]
  - id: C8
    text: "The doctor list is served by `api/src/routes/doctors.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors"]
  - id: C9
    text: "The doctor list page asks for its rows through `doctorsWithToday`, which passes its params through to the list endpoint."
    cites: ["sample-system:web/src/api/client.ts#L60"]
  - id: C10
    text: "The repository counts the total with the same WHERE clause as the page query (`count(*)`)."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L92"]
  - id: C11
    text: "The doctor list is ordered by `lower(d.full_name)`."
    cites: ["sample-system:api/src/repositories/doctorRepo.ts#L94"]
  - id: C12
    text: "The service adds per-doctor extras only when `withToday` is set."
    cites: ["sample-system:api/src/services/doctorService.ts#L54"]
  - id: C13
    text: "The doctor list keeps its filters in the page address with `useSearchParams`."
    cites: ["sample-system:web/src/pages/DoctorsPage.tsx#L16"]
  - id: C14
    text: "Availability blocks store the weekday as `1 = Monday .. 7 = Sunday`."
    cites: ["sample-system:db/migrations/20261016T090100__create_doctors.sql#L21"]
  - id: C15
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C16
    text: "Invalid input is answered 400 with `ValidationError`."
    cites: ["context/conventions.md#errors"]
  - id: C17
    text: "The status filter reads a string query value with `oneOf`, which suits values that arrive as strings."
    cites: ["sample-system:api/src/services/doctorService.ts#L42"]
  - id: C18
    text: "Fields.int rejects anything that is not a number (`must be a whole number`), so it cannot read a query string value."
    cites: ["sample-system:api/src/lib/validate.ts#L56"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, CO-8, EP-1]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: "DoctorRepo.list and DoctorFilter: weekday filter"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorFilter", "context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo", "sample-system:api/src/repositories/doctorRepo.ts#L92"]
  - id: CO-2
    name: "DoctorRepo.weekdaysByDoctor"
    module: module:sample-system:api
    change: new
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/doctorRepo.ts:DoctorRepo"]
  - id: CO-3
    name: "DoctorService.list: weekday parsing and weekdays on rows"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/doctorService.ts:DoctorService", "sample-system:api/src/services/doctorService.ts#L42", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-04", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-05"]
  - id: CO-4
    name: "API type DoctorListRow: adds weekdays: number[]"
    module: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:DoctorListRow", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-03", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-04"]
  - id: CO-5
    name: "Web type DoctorListRow: adds weekdays: number[]"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:DoctorListRow", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-03"]
  - id: CO-6
    name: "DoctorsPage: Works on column and filter"
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DoctorsPage.tsx:DoctorsPage", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-01", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-02", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-05"]
  - id: CO-7
    name: "WEEKDAYS in web/src/lib/format.ts: full names for the filter options; first three letters for the column"
    module: module:sample-system:web
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:WEEKDAYS"]
  - id: CO-8
    name: "api.doctorsWithToday and doctorsRouter (pass-through)"
    module: module:sample-system:web
    change: existing
    cites: ["sample-system:web/src/api/client.ts#L60", "context/generated/api-inventory.json#http:GET:/api/doctors"]
endpoints:
  - id: EP-1
    method: GET
    path: /api/doctors
    change: changed
    fields:
      - name: weekday
        type: "integer (query parameter, sent as a string)"
        nullable: true
        format: "one of \"1\" to \"7\", 1 = Monday .. 7 = Sunday (availability block coding); absent or empty = Any day; any other value is 400 ValidationError on field weekday"
        example: 6
        status: known
      - name: "items[].weekdays"
        type: "integer[]"
        nullable: false
        format: "distinct weekdays of the doctor's availability blocks, ascending, 1 = Monday .. 7 = Sunday; [] when the doctor has no blocks; present only when with_today=true"
        example: "[1, 3, 6, 7]"
        status: known
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors", "sample-system:db/migrations/20261016T090100__create_doctors.sql#L21", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-03", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-04", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-05"]
answers:
  - id: A-03
    kind: question
    about: Q-01
    answer: "weekdays: int[] 1-7 (proposed default)"
    decides: [EP-1, CO-4, CO-5]
  - id: A-04
    kind: question
    about: Q-02
    answer: "Only with with_today=true (proposed default)"
    decides: [EP-1, CO-3, CO-4]
  - id: A-05
    kind: question
    about: Q-03
    answer: "weekday=1..7 (proposed default)"
    decides: [EP-1, CO-3, CO-6]
---
# Design: HMS-28

## Overview
The change extends the existing doctor list end to end, with no new endpoint, table or migration.
In the API, `GET /api/doctors` (EP-1, C8) gets an optional `weekday` filter and, when the list page
asks with `with_today=true`, a `weekdays` array on each row. In the web app, `DoctorsPage` (C6) gets
a Works on column and a Works on filter.

How the stories are served:
- S-1 (Works on column): CO-2 builds each row's weekdays, CO-3 attaches them, CO-4 and CO-5 type them, and CO-6 renders them with CO-7 short names (Mon to Sun in number order, so Sunday is last; [] shows "No availability").
- S-2 (filter to one weekday): CO-6 offers Any day and Monday to Sunday from CO-7 and sends `weekday`; CO-3 validates it; CO-1 filters.
- S-3 (combine with search and filters): CO-1 adds the weekday condition with AND to the existing search, department, specialization and status conditions.
- S-4 (page address): CO-6 keeps `weekday` in the search params like the other filters (C13).
- S-5 (rest unchanged): no existing column, filter or field changes; `today_count` and the other callers of the list (without `with_today`) are untouched (C12).
- S-6 (paging and count): CO-1 puts the weekday condition in the shared WHERE clause, so the `count(*)` total (C10) and the name order (C11) reflect the filter, 20 per page as before.

Trade-offs:
- The weekday filter is `EXISTS (SELECT 1 FROM availability_block ab WHERE ab.doctor_id = d.id AND ab.weekday = $n)`, not a JOIN. This avoids listing or counting a doctor twice when they have two blocks on that day (S-6 AC3), at the cost of a correlated subquery.
- Weekdays come from a separate query per page (CO-2, like the existing today counts), not from the shared SELECT. This leaves `DoctorSummary` and the doctor detail read unchanged, at the cost of one extra query per page.
- `weekdays` is returned only with `with_today=true` (A-04). This keeps other list callers unchanged, but couples the field to that flag.
- The `weekday` parameter uses the stored coding 1 to 7 (C14), named like the existing filter keys. This is the default the requirements left to design, not an open choice; A-05 only confirms it. One coding runs end to end; the trade-off is that a copied link reads `weekday=6`, not a day name.
- `weekday` is read with `oneOf` over the strings "1" to "7", like the status filter (C17), not with Fields.int, which rejects query strings (C18). An unknown value gets the same ValidationError as other filters; the trade-off is a fixed list of seven strings in the service.

Resolved review findings:
- F-1: CO-3 now reads weekday with Fields.oneOf over the strings 1 to 7, as the status filter does, then converts it with Number.
- F-3: The weekday parameter now follows from the stored coding (C14) and the existing filter keys; A-05 only confirms that default.

## Components
- CO-1 DoctorRepo.list and DoctorFilter (changed, api): adds `weekday?: number` to DoctorFilter (C1). When set, the EXISTS condition on availability_block goes into the WHERE clause shared by the count and page queries (C2, C10).
- CO-2 DoctorRepo.weekdaysByDoctor(ids) (new, api): a new method returning, per doctor id, the distinct weekdays of its availability blocks in ascending order. It runs one query, `SELECT doctor_id, array_agg(DISTINCT weekday ORDER BY weekday) FROM availability_block WHERE doctor_id = ANY($1) GROUP BY doctor_id`, and returns a Map. Doctors with no blocks are absent from the Map (the service uses []). It lives in DoctorRepo (C2).
- CO-3 DoctorService.list (changed, api): reads `weekday` with `f.oneOf('weekday', ['1', '2', '3', '4', '5', '6', '7'])`, like status (C17), and converts it with `Number` into the filter. An absent or empty value means Any day; any other value fails as a 400 ValidationError through `f.done()` (C16). Fields.int is not used because query values are strings (C18). In the `with_today` branch, rows also get `weekdays` from CO-2, with [] by default (C3, C12; A-04, A-05).
- CO-4 API type DoctorListRow (changed, api): adds `weekdays: number[]` (C4; A-03).
- CO-5 Web type DoctorListRow (changed, web): adds `weekdays: number[]` (C5; A-03).
- CO-6 DoctorsPage (changed, web): reads `weekday` from the search params next to q, departmentId, specializationId and status (C13), and sets it through the existing `set` helper, which also resets the page. It adds a select labelled "Works on" with "Any day" (value empty) and Monday to Sunday (values 1 to 7) from WEEKDAYS. It adds a Works on column after Status and before Today (A-01), showing the short names joined by ", ", or "No availability" when weekdays is empty. Filter options use full names (A-02).
- CO-7 WEEKDAYS (existing, web): full names Monday to Sunday (C7). The short name is the first three letters.
- CO-8 api.doctorsWithToday and doctorsRouter (existing): the client in web/src/api/client.ts passes the filters object through as query params (C9, C15). The router in api/src/routes/doctors.ts passes `req.query` to the service (C8). Neither needs a change.
- EP-1 GET /api/doctors (changed):
  - `weekday`: integer query parameter, sent as a string; nullable (absent or empty = Any day). One of "1" to "7", 1 = Monday .. 7 = Sunday; any other value is 400 ValidationError on field weekday. Example 6. Known (C14, A-05).
  - `items[].weekdays`: integer[], not nullable. Distinct, ascending, 1 = Monday .. 7 = Sunday; [] when the doctor has no blocks; present only when with_today=true. Example [1, 3, 6, 7]. Known (A-03, A-04).

## Known
- CO-1, CO-2: DoctorFilter and DoctorRepo exist (C1, C2); the count and page queries share the WHERE clause (C10).
- CO-3: DoctorService exists (C3), adds extras only with `withToday` (C12), and reads string filters with `oneOf` (C17); decided by A-04 and A-05.
- CO-4, CO-5: both DoctorListRow types exist (C4, C5); field decided by A-03.
- CO-6: DoctorsPage exists (C6) and keeps filters in the address (C13); position from A-01, labels from A-02, parameter from C14 (confirmed by A-05).
- CO-7: WEEKDAYS exists (C7).
- CO-8: the client pass-through (C9) and the list route (C8) exist.
- EP-1: the endpoint exists (C8); its fields come from the stored coding (C14) and A-03, A-04 and A-05.

## Assumed
None

## Unknown
None

## Open questions
No questions are open.

Answered in the IDE:
- A-03 (Q-01): Which field name and format for each row's weekdays? Answer: "weekdays: int[] 1-7 (proposed default)". Decided EP-1, CO-4, CO-5. No mismatch with the inputs.
- A-04 (Q-02): When should GET /api/doctors include weekdays? Answer: "Only with with_today=true (proposed default)". Decided EP-1, CO-3, CO-4. No mismatch with the inputs.
- A-05 (Q-03): Which query parameter carries the chosen weekday? Answer: "weekday=1..7 (proposed default)". Confirms the default from the stored coding (C14). Decided EP-1, CO-3, CO-6. No mismatch with the inputs.

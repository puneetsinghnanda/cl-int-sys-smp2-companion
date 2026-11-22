---
schema: design.v1
run_id: run_01M81VYRN04SNXNMA559N6SS01
step: design
attempt: 1
skill: design@1.9.0
inputs:
  - path: artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/split.md
    sha256: 6b281327d9f889c19c469ea35fc7f1c182a3fdd5f01f45c22c6444f35aeade95
  - path: artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/req.md
    sha256: 52b4c52bad178b06dd5fc4346654ab569aa7276ac8daae7f6119485236c9cd18
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
    text: "The appointment list endpoint is served by `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C2
    text: "Appointment status is SCHEDULED, COMPLETED, CANCELLED or `NO_SHOW`."
    cites: ["context/data-model.md#appointments"]
  - id: C3
    text: "`AppointmentFilter` holds the doctor, patient, date-range and status filters, so the counts query reuses it with status removed."
    cites:
      - "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentFilter"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-04"
  - id: C4
    text: "`AppointmentsPage` keeps its filters in the URL query string, so each count is a link with `status` set and `page` reset to 1."
    cites:
      - "context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-05"
  - id: C5
    text: "Lists answer `{ items, page, page_size, total }`, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C6
    text: "The appointment SQL lives in `AppointmentRepo`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"]
  - id: C7
    text: "Appointment rules live in `AppointmentService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"]
  - id: C8
    text: "The API status type `AppointmentStatus` is declared in `api/src/types.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus"]
  - id: C9
    text: "The web app's list type `Page` is declared in `web/src/api/types.ts`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Page"]
  - id: C10
    text: "The web app talks to the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C11
    text: "Local days use the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C12
    text: "The route `/api/appointments/:id` already exists, so a sibling path such as status-counts would need careful route order."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments/:id"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, EP-1]
assumed: []
unknown: []
questions: []
components:
  - id: CO-1
    name: AppointmentRepo.countByStatus
    module: module:sample-system:api
    change: changed
    cites:
      - "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentRepo"
      - "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentFilter"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-03"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-04"
  - id: CO-2
    name: AppointmentService list adds status_counts
    module: module:sample-system:api
    change: changed
    cites:
      - "context/generated/module-map.json#sym:sample-system:api/src/services/appointmentService.ts:AppointmentService"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-03"
  - id: CO-3
    name: AppointmentStatusCounts type in api/src/types.ts
    module: module:sample-system:api
    change: new
    cites:
      - "context/generated/module-map.json#sym:sample-system:api/src/types.ts:AppointmentStatus"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-03"
  - id: CO-4
    name: appointmentsRouter GET /api/appointments
    module: module:sample-system:api
    change: existing
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/appointments.ts:appointmentsRouter"]
  - id: CO-5
    name: AppointmentStatusCounts type in web/src/api/types.ts
    module: module:sample-system:web
    change: new
    cites:
      - "context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Page"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-03"
  - id: CO-6
    name: web api client appointments list return type
    module: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/client.ts:api"]
  - id: CO-7
    name: AppointmentsPage status counts line
    module: module:sample-system:web
    change: changed
    cites:
      - "context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-05"
endpoints:
  - id: EP-1
    method: GET
    path: /api/appointments
    change: changed
    fields:
      - {name: status_counts, type: object, nullable: false, format: "object with exactly the keys SCHEDULED, COMPLETED, CANCELLED, NO_SHOW", example: "{\"SCHEDULED\": 4, \"COMPLETED\": 25, \"CANCELLED\": 3, \"NO_SHOW\": 2}", status: known}
      - {name: status_counts.SCHEDULED, type: integer, nullable: false, format: "count >= 0 over all pages, ignoring the status query param", example: "4", status: known}
      - {name: status_counts.COMPLETED, type: integer, nullable: false, format: "count >= 0 over all pages, ignoring the status query param", example: "25", status: known}
      - {name: status_counts.CANCELLED, type: integer, nullable: false, format: "count >= 0 over all pages, ignoring the status query param", example: "3", status: known}
      - {name: status_counts.NO_SHOW, type: integer, nullable: false, format: "count >= 0 over all pages, ignoring the status query param", example: "2", status: known}
    cites:
      - "context/generated/api-inventory.json#http:GET:/api/appointments"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-03"
answers:
  - id: A-03
    kind: question
    about: Q-01
    answer: "status_counts on list"
    decides: [EP-1, CO-1, CO-2, CO-3, CO-5]
    mismatch:
      text: "Conventions say lists answer exactly { items, page, page_size, total }; the user decided GET /api/appointments also returns status_counts."
      cites: ["context/conventions.md#lists"]
  - id: A-04
    kind: assumption
    about: C3
    answer: "Yes, confirm"
    decides: [C3, CO-1]
  - id: A-05
    kind: assumption
    about: C4
    answer: "Yes, confirm"
    decides: [C4, CO-7]
---
# Design: HMS-30

## Overview
The existing `GET /api/appointments` list (EP-1) gains one field, `status_counts`, holding one integer
per status (SCHEDULED, COMPLETED, CANCELLED, NO_SHOW). The service (CO-2) builds it with a new repository
query (CO-1) that runs the same WHERE clause as the list, from `AppointmentFilter` with the status
filter removed, and groups by status. It ignores page and page size, so the counts cover every
matching appointment (S-2) and all four statuses always appear (S-1). Statuses with no rows are filled
with 0 in the service. The date-range bounds stay exactly as the list computes them in
`HOSPITAL_TZ` today, because both queries share one filter builder (S-2 AC2). The web page (CO-7) shows
"Scheduled n · Completed n · Cancelled n · No-show n" next to the existing total. Each count is a link
to the same URL with `status` set to that status and `page` set to 1 (S-3). The table, order, paging
and filters are not touched (S-4).

Trade-offs:
- Counts are part of the list response rather than a new endpoint. That means one request and no way for the two to drift, at the cost of breaking the plain `{ items, page, page_size, total }` list shape (A-03). A sibling `/api/appointments/status-counts` path would also need to be mounted before the existing `:id` route (C12).
- One extra GROUP BY query runs on every list call. That is cheap next to the list query on the same indexes, and it avoids a second HTTP round trip.
- Counts sit inline in AppointmentsPage rather than in a new shared component. Only one page uses them; a component can be extracted later if another page needs them.
- The total under any Status filter stays as it is today, the filtered total. Under "Any status" the four counts add up to it because both come from the same filter (REQ-8).

## Components
- CO-1 `AppointmentRepo.countByStatus(filter)` (changed, api/src/repositories/appointmentRepo.ts): `SELECT status, count(*) … GROUP BY status` with the list's WHERE clause minus status.
- CO-2 `AppointmentService` list (changed, api/src/services/appointmentService.ts): calls the list and CO-1 with the status filter removed, fills missing statuses with 0, returns `{ items, page, page_size, total, status_counts }`.
- CO-3 `AppointmentStatusCounts` type (new, in api/src/types.ts next to `AppointmentStatus`): `Record<AppointmentStatus, number>`.
- CO-4 `appointmentsRouter` `GET /api/appointments` (existing): passes the service result through; query parsing unchanged.
- CO-5 `AppointmentStatusCounts` type (new, in web/src/api/types.ts next to `Page`): the same shape, plus `status_counts` on the appointments list response type.
- CO-6 web `api` client (changed, web/src/api/client.ts): the appointments list call returns the response type with `status_counts`.
- CO-7 `AppointmentsPage` (changed, web/src/pages/AppointmentsPage.tsx): renders the four counts in the order Scheduled, Completed, Cancelled, No-show, joined by " · ", next to the total. Each one links to the current query string with `status=<STATUS>` and `page=1`.
- EP-1 `GET /api/appointments` (changed). Fields:
  - `status_counts`: object, not null, exactly the keys SCHEDULED, COMPLETED, CANCELLED, NO_SHOW, e.g. `{"SCHEDULED": 4, "COMPLETED": 25, "CANCELLED": 3, "NO_SHOW": 2}`. Known (A-03).
  - `status_counts.SCHEDULED`, `.COMPLETED`, `.CANCELLED`, `.NO_SHOW`: integer, not null, ≥ 0, counted over all pages and ignoring the `status` query param, e.g. 4 / 25 / 3 / 2. Known (A-03).

## Known
- CO-1: the repository and its `AppointmentFilter` are module-map facts; reusing the filter was confirmed (A-04).
- CO-2: `AppointmentService` is a module-map fact; the response shape was decided by A-03.
- CO-3: a new type in api/src/types.ts, a file proven by the `AppointmentStatus` fact; the shape was decided by A-03.
- CO-4: the route exists in the api inventory and is unchanged.
- CO-5: a new type in web/src/api/types.ts, a file proven by the `Page` fact; the shape was decided by A-03.
- CO-6: the `api` client is a module-map fact, and all web calls go through it (C10).
- CO-7: `AppointmentsPage` is a module-map fact; URL-held filters were confirmed (A-05).
- EP-1: `GET /api/appointments` is in the api inventory; its new field was decided by A-03.

## Assumed
None

## Unknown
None

## Open questions
No open questions.

Answered in the IDE:
- A-03 (Q-01): how should the API return the per-status counts? Answer: "status_counts on list". Decides EP-1, CO-1, CO-2, CO-3, CO-5. Mismatch: the conventions say lists answer exactly `{ items, page, page_size, total }`; the user decided `GET /api/appointments` also returns `status_counts`.
- A-04 (C3): does `AppointmentFilter` hold the doctor, patient, date-range and status filters, so it can be reused without status? Answer: "Yes, confirm". Decides C3, CO-1.
- A-05 (C4): does `AppointmentsPage` keep its filters in the URL, so each count can link with status set and page 1? Answer: "Yes, confirm". Decides C4, CO-7.

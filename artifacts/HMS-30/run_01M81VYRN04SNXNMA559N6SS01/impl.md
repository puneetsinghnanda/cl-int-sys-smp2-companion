---
schema: change.v1
run_id: run_01M81VYRN04SNXNMA559N6SS01
step: impl
attempt: 1
skill: implement@1.3.0
inputs:
  - path: artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/design.md
    sha256: 780fcf8b1905e1bf5475a047c4e2e5cd45a0839d20cc21711c73e9c21fe49b2f
  - path: artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/split.md
    sha256: 6b281327d9f889c19c469ea35fc7f1c182a3fdd5f01f45c22c6444f35aeade95
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: e4585b0e3af6a59148b401829aea9db4f66237aa
    diff_sha256: 9360cbd37caee980ead125af37b11b22ba4115f233d50954d55cb97d62067b8f
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/services/appointmentService.ts
      - api/src/types.ts
      - web/src/api/client.ts
      - web/src/api/types.ts
      - web/src/pages/AppointmentsPage.tsx
claims:
  - id: C1
    text: "`AppointmentFilter` holds doctorId, patientId, status, from and to."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L40"]
  - id: C2
    text: "The list builds its WHERE clause from the filter and counts the `total` with it."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L74"]
  - id: C3
    text: "The service turns the from and to days into hospital-local bounds with `dayBounds`."
    cites: ["sample-system:api/src/services/appointmentService.ts#L6"]
  - id: C4
    text: "The route passes the service result straight to `res.json`."
    cites: ["sample-system:api/src/routes/appointments.ts#L14"]
  - id: C5
    text: "`APPOINTMENT_STATUSES` lists SCHEDULED, COMPLETED, CANCELLED, NO_SHOW in that order."
    cites: ["sample-system:api/src/types.ts#L5"]
  - id: C6
    text: "`STATUS_LABEL` maps the statuses to Scheduled, Completed, Cancelled, No-show in that order."
    cites: ["sample-system:web/src/lib/format.ts#L36"]
  - id: C7
    text: "AppointmentsPage reads its filters from the URL with `useSearchParams`."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L12"]
  - id: C8
    text: "The list response is decided as `status_counts on list`."
    cites: ["runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-03"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7, EP-1]
assumed: []
unknown: []
questions: []
summary: "GET /api/appointments now also returns status_counts (one integer per status, over all pages, ignoring the status filter), and the appointments page shows the four counts next to the total, each linking to the list filtered by that status on page 1."
---
# Change: HMS-30

## Summary
- CO-1: `AppointmentRepo` moves the list's WHERE builder into a shared `listWhere` helper and adds `countByStatus`, which runs `SELECT a.status, count(*) … GROUP BY a.status` with the same doctor, patient and date-range conditions but no status condition, page or limit. Because both queries use one builder, the counts can't drift from the list (REQ-4, REQ-8).
- CO-2 / EP-1: `AppointmentService.list` reuses the same hospital-local `dayBounds` range for both queries (S-2 AC2), fills statuses with no rows with 0, and returns `{ items, page, page_size, total, status_counts }` (A-03).
- CO-3 / CO-5: a new `AppointmentStatusCounts = Record<AppointmentStatus, number>` type, in api/src/types.ts and its web mirror web/src/api/types.ts.
- CO-6: the web `api.appointments` call now types its response with `status_counts`.
- CO-7: `AppointmentsPage` renders "N appointment(s) · Scheduled n · Completed n · Cancelled n · No-show n", in `STATUS_LABEL` order. Each count is a `Link` to the current query string with `status` set and `page` removed, which means page 1 (REQ-7, A-05). Table, ordering, `Pagination` and the filter form are untouched (REQ-9 to REQ-11).
- CO-4: the router is unchanged; it already passes the service result through.

## Changes
- api/src/repositories/appointmentRepo.ts: shared `listWhere` helper and new `countByStatus` (CO-1).
- api/src/services/appointmentService.ts: `list` adds `status_counts` (CO-2, EP-1).
- api/src/types.ts: new `AppointmentStatusCounts` type (CO-3).
- web/src/api/types.ts: new `AppointmentStatusCounts` type (CO-5).
- web/src/api/client.ts: `appointments` response type includes `status_counts` (CO-6).
- web/src/pages/AppointmentsPage.tsx: counts line with per-status links (CO-7).
- Tests: none added. The design asks for no regression test; broad tests belong to the unit-test step.

## Known
- CO-1: the filter fields and list WHERE clause were read in the product code (C1, C2).
- CO-2: the date bounds come from `dayBounds` as before (C3); the response shape was decided by A-03 (C8).
- CO-3: status values are taken from `APPOINTMENT_STATUSES` (C5).
- CO-4: the route passes the result through unchanged (C4).
- CO-5: mirrors CO-3 in the web types file.
- CO-6: the client is the only path from web to API.
- CO-7: labels and order come from `STATUS_LABEL` (C6); filters live in the URL (C7).
- EP-1: `GET /api/appointments` gains `status_counts` as the design specifies.

## Assumed
None

## Unknown
None

## Open questions
None

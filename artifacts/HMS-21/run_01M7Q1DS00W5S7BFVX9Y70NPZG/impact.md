---
schema: impact.v1
run_id: run_01M7Q1DS00W5S7BFVX9Y70NPZG
step: impact
attempt: 1
skill: impact-analysis@1.5.0
inputs:
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/req.md
    sha256: c5c9d980373a9426a9af4c3b4d72cca66f0eb9f6023b195e786aa8d084ba20f0
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
  - path: context/generated/db-schema.json
    sha256: e2a9d2990f6618ea5d55994a16c6264b0186b851cc3216014b812c4a4b98908a
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The dashboard route is declared in `api/src/routes/dashboard.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C2
    text: "Dashboard counts are computed in `DashboardService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C3
    text: "The dashboard's period choices are declared as `PERIODS` in the dashboard service."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:PERIODS"]
  - id: C4
    text: "The dashboard response shape is the `Dashboard` type in the API service."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:Dashboard"]
  - id: C5
    text: "The web app mirrors the dashboard response as `Dashboard` in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Dashboard"]
  - id: C6
    text: "The dashboard tiles are rendered by `DashboardPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]
  - id: C7
    text: "The appointment list route is declared in `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C8
    text: "The appointment list's filters are the `AppointmentFilter` interface in the appointment repository."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentFilter"]
  - id: C9
    text: "The appointment list page is `AppointmentsPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage"]
  - id: C10
    text: "Appointment list rows name the patient by `p.full_name AS patient_name`."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L21"]
  - id: C11
    text: "Appointment instants are stored in `UTC`; today uses the hospital time zone."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C12
    text: "The appointment table has a non-null `starts_at` column."
    cites: ["context/generated/db-schema.json#column:appointment.starts_at"]
  - id: C13
    text: "The appointment table has a non-null `status` column."
    cites: ["context/generated/db-schema.json#column:appointment.status"]
  - id: C14
    text: "Appointment status is SCHEDULED, COMPLETED, CANCELLED or `NO_SHOW`."
    cites: ["context/data-model.md#appointments"]
  - id: C15
    text: "The web app calls the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C16
    text: "`No background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
  - id: C17
    text: "The API dashboard test asserts the whole totals object with `expect(res.body.totals).toEqual({`."
    cites: ["sample-system:api/test/dashboard.test.ts#L32"]
  - id: C18
    text: "The web dashboard test builds a typed totals fixture starting `totals: { patients: 200`."
    cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L10"]
  - id: C19
    text: "The list opened from the tile filters to appointments that start before the server's now (`New filter, server now`)."
    cites: ["runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
questions: []
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentFilter", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/AppointmentsPage.tsx:AppointmentsPage", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#column:appointment.starts_at"]
apis:
  - id: http:GET:/api/dashboard
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard", "sample-system:api/test/dashboard.test.ts#L32"]
  - id: http:GET:/api/appointments
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/module-map.json#sym:sample-system:api/src/repositories/appointmentRepo.ts:AppointmentFilter", "runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01"]
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/generated/db-schema.json#column:appointment.starts_at", "context/generated/db-schema.json#column:appointment.status"]
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "New filter, server now (proposed default)"
    decides: [http:GET:/api/appointments, module:sample-system:api, module:sample-system:web, C19]
---
# Impact analysis: HMS-21

## Summary
The change adds an Awaiting outcome count to the dashboard API and tile. It also adds an optional "started before now" filter to the appointment list API and page, so the tile can link to exactly the counted appointments (A-01). The count reads only `starts_at` and `status` on the appointment table, which already exist (C12, C13), so no migration is expected. The main risk is the boundary at now: the count and the list must both compare instants with the server's current time, never local dates. Two existing tests read the full dashboard totals shape and must change with it (C17, C18).

## Impacted modules
- module:sample-system:api — changed. `DashboardService` gains the new count, computed without the `PERIODS` window (C2, C3). The appointment list's `AppointmentFilter` gains an optional filter for start before the server's now (C8, A-01). The API dashboard test asserts the whole totals object with `toEqual` (C17), so it must add the new field.
- module:sample-system:web — changed. `DashboardPage` adds the Awaiting outcome tile and its link (C6), the web `Dashboard` type gains the count (C5), and `AppointmentsPage` must pass the new filter from its URL to the API (C9, A-01). The web dashboard test's typed totals fixture (C18) stops type-checking once the type gains a required field, so it must add the count. Any web assertion on the set of tiles must also expect the new tile.
- module:sample-system:db — existing. `starts_at` and `status` already exist (C12, C13), so no migration is needed.
- http:GET:/api/dashboard — changed. Its totals gain the Awaiting outcome count, and the existing totals and the next upcoming list keep their values (REQ-10, REQ-11). Its shape is the `Dashboard` type (C4).
- http:GET:/api/appointments — changed. It gains an optional filter that keeps appointments whose start is before the server's now (A-01). Without the filter, its behaviour is unchanged.
- table:appointment — existing. It is read by `status` and `starts_at` and is not changed.

Indirect effects:
- Existing callers of `GET /api/appointments` go through `web/src/api/client.ts` (C15). They do not send the new optional filter, so their results are unchanged. The facts hold no call edges, so this artifact does not list every caller.
- The list's rows name the patient by `p.full_name AS patient_name` (C10). REQ-12 and REQ-13 (no medical history or contact details on the linked list) hold as long as the new filter adds no columns.
- Instants are stored in `UTC` (C11). The count and the new filter must compare `starts_at` with the server's now, not with local dates, or the list will drift from the count around local midnight.
- There are `No background jobs` (C16), so no job reads the changed data.

## Known
- REQ-1, REQ-9, REQ-10, REQ-11: the tile and the other totals are in `DashboardPage` (C6) and `DashboardService` (C2). The existing totals keep their values, and only the tests that read the full shape change (C17, C18).
- REQ-2, REQ-3, REQ-4, REQ-5, REQ-6: the count reads `status` (values in C14) and `starts_at` (C12, C13) against the server's now.
- REQ-7: the count does not use the dashboard's `PERIODS` window (C3).
- REQ-8: the tile links to the appointment list with the new filter (C19, A-01), served from `api/src/routes/appointments.ts` (C7).
- REQ-12, REQ-13: the dashboard returns counts, and the list rows carry the patient name only (C10).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (runs/run_01M7Q1DS00W5S7BFVX9Y70NPZG/ide-answers.md#a-01): how should the appointment list express "started before now"? Answer: "New filter, server now (proposed default)", by the run owner. It decides http:GET:/api/appointments, module:sample-system:api, module:sample-system:web and C19. No mismatch with the facts.

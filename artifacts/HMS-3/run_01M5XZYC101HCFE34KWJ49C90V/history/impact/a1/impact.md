---
schema: impact.v1
run_id: run_01M5XZYC101HCFE34KWJ49C90V
step: impact
attempt: 1
skill: impact-analysis@1.1.1
inputs:
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/req.md
    sha256: 50be375fd64aa765d6499709646b264f9c247964fabbc6dc3fdcf28aee13fa9c
  - path: context/generated/module-map.json
    sha256: 31d9ca15805fc1b6488e8da89fdb0ee7714adbcdfda176d69581ab8f3d6482da
  - path: context/generated/api-inventory.json
    sha256: 8dfdbde2e1d64359cb29b8f710a2a467d5e19b383d90def7c856e7738d4ca104
  - path: context/generated/db-schema.json
    sha256: 43f6d599b6cbbd10eeb62bfbc1f7b947dea565e519d28b267856a8570f6de2dc
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "The dashboard data is served by the route `/api/dashboard`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C2
    text: "Dashboard routes are defined in `dashboardRouter`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/routes/dashboard.ts:dashboardRouter"]
  - id: C3
    text: "Dashboard counts are computed in `DashboardService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C4
    text: "The dashboard periods are defined in the service as `PERIODS`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:PERIODS"]
  - id: C5
    text: "The web dashboard is the page `DashboardPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]
  - id: C6
    text: "Each dashboard tile is drawn by the component `Tile`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:Tile"]
  - id: C7
    text: "The appointment list is served by the route `/api/appointments`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C8
    text: "The web app has an appointment list at `/appointments`."
    cites: ["context/generated/api-inventory.json#ui:/appointments"]
  - id: C9
    text: "Appointment status values include `NO_SHOW`."
    cites: ["context/data-model.md#appointments"]
  - id: C10
    text: "The appointment table has a `status` column."
    cites: ["context/generated/db-schema.json#column:appointment.status"]
  - id: C11
    text: "The appointment table has a `starts_at` column."
    cites: ["context/generated/db-schema.json#column:appointment.starts_at"]
  - id: C12
    text: "Instants are stored in `UTC`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C13
    text: "Today uses the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C14
    text: "The web app calls the API only through `client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C15
    text: "Medical history leaves the database only for the patient's `own page`."
    cites: ["context/data-model.md#patients"]
  - id: C16
    text: "A patient row holds `contact details`."
    cites: ["context/data-model.md#patients"]
  - id: C17
    text: "No `background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
  - id: C18
    text: "Staff mark an appointment No-show through `/api/appointments/:id/no-show`."
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show"]
  - id: C19
    text: "The dashboard has API tests in `dashboard.test.ts`."
    cites: ["context/generated/test-layout.json#test:sample-system:api/test/dashboard.test.ts"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [REQ-1, REQ-2, REQ-3, REQ-5, REQ-6, REQ-8, REQ-9, REQ-10]
assumed: [REQ-4, REQ-7]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Do DashboardService's Completed and Cancelled counts select by appointment status and starts_at within the period bounds of PERIODS, computed in HOSPITAL_TZ?"
    why: "REQ-4 asks to count No-shows the same way; the facts name the service and PERIODS but not the query, so design must read it and copy it."
    evidence: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:PERIODS", "context/architecture.md#time-and-slots"]
    options: ["Yes: add a third count on status NO_SHOW in the same query", "No: the tiles count another way, design reads it first"]
    default: "Add a NO_SHOW count beside the Completed and Cancelled counts, with the same period bounds."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Does GET /api/appointments accept a status filter of NO_SHOW together with the period's from and to dates, as the existing Completed and Cancelled tiles' links use?"
    why: "REQ-6 and REQ-7 link the tile to that list filtered to No-show and the period; if the list cannot filter so, the API changes too."
    evidence: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/api-inventory.json#ui:/appointments", "context/data-model.md#appointments"]
    options: ["Yes, link like the other tiles", "No, the list filter must be extended"]
    default: "Assume yes and link exactly like the Cancelled tile, with status NO_SHOW."
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "context/generated/module-map.json#sym:sample-system:api/src/routes/dashboard.ts:dashboardRouter"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage", "context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:Tile"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#table:appointment"]
apis:
  - id: http:GET:/api/dashboard
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: http:GET:/api/appointments
    change: existing
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "context/generated/api-inventory.json#ui:/appointments"]
  - id: http:POST:/api/appointments/:id/no-show
    change: existing
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show"]
tables:
  - id: table:appointment
    change: existing
    cites: ["context/generated/db-schema.json#table:appointment", "context/generated/db-schema.json#column:appointment.status", "context/generated/db-schema.json#column:appointment.starts_at"]
---
# Impact analysis: HMS-3

## Summary
The change is small and read-only on data: GET /api/dashboard (C1), computed in DashboardService
(C3), gains a No-show count for the selected period, and DashboardPage (C5) draws one more Tile (C6)
that links to the appointment list (C8). No table, column or route is added: No-show is already an
appointment status (C9), stored in table:appointment's status column (C10). The main risks are
counting No-shows differently from the Completed and Cancelled tiles (period bounds, time zone; Q-01)
and a link the appointment list cannot filter (Q-02). Every reader of GET /api/dashboard is in the
web app, which calls the API only through client.ts (C14), so the added field is the only change
callers see.

## Impacted modules
- module:sample-system:api (changed): DashboardService adds the No-show count; dashboardRouter is unchanged in shape (C2, C3).
- module:sample-system:web (changed): DashboardPage adds a No-shows Tile and its link (C5, C6).
- module:sample-system:db (existing): no migration; the appointment table is read only.
- http:GET:/api/dashboard (changed): the response gains a No-show count.
- http:GET:/api/appointments (existing): the tile's link target, filtered by status and period (Q-02).
- http:POST:/api/appointments/:id/no-show (existing): how appointments become No-show (C18); unchanged.
- table:appointment (existing): read by status and starts_at (C10, C11).

## Known
- REQ-1, REQ-8: a new Tile on DashboardPage beside the others (C5, C6).
- REQ-2, REQ-3, REQ-5: the count comes from DashboardService, which already holds the periods (C3, C4); No-show is an existing status (C9).
- REQ-6: the appointment list exists at /appointments, served by GET /api/appointments (C7, C8).
- REQ-9: medical history leaves the database only for the patient's own page (C15); the count reads no patient columns.
- REQ-10: contact details live on the patient row (C16); the count reads none.

## Assumed
- REQ-4: counted the same way as Completed and Cancelled. Basis: those counts live in the same DashboardService with PERIODS (C3, C4) and today uses HOSPITAL_TZ (C13); the exact query is not in the facts (Q-01).
- REQ-7: the link carries the selected period. Basis: the list route takes the tile's link today, as the ticket says of the other tiles; the accepted filters are not in the facts (Q-02).

## Unknown
- U-1 (non-blocking): the exact counting query of the existing tiles (Q-01).
- U-2 (non-blocking): whether GET /api/appointments filters by NO_SHOW plus from and to (Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): do the existing counts select by status and starts_at within PERIODS bounds in HOSPITAL_TZ? Default: add a NO_SHOW count beside them.
- Q-02 (non-blocking, tech lead): does the appointment list filter by NO_SHOW with the period's dates? Default: link exactly like the Cancelled tile.

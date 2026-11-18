---
schema: impact.v1
run_id: run_01M7PX5XB0C1P9DTAW82D9MDVX
step: impact
attempt: 2
skill: impact-analysis@1.5.0
inputs:
  - path: artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/req.md
    sha256: cce290751712fe7cab482e213a87e79b5617807ebafe66bc494ea520b72f724a
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
resolves:
  - id: F-1
    how: "The dashboard test that asserts totals with toEqual and the typed web dashboard fixture are now listed as readers that must change."
claims:
  - id: C1
    text: "The dashboard route is declared in `api/src/routes/dashboard.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C2
    text: "Dashboard counts are computed in `DashboardService`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]
  - id: C3
    text: "The dashboard period window comes from `lastDays`."
    cites: ["context/generated/module-map.json#sym:sample-system:api/src/lib/periods.ts:lastDays"]
  - id: C4
    text: "Today and period days use the hospital time zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C5
    text: "The Total patients count reads `FROM patient WHERE status = 'ACTIVE'`."
    cites: ["sample-system:api/src/services/dashboardService.ts#L51"]
  - id: C6
    text: "The dashboard service returns counts only, with no contact details or medical history (`DSH-9`)."
    cites: ["sample-system:api/src/services/dashboardService.ts#L30"]
  - id: C7
    text: "The dashboard tiles and their links are rendered by `DashboardPage`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]
  - id: C8
    text: "Period tiles link to their list with `from: data.period.from` and `to: data.period.to`."
    cites: ["sample-system:web/src/pages/DashboardPage.tsx#L48"]
  - id: C9
    text: "The web dashboard response type is `Dashboard` in web/src/api/types.ts."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Dashboard"]
  - id: C10
    text: "The patient list route is declared in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C11
    text: "The patient list service reads a text search and `f.oneOf('status', RECORD_STATUSES)`, and no registration-date filter."
    cites: ["sample-system:api/src/services/patientService.ts#L24-L31"]
  - id: C12
    text: "The patient list query orders by `ORDER BY created_at DESC, id DESC`."
    cites: ["sample-system:api/src/repositories/patientRepo.ts#L120"]
  - id: C13
    text: "The patients page reads its filters from the URL with `params.get('q')`."
    cites: ["sample-system:web/src/pages/PatientsPage.tsx#L24"]
  - id: C14
    text: "The page for the patient list is the UI route `/patients`."
    cites: ["context/generated/api-inventory.json#ui:/patients"]
  - id: C15
    text: "Index `patient_created_idx` already covers patient created_at."
    cites: ["sample-system:db/migrations/20261016T090200__create_patients.sql#L25"]
  - id: C16
    text: "The patient table has a non-null `created_at` column."
    cites: ["context/generated/db-schema.json#column:patient.created_at"]
  - id: C17
    text: "The web app calls the API only through `web/src/api/client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C18
    text: "`No background jobs` exist yet."
    cites: ["context/architecture.md#open-points"]
  - id: C19
    text: "Registered in the period means the patient's `created_at` falls in the period."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/req.md#item:REQ-2"]
  - id: C20
    text: "The API dashboard test asserts the whole totals object with `expect(res.body.totals).toEqual({`."
    cites: ["sample-system:api/test/dashboard.test.ts#L32"]
  - id: C21
    text: "The web dashboard test builds a typed totals fixture starting `totals: { patients: 200`."
    cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L10"]
  - id: C22
    text: "Other API dashboard tests check totals with `toMatchObject`, which tolerates an added field."
    cites: ["sample-system:api/test/dashboard-no-shows.test.ts#L36"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
assumed: []
unknown: []
questions: []
modules:
  - id: module:sample-system:api
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "sample-system:api/src/services/patientService.ts#L24-L31", "sample-system:api/test/dashboard.test.ts#L32"]
  - id: module:sample-system:web
    change: changed
    cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage", "sample-system:web/src/__tests__/DashboardPage.test.tsx#L10"]
  - id: module:sample-system:db
    change: existing
    cites: ["context/generated/module-map.json#module:sample-system:db", "sample-system:db/migrations/20261016T090200__create_patients.sql#L25"]
apis:
  - id: http:GET:/api/dashboard
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard", "sample-system:api/test/dashboard.test.ts#L32"]
  - id: http:GET:/api/patients
    change: changed
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients", "sample-system:api/src/services/patientService.ts#L24-L31"]
tables:
  - id: table:patient
    change: existing
    cites: ["context/generated/db-schema.json#table:patient", "context/generated/db-schema.json#column:patient.created_at"]
answers: []
---
# Impact analysis: HMS-22

## Resolves
- F-1: The dashboard test that asserts totals with toEqual and the typed web dashboard fixture are now listed as readers that must change.

## Summary
The change adds a New patients count to the dashboard API and tile, and a registration-period filter to the patient list API and page. The dashboard already computes a period window in the hospital time zone with `lastDays` (C3, C4), so the new count can reuse it. The patient list today filters only by text search and status (C11), so `GET /api/patients` must gain an optional filter on `created_at` that uses the same local-day bounds as the tile. Two existing tests read the full dashboard totals shape and must change with it (C20, C21). No migration is expected, because `patient.created_at` exists and is already indexed (C15, C16).

## Impacted modules
- module:sample-system:api — changed. `DashboardService` gains the new count (C2), and `PatientService.list` and its repository gain an optional registration-period filter (C11). The API dashboard test asserts the whole totals object with `toEqual` (C20), so it fails once a field is added and must include the new count.
- module:sample-system:web — changed. `DashboardPage` adds the New patients tile and its link (C7), the `Dashboard` type gains the count (C9), and the patients page must pass the new filter from the URL to the API (C13). The web dashboard test's typed totals fixture (C21) stops type-checking once the `Dashboard` type gains a required field, so it must add the count.
- module:sample-system:db — existing. `patient_created_idx` already covers `created_at` (C15), so no migration is needed.
- http:GET:/api/dashboard — changed. Its response gains a New patients count, and the existing totals keep their values (C5). Readers of the full totals shape: the API test (C20) and the web fixture (C21). The other API dashboard tests use `toMatchObject` (C22), which tolerates the added field.
- http:GET:/api/patients — changed. It gains an optional registration-period filter. The existing `q`, `status` and page parameters keep their meaning (C11).
- table:patient — existing. It is read by `created_at` (C16) and is not changed.

Indirect effects:
- The UI route `/patients` (C14) renders the list opened by the tile. It reads its filters from the URL (C13), so a tile link that carries no `q` and no `status` clears the typed search (REQ-9) and shows all statuses (REQ-7) without extra state.
- The list order is fixed by the query, `ORDER BY created_at DESC, id DESC` (C12), and the facts show no user-chosen sort. REQ-10 (keep the sort order) is therefore met by keeping that query order. Story S-3 AC5 in the split step assumes a user-chosen sort order, which the code does not offer. The design step should test the fixed order instead.
- `GET /api/patients` gains only optional parameters, so existing callers through `web/src/api/client.ts` (C17) keep their behaviour. The facts hold no call edges, so this artifact does not list every caller.
- There are `No background jobs` (C18), so no job reads the changed data.

Default (no question): name the new list filter after the period tiles' existing `from`/`to` local-date link parameters (C8). Trade-off: this stays consistent with the appointment list links, but the parameter names do not say they filter by registration date.

## Known
- REQ-1, REQ-5, REQ-11: the tile and its period behaviour are in `DashboardPage` (C7) and `DashboardService` (C2).
- REQ-2, REQ-3, REQ-4: the count uses `created_at` (C19, C16) inside the local-day window from `lastDays` (C3, C4), with no status filter.
- REQ-6, REQ-7, REQ-8, REQ-9: the tile link opens `/patients` (C14), which takes its filters from the URL (C13). The list API needs a new registration-period filter (C11).
- REQ-10: the list order is fixed by the query (C12).
- REQ-12, REQ-13: the existing totals, including `FROM patient WHERE status = 'ACTIVE'` (C5), keep their values. The tests that read the full totals shape (C20, C21) change only to add the new field.
- REQ-14, REQ-15: the dashboard returns counts only (C6). The new field is also a count.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE: none in this step. The decisions on "registered" and the tile list come from the intent (C19; req.md REQ-7 and REQ-9).

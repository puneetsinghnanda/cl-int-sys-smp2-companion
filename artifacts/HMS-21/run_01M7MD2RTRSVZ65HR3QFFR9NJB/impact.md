---
schema: impact.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
step: impact
attempt: 2
skill: impact-analysis@1.4.0
inputs:
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md, sha256: cdf917be0f8dfa8f1b3a5b08603bd0a50957b5f840e47e335b5c6a52a24591fc}
  - {path: context/generated/module-map.json, sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b}
  - {path: context/generated/api-inventory.json, sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9}
  - {path: context/generated/db-schema.json, sha256: e2a9d2990f6618ea5d55994a16c6264b0186b851cc3216014b812c4a4b98908a}
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/data-model.md, sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The dashboard counts are computed in the `DashboardService` class.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService"]}
  - {id: C2, text: "The API's dashboard response type is `Dashboard` in dashboardService.ts.", cites: ["context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:Dashboard"]}
  - {id: C3, text: "The web's dashboard response type is `Dashboard` in web/src/api/types.ts.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/api/types.ts:Dashboard"]}
  - {id: C4, text: "The dashboard page component is `DashboardPage`.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage"]}
  - {id: C5, text: "Each count is rendered by the `Tile` component on the dashboard page.", cites: ["context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:Tile"]}
  - {id: C6, text: "The Upcoming count is Scheduled appointments with `starts_at > $3`, where $3 is now.", cites: ["sample-system:api/src/services/dashboardService.ts#L52"]}
  - {id: C7, text: "The Completed, Cancelled and No-show counts apply the period with `starts_at >= $4`.", cites: ["sample-system:api/src/services/dashboardService.ts#L53"]}
  - {id: C8, text: "The next upcoming appointments list is capped with `LIMIT 10`.", cites: ["sample-system:api/src/services/dashboardService.ts#L61"]}
  - {id: C9, text: "The dashboard service carries no contact details or medical history (`DSH-9`).", cites: ["sample-system:api/src/services/dashboardService.ts#L28"]}
  - {id: C10, text: "The web app fetches the dashboard from `/api/dashboard` through the api client.", cites: ["sample-system:web/src/api/client.ts#L49"]}
  - {id: C11, text: "The Upcoming tile links to the list with `status: 'SCHEDULED', from: data.today`.", cites: ["sample-system:web/src/pages/DashboardPage.tsx#L44"]}
  - {id: C12, text: "The appointment list filters by date range in `hospital-local days` and by status.", cites: ["sample-system:api/src/services/appointmentService.ts#L24"]}
  - {id: C13, text: "The list's upper bound is an exclusive `a.starts_at < ?` on the end of the `to` day.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L72"]}
  - {id: C14, text: "The appointment list page reads its filters from the URL with `useSearchParams`.", cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L1"]}
  - {id: C15, text: "The web app fetches the appointment list through `appointments:` in the api client.", cites: ["sample-system:web/src/api/client.ts#L76"]}
  - {id: C16, text: "Appointment list rows carry the patient's name as `p.full_name AS patient_name`, not contact details or medical history.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L22"]}
  - {id: C17, text: "Index `appointment_start_idx` already exists on appointment.starts_at.", cites: ["context/generated/db-schema.json#index:appointment_start_idx"]}
  - {id: C18, text: "Appointment status is SCHEDULED, COMPLETED, CANCELLED or `NO_SHOW`.", cites: ["context/data-model.md#appointments"]}
  - {id: C19, text: "Routes parse input, `services hold the rules` and repositories hold the SQL.", cites: ["context/architecture.md#components"]}
  - {id: C20, text: "No `background jobs` exist yet.", cites: ["context/architecture.md#open-points"]}
  - {id: C21, text: "The user chose a `New filter on GET /api/appointments` for the list behind the tile.", cites: ["runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-01"]}
  - {id: C22, text: "The API dashboard test compares the whole totals object with `toEqual`.", cites: ["sample-system:api/test/dashboard.test.ts#L32"]}
  - {id: C23, text: "The web dashboard test fixture is typed `const DATA: Dashboard`.", cites: ["sample-system:web/src/__tests__/DashboardPage.test.tsx#L7"]}
  - {id: C24, text: "The doctor detail page links to the appointment list as `All upcoming appointments`.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L148"]}
  - {id: C25, text: "The doctors page links each doctor's `today_count` to the appointment list.", cites: ["sample-system:web/src/pages/DoctorsPage.tsx#L86"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: []
unknown: []
questions: []
resolves:
  - {id: F-1, how: "Both dashboard tests are named as indirect effects that must gain the new count."}
  - {id: F-2, how: "The patient-page statement is replaced by the cited doctor-page links."}
modules:
  - {id: "module:sample-system:api", change: changed, cites: ["context/generated/module-map.json#module:sample-system:api", "context/generated/module-map.json#sym:sample-system:api/src/services/dashboardService.ts:DashboardService", "sample-system:api/src/services/appointmentService.ts#L24", "sample-system:api/test/dashboard.test.ts#L32", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-01"]}
  - {id: "module:sample-system:web", change: changed, cites: ["context/generated/module-map.json#module:sample-system:web", "context/generated/module-map.json#sym:sample-system:web/src/pages/DashboardPage.tsx:DashboardPage", "sample-system:web/src/pages/AppointmentsPage.tsx#L1", "sample-system:web/src/__tests__/DashboardPage.test.tsx#L7"]}
  - {id: "module:sample-system:db", change: existing, cites: ["context/generated/module-map.json#module:sample-system:db", "context/generated/db-schema.json#index:appointment_start_idx"]}
apis:
  - {id: "http:GET:/api/dashboard", change: changed, cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard", "sample-system:api/src/services/dashboardService.ts#L52"]}
  - {id: "http:GET:/api/appointments", change: changed, cites: ["context/generated/api-inventory.json#http:GET:/api/appointments", "sample-system:api/src/services/appointmentService.ts#L24", "runs/run_01M7MD2RTRSVZ65HR3QFFR9NJB/ide-answers.md#a-01"]}
  - {id: "http:POST:/api/appointments/:id/complete", change: existing, cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/complete"]}
  - {id: "http:POST:/api/appointments/:id/no-show", change: existing, cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/no-show"]}
  - {id: "http:POST:/api/appointments/:id/cancel", change: existing, cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/cancel"]}
tables:
  - {id: "table:appointment", change: existing, cites: ["context/generated/db-schema.json#table:appointment", "context/data-model.md#appointments"]}
answers:
  - {id: A-01, kind: question, about: Q-01, answer: "New filter on GET /api/appointments (proposed default)", decides: ["http:GET:/api/appointments", C21]}
---
# Impact analysis: HMS-21

## Summary
HMS-21 adds an Awaiting outcome count to the dashboard and a list behind it. The count belongs with the other tile counts in `DashboardService` (C1), so `GET /api/dashboard` gains one count and both `Dashboard` types gain its field (C2, C3). `DashboardPage` gains one more `Tile` (C4, C5). The appointment list can filter only by status and whole hospital-local days (C12, C13), so it cannot show "Scheduled and started before now". You chose to add a new filter to `GET /api/appointments` for that (A-01, C21). No table, column, index or migration changes: the count reads `appointment` by status and start time, and an index on `starts_at` already exists (C17). The main risks are:
- keeping the count and the list on the same rule (strictly before now, no period);
- leaving the existing Upcoming tile (C6, C11), the period tiles (C7) and the next-10 list (C8) unchanged;
- updating the two existing dashboard tests that pin the totals shape (C22, C23).

## Impacted modules
- module:sample-system:api, changed:
  - `DashboardService.get` adds the Awaiting outcome count to its totals, with no period applied (REQ-1, REQ-2 to REQ-5, REQ-7).
  - The appointment list service and repository gain the new "starts before now" filter (REQ-6, A-01).
- module:sample-system:web, changed:
  - `DashboardPage` renders a new `Awaiting outcome` tile that links to the appointment list with the new filter (REQ-1, REQ-6).
  - `AppointmentsPage` reads its filters from the URL (C14), so it must pass the new filter to the API through the client (C15).
  - The web `Dashboard` type gains the new count (C3).
- module:sample-system:db, existing: no migration. Index `appointment_start_idx` already exists on `starts_at` (C17).
- http:GET:/api/dashboard, changed: the response gains the Awaiting outcome count. The other totals and the `next` list stay as they are (REQ-8, REQ-9; C6 to C8).
- http:GET:/api/appointments, changed (A-01): it gains an optional filter for appointments that start before now. The existing filters (status, from, to, doctor, patient) are unchanged (C12, C13).
- http:POST:/api/appointments/:id/complete, /no-show and /cancel, existing: closing an appointment moves it out of the count. These routes are not changed.
- table:appointment, existing: read by status and `starts_at` (C18). Nothing is written.

Indirect effects:
- The web app reaches the dashboard through `/api/dashboard` in the api client (C10), and the appointment list through `appointments:` in the client (C15).
- Existing tests that pin the totals shape must gain the new count. Both dashboard tests are named as indirect effects that must gain the new count.
  - `api/test/dashboard.test.ts` compares the whole totals object with `toEqual` (C22).
  - `web/src/__tests__/DashboardPage.test.tsx` types its fixture as `Dashboard` (C23).
- Other pages link to the appointment list: the doctor detail page (C24) and the doctors page (C25), besides the dashboard tiles (C11). The patient-page statement is replaced by the cited doctor-page links. The new filter is optional, so these links are unaffected.
- There are no background jobs to update (C20).

Data protection: the dashboard carries counts only, with no contact details or medical history (DSH-9, C9). List rows carry the patient's name only (C16). So REQ-10 and REQ-11 hold as long as the change adds only a count and a filter.

## Known
- REQ-1, REQ-7: a new tile on `DashboardPage` (C4, C5) fed by a new total from `DashboardService` (C1).
- REQ-2, REQ-3, REQ-4: the count is Scheduled appointments with a start before now. The Upcoming count already uses the opposite rule, `starts_at > $3` (C6), so the new count is its mirror on the same table (C18).
- REQ-5: the period tiles apply `starts_at >= $4` (C7); the new count must not.
- REQ-6: the list needs the new filter that you chose (A-01, C21), because days alone cannot express "before now" (C12, C13).
- REQ-8, REQ-9: the other totals and the `LIMIT 10` next list (C8) stay unchanged. The two existing dashboard tests guard this and need only the new field added (C22, C23).
- REQ-10, REQ-11: counts and list rows carry no medical history or contact details (C9, C16).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by the user in this run): how the list behind the tile expresses "started before now". Answer: "New filter on GET /api/appointments (proposed default)". It decides http:GET:/api/appointments (changed) and C21. No mismatch with the facts: the facts show the list has no such filter today (C12, C13), so this is a new capability.

Default (no question): the count is added to `GET /api/dashboard` rather than computed in the browser. Every other tile's count comes from that endpoint (C1, C10). Trade-off: one more count in the dashboard query, in exchange for one source for all tiles.

Retry fixes:
- F-1: Both dashboard tests are named as indirect effects that must gain the new count.
- F-2: The patient-page statement is replaced by the cited doctor-page links.

---
schema: intent.v1
run_id: run_01M7PX5XB0C1P9DTAW82D9MDVX
step: req
attempt: 2
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md
    sha256: 114ce2f2159d68c2569ed246f9c09d12c13e2ccaa9dcf724253742c2ea0311c1
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The patient table has a non-null `created_at` timestamptz column."
    cites: ["context/generated/db-schema.json#column:patient.created_at"]
  - id: C2
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C3
    text: "Lists answer with `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C4
    text: "The dashboard data is served from `api/src/routes/dashboard.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C5
    text: "The patient list is served from `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C6
    text: "`Medical history` never appears in lists, the dashboard or logs."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C7
    text: "A period is a run of whole hospital-local days ending today: Today, Last 7 days or `Last 30 days`."
    cites: ["context/glossary.md#period"]
  - id: C8
    text: "A patient row holds identity, `contact details` and basic medical history."
    cites: ["context/data-model.md#patients"]
  - id: C9
    text: "Registered in the period means the patient's `created_at` falls in the period (answered in ended run run_01M7PR1TBRN7418PYZMJTX55D2)."
    cites: ["runs/run_01M7PR1TBRN7418PYZMJTX55D2/ide-answers.md#a-01"]
  - id: C10
    text: "The list opened from the New patients tile uses `All statuses from tile` (answered in ended run run_01M7PR1TBRN7418PYZMJTX55D2)."
    cites: ["runs/run_01M7PR1TBRN7418PYZMJTX55D2/ide-answers.md#a-02"]
  - id: C11
    text: "A search already typed on the list is `cleared` when the list is opened from the tile (answered at the pause of this run)."
    cites: ["runs/run_01M7PX5XB0C1P9DTAW82D9MDVX/open-questions.md#q-01"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: New patients tile
    type: functional
    status: known
    statement: "The dashboard has a tile labelled `New patients`."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L17"]
  - id: REQ-2
    title: Count by registration time
    type: functional
    status: known
    statement: "The tile shows the number of patients whose `created_at` falls in the selected period."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L18", "runs/run_01M7PR1TBRN7418PYZMJTX55D2/ide-answers.md#a-01", "context/generated/db-schema.json#column:patient.created_at"]
  - id: REQ-3
    title: Period in hospital time zone
    type: constraint
    status: known
    statement: "The period's days are whole hospital-local days in `Asia/Kolkata`."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L18-L19", "context/intent-baseline.md#constraints", "context/glossary.md#period"]
  - id: REQ-4
    title: Active and Inactive both counted
    type: functional
    status: known
    statement: "Both Active and `Inactive` patients registered in the period are counted."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L20"]
  - id: REQ-5
    title: Count follows the period selector
    type: functional
    status: known
    statement: "Changing the period updates the count, the same way it does for the `Completed and Cancelled` tiles."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L21"]
  - id: REQ-6
    title: Tile links to filtered patient list
    type: functional
    status: known
    statement: "Selecting the tile opens the patient list filtered to the patients registered in that period (`DSH-8`)."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L22-L23"]
  - id: REQ-7
    title: Tile list shows all statuses
    type: functional
    status: known
    statement: "The list opened from the tile shows all statuses (`All statuses from tile`); with the search cleared (REQ-9) it holds exactly the patients the tile counts."
    cites: ["runs/run_01M7PR1TBRN7418PYZMJTX55D2/ide-answers.md#a-02", "artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L20"]
  - id: REQ-8
    title: Filtered list page size
    type: constraint
    status: known
    statement: "The filtered patient list stays at `20 per page`."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L24", "context/conventions.md#lists"]
  - id: REQ-9
    title: Tile clears the typed search
    type: functional
    status: known
    statement: "When the list is opened from the tile, a search already typed on the list is `cleared`."
    cites: ["runs/run_01M7PX5XB0C1P9DTAW82D9MDVX/open-questions.md#q-01", "artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L24"]
  - id: REQ-10
    title: Filtered list keeps sort order
    type: functional
    status: known
    statement: "The filtered patient list keeps its current `sort order`."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L24-L25"]
  - id: REQ-11
    title: Zero registrations
    type: functional
    status: known
    statement: "When no patient was registered in the period, the tile `shows 0`."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L26"]
  - id: REQ-12
    title: Total patients tile unchanged
    type: constraint
    status: known
    statement: "The `Total patients` tile is unchanged."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L27"]
  - id: REQ-13
    title: Rest of dashboard unchanged
    type: constraint
    status: known
    statement: "The `rest of the dashboard` is unchanged."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L27"]
  - id: REQ-14
    title: No medical history on dashboard
    type: constraint
    status: known
    statement: "No `medical history` appears on the dashboard."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L28", "context/intent-baseline.md#data-protection"]
  - id: REQ-15
    title: No contact details on dashboard
    type: constraint
    status: known
    statement: "No `contact details` appear on the dashboard."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/ticket.md#L28", "context/data-model.md#patients"]
unknowns: []
questions: []
answers: []
glossary:
  - {term: Dashboard, found: true}
  - {term: Tile, found: true}
  - {term: Period, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Inactive, found: true}
  - {term: Medical history, found: true}
  - {term: Completed, found: true}
  - {term: Cancelled, found: true}
  - {term: Registered, found: false}
  - {term: Contact details, found: false}
---
# Requirements review: HMS-22

## Summary
HMS-22 adds a `New patients` tile to the dashboard that counts patients registered in the selected period, in hospital-local days, and links to the matching patient list (ticket L17–L23). "Registered" is not in the glossary, so this run reuses an earlier answer: a patient counts when their `created_at` falls in the period (answer A-01 of the ended run). The main risk was that the tile's list could hide counted patients, either through its status filter or a typed search. Answer A-02 of the ended run opens the list with all statuses, and the answer at this run's pause clears the typed search, overriding ticket AC6 for both. The dashboard must still carry no medical history or contact details.

## Requirements
- REQ-1 New patients tile — known — The dashboard has a tile labelled `New patients`.
- REQ-2 Count by registration time — known — The tile counts patients whose `created_at` falls in the selected period.
- REQ-3 Period in hospital time zone — known — The period's days are whole hospital-local days in `Asia/Kolkata`.
- REQ-4 Active and Inactive both counted — known — Active and `Inactive` patients registered in the period are both counted.
- REQ-5 Count follows the period selector — known — Changing the period updates the count, as for the `Completed and Cancelled` tiles.
- REQ-6 Tile links to filtered patient list — known — The tile opens the patient list filtered to patients registered in that period (`DSH-8`).
- REQ-7 Tile list shows all statuses — known — The list opened from the tile shows all statuses; with the search cleared (REQ-9) it matches the count.
- REQ-8 Filtered list page size — known — The filtered list stays at `20 per page`.
- REQ-9 Tile clears the typed search — known — When the list is opened from the tile, a typed search is `cleared`.
- REQ-10 Filtered list keeps sort order — known — The filtered list keeps its current `sort order`.
- REQ-11 Zero registrations — known — With no registrations in the period, the tile `shows 0`.
- REQ-12 Total patients tile unchanged — known — The `Total patients` tile is unchanged.
- REQ-13 Rest of dashboard unchanged — known — The `rest of the dashboard` is unchanged.
- REQ-14 No medical history on dashboard — known — No `medical history` appears on the dashboard.
- REQ-15 No contact details on dashboard — known — No `contact details` appear on the dashboard.

## Known
- REQ-1: ticket AC1 (L17).
- REQ-2: ticket AC2 (L18); "registered" means `created_at`, per runs/run_01M7PR1TBRN7418PYZMJTX55D2/ide-answers.md#a-01; the column exists and is non-null (C1).
- REQ-3: ticket AC2 (L18–L19); the zone is `Asia/Kolkata` (C2); period rule in glossary (C7).
- REQ-4: ticket AC3 (L20).
- REQ-5: ticket AC4 (L21).
- REQ-6: ticket AC5 (L22–L23).
- REQ-7: decided by runs/run_01M7PR1TBRN7418PYZMJTX55D2/ide-answers.md#a-02, consistent with ticket AC3 (L20).
- REQ-8: ticket AC6 (L24); matches the list convention (C3).
- REQ-9: decided by the answer to runs/run_01M7PX5XB0C1P9DTAW82D9MDVX/open-questions.md#q-01 (C11).
- REQ-10: ticket AC6 (L24).
- REQ-11: ticket AC7 (L26).
- REQ-12, REQ-13: ticket AC8 (L27).
- REQ-14: ticket AC9 (L28); baseline data protection (C6).
- REQ-15: ticket AC9 (L28); the patient row holds contact details (C8).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered at the run pause:
- runs/run_01M7PX5XB0C1P9DTAW82D9MDVX/open-questions.md#q-01 carries forward the ended run's blocking question (runs/run_01M7PR1TBRN7418PYZMJTX55D2/open-questions.md#q-01): clear or keep a typed search when the list is opened from the tile. Answer: "It should be cleared", by eng-dbe4. Decides REQ-9. Mismatch: ticket AC6 (L24) says the list keeps its current search. The answer clears it for the tile link; page size and sort order are still kept (REQ-8, REQ-10).

Answered in the IDE (ended run run_01M7PR1TBRN7418PYZMJTX55D2, same ticket, reused):
- runs/run_01M7PR1TBRN7418PYZMJTX55D2/ide-answers.md#a-01: what does "registered in the period" mean? Answered by the run owner: "Yes, created_at (proposed default)". Decides REQ-2. No mismatch.
- runs/run_01M7PR1TBRN7418PYZMJTX55D2/ide-answers.md#a-02: which status filter applies when the list is opened from the tile? Answered by the run owner: "All statuses from tile (proposed default)". Decides REQ-7. Mismatch: ticket AC6 (L24) says the list keeps its current status filter. The answer overrides that for the tile link only.

## Out of scope
- Glossary: "Registered" is not defined. REQ-2 uses the reused answer above; adding the term is left to the glossary owner.
- Glossary: "Contact details" is not defined. The data model says the patient row holds contact details (C8), which is enough for REQ-15.
- Front-matter `answers` lists only answers given in this run's IDE (none). tsl gate H4 (RR-C13) rejects the reused ended-run answers there. Their mismatches with ticket AC6 are recorded under Open questions above.
- The PRD references DSH-1, DSH-7, DSH-8 and DSH-9 are cited by the ticket. The PRD itself is not an input to this step and was not read.

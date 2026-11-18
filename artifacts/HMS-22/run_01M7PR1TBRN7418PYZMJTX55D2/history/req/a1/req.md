---
schema: intent.v1
run_id: run_01M7PR1TBRN7418PYZMJTX55D2
step: req
attempt: 1
skill: requirement-review@1.3.0
inputs:
  - {path: artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md, sha256: 173c409c6b9744c3af6b4ca3e10ac9cb51725d151610e93ecc7caf1c2d0bd306}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The hospital time zone is `Asia/Kolkata`.", cites: ["context/intent-baseline.md#constraints"]}
  - {id: C2, text: "A period is a run of `whole hospital-local days ending today`.", cites: ["context/glossary.md#period"]}
  - {id: C3, text: "The dashboard tiles and the doctor page's No-shows count `use the same rule` for periods.", cites: ["context/glossary.md#period"]}
  - {id: C4, text: "The patient table has a non-null `created_at` column.", cites: ["context/generated/db-schema.json#column:patient.created_at"]}
  - {id: C5, text: "Medical history appears `never in lists, the dashboard or logs`.", cites: ["context/intent-baseline.md#data-protection"]}
  - {id: C6, text: "An `Inactive` patient is kept for history but not offered for new use.", cites: ["context/glossary.md#inactive"]}
  - {id: C8, text: "Registered means the patient record's creation time: `Yes, created_at (proposed default)`.", cites: ["runs/run_01M7PR1TBRN7418PYZMJTX55D2/ide-answers.md#a-01"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: []
unknown: []
requirements:
  - {id: REQ-1, title: "New patients tile", type: functional, status: known, statement: "The dashboard has a tile labelled `New patients`.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L17"]}
  - {id: REQ-2, title: "Count patients registered in the period", type: functional, status: known, statement: "The tile shows the number of patients whose record was created (`created_at`) within the selected period's hospital-local days.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L18-L19", "runs/run_01M7PR1TBRN7418PYZMJTX55D2/ide-answers.md#a-01"]}
  - {id: REQ-3, title: "Active and Inactive both counted", type: functional, status: known, statement: "Patients registered in the period are counted whether they are now `Active and Inactive`.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L20"]}
  - {id: REQ-4, title: "Count follows the period selector", type: functional, status: known, statement: "Changing the period updates the count, as it does for the `Completed and Cancelled tiles`.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L21"]}
  - {id: REQ-5, title: "Tile links to the registered patients", type: functional, status: known, statement: "The tile links to the `patient list filtered to the patients registered in that period`, showing exactly the patients it counts.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L22-L23"]}
  - {id: REQ-6, title: "Filtered list keeps its page size", type: constraint, status: known, statement: "The filtered patient list stays `20 per page`.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L24"]}
  - {id: REQ-7, title: "Filtered list keeps search, status and sort", type: constraint, status: known, statement: "The filtered patient list keeps its current `search, status filter` and sort order.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L24-L25"]}
  - {id: REQ-8, title: "Zero when none registered", type: functional, status: known, statement: "When no patient was registered in the period the tile `shows 0`.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L26"]}
  - {id: REQ-9, title: "Total patients tile unchanged", type: constraint, status: known, statement: "The `Total patients tile` is unchanged.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L27"]}
  - {id: REQ-10, title: "Rest of the dashboard unchanged", type: constraint, status: known, statement: "The `rest of the dashboard` is unchanged.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L27"]}
  - {id: REQ-11, title: "No medical history on the dashboard", type: constraint, status: known, statement: "`No medical history` appears on the dashboard.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L28", "context/intent-baseline.md#data-protection"]}
  - {id: REQ-12, title: "No contact details on the dashboard", type: constraint, status: known, statement: "No `contact details` appear on the dashboard.", cites: ["artifacts/HMS-22/run_01M7PR1TBRN7418PYZMJTX55D2/ticket.md#L28"]}
unknowns: []
questions: []
answers:
  - {id: A-01, kind: question, about: Q-01, answer: "Yes, created_at (proposed default)", requirements: [REQ-2]}
glossary:
  - {term: Dashboard, found: true}
  - {term: Tile, found: true}
  - {term: Period, found: true}
  - {term: Inactive, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Medical history, found: true}
  - {term: registered, found: false}
---
# Requirements review: HMS-22

## Summary
HMS-22 adds a "New patients" tile to the dashboard. The tile counts patients registered in the selected period, both Active and Inactive, and links to the patient list filtered to those patients. The user decided (A-01) that "registered" means the record's creation time `created_at` (C4, C8), counted by whole hospital-local days in Asia/Kolkata (C1, C2, C3). The main risk is that the tile's list must show exactly the counted patients, Inactive ones included, while keeping its page size, search, status filter and sort. The dashboard must also not gain medical history or contact details (C5).

## Requirements
- REQ-1, New patients tile (known): the dashboard has a tile labelled New patients.
- REQ-2, Count patients registered in the period (known): the tile counts patients whose record was created within the selected period's hospital-local days (A-01).
- REQ-3, Active and Inactive both counted (known): registered patients count whether they are now Active or Inactive (C6).
- REQ-4, Count follows the period selector (known): changing the period updates the count, like the Completed and Cancelled tiles.
- REQ-5, Tile links to the registered patients (known): the tile links to the patient list filtered to the patients registered in that period, exactly the ones counted.
- REQ-6, Filtered list keeps its page size (known): 20 per page.
- REQ-7, Filtered list keeps search, status and sort (known): the current search, status filter and sort order stay.
- REQ-8, Zero when none registered (known): the tile shows 0.
- REQ-9, Total patients tile unchanged (known).
- REQ-10, Rest of the dashboard unchanged (known).
- REQ-11, No medical history on the dashboard (known; C5).
- REQ-12, No contact details on the dashboard (known).

## Known
- REQ-1, REQ-3 to REQ-12: each is stated in the ticket's acceptance criteria (ticket lines 17 to 28).
- REQ-2: stated in the ticket. The meaning of "registered" was decided by A-01.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by the user): does "registered in the period" mean the patient record's creation time (`created_at`) falls in the period's hospital-local days? Answer: "Yes, created_at (proposed default)". Decides REQ-2. No mismatch with the ticket or baseline.

## Out of scope
- How the period filter is passed to the patient list, and whether a new API filter is needed. That belongs to impact and design.
- The glossary term "registered" is still undefined in the context pack. A-01 settles it for this run only.

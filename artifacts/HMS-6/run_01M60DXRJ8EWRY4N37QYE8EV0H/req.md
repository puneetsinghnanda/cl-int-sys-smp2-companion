---
schema: intent.v1
run_id: run_01M60DXRJ8EWRY4N37QYE8EV0H
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md
    sha256: 93e0a989e2087effd7cd15acc62d66a8b6ebf91af3b48c790ea3ec50d8521591
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C2
    text: "The doctors list already shows code, name, specialization, department, fee and status (`PRD DOC-2`)."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L16"]
  - id: C3
    text: "The dashboard is made of `tiles` that count patients, doctors and appointments, each linking to the list it counts."
    cites: ["context/glossary.md#dashboard"]
  - id: C4
    text: "Selecting a tile `opens the list` behind the count."
    cites: ["context/glossary.md#tile"]
  - id: C5
    text: "A Cancelled appointment is one `called off` before it took place."
    cites: ["context/glossary.md#cancelled"]
  - id: C6
    text: "In the MVP every user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
  - id: C7
    text: "A doctor kept for history but not offered for new use is `Inactive`."
    cites: ["context/glossary.md#inactive"]
  - id: C8
    text: "A system goal is that `the dashboard shows the day` in one screen."
    cites: ["context/intent-baseline.md#goals"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: [REQ-13]
unknown: [U-1, U-2, U-3, U-4, U-5, U-6]
requirements:
  - id: REQ-1
    title: Today column
    type: functional
    status: known
    statement: "The doctors list has a column titled `Today`."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L19"]
  - id: REQ-2
    title: Today counts the doctor's appointments starting today
    type: functional
    status: known
    statement: "For each doctor, the Today column shows the number of that doctor's appointments that `start today`."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L20"]
  - id: REQ-3
    title: Today is the hospital-time day
    type: constraint
    status: known
    statement: "\"Today\" is the current calendar day in the hospital time zone, `Asia/Kolkata`."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L20", "context/intent-baseline.md#constraints"]
  - id: REQ-4
    title: Cancelled appointments not counted
    type: functional
    status: known
    statement: "A `Cancelled` appointment is never counted in the Today column."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L22"]
  - id: REQ-5
    title: All other statuses counted
    type: functional
    status: known
    statement: "Appointments in `any status except` Cancelled (Scheduled, Completed, No-show) are counted in the Today column."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L21"]
  - id: REQ-6
    title: Same counting rule as the dashboard tile
    type: constraint
    status: known
    statement: "The Today count uses the same counting rule as the dashboard's `Appointments today` tile, so the sum of all doctors' Today counts equals that tile's count."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L21"]
  - id: REQ-7
    title: Zero when no appointments today
    type: functional
    status: known
    statement: "A doctor with no appointments today `shows 0` in the Today column."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L23"]
  - id: REQ-8
    title: Count links to filtered appointment list
    type: functional
    status: known
    statement: "Each Today count links to the appointment list `filtered to that doctor and today`."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L24"]
  - id: REQ-9
    title: Page size unchanged
    type: constraint
    status: known
    statement: "The doctors list stays at `20 per page`."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L25"]
  - id: REQ-10
    title: Sort by name unchanged
    type: constraint
    status: known
    statement: "The doctors list stays `sorted by name`."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L25"]
  - id: REQ-11
    title: Filters unchanged
    type: constraint
    status: known
    statement: "The doctors list keeps its `current filters` unchanged."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L25"]
  - id: REQ-12
    title: Search unchanged
    type: constraint
    status: known
    statement: "The doctors list keeps its `search` unchanged."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L25"]
  - id: REQ-13
    title: Existing columns kept
    type: constraint
    status: assumed
    statement: "The doctors list keeps its existing columns (code, name, `specialization`, department, fee, status); Today is added alongside them."
    basis: "The ticket lists the current columns as context and asks only to add a column; nothing asks to remove or change the others."
    cites: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L16", "artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L19"]
unknowns:
  - id: U-1
    text: "The inputs do not show how the existing `Appointments today` tile actually counts; if it differs from \"any status except Cancelled\", ticket lines 21 and 22 conflict with each other."
    question: Q-01
  - id: U-2
    text: "It is not known whether the appointment list can already be filtered by doctor and by date (needed for REQ-8)."
    question: Q-02
  - id: U-3
    text: "The ticket does not say whether `Inactive` doctors in the list also show a Today count."
    question: Q-03
  - id: U-4
    text: "Term `doctors list` is not in the glossary."
    question: Q-04
  - id: U-5
    text: "Term `appointment list` is not in the glossary."
    question: Q-04
  - id: U-6
    text: "Term `Appointments today` (the tile label) is not in the glossary."
    question: Q-04
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Does the dashboard's Appointments today tile count every status except Cancelled, by start time in Asia/Kolkata? If not, which rule wins?"
    why: "The ticket states both a rule and 'the same way as the tile'; if the tile differs, the two cannot both hold."
    evidence: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L21", "artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L22", "context/glossary.md#tile"]
    options: ["Tile already matches; reuse its rule", "Tile differs; follow the ticket's explicit rule", "Tile differs; follow the tile"]
    default: "Reuse the tile's counting rule; impact analysis confirms it excludes only Cancelled"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Does the appointment list already support filtering by doctor and by a single date, so the count can link to it?"
    why: "REQ-8 needs a link target; if the filters do not exist, the change grows to the appointment list."
    evidence: ["artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L24"]
    options: ["Filters exist; link with query parameters", "Add the missing filter(s) as part of HMS-6"]
    default: "Link with doctor and date filters; add any missing filter as part of HMS-6"
  - id: Q-03
    blocking: false
    ask: product owner
    text: "Should Inactive doctors show their Today count like any other doctor?"
    why: "Inactive doctors are not offered for new use, so a count may mislead walk-in booking; but they can still have existing appointments today."
    evidence: ["context/glossary.md#inactive", "artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L16"]
    options: ["Show the count for every doctor in the list", "Show the count only for Active doctors"]
    default: "Show the count for every doctor in the list (acceptance criterion 3 applies to all)"
  - id: Q-04
    blocking: false
    ask: tech lead
    text: "Should 'doctors list', 'appointment list' and 'Appointments today' be added to the glossary?"
    why: "Agreed terms keep later stories and tests consistent."
    evidence: ["context/glossary.md#glossary", "artifacts/HMS-6/run_01M60DXRJ8EWRY4N37QYE8EV0H/ticket.md#L19"]
    options: ["Add the three terms", "Leave as is"]
    default: "Add: Appointments today = appointments starting today in hospital time, any status except Cancelled"
glossary:
  - {term: Front-desk staff, found: true}
  - {term: Dashboard, found: true}
  - {term: Tile, found: true}
  - {term: Cancelled, found: true}
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: No-show, found: true}
  - {term: Inactive, found: true}
  - {term: doctors list, found: false}
  - {term: appointment list, found: false}
  - {term: Appointments today, found: false}
---
# Requirements review: HMS-6

## Summary
HMS-6 asks for a "Today" column on the doctors list so front-desk staff can book walk-in patients with the least busy doctors. The column counts each doctor's appointments that start today in hospital time (Asia/Kolkata), excluding only Cancelled, using the same rule as the dashboard's "Appointments today" tile; doctors with none show 0, and each count links to the appointment list filtered to that doctor and today. Paging, sort by name, filters and search must not change. The main risks are a mismatch between the stated rule and the tile's real rule, a missing doctor/date filter on the appointment list, and breaking the existing list query. All open questions are non-blocking.

## Requirements
- REQ-1 Today column — known — The doctors list has a column "Today".
- REQ-2 Appointments starting today — known — Shows the number of the doctor's appointments that start today.
- REQ-3 Hospital-time day — known — "Today" is the day in Asia/Kolkata.
- REQ-4 Cancelled not counted — known — Cancelled appointments are excluded.
- REQ-5 Other statuses counted — known — Scheduled, Completed and No-show are counted.
- REQ-6 Same rule as tile — known — Counted the same way as the "Appointments today" tile.
- REQ-7 Zero — known — No appointments today shows 0.
- REQ-8 Link — known — The count links to the appointment list filtered to the doctor and today.
- REQ-9 Page size unchanged — known — 20 per page.
- REQ-10 Sort unchanged — known — Sorted by name.
- REQ-11 Filters unchanged — known — Current filters kept.
- REQ-12 Search unchanged — known — Current search kept.
- REQ-13 Existing columns kept — assumed — Today is added beside the existing columns.

## Known
- REQ-1, REQ-2, REQ-4 to REQ-12: stated directly in the ticket's acceptance criteria (ticket L19–L25).
- REQ-3: ticket L20 asks for hospital time zone; baseline constraints set it to Asia/Kolkata.

## Assumed
- REQ-13: existing columns (code, name, specialization, department, fee, status) stay. Basis: ticket L16 lists them as current state and only a new column is asked for.

## Unknown
- U-1: how the existing tile really counts, and which rule wins if it differs (Q-01).
- U-2: whether the appointment list can filter by doctor and date (Q-02).
- U-3: whether Inactive doctors show a count (Q-03).
- U-4, U-5, U-6: "doctors list", "appointment list" and "Appointments today" are not glossary terms (Q-04).

## Open questions
- Q-01 (non-blocking, tech lead): does the tile exclude only Cancelled; default is reuse the tile's rule.
- Q-02 (non-blocking, tech lead): appointment list doctor/date filters; default is add any missing filter in HMS-6.
- Q-03 (non-blocking, product owner): Inactive doctors; default is show the count for every doctor.
- Q-04 (non-blocking, tech lead): add the three terms to the glossary.

## Out of scope
- Ranking, highlighting or sorting doctors by "least busy": the ticket states this as the motivation but acceptance criterion 5 keeps sort by name, so no requirement is written.
- Live refresh of the count while the page is open: not asked for in the ticket.
- Free-slot or availability counts: the ticket asks only for appointment counts.
- PRD DOC-2 itself is not among the inputs; REQ-13 relies on the ticket line only.

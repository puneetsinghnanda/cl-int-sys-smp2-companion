---
schema: intent.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md
    sha256: 3e6453d85bd08109259b00e91fed9847a0982d3341bdc6366933631b4be342ee
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
    text: "The `dashboard` is meant to show the day in one screen."
    cites: ["context/intent-baseline.md#goals"]
  - id: C3
    text: "Dashboard `tiles` count patients, doctors and appointments, each linking to the list it counts."
    cites: ["context/glossary.md#dashboard"]
  - id: C4
    text: "Selecting a tile `opens the list behind the count`."
    cites: ["context/glossary.md#tile"]
  - id: C5
    text: "A Cancelled appointment is one `called off` before it took place; its slot becomes free again."
    cites: ["context/glossary.md#cancelled"]
  - id: C6
    text: "Front-desk staff book appointments; in the MVP every user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
  - id: C7
    text: "The doctors list currently shows code, name, `specialization`, department, fee and status."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L16"]
  - id: C8
    text: "Doctors with appointments are made `Inactive`, never deleted."
    cites: ["context/intent-baseline.md#data-protection"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
assumed: [REQ-11]
unknown: [U-1, U-2, U-3, U-4, U-5, U-6]
requirements:
  - id: REQ-1
    title: Today column on the doctors list
    type: functional
    status: known
    statement: "The doctors list has a column labelled `Today`."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L19"]
  - id: REQ-2
    title: Count appointments that start today in hospital time
    type: functional
    status: known
    statement: "For each doctor, the Today column shows the `number` of that doctor's appointments whose start falls on the current day in the hospital time zone (`Asia/Kolkata`)."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L20", "context/intent-baseline.md#constraints"]
  - id: REQ-3
    title: Cancelled appointments are excluded
    type: functional
    status: known
    statement: "The count includes appointments in any status except `Cancelled`."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L22"]
  - id: REQ-4
    title: Same counting rule as the dashboard tile
    type: constraint
    status: known
    statement: "The count uses the same rule as the dashboard's `Appointments today` tile, so the sum of all doctors' Today counts equals that tile's value for the same moment."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L21"]
  - id: REQ-5
    title: Zero for doctors without appointments today
    type: functional
    status: known
    statement: "A doctor with no counted appointments today shows `0` in the Today column."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L23"]
  - id: REQ-6
    title: Count links to the appointment list for that doctor
    type: functional
    status: known
    statement: "The count is a `link` to the appointment list filtered to that doctor."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L24"]
  - id: REQ-7
    title: Linked appointment list is filtered to today
    type: functional
    status: known
    statement: "The appointment list opened from the count is also filtered to `today` (hospital time zone)."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L24", "context/intent-baseline.md#constraints"]
  - id: REQ-8
    title: Page size stays 20
    type: constraint
    status: known
    statement: "The doctors list still shows `20 per page`."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L25"]
  - id: REQ-9
    title: Sort order stays by name
    type: constraint
    status: known
    statement: "The doctors list stays `sorted by name`."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L25"]
  - id: REQ-10
    title: Filters and search unchanged
    type: constraint
    status: known
    statement: "The doctors list keeps its current `filters and search` with unchanged behaviour."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L25"]
  - id: REQ-11
    title: Existing columns kept
    type: constraint
    status: assumed
    statement: "The doctors list keeps its existing columns code, name, `specialization`, department, fee and status; Today is added alongside them."
    basis: "The ticket lists these as the current columns and asks only to add one; nothing asks to remove or change them."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L16"]
unknowns:
  - id: U-1
    text: "The term `walk-in` is not in the glossary."
    question: Q-01
  - id: U-2
    text: "The term `doctors list` is not in the glossary."
    question: Q-02
  - id: U-3
    text: "The term `appointment list` is not in the glossary, and the inputs do not say whether it can be filtered by doctor and by day."
    question: Q-03
  - id: U-4
    text: "The term `hospital time zone` is not in the glossary; the intent baseline gives Asia/Kolkata."
    question: Q-04
  - id: U-5
    text: "The cited inputs do not state how the dashboard's `Appointments today` tile counts; the ticket says any status except Cancelled."
    question: Q-05
  - id: U-6
    text: "The ticket does not say whether a count of `0` is also a link."
    question: Q-06
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Does 'walk-in' imply any new booking behaviour, or is it only the motivation for the Today column?"
    why: "If walk-ins need their own flow, it is a separate requirement."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L14"]
    options: ["Motivation only; no new behaviour", "New walk-in booking flow (separate ticket)"]
    default: "Motivation only; no new behaviour"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is the 'doctors list' the existing doctors list page described by PRD DOC-2?"
    why: "Impact analysis needs the exact screen and endpoint to change."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L16"]
    options: ["Existing doctors list (PRD DOC-2)", "Another view"]
    default: "Existing doctors list (PRD DOC-2)"
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Does the existing appointment list support filtering by doctor and by a single day? If not, may this change add those filters?"
    why: "REQ-6 and REQ-7 need a link target that applies both filters."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L24", "context/glossary.md#dashboard"]
    options: ["Filters exist; link to them", "Add the missing filter(s) in this change"]
    default: "Use existing filters; add any missing doctor/day filter in this change"
  - id: Q-04
    blocking: false
    ask: tech lead
    text: "Confirm 'today' is the calendar day in the hospital time zone Asia/Kolkata, regardless of the server's or browser's zone."
    why: "Day boundaries decide which appointments are counted."
    evidence: ["context/intent-baseline.md#constraints", "artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L20"]
    options: ["Asia/Kolkata calendar day", "Server or browser local day"]
    default: "Asia/Kolkata calendar day"
  - id: Q-05
    blocking: false
    ask: tech lead
    text: "Does the dashboard's 'Appointments today' tile count every status except Cancelled? If the tile differs, which rule wins?"
    why: "REQ-3 and REQ-4 must agree; a mismatch would make the column and tile disagree."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L21", "context/glossary.md#tile"]
    options: ["Tile already matches; reuse its rule", "Tile differs; follow the ticket and raise a separate fix"]
    default: "Reuse the tile's counting rule; impact analysis confirms it excludes only Cancelled"
  - id: Q-06
    blocking: false
    ask: product owner
    text: "Should a count of 0 also be a link to the (empty) filtered appointment list?"
    why: "Affects rendering and tests for REQ-5 and REQ-6."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/ticket.md#L23", "context/glossary.md#tile"]
    options: ["Every count links, including 0", "0 is plain text"]
    default: "Every count links, including 0, like dashboard tiles"
glossary:
  - {term: front-desk staff, found: true}
  - {term: dashboard, found: true}
  - {term: tile, found: true}
  - {term: cancelled, found: true}
  - {term: inactive, found: true}
  - {term: walk-in, found: false}
  - {term: doctors list, found: false}
  - {term: appointment list, found: false}
  - {term: hospital time zone, found: false}
---
# Requirements review: HMS-6

## Summary
HMS-6 asks for a "Today" column on the doctors list showing how many of each doctor's appointments start today in the hospital time zone, excluding Cancelled ones, so front-desk staff can send walk-ins to the least busy doctor. The count must match the dashboard's "Appointments today" tile rule and link to the appointment list filtered to that doctor and today. Paging (20 per page), sort by name, filters and search must not change. Main risks: the cited inputs do not confirm the tile's counting rule (U-5) or that the appointment list can filter by doctor and day (U-3). No question blocks design.

## Requirements
- REQ-1 Today column on the doctors list (known): the list has a "Today" column.
- REQ-2 Count appointments that start today in hospital time (known): per doctor, count appointments starting on today's date in Asia/Kolkata.
- REQ-3 Cancelled appointments are excluded (known): any status except Cancelled counts.
- REQ-4 Same counting rule as the dashboard tile (known): counts agree with the "Appointments today" tile.
- REQ-5 Zero for doctors without appointments today (known): shows 0.
- REQ-6 Count links to the appointment list for that doctor (known).
- REQ-7 Linked appointment list is filtered to today (known).
- REQ-8 Page size stays 20 (known).
- REQ-9 Sort order stays by name (known).
- REQ-10 Filters and search unchanged (known).
- REQ-11 Existing columns kept (assumed).

## Known
- REQ-1, REQ-5, REQ-6, REQ-7: acceptance criteria 1, 3 and 4 of the ticket.
- REQ-2, REQ-3, REQ-4: acceptance criterion 2; time zone from the intent baseline constraints.
- REQ-8, REQ-9, REQ-10: acceptance criterion 5.

## Assumed
- REQ-11: existing columns (code, name, specialization, department, fee, status) stay; the ticket lists them as current and asks only to add one.

## Unknown
- U-1: "walk-in" not in glossary → Q-01.
- U-2: "doctors list" not in glossary → Q-02.
- U-3: "appointment list" not in glossary; doctor/day filter support unknown → Q-03.
- U-4: "hospital time zone" not in glossary (baseline says Asia/Kolkata) → Q-04.
- U-5: tile counting rule not confirmed by the inputs → Q-05.
- U-6: whether a 0 count links → Q-06.

## Open questions
- Q-01 (non-blocking, product owner): does "walk-in" imply new behaviour? Default: no.
- Q-02 (non-blocking, tech lead): is this the existing DOC-2 doctors list? Default: yes.
- Q-03 (non-blocking, tech lead): can the appointment list filter by doctor and day? Default: use or add those filters.
- Q-04 (non-blocking, tech lead): "today" is the Asia/Kolkata calendar day? Default: yes.
- Q-05 (non-blocking, tech lead): does the tile exclude only Cancelled? Default: reuse the tile's rule.
- Q-06 (non-blocking, product owner): does 0 link? Default: yes, every count links.

## Out of scope
- Booking walk-in patients or choosing a doctor automatically: the ticket gives this only as motivation.
- Sorting or filtering the list by the Today count: not asked.
- Changes to the dashboard tile itself: the ticket only requires consistency with it.

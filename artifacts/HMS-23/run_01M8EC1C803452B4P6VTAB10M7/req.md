---
schema: intent.v1
run_id: run_01M8EC1C803452B4P6VTAB10M7
step: req
attempt: 2
skill: requirement-review@1.6.0
inputs:
  - {path: artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md, sha256: 3d2541f0ea09bb4ec13ab8028e574ce79b4f1d7b9b5e518a80c230ad0aeded60}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The dashboard today shows the tiles followed by a `Next appointments` list.", cites: ["sample-system:web/src/pages/DashboardPage.tsx#L61"]}
  - {id: C2, text: "In the day schedule a slot taken by an appointment is `BOOKED`; otherwise it is PAST when its start is not after now, else FREE.", cites: ["sample-system:api/src/services/doctorService.ts#L161"]}
  - {id: C3, text: "The day schedule and the doctor page's Next 7 days share one slot-state rule, `slotStates`.", cites: ["sample-system:api/src/services/doctorService.ts#L158"]}
  - {id: C4, text: "Next 7 days counts free slots as those in state `FREE`.", cites: ["sample-system:api/src/services/doctorService.ts#L182"]}
  - {id: C5, text: "The doctor page opens its day schedule on a given day through a `?date=` link.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L115"]}
  - {id: C6, text: "The doctor page appends `(Inactive)` to an Inactive department.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L74"]}
  - {id: C7, text: "The hospital time zone is `Asia/Kolkata`.", cites: ["context/intent-baseline.md#constraints"]}
  - {id: C8, text: "DSH-9: the dashboard shows no `contact details`.", cites: ["sample-system:docs/PRD.md#L177"]}
  - {id: C9, text: "The day schedule shows each slot's time as its `label`.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L168"]}
  - {id: C10, text: "The dashboard Period selector offers Today, Last 7 days and `Last 30 days`.", cites: ["sample-system:web/src/pages/DashboardPage.tsx#L13"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
assumed: []
unknown: []
requirements:
  - {id: REQ-1, title: "On duty today section", type: functional, status: known, statement: "Below the tiles, the dashboard shows a section titled `On duty today`.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L21"]}
  - {id: REQ-2, title: "Who is listed", type: functional, status: known, statement: "The section has one row per `Active` doctor who has availability today.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L21-L22"]}
  - {id: REQ-3, title: "Today in hospital time", type: constraint, status: known, statement: "Today means the current day in the hospital time zone, `Asia/Kolkata`.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L22", "context/intent-baseline.md#constraints"]}
  - {id: REQ-4, title: "Row shows doctor name", type: functional, status: known, statement: "Each row shows the doctor's `name`.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L22"]}
  - {id: REQ-5, title: "Row shows department", type: functional, status: known, statement: "Each row shows the doctor's `department`.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L22"]}
  - {id: REQ-6, title: "Inactive department marker", type: functional, status: known, statement: "When the doctor's department is Inactive, the row appends ` (Inactive)` to the department name, as the doctor page does.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L74", "runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-02"]}
  - {id: REQ-7, title: "Booked slots count", type: functional, status: known, statement: "Each row shows the number of `booked` slots today, counted the same way as the doctor's day schedule shows them for today.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L22-L24"]}
  - {id: REQ-8, title: "Free slots count", type: functional, status: known, statement: "Each row shows the number of `free` slots today, counted the same way as the doctor's day schedule; slots that have already passed are not free.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L23-L25"]}
  - {id: REQ-9, title: "Next free slot", type: functional, status: known, statement: "Each row shows the time of the `next free slot`: the earliest free slot that starts after now.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L23-L26"]}
  - {id: REQ-10, title: "Sort by free slots", type: functional, status: known, statement: "Rows are sorted by `free slots`, most first.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L27"]}
  - {id: REQ-11, title: "Tie-break by name", type: functional, status: known, statement: "Rows with the same number of free slots are ordered `by doctor name`.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L27"]}
  - {id: REQ-12, title: "Fully booked", type: functional, status: known, statement: "A doctor with no free slots left shows `Fully booked` instead of a time.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L28", "runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-01"]}
  - {id: REQ-13, title: "Name links to today's schedule", type: functional, status: known, statement: "The doctor's name links to that doctor's `day schedule` for today.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L29"]}
  - {id: REQ-14, title: "Empty state", type: functional, status: known, statement: "When no doctor is on duty today, the section shows `No doctors on duty today`.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L30"]}
  - {id: REQ-15, title: "Inactive doctors excluded", type: constraint, status: known, statement: "`Inactive doctors` are not listed.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L31"]}
  - {id: REQ-16, title: "Ignores Period", type: constraint, status: known, statement: "The section ignores the `Period` selector.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L31"]}
  - {id: REQ-17, title: "No contact details", type: constraint, status: known, statement: "The section shows no `contact` details (DSH-9).", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L31-L32", "sample-system:docs/PRD.md#L177"]}
  - {id: REQ-18, title: "Rest unchanged", type: constraint, status: known, statement: "The rest of the dashboard is `unchanged`.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L33"]}
  - {id: REQ-19, title: "Doctor with all slots passed stays listed", type: functional, status: known, statement: "A doctor with availability today whose slots have all passed stays listed in the section, with `Fully booked`.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/ticket.md#L28", "runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-01"]}
unknowns: []
questions: []
answers:
  - {id: A-01, kind: question, about: Q-01, answer: "Keep, show Fully booked (proposed default)", requirements: [REQ-12, REQ-19]}
  - {id: A-02, kind: question, about: Q-02, answer: "Append \" (Inactive)\" (proposed default)", requirements: [REQ-6]}
glossary:
  - {term: Dashboard, found: true}
  - {term: Slot, found: true}
  - {term: Availability block, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Tile, found: true}
  - {term: Period, found: true}
  - {term: Inactive, found: true}
  - {term: Scheduled, found: true}
---
# Requirements review: HMS-23

## Summary
HMS-23 asks for a new "On duty today" section on the dashboard. It lists every Active doctor with availability today, with booked and free slot counts and the next free slot, so front-desk staff can send a walk-in patient to a doctor who can see them. The main risk is counting slots differently from the doctor page. The new section must use the slot-state rule that the day schedule and Next 7 days already share (`slotStates`, C3), where a booked slot stays `BOOKED` and an unbooked slot that has started is PAST (C2). The section must also stay outside the Period selector and must not show contact details (DSH-9, C8).

## Requirements
- REQ-1 On duty today section (known): Below the tiles, the dashboard shows a section titled "On duty today".
- REQ-2 Who is listed (known): one row per Active doctor who has availability today.
- REQ-3 Today in hospital time (known): today is the current day in Asia/Kolkata.
- REQ-4 Row shows doctor name (known).
- REQ-5 Row shows department (known).
- REQ-6 Inactive department marker (known): an Inactive department gets " (Inactive)" appended, as on the doctor page.
- REQ-7 Booked slots count (known): counted as the day schedule shows them for today.
- REQ-8 Free slots count (known): counted as the day schedule shows them; passed slots are not free.
- REQ-9 Next free slot (known): the earliest free slot that starts after now.
- REQ-10 Sort by free slots (known): most first.
- REQ-11 Tie-break by name (known): ties are ordered by doctor name.
- REQ-12 Fully booked (known): no free slots left shows "Fully booked" instead of a time.
- REQ-13 Name links to today's schedule (known).
- REQ-14 Empty state (known): "No doctors on duty today".
- REQ-15 Inactive doctors excluded (known).
- REQ-16 Ignores Period (known).
- REQ-17 No contact details (known): DSH-9.
- REQ-18 Rest unchanged (known).
- REQ-19 Doctor with all slots passed stays listed (known): such a doctor stays in the section with "Fully booked".

## Known
- REQ-1 to REQ-5, REQ-7 to REQ-11, REQ-13 to REQ-18: stated in the ticket's acceptance criteria (ticket.md#L21-L33). REQ-3 is also in the baseline constraints, and REQ-17 is also in PRD DSH-9.
- REQ-6: decided in the IDE (A-02). It matches the doctor page (C6).
- REQ-12: stated in the ticket (#L28); confirmed in the IDE (A-01).
- REQ-19: decided in the IDE (A-01): a doctor whose slots have all passed stays listed.
- Facts looked up, not asked: the day schedule's slot states (C2, C3, C4); the doctor page opens a given day through `?date=` (C5); the slot time is shown as its label (C9); the Period options (C10).
- Defaults for minor choices (not questions):
  - The next free slot time uses the same slot label the day schedule shows (C9). Trade-off: it stays consistent with the doctor page, but it shows no date, which is fine because every row is for today.
  - REQ-13 links with today's date in `?date=` (C5). Trade-off: it reuses the existing deep link; a plain doctor link would also open on today but would not say so.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by eng-3c51): Should a doctor whose slots today have all passed stay listed? Answer: "Keep, show Fully booked (proposed default)". Decides REQ-12 and REQ-19. No mismatch with the ticket.
- A-02 (Q-02, answered by eng-3c51): Should an Inactive department get a marker? Answer: "Append " (Inactive)" (proposed default)". Decides REQ-6. No mismatch with the ticket.

## Out of scope
- The user story's goal of sending a walk-in patient to a doctor (ticket.md#L13-L14) is the motivation, not a separate behaviour. No booking action from the section is asked for.
- Auto-refresh of the section while the page stays open is not asked for in the ticket, so it is not a requirement.
- How the data reaches the page (dashboard endpoint or a new endpoint) is a design decision.

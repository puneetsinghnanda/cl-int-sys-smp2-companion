---
schema: intent.v1
run_id: run_01M7SQ3HAGJ230D5FQ7P2H5AA4
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md
    sha256: 6cf1992f1c5e4eaffca0bf69b66105c5f10a4a507c9bce718812da53e7028fbf
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Today the dashboard shows count tiles and the `next 10 appointments`."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L16"]
  - id: C2
    text: "Booked and free slots per day are shown only on the doctor page's `day schedule`."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L17"]
  - id: C3
    text: "The dashboard data is served by `api/src/routes/dashboard.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C4
    text: "A doctor's slots for a day are served by `api/src/routes/doctors.ts` (GET /api/doctors/:id/slots)."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: C5
    text: "The doctor page route /doctors/:id is declared in `web/src/App.tsx`."
    cites: ["context/generated/api-inventory.json#ui:/doctors/:id"]
  - id: C6
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C7
    text: "A slot is free only inside availability, `in the future` and not overlapping a Scheduled appointment."
    cites: ["context/intent-baseline.md#booking-rules"]
  - id: C8
    text: "Availability is defined as weekly blocks with a `weekday`, start time and end time in hospital local time."
    cites: ["context/glossary.md#availability-block"]
  - id: C9
    text: "Medical history never appears on the `dashboard`."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C10
    text: "A `Period` is Today, Last 7 days or Last 30 days and drives the dashboard tiles."
    cites: ["context/glossary.md#period"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: On duty today section
    type: functional
    status: known
    statement: "Below the tiles, the dashboard shows a section titled `On duty today`."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L21"]
  - id: REQ-2
    title: Who is on duty
    type: functional
    status: known
    statement: "The section has one row per `Active` doctor who has at least one availability block on today's weekday, with today taken in hospital time (Asia/Kolkata)."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L21-L22", "context/intent-baseline.md#constraints", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-01"]
  - id: REQ-3
    title: Row contents
    type: functional
    status: known
    statement: "Each row shows the doctor's name, `department`, booked slots, free slots and the time of the next free slot."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L22-L23"]
  - id: REQ-4
    title: Same counting as day schedule
    type: constraint
    status: known
    statement: "Booked and free slots are counted by the same logic as the doctor's `day schedule` for today (the slot logic behind GET /api/doctors/:id/slots), whatever appointment statuses it counts."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L24", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-02"]
  - id: REQ-5
    title: Passed slots are not free
    type: functional
    status: known
    statement: "Slots that have `already passed` are not counted as free."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L25", "context/intent-baseline.md#booking-rules"]
  - id: REQ-6
    title: Next free slot
    type: functional
    status: known
    statement: "The `next free slot` is the earliest free slot that starts after now."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L25-L26"]
  - id: REQ-7
    title: Fully booked
    type: functional
    status: known
    statement: "A doctor on duty with no free slots left (all remaining slots booked or passed) is still listed, with 0 free slots and `Fully booked` instead of a time."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L28", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-03"]
  - id: REQ-8
    title: Name links to today's day schedule
    type: functional
    status: known
    statement: "The doctor's name links to the doctor page (/doctors/:id) showing that doctor's `day schedule` for today."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L29", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-04"]
  - id: REQ-9
    title: Sort order
    type: functional
    status: known
    statement: "Rows are sorted by free slots, `most first`; ties are sorted by doctor name."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L27"]
  - id: REQ-10
    title: Empty state
    type: functional
    status: known
    statement: "When no doctor is on duty today, the section shows `No doctors on duty today`."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L30"]
  - id: REQ-11
    title: Inactive doctors excluded
    type: functional
    status: known
    statement: "`Inactive` doctors are not listed."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L31"]
  - id: REQ-12
    title: Ignores Period selector
    type: functional
    status: known
    statement: "The section ignores the `Period` selector and always shows today."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L31"]
  - id: REQ-13
    title: No contact details
    type: constraint
    status: known
    statement: "The section shows no `contact` details."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L31-L32"]
  - id: REQ-14
    title: Rest of dashboard unchanged
    type: constraint
    status: known
    statement: "The rest of the dashboard is `unchanged`."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ticket.md#L33"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: assumption
    about: REQ-2
    answer: "Yes, confirm"
    requirements: [REQ-2]
  - id: A-02
    kind: question
    about: Q-01
    answer: "Same as day schedule code (proposed default)"
    requirements: [REQ-4]
  - id: A-03
    kind: assumption
    about: REQ-7
    answer: "Yes, confirm"
    requirements: [REQ-7]
  - id: A-04
    kind: assumption
    about: REQ-8
    answer: "Yes, confirm"
    requirements: [REQ-8]
glossary:
  - {term: Dashboard, found: true}
  - {term: Tile, found: true}
  - {term: Period, found: true}
  - {term: Slot, found: true}
  - {term: Availability block, found: true}
  - {term: Inactive, found: true}
  - {term: Front-desk staff, found: true}
---
# Requirements review: HMS-23

## Summary
HMS-23 adds an "On duty today" section to the dashboard. It lists each Active doctor with availability today, with booked and free slot counts and the next free slot, so front-desk staff can send a walk-in patient to a doctor without opening each doctor page. The main risk is the slot counts drifting from the day schedule, so REQ-4 requires the same slot logic as GET /api/doctors/:id/slots. Today's date and "passed" slots depend on the hospital time zone (Asia/Kolkata). PRD §4.4 and DSH-9 are referenced by the ticket but are not in the inputs.

## Requirements
- REQ-1 On duty today section (known): below the tiles, the dashboard shows a section "On duty today".
- REQ-2 Who is on duty (known): one row per Active doctor with an availability block on today's weekday, in hospital time.
- REQ-3 Row contents (known): name, department, booked slots, free slots, time of the next free slot.
- REQ-4 Same counting as day schedule (known): booked and free slots come from the same logic as the day schedule for today.
- REQ-5 Passed slots are not free (known).
- REQ-6 Next free slot (known): the earliest free slot that starts after now.
- REQ-7 Fully booked (known): a doctor with no free slots left is still listed, with 0 free and "Fully booked".
- REQ-8 Name links to today's day schedule (known): the link goes to /doctors/:id, showing today.
- REQ-9 Sort order (known): free slots, most first, then doctor name.
- REQ-10 Empty state (known): "No doctors on duty today".
- REQ-11 Inactive doctors excluded (known).
- REQ-12 Ignores Period selector (known).
- REQ-13 No contact details (known).
- REQ-14 Rest of dashboard unchanged (known).

## Known
- REQ-1, REQ-3, REQ-5, REQ-6, REQ-9 to REQ-14: stated in the ticket's acceptance criteria.
- REQ-2: from the ticket, with the meaning of "has availability today" confirmed by the user (A-01).
- REQ-4: from the ticket, with the counting source decided by the user (A-02).
- REQ-7: from the ticket, with the passed-or-booked case confirmed by the user (A-03).
- REQ-8: from the ticket, with the link target confirmed by the user (A-04).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (REQ-2): Is a doctor "on duty" when they have an availability block on today's weekday in hospital time? "Yes, confirm". Answered by the user. Decides REQ-2. No mismatch.
- A-02 (Q-01): Which of today's appointments count as booked? "Same as day schedule code (proposed default)". Answered by the user. Decides REQ-4. No mismatch.
- A-03 (REQ-7): Is a doctor whose remaining slots are all passed or booked still listed as "Fully booked"? "Yes, confirm". Answered by the user. Decides REQ-7. No mismatch.
- A-04 (REQ-8): Does the name link to /doctors/:id showing today's day schedule? "Yes, confirm". Answered by the user. Decides REQ-8. No mismatch.

## Out of scope
- Changes to the day schedule, to "Next 7 days" (HMS-9) or to the booking rules: the ticket only reads them.
- The PRD §4.4 and DSH-9 texts that the ticket references are not in the inputs. REQ-13 relies only on the ticket's wording.
- Booking from the dashboard section: not asked for in the ticket.

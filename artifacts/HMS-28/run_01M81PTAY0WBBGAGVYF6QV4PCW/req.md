---
schema: intent.v1
run_id: run_01M81PTAY0WBBGAGVYF6QV4PCW
step: req
attempt: 3
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md
    sha256: dd67ddaf6d481e0d71df6f45fe4b9b415492d1941b0f492caf77b86f61ac481b
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The doctor list (`DOC-2`) shows code, name, specialization, department, fee and status."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L17"]
  - id: C2
    text: "Weekly availability is shown today only on the doctor's own page (`DOC-3`)."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L19"]
  - id: C3
    text: "An availability block is a weekly window with a `weekday`, start time and end time."
    cites: ["context/glossary.md#availability-block"]
  - id: C4
    text: "Availability blocks are in hospital local time; the hospital zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C5
    text: "An Inactive doctor is `kept for history` but not offered for new use."
    cites: ["context/glossary.md#inactive"]
  - id: C6
    text: "In the MVP every front-desk user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
  - id: C7
    text: "The doctor list already has a `Today` column after Status."
    cites: ["runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-01"]
  - id: C8
    text: "Booking rules allow a slot only inside the doctor's `availability`; this ticket does not change them."
    cites: ["context/intent-baseline.md#booking-rules"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Works on column lists working weekdays
    type: functional
    status: known
    statement: "The doctor list shows a column `Works on` with the weekdays on which the doctor has at least one availability block."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L23-L24", "context/glossary.md#availability-block"]
  - id: REQ-2
    title: Works on uses short names in fixed order
    type: functional
    status: known
    statement: "The weekdays in the Works on column are short names in the order `Mon, Tue, Wed, Thu, Fri, Sat, Sun`, separated by commas (for example \"Mon, Wed, Sat\")."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L24-L25"]
  - id: REQ-3
    title: No availability text
    type: functional
    status: known
    statement: "A doctor with no availability blocks shows `No availability` in the Works on column."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L26"]
  - id: REQ-4
    title: Works on filter choices
    type: functional
    status: known
    statement: "The filters have a new `Works on` choice offering \"Any day\" or one weekday, Monday to Sunday."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L27-L28"]
  - id: REQ-5
    title: Any day is the default
    type: functional
    status: known
    statement: "The Works on filter defaults to `Any day`, which does not narrow the list."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L27"]
  - id: REQ-6
    title: Weekday filter narrows the list
    type: functional
    status: known
    statement: "Choosing a weekday lists only the doctors who have `at least one availability block on` that weekday."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L28-L29"]
  - id: REQ-7
    title: Works on combines with existing filters
    type: functional
    status: known
    statement: "The Works on filter `combines with search and the department, specialization and status` filters (DOC-2)."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L30-L31"]
  - id: REQ-8
    title: Order and page size unchanged
    type: constraint
    status: known
    statement: "With the Works on filter set, the list stays `sorted by name, 20 per page`."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L31"]
  - id: REQ-9
    title: Page count reflects the filter
    type: functional
    status: known
    statement: "The `page count reflects the filter`: the total and pages count only the doctors matching the chosen weekday together with the other filters."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L31-L32"]
  - id: REQ-10
    title: Chosen weekday kept in the page address
    type: functional
    status: known
    statement: "The chosen weekday is `kept in the page address`, like the other filters, so going back or opening a copied link shows the same filtered list."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L33-L34"]
  - id: REQ-11
    title: Inactive doctors show their weekdays
    type: functional
    status: known
    statement: "`Inactive doctors show their weekdays` in the Works on column, the same as active doctors."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L35"]
  - id: REQ-12
    title: Status filter works as before
    type: constraint
    status: known
    statement: "The `status filter works as before`; adding the Works on column and filter does not change which doctors it shows."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L35"]
  - id: REQ-13
    title: Works on column position
    type: functional
    status: known
    statement: "The Works on column is placed `After Status, before Today`."
    cites: ["runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-01"]
  - id: REQ-14
    title: Filter options use full weekday names
    type: functional
    status: known
    statement: "The Works on filter options read \"Any day\" and the full weekday names `Monday` to Sunday."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L27-L28", "runs/run_01M81PTAY0WBBGAGVYF6QV4PCW/ide-answers.md#a-02"]
  - id: REQ-15
    title: Rest of the doctor list unchanged
    type: constraint
    status: known
    statement: "The `rest of the doctor list is unchanged`: its existing columns, including Today, stay as they are."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/ticket.md#L36"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "After Status, before Today (proposed default)"
    requirements: [REQ-13]
  - id: A-02
    kind: assumption
    about: REQ-14
    answer: "Yes, confirm"
    requirements: [REQ-14]
glossary:
  - {term: Availability block, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Inactive, found: true}
---
# Requirements review: HMS-28

## Summary
HMS-28 asks for the doctor list (DOC-2) to show, per doctor, the weekdays on which they have at
least one availability block, and to add a "Works on" filter that narrows the list to doctors who
work on one chosen weekday. Today, availability is visible only on each doctor's own page (DOC-3,
C2). The filter must combine with the existing search and filters, keep the name order and
20-per-page paging with a correct page count, and live in the page address. The main risk is
paging: the weekday filter has to be applied where the list is counted, not only on screen, or the
page count will be wrong.

## Requirements
- REQ-1 Works on column lists working weekdays (known): column "Works on" shows weekdays with at least one availability block.
- REQ-2 Works on uses short names in fixed order (known): Mon, Tue, Wed, Thu, Fri, Sat, Sun, comma-separated.
- REQ-3 No availability text (known): a doctor with no blocks shows "No availability".
- REQ-4 Works on filter choices (known): new filter with "Any day" or one weekday, Monday to Sunday.
- REQ-5 Any day is the default (known): the filter defaults to "Any day" and does not narrow the list.
- REQ-6 Weekday filter narrows the list (known): a chosen weekday lists only doctors with at least one block on it.
- REQ-7 Works on combines with existing filters (known): combines with search, department, specialization and status.
- REQ-8 Order and page size unchanged (known): sorted by name, 20 per page.
- REQ-9 Page count reflects the filter (known): total and pages count only matching doctors.
- REQ-10 Chosen weekday kept in the page address (known): back navigation and copied links keep the filter.
- REQ-11 Inactive doctors show their weekdays (known).
- REQ-12 Status filter works as before (known).
- REQ-13 Works on column position (known): after Status, before Today.
- REQ-14 Filter options use full weekday names (known): "Any day", "Monday" … "Sunday".
- REQ-15 Rest of the doctor list unchanged (known): existing columns, including Today (C7), stay as they are.

## Known
- REQ-1 to REQ-12 and REQ-15: stated in the ticket acceptance criteria (ticket.md#L23-L36).
- REQ-13: decided by the user in the IDE (A-01).
- REQ-14: assumption confirmed by the user in the IDE (A-02).
- Default (not a question): the name and coding of the weekday in the page address follow the existing filter keys and the stored weekday coding; design picks them. Trade-off: consistent with neighbouring filters, but a coded value is less readable in a link than a day name.

## Assumed
None

## Unknown
None

## Open questions
No questions are open.

Answered in the IDE:
- A-01 (Q-01): Where should the new "Works on" column go? Answer: "After Status, before Today (proposed default)", answered by eng-1df5. Decided REQ-13. No mismatch with the ticket or baseline.
- A-02 (REQ-14): Do the filter options use full weekday names? Answer: "Yes, confirm", answered by eng-1df5. Decided REQ-14. No mismatch with the ticket or baseline.

## Out of scope
- The doctor's own page (DOC-3) and how availability is edited there: the ticket only reads availability on the list.
- Filtering by time of day or by free slots: the ticket asks only for "at least one availability block" on a weekday; booking rules and slot calculation are unchanged (C8).
- No instruction-like text and no credentials or personal data were found in the inputs.

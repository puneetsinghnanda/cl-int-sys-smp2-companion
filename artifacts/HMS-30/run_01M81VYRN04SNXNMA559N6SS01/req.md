---
schema: intent.v1
run_id: run_01M81VYRN04SNXNMA559N6SS01
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md
    sha256: 1222ddd073b3fa05a6a55105f79b649e57ab28316aec58c059af756293b29928
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The appointment list today shows `20 per page`, ordered by start time."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L18"]
  - id: C2
    text: "System lists answer with a total and `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C3
    text: "Above the table the list currently shows only the total, for example `appointment(s)`."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L18-L19"]
  - id: C4
    text: "`Scheduled` is an appointment that is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C5
    text: "`Completed` is a past appointment the patient attended."
    cites: ["context/glossary.md#completed"]
  - id: C6
    text: "`Cancelled` is an appointment called off before it took place."
    cites: ["context/glossary.md#cancelled"]
  - id: C7
    text: "`No-show` is a past appointment the patient did not attend."
    cites: ["context/glossary.md#no-show"]
  - id: C8
    text: "Front-desk staff act as `staff` in the MVP."
    cites: ["context/glossary.md#front-desk-staff"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Per-status counts next to the total
    type: functional
    status: known
    statement: "Above the table, next to the total, the appointment list shows one count per status."
    cites:
      - "artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L22"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-02"
  - id: REQ-2
    title: Fixed status order
    type: functional
    status: known
    statement: "The counts appear in the order Scheduled, Completed, Cancelled, No-show."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L23"]
  - id: REQ-3
    title: Count label format
    type: functional
    status: known
    statement: "Each count shows the status label followed by its number, separated by ' · ', for example \"Scheduled 4 · Completed 25 · Cancelled 3 · No-show 2\"."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L23-L24"]
  - id: REQ-4
    title: Counts cover all matching appointments
    type: functional
    status: known
    statement: "The counts cover every appointment that matches the current doctor, patient and date range filters, not only the current page."
    cites:
      - "artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L25-L26"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-02"
  - id: REQ-5
    title: Counts ignore the Status filter
    type: functional
    status: known
    statement: "The counts ignore the Status filter, so all four statuses are always shown."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L27"]
  - id: REQ-6
    title: Zero counts are shown
    type: functional
    status: known
    statement: "A status with no matching appointments shows 0."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L27-L28"]
  - id: REQ-7
    title: Each count links to the filtered list
    type: functional
    status: known
    statement: "Each count links to the same appointment list with the Status filter set to that status, keeping the doctor or patient and date range filters and showing page 1."
    cites:
      - "artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L30"
      - "runs/run_01M81VYRN04SNXNMA559N6SS01/ide-answers.md#a-01"
  - id: REQ-8
    title: Counts add up to the total
    type: constraint
    status: known
    statement: "When the Status filter is \"Any status\", the four counts add up to the total shown."
    cites: ["artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L29"]
  - id: REQ-9
    title: Table and order unchanged
    type: constraint
    status: known
    statement: "The table and its order by start time are unchanged."
    cites:
      - "artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L31"
      - "artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L18"
  - id: REQ-10
    title: Paging unchanged
    type: constraint
    status: known
    statement: "The 20-per-page paging of the list is unchanged."
    cites:
      - "artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L31"
      - "context/conventions.md#lists"
  - id: REQ-11
    title: Existing filters unchanged
    type: constraint
    status: known
    statement: "The existing doctor, patient, date range and Status filters are unchanged."
    cites:
      - "artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L31"
      - "artifacts/HMS-30/run_01M81VYRN04SNXNMA559N6SS01/ticket.md#L17"
unknowns: []
questions: []
answers:
  - id: A-01
    kind: assumption
    about: REQ-7
    answer: "Yes, confirm"
    requirements: [REQ-7]
  - id: A-02
    kind: question
    about: Q-01
    answer: "Use ticket definition"
    requirements: [REQ-1, REQ-4]
glossary:
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: Cancelled, found: true}
  - {term: No-show, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Appointment list, found: false}
---
# Requirements review: HMS-30

## Summary
HMS-30 asks the appointment list to show, next to its existing total, one count per appointment
status (Scheduled, Completed, Cancelled, No-show), each linking to the list filtered by that status.
The counts span all appointments matching the doctor/patient and date-range filters, ignore the
Status filter, and must sum to the total under "Any status". The main risk is regressing the
existing table, paging or filters, which the ticket requires to stay unchanged; a second risk is the
counts drifting from the total if they are computed differently from the list query.

## Requirements
- REQ-1 Per-status counts next to the total (known): above the table, next to the total, one count per status.
- REQ-2 Fixed status order (known): Scheduled, Completed, Cancelled, No-show.
- REQ-3 Count label format (known): label then number, joined by " · ".
- REQ-4 Counts cover all matching appointments (known): doctor, patient and date range filters, all pages.
- REQ-5 Counts ignore the Status filter (known): all four statuses always shown.
- REQ-6 Zero counts are shown (known): an empty status shows 0.
- REQ-7 Each count links to the filtered list (known): Status set to that status, other filters kept, page 1.
- REQ-8 Counts add up to the total (known): under "Any status" the four counts sum to the total.
- REQ-9 Table and order unchanged (known).
- REQ-10 Paging unchanged (known): 20 per page.
- REQ-11 Existing filters unchanged (known).

## Known
- REQ-1 to REQ-6, REQ-8 to REQ-11: stated in the ticket's acceptance criteria (lines 22-31).
- REQ-1, REQ-4: "appointment list" taken as the ticket defines it (A-02).
- REQ-7: ticket line 30, with the kept filters and page 1 confirmed by the user (A-01).

## Assumed
None

## Unknown
None

## Open questions
No open questions.

Answered in the IDE:
- A-01 (REQ-7 assumption): count links keep the doctor/patient and date-range filters, set Status and show page 1 → "Yes, confirm" (user). Decides REQ-7. No mismatch.
- A-02 (Q-01): "Appointment list" is not in the glossary; use the ticket's definition (lines 17-19)? → "Use ticket definition" (user). Decides REQ-1, REQ-4. No mismatch.

## Out of scope
- Adding "Appointment list" to the glossary: the glossary is context, not part of this ticket.
- How the counts reach the page (API shape or query): design decides it.

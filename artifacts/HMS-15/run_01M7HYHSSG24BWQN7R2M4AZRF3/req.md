---
schema: intent.v1
run_id: run_01M7HYHSSG24BWQN7R2M4AZRF3
step: req
attempt: 1
skill: requirement-review@1.3.0
inputs:
  - path: artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/ticket.md
    sha256: fabcba875260b5f02519d540d4623977ee7b6804fa470c405ae1de5d4702e96a
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The patient page is the web route `/patients/:id`."
    cites: ["context/generated/api-inventory.json#ui:/patients/:id"]
  - id: C2
    text: "An appointment's detail page is the web route `/appointments/:id`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]
  - id: C3
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C4
    text: "A patient never has two `overlapping` Scheduled appointments, so the earliest upcoming Scheduled appointment is unique."
    cites: ["context/intent-baseline.md#booking-rules"]
  - id: C5
    text: "A `Scheduled` appointment is one that is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C6
    text: "The web app has a shared date-and-time formatter `dateTime`."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/lib/format.ts:dateTime"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Next appointment line above the Appointments table
    type: functional
    status: known
    statement: "Above the Appointments table, the patient page shows the label `Next appointment:` when the patient has a next appointment."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/ticket.md#L20"]
  - id: REQ-2
    title: Date and time in the table's format
    type: functional
    status: known
    statement: "The Next appointment line shows the appointment's date and time in the `same format as the table`."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/ticket.md#L20-L21"]
  - id: REQ-3
    title: Doctor's name on the line
    type: functional
    status: known
    statement: "The Next appointment line shows the `doctor's name` of that appointment."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/ticket.md#L21"]
  - id: REQ-4
    title: Which appointment is next
    type: functional
    status: known
    statement: "The next appointment is the patient's `earliest` Scheduled appointment that starts after now."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/ticket.md#L21-L22", "context/intent-baseline.md#booking-rules"]
  - id: REQ-5
    title: Empty state
    type: functional
    status: known
    statement: "When the patient has no Scheduled appointment that starts after now, the page shows `No upcoming appointment`."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/ticket.md#L23"]
  - id: REQ-6
    title: Closed appointments are never next
    type: functional
    status: known
    statement: "Cancelled, Completed and `No-show` appointments are never used as the next appointment."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/ticket.md#L24"]
  - id: REQ-7
    title: Started appointments are never next
    type: functional
    status: known
    statement: "A Scheduled appointment that has `already started` is never used as the next appointment."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/ticket.md#L24-L25"]
  - id: REQ-8
    title: Rest of the patient page unchanged
    type: constraint
    status: known
    statement: "The rest of the patient page, including the `Book appointment` button and the Appointments table, is unchanged."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/ticket.md#L26-L27"]
  - id: REQ-9
    title: Hospital time zone
    type: constraint
    status: known
    statement: "Now and appointment start times are compared and shown in the hospital time zone `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: REQ-10
    title: Now is taken at page load
    type: functional
    status: known
    statement: "Now is taken when the patient page loads; the Next appointment line does not refresh by itself while the page stays open (`Yes, confirm`)."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/ticket.md#L22", "runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-02"]
  - id: REQ-11
    title: Link to the appointment detail page
    type: functional
    status: known
    statement: "The next appointment on the line links to that appointment's detail page `/appointments/:id`."
    cites: ["runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-01", "context/generated/api-inventory.json#ui:/appointments/:id"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Link to appointment detail"
    requirements: [REQ-11]
  - id: A-02
    kind: assumption
    about: REQ-10
    answer: "Yes, confirm"
    requirements: [REQ-10]
glossary:
  - {term: Front-desk staff, found: true}
  - {term: Scheduled, found: true}
  - {term: Cancelled, found: true}
  - {term: Completed, found: true}
  - {term: No-show, found: true}
---
# Requirements review: HMS-15

## Summary
HMS-15 asks for a "Next appointment:" line above the Appointments table on the patient page (`/patients/:id`), showing the date, time and doctor of the patient's earliest Scheduled appointment that starts after now, or "No upcoming appointment" when there is none. Closed appointments (Cancelled, Completed, No-show) and Scheduled appointments that have already started are excluded. The rest of the page must stay unchanged. In the IDE the user decided the line links to the appointment's detail page and confirmed that "now" is taken at page load. The main risks are matching the table's date format exactly and applying the hospital time zone `Asia/Kolkata` when comparing with now.

## Requirements
- REQ-1 Next appointment line above the Appointments table (known): the patient page shows "Next appointment:" above the Appointments table.
- REQ-2 Date and time in the table's format (known): the line shows the date and time in the same format as the table.
- REQ-3 Doctor's name on the line (known): the line shows the appointment's doctor's name.
- REQ-4 Which appointment is next (known): the earliest Scheduled appointment that starts after now; it is unique because a patient never has two overlapping Scheduled appointments.
- REQ-5 Empty state (known): with no such appointment the page shows "No upcoming appointment".
- REQ-6 Closed appointments are never next (known): Cancelled, Completed and No-show appointments are excluded.
- REQ-7 Started appointments are never next (known): Scheduled appointments that have already started are excluded.
- REQ-8 Rest of the patient page unchanged (known): including the Book appointment button and the Appointments table.
- REQ-9 Hospital time zone (known): now and start times are compared and shown in Asia/Kolkata.
- REQ-10 Now is taken at page load (known): no live refresh while the page stays open.
- REQ-11 Link to the appointment detail page (known): the next appointment links to `/appointments/:id`.

## Known
- REQ-1 to REQ-8: stated in the ticket's acceptance criteria (ticket L20-L27).
- REQ-9: baseline constraint, hospital time zone Asia/Kolkata.
- REQ-10: assumption confirmed by the user in the IDE (A-02).
- REQ-11: decided by the user in the IDE (A-01).

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, product decision, answered by the user): should the Next appointment line link to the appointment's detail page? Answer: "Link to appointment detail". Decided REQ-11. No mismatch: the ticket is silent on a link.
- A-02 (REQ-10 assumption, answered by the user): is now taken at page load with no live refresh? Answer: "Yes, confirm". Decided REQ-10. No mismatch.

## Out of scope
- Changes to the Appointments table, its sorting or the Book appointment button (excluded by ticket L26-L27).
- Showing the next appointment anywhere other than the patient page (the ticket names only the patient page).
- Live updating of the line while the page is open (excluded by A-02).

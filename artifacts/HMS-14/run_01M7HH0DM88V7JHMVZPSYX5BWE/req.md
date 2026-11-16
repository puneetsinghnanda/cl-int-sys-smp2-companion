---
schema: intent.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
step: req
attempt: 1
skill: requirement-review@1.2.0
inputs:
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md
    sha256: a5a319e9733e23864cc3c64cf9006acef2ba8b11f0d9873512230390046914bd
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The appointment page today offers a Scheduled appointment `reschedule (APT-5)`, cancel and mark Completed or No-show."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L15"]
  - id: C2
    text: "A Cancelled or No-show appointment cannot be changed (`APT-8`) and the page offers no next step."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L16"]
  - id: C3
    text: "The book appointment page already accepts doctor, patient, date and `slot from its address`."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L19"]
  - id: C4
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C5
    text: "Completed, Cancelled and No-show appointments `never change`."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C6
    text: "An Inactive doctor or patient is kept for history but `not offered for new use`."
    cites: ["context/glossary.md#inactive"]
  - id: C7
    text: "Concurrent bookings of one slot leave `exactly one`."
    cites: ["context/intent-baseline.md#booking-rules"]
  - id: C8
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C9
    text: "A `cancellation reason` exists exactly when an appointment is CANCELLED."
    cites: ["context/data-model.md#appointments"]
  - id: C10
    text: "The reason for visit has a `300-character` limit."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L32"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Book again on a Cancelled appointment
    type: functional
    status: known
    statement: 'The appointment page of a Cancelled appointment shows a "Book again" button.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L23"]
  - id: REQ-2
    title: Book again on a No-show appointment
    type: functional
    status: known
    statement: 'The appointment page of a No-show appointment shows a "Book again" button.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L23"]
  - id: REQ-3
    title: No Book again on Scheduled or Completed
    type: functional
    status: known
    statement: 'The appointment page of a Scheduled or Completed appointment does not show "Book again".'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L23-L24"]
  - id: REQ-4
    title: Same doctor chosen
    type: functional
    status: known
    statement: '"Book again" opens the book appointment page with the original appointment''s doctor chosen.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L25"]
  - id: REQ-5
    title: Same patient chosen
    type: functional
    status: known
    statement: '"Book again" opens the book appointment page with the original appointment''s patient chosen.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L25"]
  - id: REQ-6
    title: Reason for visit filled in
    type: functional
    status: known
    statement: 'The book appointment page opened by "Book again" has the reason for visit filled in from the original appointment''s own reason for visit.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L25-L26", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-02"]
  - id: REQ-7
    title: Date is today in hospital time
    type: functional
    status: known
    statement: 'The book appointment page opened by "Book again" has today''s date in the hospital time zone (Asia/Kolkata) chosen.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L26", "context/intent-baseline.md#constraints"]
  - id: REQ-8
    title: No slot chosen
    type: functional
    status: known
    statement: 'The book appointment page opened by "Book again" has no slot chosen.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L26"]
  - id: REQ-9
    title: Disabled when doctor Inactive
    type: functional
    status: known
    statement: 'When the doctor is now Inactive, "Book again" is disabled and says "Doctor is inactive".'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L27-L28"]
  - id: REQ-10
    title: Disabled when patient Inactive
    type: functional
    status: known
    statement: 'When the patient is now Inactive, "Book again" is disabled and says "Patient is inactive".'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L27-L28"]
  - id: REQ-11
    title: Both reasons when both Inactive
    type: functional
    status: known
    statement: 'When both the doctor and the patient are now Inactive, the disabled "Book again" shows both reasons: "Doctor is inactive" and "Patient is inactive".'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L27-L28", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-01"]
  - id: REQ-12
    title: Existing booking rules unchanged
    type: constraint
    status: known
    statement: 'Booking from the page opened by "Book again" follows the existing rules unchanged, including free slots (APT-2), no overlapping appointments for the patient (APT-3) and "slot no longer available" (APT-4).'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L29-L30", "context/intent-baseline.md#booking-rules"]
  - id: REQ-13
    title: New appointment has its own ID
    type: functional
    status: known
    statement: 'An appointment booked via "Book again" is a new appointment with its own ID.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L30-L31"]
  - id: REQ-14
    title: Original appointment untouched
    type: constraint
    status: known
    statement: 'Booking again leaves the original appointment and its history as they were (APT-8, APT-11).'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L31", "context/intent-baseline.md#closed-appointments"]
  - id: REQ-15
    title: Reason stays editable
    type: functional
    status: known
    statement: 'The filled-in reason for visit can still be edited before booking.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L32"]
  - id: REQ-16
    title: Reason keeps its limit
    type: constraint
    status: known
    statement: 'The reason for visit keeps its 300-character limit.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L32"]
  - id: REQ-17
    title: No regression on both pages
    type: non-functional
    status: known
    statement: 'The rest of the appointment page and the book appointment page work as today.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L33"]
  - id: REQ-18
    title: Empty reason stays empty
    type: functional
    status: known
    statement: 'When the original appointment has no reason for visit, the book appointment page opens with the reason field empty.'
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/ticket.md#L25-L26", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-03"]
  - id: REQ-19
    title: Cancellation reason not copied
    type: functional
    status: known
    statement: 'The cancellation reason of a Cancelled original appointment is not copied into the new booking.'
    cites: ["context/data-model.md#appointments", "runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-02"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Show both (proposed default)"
    requirements: [REQ-11]
  - id: A-02
    kind: question
    about: Q-02
    answer: "Yes, visit reason (proposed default)"
    requirements: [REQ-6, REQ-19]
  - id: A-03
    kind: assumption
    about: REQ-18
    answer: "Yes, confirm"
    requirements: [REQ-18]
glossary:
  - {term: Cancelled, found: true}
  - {term: No-show, found: true}
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: Inactive, found: true}
  - {term: Slot, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Reason for visit, found: false}
---
# Requirements review: HMS-14

## Summary
HMS-14 adds a "Book again" button to the appointment page of Cancelled and No-show appointments, opening the book appointment page with the same doctor and patient, the reason for visit filled in, today's date in hospital time and no slot. The button is disabled with a stated reason when the doctor or patient is now Inactive. Booking stays under the existing rules (APT-1 to APT-4) and the original, closed appointment and its history do not change. The main risks are passing the reason for visit to the book page, which today takes only doctor, patient, date and slot from its address, and regressions on two existing pages.

## Requirements
- REQ-1 Book again on a Cancelled appointment (known): the Cancelled appointment page shows "Book again".
- REQ-2 Book again on a No-show appointment (known): the No-show appointment page shows "Book again".
- REQ-3 No Book again on Scheduled or Completed (known): those pages do not show it.
- REQ-4 Same doctor chosen (known): the book page opens with the original doctor chosen.
- REQ-5 Same patient chosen (known): the book page opens with the original patient chosen.
- REQ-6 Reason for visit filled in (known): from the original appointment's own reason for visit.
- REQ-7 Date is today in hospital time (known): today in Asia/Kolkata.
- REQ-8 No slot chosen (known).
- REQ-9 Disabled when doctor Inactive (known): says "Doctor is inactive".
- REQ-10 Disabled when patient Inactive (known): says "Patient is inactive".
- REQ-11 Both reasons when both Inactive (known): shows "Doctor is inactive" and "Patient is inactive".
- REQ-12 Existing booking rules unchanged (known): APT-2, APT-3, APT-4 apply as today.
- REQ-13 New appointment has its own ID (known).
- REQ-14 Original appointment untouched (known): original and its history stay as they were.
- REQ-15 Reason stays editable (known).
- REQ-16 Reason keeps its limit (known): 300 characters.
- REQ-17 No regression on both pages (known).
- REQ-18 Empty reason stays empty (known).
- REQ-19 Cancellation reason not copied (known).

## Known
- REQ-1 to REQ-5, REQ-7 to REQ-10, REQ-12 to REQ-17: stated in the ticket acceptance criteria (L23-L33); REQ-7 also uses the baseline time zone, REQ-12 and REQ-14 match the baseline booking and closed-appointment rules.
- REQ-6, REQ-19: ticket L25-L26 plus A-02 (the appointment's own reason for visit, never the cancellation reason).
- REQ-11: decided by A-01.
- REQ-18: assumption confirmed by A-03.

## Assumed
None

## Unknown
None

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by eng-dffd): what a disabled "Book again" says when both doctor and patient are Inactive → "Show both (proposed default)". Decides REQ-11. No mismatch; the ticket did not cover this case.
- A-02 (Q-02, answered by eng-dffd): "reason for visit" is the appointment's own reason-for-visit field, not the cancellation reason → "Yes, visit reason (proposed default)". Decides REQ-6, REQ-19. No mismatch.
- A-03 (REQ-18, answered by eng-dffd): an original with no reason for visit opens the book page with the reason empty → "Yes, confirm". Decides REQ-18. No mismatch.

## Out of scope
- How the reason for visit reaches the book appointment page (the page today takes doctor, patient, date and slot from its address, ticket L18-L19): a design decision for the design step.
- Any change to booking rules APT-1 to APT-4 or to the appointment API: the ticket requires them unchanged.
- Glossary: "Reason for visit" is not in the glossary; its meaning was settled by A-02. Adding it to the glossary is left to the glossary owner.

---
schema: intent.v1
run_id: run_01M893GZKGGY9C7GYRJCH088PF
step: req
attempt: 2
skill: requirement-review@1.5.0
inputs:
  - path: artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md
    sha256: 5226a3c6aef9dfec7d21c40f09a58f06604984ce196e2f16ff8e86e20d37a5ee
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The ticket asks for a new appointment state `Stale`."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md#L16"]
  - id: C2
    text: "Today only `Scheduled` appointments can change."
    cites: ["context/glossary.md#scheduled"]
  - id: C3
    text: "`Completed, Cancelled and No-show` appointments never change."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C4
    text: "Front-desk staff `book, move and close appointments`."
    cites: ["context/glossary.md#front-desk-staff"]
  - id: C5
    text: "A Completed appointment is closed by staff once its `start time` has passed."
    cites: ["context/glossary.md#completed"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: New appointment state Stale
    type: functional
    status: known
    statement: "Appointments have a new state `Stale`."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md#L16"]
  - id: REQ-2
    title: Past Scheduled appointment becomes Stale after 24h
    type: functional
    status: known
    statement: "A Scheduled appointment that has not been marked Completed or `rescheduled` by 24h after its start time is marked Stale."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md#L17-L18", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-02"]
  - id: REQ-3
    title: An automatic job marks Stale
    type: functional
    status: known
    statement: "An automatic background job, not staff, marks qualifying appointments `Stale`."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md#L17-L18", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-01"]
  - id: REQ-4
    title: Completed within the day is not Stale
    type: functional
    status: known
    statement: "A past Scheduled appointment marked Completed within 24h after its start time is not marked `Stale`."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md#L19", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-02"]
  - id: REQ-5
    title: Rescheduled within the day is not Stale
    type: functional
    status: known
    statement: "A past Scheduled appointment rescheduled within 24h after its start time is not marked `Stale`."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md#L20", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-02"]
  - id: REQ-6
    title: Cancelled or No-show within the day is not Stale
    type: functional
    status: known
    statement: "An appointment cancelled or marked No-show within the one day is no longer `Scheduled` and is not marked Stale."
    cites: ["context/glossary.md#scheduled", "artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md#L17-L18", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-05"]
  - id: REQ-7
    title: Staff can mark a Stale appointment Completed
    type: functional
    status: known
    statement: "Staff can still mark a Stale appointment `Completed`."
    cites: ["context/glossary.md#completed", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-03"]
  - id: REQ-8
    title: Staff can mark a Stale appointment No-show
    type: functional
    status: known
    statement: "Staff can still mark a Stale appointment `No-show`."
    cites: ["context/glossary.md#no-show", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-03"]
  - id: REQ-9
    title: Staff can cancel a Stale appointment
    type: functional
    status: known
    statement: "Staff can still cancel a Stale appointment `with a reason`."
    cites: ["context/glossary.md#cancelled", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-03"]
  - id: REQ-10
    title: Staff can reschedule a Stale appointment
    type: functional
    status: known
    statement: "Staff can also `reschedule` a Stale appointment."
    cites: ["runs/run_01M893GZKGGY9C7GYRJCH088PF/open-questions.md#q-01", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-07"]
  - id: REQ-11
    title: Stale shows wherever a status shows
    type: functional
    status: known
    statement: "The web app shows `Stale` as the status label wherever an appointment status is shown today."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md#L16", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-04"]
  - id: REQ-12
    title: Stale in the list's status filter
    type: functional
    status: known
    statement: "The appointment list's status filter offers `Stale`."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md#L16", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-04"]
  - id: REQ-13
    title: No new dashboard tile
    type: constraint
    status: known
    statement: "The `Dashboard` gets no new tile for Stale appointments."
    cites: ["context/glossary.md#dashboard", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-04"]
  - id: REQ-14
    title: Glossary defines Stale
    type: constraint
    status: known
    statement: "The glossary has an entry for `Stale` that matches REQ-2."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/ticket.md#L16", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-06"]
  - id: REQ-15
    title: A rescheduled Stale appointment is Scheduled again
    type: functional
    status: known
    statement: "A Stale appointment that staff reschedule becomes `Scheduled` at its new time."
    cites: ["context/glossary.md#scheduled", "runs/run_01M893GZKGGY9C7GYRJCH088PF/open-questions.md#q-01", "runs/run_01M893GZKGGY9C7GYRJCH088PF/ide-answers.md#a-07"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Automatic job (proposed default)"
    requirements: [REQ-3]
  - id: A-02
    kind: question
    about: Q-02
    answer: "24h after start time (proposed default)"
    requirements: [REQ-2, REQ-4, REQ-5]
  - id: A-03
    kind: question
    about: Q-03
    answer: "Staff can still close it (proposed default)"
    requirements: [REQ-7, REQ-8, REQ-9]
    mismatch:
      text: "The glossary says only Scheduled appointments can change; the user decided staff can still close a Stale appointment."
      cites: ["context/glossary.md#scheduled"]
  - id: A-04
    kind: question
    about: Q-04
    answer: "Wherever status shows (proposed default)"
    requirements: [REQ-11, REQ-12, REQ-13]
  - id: A-05
    kind: assumption
    about: REQ-6
    answer: "Yes, confirm"
    requirements: [REQ-6]
  - id: A-06
    kind: question
    about: Q-05
    answer: "Add entry from REQ-2 (proposed default)"
    requirements: [REQ-14]
  - id: A-07
    kind: assumption
    about: REQ-10
    answer: "Yes, confirm"
    requirements: [REQ-10, REQ-15]
glossary:
  - {term: Stale, found: false}
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: No-show, found: true}
  - {term: Cancelled, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Dashboard, found: true}
---
# Requirements review: HMS-34

## Summary
HMS-34 adds an appointment state Stale for Scheduled appointments whose time is past and that were
neither marked Completed nor rescheduled within one day. The user decided an automatic background job
marks them, 24h after the start time. Stale only flags that the appointment needs attention: staff can
still close it or reschedule it, and a rescheduled one is Scheduled again. The main risk is that Stale
is a state that can still change, unlike the existing closed states (C3), which departs from the
glossary rule that only Scheduled appointments can change (C2).

## Requirements
- REQ-1 New appointment state Stale (known): appointments have a new state Stale.
- REQ-2 Past Scheduled appointment becomes Stale after 24h (known): not Completed or rescheduled by 24h after its start time means Stale.
- REQ-3 An automatic job marks Stale (known): an automatic background job, not staff, marks them.
- REQ-4 Completed within the day is not Stale (known).
- REQ-5 Rescheduled within the day is not Stale (known).
- REQ-6 Cancelled or No-show within the day is not Stale (known): no longer Scheduled.
- REQ-7 Staff can mark a Stale appointment Completed (known).
- REQ-8 Staff can mark a Stale appointment No-show (known).
- REQ-9 Staff can cancel a Stale appointment with a reason (known).
- REQ-10 Staff can reschedule a Stale appointment (known).
- REQ-11 Stale shows as the status label wherever a status shows (known).
- REQ-12 The appointment list's status filter offers Stale (known).
- REQ-13 No new dashboard tile (known).
- REQ-14 The glossary has an entry for Stale that matches REQ-2 (known).
- REQ-15 A rescheduled Stale appointment becomes Scheduled at its new time (known).

## Known
- REQ-1, REQ-4, REQ-5: stated in the ticket (#L16, #L19, #L20); A-02 fixes "one day" as 24h after the start time.
- REQ-2: ticket #L17-L18 with A-02.
- REQ-3: A-01.
- REQ-6: assumption confirmed in A-05; follows from glossary#scheduled.
- REQ-7 to REQ-9: A-03.
- REQ-10: answer to runs/run_01M893GZKGGY9C7GYRJCH088PF/open-questions.md#q-01, confirmed in A-07.
- REQ-11 to REQ-13: A-04.
- REQ-14: A-06.
- REQ-15: assumption confirmed in A-07, based on the answer to runs/run_01M893GZKGGY9C7GYRJCH088PF/open-questions.md#q-01.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in open-questions.md: runs/run_01M893GZKGGY9C7GYRJCH088PF/open-questions.md#q-01 (can staff also reschedule a Stale appointment): "Yes, they can reschedule as well, its only a state change to let them know that this appointment needs attention.", answered by the product owner; decides REQ-10 and is the basis for REQ-15.

Answered in the IDE:
- A-01 (Q-01, who or what marks Stale): "Automatic job (proposed default)", answered by eng-dbe4; decides REQ-3.
- A-02 (Q-02, when the one day is counted from): "24h after start time (proposed default)", answered by eng-dbe4; decides REQ-2, REQ-4, REQ-5.
- A-03 (Q-03, can Stale change afterwards): "Staff can still close it (proposed default)", answered by eng-dbe4; decides REQ-7 to REQ-9. Mismatch: the glossary says only Scheduled appointments can change; the user decided staff can still close a Stale appointment.
- A-04 (Q-04, where Stale shows): "Wherever status shows (proposed default)", answered by eng-dbe4; decides REQ-11, REQ-12, REQ-13.
- A-05 (REQ-6 assumption): "Yes, confirm", answered by eng-dbe4; decides REQ-6.
- A-06 (Q-05, glossary entry): "Add entry from REQ-2 (proposed default)", answered by eng-dbe4; decides REQ-14.
- A-07 (REQ-10 assumption that a rescheduled Stale appointment is Scheduled again): "Yes, confirm", answered by eng-dbe4; decides REQ-10 and REQ-15.

## Out of scope
- How often the background job runs, and how soon after the 24h an appointment must show as Stale: neither the ticket nor the answers give a value, so this is left to design.

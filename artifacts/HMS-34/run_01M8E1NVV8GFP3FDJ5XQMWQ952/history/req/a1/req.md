---
schema: intent.v1
run_id: run_01M8E1NVV8GFP3FDJ5XQMWQ952
step: req
attempt: 1
skill: requirement-review@1.5.0
inputs:
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ticket.md
    sha256: be5af0f17b0fd320df97277e8bf54a73778f08b5c9b6b2283d6348ef18954927
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Appointment status today is SCHEDULED, COMPLETED, CANCELLED or `NO_SHOW`; there is no Stale value."
    cites: ["context/data-model.md#appointments"]
  - id: C2
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C3
    text: "No `background jobs` exist yet in the system."
    cites: ["context/architecture.md#open-points"]
  - id: C4
    text: "Completed, Cancelled and No-show appointments `never change`."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C5
    text: "In the MVP the actor is always `staff`."
    cites: ["context/glossary.md#actor"]
  - id: C6
    text: "Appointment instants are stored in `UTC`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C7
    text: "A Scheduled appointment can be `rescheduled`, cancelled, or marked Completed or No-show."
    cites: ["context/system-baseline.md#key-flows"]
  - id: C8
    text: "Only `Scheduled` appointments can change today."
    cites: ["context/glossary.md#scheduled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: New appointment state Stale
    type: functional
    status: known
    statement: "Appointments have a new state `Stale`, alongside SCHEDULED, COMPLETED, CANCELLED and NO_SHOW."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ticket.md#L16", "context/data-model.md#appointments"]
  - id: REQ-2
    title: Overdue Scheduled appointment becomes Stale
    type: functional
    status: known
    statement: "A Scheduled appointment that is still not marked Completed or rescheduled `24h after start time` is marked Stale."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ticket.md#L17-L18", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-01"]
  - id: REQ-3
    title: Completed within 24h is not Stale
    type: functional
    status: known
    statement: "A past Scheduled appointment that is marked `Completed` before 24h after start time is not marked Stale."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ticket.md#L19", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-01"]
  - id: REQ-4
    title: Rescheduled within 24h is not Stale
    type: functional
    status: known
    statement: "A past Scheduled appointment that is `rescheduled` before 24h after start time is not marked Stale."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ticket.md#L20", "runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-01"]
  - id: REQ-5
    title: Marking runs as a scheduled background job
    type: constraint
    status: known
    statement: "Appointments are marked Stale by a `Scheduled background job`, the system's first background job."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-02", "context/architecture.md#open-points"]
  - id: REQ-6
    title: Staff can still change a Stale appointment
    type: functional
    status: known
    statement: "Staff can still mark a Stale appointment Completed or No-show, cancel it or reschedule it (`complete, no-show, cancel, reschedule`)."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-03"]
  - id: REQ-7
    title: Closed appointments never become Stale
    type: constraint
    status: known
    statement: "Completed, Cancelled and No-show appointments are never marked Stale, because they `never change`."
    cites: ["context/intent-baseline.md#closed-appointments", "artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ticket.md#L17"]
  - id: REQ-8
    title: Stale marking is recorded with actor system
    type: functional
    status: known
    statement: "Each change to Stale is recorded in appointment_event with actor `system`."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-04", "context/data-model.md#appointments"]
  - id: REQ-9
    title: Glossary gains the term Stale
    type: constraint
    status: known
    statement: "The glossary gains a `Stale` entry: a Scheduled appointment whose start time passed 24 hours ago without being marked Completed or rescheduled; it stays open."
    cites: ["runs/run_01M8E1NVV8GFP3FDJ5XQMWQ952/ide-answers.md#a-05"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "24h after start time (proposed default)"
    requirements: [REQ-2, REQ-3, REQ-4]
  - id: A-02
    kind: question
    about: Q-02
    answer: "Scheduled background job (proposed default)"
    requirements: [REQ-5]
  - id: A-03
    kind: question
    about: Q-03
    answer: "Yes: complete, no-show, cancel, reschedule (proposed default)"
    requirements: [REQ-6]
    mismatch:
      text: "The glossary says only Scheduled appointments can change; the user decided a Stale appointment can also be completed, marked no-show, cancelled or rescheduled."
      cites: ["context/glossary.md#scheduled"]
  - id: A-04
    kind: assumption
    about: REQ-8
    answer: "Yes, but actor `system`"
    requirements: [REQ-8]
    mismatch:
      text: "The glossary says the MVP actor is always staff; the user decided the Stale marking is recorded with a new actor system."
      cites: ["context/glossary.md#actor"]
  - id: A-05
    kind: question
    about: Q-04
    answer: "Yes, add that entry (proposed default)"
    requirements: [REQ-9]
glossary:
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: No-show, found: true}
  - {term: Cancelled, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Actor, found: true}
  - {term: Stale, found: false}
---
# Requirements review: HMS-34

## Summary
HMS-34 asks for a new appointment state, Stale, for Scheduled appointments whose time has passed
and that were neither marked Completed nor rescheduled within one day, so front-desk staff do not
see them as upcoming. The user fixed "one day" as 24 hours after the start time, chose a scheduled
background job to do the marking, kept Stale an open state staff can still change, and asked for
the change to be recorded with a new actor `system`. Main risks: this is the system's first
background job, and both the open Stale state and the `system` actor depart from today's glossary
rules ("only Scheduled can change", "actor is always staff").

## Requirements
- REQ-1 New appointment state Stale (known): appointments have a new state Stale.
- REQ-2 Overdue Scheduled appointment becomes Stale (known): Scheduled, start time more than 24 hours past, not Completed or rescheduled in that time → Stale.
- REQ-3 Completed within 24 hours is not Stale (known).
- REQ-4 Rescheduled within 24 hours is not Stale (known).
- REQ-5 Marking runs as a scheduled background job (known).
- REQ-6 Staff can still change a Stale appointment (known): complete, no-show, cancel or reschedule.
- REQ-7 Closed appointments never become Stale (known).
- REQ-8 Stale marking is recorded with actor system (known).
- REQ-9 Glossary gains the term Stale (known).

## Known
- REQ-1: ticket acceptance criterion 1.
- REQ-2: ticket acceptance criterion 2, window from A-01.
- REQ-3: ticket acceptance criterion 3, window from A-01.
- REQ-4: ticket acceptance criterion 4, window from A-01.
- REQ-5: A-02; no background jobs exist yet.
- REQ-6: A-03.
- REQ-7: baseline closed-appointments rule; the ticket only targets Scheduled appointments.
- REQ-8: A-04; appointment_event records every change.
- REQ-9: A-05.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by eng-dffd): when does "one day" run from? → 24h after start time. Decides REQ-2, REQ-3, REQ-4.
- A-02 (Q-02, answered by eng-dffd): how is Stale applied? → Scheduled background job. Decides REQ-5.
- A-03 (Q-03, answered by eng-dffd): can staff still change a Stale appointment? → Yes: complete, no-show, cancel, reschedule. Decides REQ-6. Mismatch: the glossary says only Scheduled appointments can change; the user decided Stale ones can too.
- A-04 (REQ-8, answered by eng-dffd): record the change in appointment_event with actor staff? → Yes, but actor `system`. Decides REQ-8. Mismatch: the glossary says the MVP actor is always staff.
- A-05 (Q-04, answered by eng-dffd): add Stale to the glossary? → Yes, add that entry. Decides REQ-9.

## Out of scope
- How Stale appointments are shown in lists, filters or dashboard tiles: the ticket does not ask for a display change.
- How often the background job runs: a design choice for the design step.

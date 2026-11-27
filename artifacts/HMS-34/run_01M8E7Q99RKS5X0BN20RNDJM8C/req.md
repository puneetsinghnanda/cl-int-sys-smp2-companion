---
schema: intent.v1
run_id: run_01M8E7Q99RKS5X0BN20RNDJM8C
step: req
attempt: 2
skill: requirement-review@1.5.0
inputs:
  - path: artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/ticket.md
    sha256: 0ab43548adb1663d10bcc98eac4e5d7ed17c971a74490b85e2de0add28d9a927
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Appointment status today is one of SCHEDULED, COMPLETED, CANCELLED or `NO_SHOW`; there is no Stale value yet."
    cites: ["context/data-model.md#appointments"]
  - id: C2
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C3
    text: "`No background jobs exist yet`; marking appointments Stale over time would be the first time-driven change."
    cites: ["context/architecture.md#open-points"]
  - id: C4
    text: "Completed, Cancelled and No-show appointments `never change`."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C5
    text: "The glossary says `Only Scheduled appointments can change`."
    cites: ["context/glossary.md#scheduled"]
  - id: C6
    text: "In the MVP the actor recorded on changes is always `staff`."
    cites: ["context/glossary.md#actor"]
  - id: C7
    text: "The hospital time zone is `Asia/Kolkata`; instants are stored in UTC."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C8
    text: "A Scheduled appointment can be `rescheduled`, cancelled with a reason, or marked Completed or No-show."
    cites: ["context/system-baseline.md#key-flows"]
  - id: C9
    text: "The glossary has no entry for `Stale`; the term comes from the ticket."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/ticket.md#L16"]
  - id: C10
    text: "Booking rules count only Scheduled appointments: `A patient never has two overlapping Scheduled appointments`."
    cites: ["context/intent-baseline.md#booking-rules"]
  - id: C11
    text: "The glossary defines Cancelled as `called off before it took place`."
    cites: ["context/glossary.md#cancelled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: New appointment state Stale
    type: functional
    status: known
    statement: "Appointments have a new state `Stale`."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/ticket.md#L16"]
  - id: REQ-2
    title: Scheduled appointment becomes Stale 24h after its start
    type: functional
    status: known
    statement: "A Scheduled appointment that is still Scheduled `24h after start` time is marked Stale."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/ticket.md#L17-L18", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-01"]
  - id: REQ-3
    title: Completed in time is not Stale
    type: functional
    status: known
    statement: "A past Scheduled appointment marked `Completed` before 24h after start is not marked Stale."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/ticket.md#L19", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-01"]
  - id: REQ-4
    title: Rescheduled in time is not Stale
    type: functional
    status: known
    statement: "A past Scheduled appointment `rescheduled` before 24h after start is not marked Stale."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/ticket.md#L20", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-01"]
  - id: REQ-5
    title: Only Scheduled appointments become Stale
    type: constraint
    status: known
    statement: "Only appointments in state `Scheduled` are marked Stale; Completed, Cancelled and No-show appointments are never marked Stale."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/ticket.md#L17", "context/intent-baseline.md#closed-appointments"]
  - id: REQ-6
    title: Staff can close a Stale appointment
    type: functional
    status: known
    statement: "Staff can still mark a Stale appointment Completed, No-show or Cancelled (`Cancelled allowed`)."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-02", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-07"]
  - id: REQ-7
    title: Rescheduling a Stale appointment makes it Scheduled again
    type: functional
    status: known
    statement: "Staff can still reschedule a Stale appointment; it is then `Scheduled again` at its new time and counts for the booking overlap rules."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-02", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-06", "context/intent-baseline.md#booking-rules"]
  - id: REQ-8
    title: Marking Stale is recorded in appointment history
    type: functional
    status: known
    statement: "When an appointment is marked Stale, the change is recorded in `appointment_event`."
    cites: ["context/data-model.md#appointments", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-03"]
  - id: REQ-9
    title: Actor for the Stale change is system
    type: functional
    status: known
    statement: "The appointment_event row for marking Stale carries the actor `system`."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-03"]
  - id: REQ-10
    title: Stale shown in list and on the appointment page
    type: functional
    status: known
    statement: "A Stale appointment is shown as Stale in the appointment list and on the appointment page (`Lists and page, with filter`)."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-04"]
  - id: REQ-11
    title: Stale offered as a status filter
    type: functional
    status: known
    statement: "The appointment list offers Stale as a status filter (`Lists and page, with filter`)."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-04"]
  - id: REQ-12
    title: Glossary entry for Stale
    type: constraint
    status: known
    statement: "The glossary gains an entry for Stale: a Scheduled appointment still open 24h after start, which staff can still close or reschedule (`Add entry`)."
    cites: ["runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-05", "runs/run_01M8E7Q99RKS5X0BN20RNDJM8C/ide-answers.md#a-01"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "24h after start (proposed default)"
    requirements: [REQ-2, REQ-3, REQ-4]
  - id: A-02
    kind: question
    about: Q-02
    answer: "Can be closed or rescheduled"
    requirements: [REQ-6, REQ-7]
    mismatch:
      text: "The glossary says only Scheduled appointments can change; the user decided a Stale appointment can still be closed or rescheduled."
      cites: ["context/glossary.md#scheduled"]
  - id: A-03
    kind: question
    about: Q-03
    answer: "Event, actor system"
    requirements: [REQ-8, REQ-9]
    mismatch:
      text: "The glossary says the actor is always staff in the MVP; the user decided the Stale change carries the actor system."
      cites: ["context/glossary.md#actor"]
  - id: A-04
    kind: question
    about: Q-04
    answer: "Lists and page, with filter (proposed default)"
    requirements: [REQ-10, REQ-11]
  - id: A-05
    kind: question
    about: Q-05
    answer: "Add entry (proposed default)"
    requirements: [REQ-12]
  - id: A-06
    kind: question
    about: Q-06
    answer: "Scheduled again (proposed default)"
    requirements: [REQ-7]
  - id: A-07
    kind: question
    about: Q-07
    answer: "Yes, Cancelled allowed (proposed default)"
    requirements: [REQ-6]
    mismatch:
      text: "The glossary defines Cancelled as called off before it took place; the user decided a Stale (past) appointment can still be marked Cancelled."
      cites: ["context/glossary.md#cancelled"]
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
HMS-34 adds a new appointment state, Stale, for Scheduled appointments that are still open 24h after
their start time, so they no longer look like upcoming appointments. Appointments that are Completed
or rescheduled within that window are left alone. The main risks: no background jobs exist yet (C3),
so the time-driven change is new to the system; the user decided Stale can still be closed (including
Cancelled) or rescheduled, which differs from the glossary (C5, C11); a rescheduled Stale appointment
becomes Scheduled again and counts for the overlap rules (C10); and the change is recorded with a new
actor `system`, which differs from the MVP rule that the actor is always staff (C6).

## Requirements
- REQ-1 New appointment state Stale — known — Appointments have a new state Stale.
- REQ-2 Becomes Stale 24h after start — known — A Scheduled appointment still Scheduled 24h after its start time is marked Stale.
- REQ-3 Completed in time is not Stale — known.
- REQ-4 Rescheduled in time is not Stale — known.
- REQ-5 Only Scheduled appointments become Stale — known — Completed, Cancelled and No-show are never marked Stale.
- REQ-6 Staff can close a Stale appointment — known — Completed, No-show or Cancelled.
- REQ-7 Rescheduling a Stale appointment makes it Scheduled again — known — it then counts for the booking overlap rules.
- REQ-8 Marking Stale is recorded in appointment_event — known.
- REQ-9 Actor for the Stale change is system — known.
- REQ-10 Stale shown in list and on the appointment page — known.
- REQ-11 Stale offered as a status filter in the appointment list — known.
- REQ-12 Glossary entry for Stale — known.

## Known
- REQ-1: ticket acceptance criterion 1.
- REQ-2, REQ-3, REQ-4: ticket acceptance criteria 2 to 4, with "within one day" fixed as 24h after the start time by A-01.
- REQ-5: ticket criterion 2 applies to Scheduled appointments only, and closed appointments never change (C4).
- REQ-6: decided by A-02 and A-07.
- REQ-7: decided by A-02 and A-06.
- REQ-8, REQ-9: decided by A-03.
- REQ-10, REQ-11: decided by A-04.
- REQ-12: decided by A-05.

Default (not a question): the stored status code follows the existing upper-case codes (C1), so `STALE`. Trade-off: consistent with SCHEDULED/NO_SHOW, at the cost of one more value in the status check.

## Assumed
None.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, user): one day is measured from when? — "24h after start (proposed default)". Decides REQ-2, REQ-3, REQ-4.
- A-02 (Q-02, user): can staff still change a Stale appointment? — "Can be closed or rescheduled". Decides REQ-6, REQ-7. Mismatch: the glossary says only Scheduled appointments can change; the user decided Stale can still be closed or rescheduled.
- A-03 (Q-03, user): record the change, and with which actor? — "Event, actor system". Decides REQ-8, REQ-9. Mismatch: the glossary says the actor is always staff in the MVP; the user decided the actor is system.
- A-04 (Q-04, user): where is Stale shown? — "Lists and page, with filter (proposed default)". Decides REQ-10, REQ-11.
- A-05 (Q-05, user): add Stale to the glossary? — "Add entry (proposed default)". Decides REQ-12.
- A-06 (Q-06, user): which state does a rescheduled Stale appointment end in? — "Scheduled again (proposed default)". Decides REQ-7.
- A-07 (Q-07, user): can a Stale (past) appointment be marked Cancelled? — "Yes, Cancelled allowed (proposed default)". Decides REQ-6. Mismatch: the glossary defines Cancelled as called off before it took place; the user decided a Stale appointment can still be Cancelled.

## Out of scope
- How the Stale change is triggered (scheduled job, check on read, or on startup) is a design decision, not a requirement; note that no background jobs exist yet (C3).
- A dashboard tile for Stale appointments: not asked for; the user chose lists and page with a filter (A-04).

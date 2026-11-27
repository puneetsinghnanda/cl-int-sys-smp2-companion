---
schema: intent.v1
run_id: run_01M8EC2AGRDG7SJJNPS29RACVE
step: req
attempt: 1
skill: requirement-review@1.6.0
inputs:
  - path: artifacts/HMS-34/run_01M8EC2AGRDG7SJJNPS29RACVE/ticket.md
    sha256: ec7b6920a1dffa08c7dc8929fc78e3ed93758257a8dbd1f21cd6eca100ce3320
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The system has `No background jobs` yet; clean-ups would be the first."
    cites: ["context/architecture.md#open-points"]
  - id: C2
    text: "A reschedule endpoint already exists in `api/src/routes/appointments.ts`."
    cites: ["context/generated/api-inventory.json#http:POST:/api/appointments/:id/reschedule"]
  - id: C3
    text: "Appointment status today is one of `SCHEDULED`, COMPLETED, CANCELLED or NO_SHOW."
    cites: ["context/data-model.md#appointments"]
  - id: C4
    text: "`appointment_event` records every change of an appointment with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C5
    text: "A reschedule is recorded with `from_starts_at` in the appointment history."
    cites: ["context/generated/db-schema.json#column:appointment_event.from_starts_at"]
  - id: C6
    text: "In the MVP the actor is always `staff`."
    cites: ["context/glossary.md#actor"]
  - id: C7
    text: "Exclusion constraints forbid overlapping `Scheduled` appointments per doctor and per patient."
    cites: ["context/architecture.md#double-booking"]
  - id: C8
    text: "Database changes are new SQL files named `YYYYMMDDTHHMMSS__what.sql`."
    cites: ["context/conventions.md#database-changes"]
  - id: C9
    text: "Hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C10
    text: "Glossary says `Only Scheduled appointments can change`."
    cites: ["context/glossary.md#scheduled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
assumed: []
unknown: [U-1]
requirements:
  - id: REQ-1
    title: New appointment state Stale
    type: functional
    status: known
    statement: "Appointments have a new state `Stale`, stored as status value STALE next to SCHEDULED, COMPLETED, CANCELLED and NO_SHOW."
    cites: ["artifacts/HMS-34/run_01M8EC2AGRDG7SJJNPS29RACVE/ticket.md#L16", "context/data-model.md#appointments"]
  - id: REQ-2
    title: Overdue Scheduled appointment becomes Stale
    type: functional
    status: known
    statement: "A `Scheduled` appointment whose time is past, and which is not marked Completed or rescheduled within 24 hours after starts_at, is marked Stale."
    cites: ["artifacts/HMS-34/run_01M8EC2AGRDG7SJJNPS29RACVE/ticket.md#L17-L18", "runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-02"]
  - id: REQ-3
    title: Completed within the window is not Stale
    type: functional
    status: known
    statement: "A past Scheduled appointment that is marked `Completed` within 24 hours after starts_at is not marked Stale."
    cites: ["artifacts/HMS-34/run_01M8EC2AGRDG7SJJNPS29RACVE/ticket.md#L19", "runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-02"]
  - id: REQ-4
    title: Rescheduled within the window is not Stale
    type: functional
    status: known
    statement: "A past Scheduled appointment that is `rescheduled` within 24 hours after starts_at is not marked Stale."
    cites: ["artifacts/HMS-34/run_01M8EC2AGRDG7SJJNPS29RACVE/ticket.md#L20", "runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-02"]
  - id: REQ-5
    title: Window is 24 hours after the start time
    type: functional
    status: known
    statement: "The one-day window ends `24 hours after starts_at` of the appointment."
    cites: ["artifacts/HMS-34/run_01M8EC2AGRDG7SJJNPS29RACVE/ticket.md#L17-L18", "runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-02"]
  - id: REQ-6
    title: A periodic background job marks Stale
    type: functional
    status: known
    statement: "A `Periodic background job` in the API finds overdue Scheduled appointments and sets them to Stale; it is the system's first background job."
    cites: ["runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-01", "context/architecture.md#open-points"]
  - id: REQ-7
    title: A Stale appointment can be rescheduled
    type: functional
    status: known
    statement: "Staff can reschedule a Stale appointment; it then becomes Scheduled again (`Yes, can be rescheduled`)."
    cites: ["runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-03", "runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-05"]
  - id: REQ-8
    title: A Stale appointment can be closed
    type: functional
    status: known
    statement: "Staff can mark a Stale appointment Completed, No-show or Cancelled (`Allow all closing actions`)."
    cites: ["runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-05"]
  - id: REQ-9
    title: Web app shows the Stale label
    type: functional
    status: known
    statement: "The web app shows the status label Stale wherever an appointment's status is shown (lists, appointment page) (`Also add a list filter`)."
    cites: ["runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-06", "artifacts/HMS-34/run_01M8EC2AGRDG7SJJNPS29RACVE/ticket.md#L13-L14"]
  - id: REQ-10
    title: Dashboard upcoming list excludes Stale
    type: functional
    status: known
    statement: "Stale appointments never appear among the dashboard's next upcoming appointments (`Yes, confirm`)."
    cites: ["runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-07", "context/glossary.md#dashboard"]
  - id: REQ-11
    title: Appointments list can be filtered by Stale
    type: functional
    status: known
    statement: "Staff can filter the appointments list by status Stale (`Also add a list filter`)."
    cites: ["runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-06"]
  - id: REQ-12
    title: Marking Stale is recorded in history
    type: functional
    status: known
    statement: "Each change to Stale is recorded in `appointment_event` with its time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: REQ-13
    title: Actor for automatic Stale marking is system
    type: functional
    status: known
    statement: "The history entry for an automatic change to Stale records the actor `system`."
    cites: ["runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-04"]
  - id: REQ-14
    title: Rescheduling a Stale appointment follows the booking rules
    type: constraint
    status: known
    statement: "Rescheduling a Stale appointment must give a slot that is free under the booking rules: inside availability, in the `future` and not overlapping a Scheduled appointment."
    cites: ["context/intent-baseline.md#booking-rules", "runs/run_01M8EC2AGRDG7SJJNPS29RACVE/ide-answers.md#a-03"]
  - id: REQ-15
    title: Status change ships as a new migration
    type: constraint
    status: known
    statement: "Allowing the new status in the database is a new SQL migration file named `YYYYMMDDTHHMMSS__what.sql`, never an edit to a merged one."
    cites: ["context/conventions.md#database-changes"]
unknowns:
  - id: U-1
    text: "The term `Stale` is not in the glossary."
    question: Q-05
questions:
  - id: Q-05
    blocking: false
    ask: tech lead
    text: "Q-05: `Stale` is not in the glossary. Add an entry?"
    why: "Later steps and staff need one agreed meaning; the glossary defines every other status."
    evidence: ["context/glossary.md#scheduled", "context/glossary.md#no-show"]
    options:
      - "Add: A past Scheduled appointment neither completed nor rescheduled within 24 hours of its start time, set by the background job; it can still be rescheduled or closed."
      - "Leave the glossary unchanged."
    default: "Add: A past Scheduled appointment neither completed nor rescheduled within 24 hours of its start time, set by the background job; it can still be rescheduled or closed."
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Periodic background job (proposed default)"
    requirements: [REQ-6]
  - id: A-02
    kind: question
    about: Q-02
    answer: "24 hours after starts_at (proposed default)"
    requirements: [REQ-2, REQ-5]
  - id: A-03
    kind: question
    about: Q-03
    answer: "Yes, can be rescheduled"
    requirements: [REQ-7, REQ-14]
    mismatch:
      text: "Glossary says only Scheduled appointments can change; user decided a Stale appointment can be rescheduled."
      cites: ["context/glossary.md#scheduled"]
  - id: A-04
    kind: question
    about: Q-04
    answer: "system"
    requirements: [REQ-13]
    mismatch:
      text: "Glossary says the actor is always staff in the MVP; user decided the automatic Stale change records the actor system."
      cites: ["context/glossary.md#actor"]
  - id: A-05
    kind: assumption
    about: REQ-7
    answer: "Allow all closing actions"
    requirements: [REQ-7, REQ-8]
    mismatch:
      text: "Glossary says only Scheduled appointments can change; user decided a Stale appointment can also be marked Completed, No-show or Cancelled."
      cites: ["context/glossary.md#scheduled"]
  - id: A-06
    kind: assumption
    about: REQ-9
    answer: "Also add a list filter"
    requirements: [REQ-9, REQ-11]
  - id: A-07
    kind: assumption
    about: REQ-10
    answer: "Yes, confirm"
    requirements: [REQ-10]
glossary:
  - {term: Stale, found: false}
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Dashboard, found: true}
  - {term: Actor, found: true}
---
# Requirements review: HMS-34

## Summary
HMS-34 adds a new appointment state, Stale, for Scheduled appointments whose time has passed and that
were neither completed nor rescheduled within one day, so they no longer look upcoming. The user
decided the window is 24 hours after the start time and that a periodic background job sets the state;
this is the system's first background job (C1), a new kind of component and the main risk. A Stale
appointment stays changeable (rescheduled or closed), which departs from the glossary rule that only
Scheduled appointments change, and the automatic change records a new actor, `system`. The stored
value is `STALE`, following the existing upper-case status values (C3); trade-off: consistent with the
schema, but the UI must map it to the label "Stale".

## Requirements
- REQ-1 New appointment state Stale — known — Appointments have a new state Stale (STALE).
- REQ-2 Overdue Scheduled appointment becomes Stale — known — past, not completed or rescheduled within the window → Stale.
- REQ-3 Completed within the window is not Stale — known.
- REQ-4 Rescheduled within the window is not Stale — known.
- REQ-5 Window is 24 hours after the start time — known — the window ends 24 hours after starts_at.
- REQ-6 A periodic background job marks Stale — known.
- REQ-7 A Stale appointment can be rescheduled — known — it becomes Scheduled again.
- REQ-8 A Stale appointment can be closed — known — Completed, No-show or Cancelled.
- REQ-9 Web app shows the Stale label — known — wherever status is shown.
- REQ-10 Dashboard upcoming list excludes Stale — known.
- REQ-11 Appointments list can be filtered by Stale — known.
- REQ-12 Marking Stale is recorded in history — known — in appointment_event with time and actor.
- REQ-13 Actor for automatic Stale marking is system — known.
- REQ-14 Rescheduling a Stale appointment follows the booking rules — known (constraint).
- REQ-15 Status change ships as a new migration — known (constraint).

## Known
- REQ-1 to REQ-4: ticket acceptance criteria 1 to 4.
- REQ-5: ticket "within one day", defined by A-02.
- REQ-6: decided by A-01; no background jobs exist yet (C1).
- REQ-7, REQ-8: decided by A-03 and A-05.
- REQ-9, REQ-11: decided by A-06.
- REQ-10: confirmed by A-07.
- REQ-12: data model records every appointment change in appointment_event (C4).
- REQ-13: decided by A-04.
- REQ-14: booking rules in the intent baseline apply to the new slot of a reschedule (A-03).
- REQ-15: database-change convention (C8).

## Assumed
None: the user confirmed or corrected every assumption (A-05, A-06, A-07).

## Unknown
- U-1: `Stale` is not in the glossary → Q-05.

## Open questions
- Q-05 (non-blocking, tech lead): add a glossary entry for Stale? Proposed default: add the entry
  given in the options.

Answered in the IDE:
- A-01 (eng-dffd): Q-01 how Stale is set → "Periodic background job (proposed default)"; decides REQ-6.
- A-02 (eng-dffd): Q-02 when the window ends → "24 hours after starts_at (proposed default)"; decides REQ-2, REQ-5.
- A-03 (eng-dffd): Q-03 can a Stale appointment change → "Yes, can be rescheduled"; decides REQ-7, REQ-14.
  Mismatch: glossary says only Scheduled appointments can change.
- A-04 (eng-dffd): Q-04 actor for the change → "system"; decides REQ-13. Mismatch: glossary says the
  MVP actor is always staff.
- A-05 (eng-dffd): REQ-7 reschedule only? → "Allow all closing actions"; decides REQ-7, REQ-8.
  Mismatch: glossary says only Scheduled appointments can change.
- A-06 (eng-dffd): REQ-9 Stale label → "Also add a list filter"; decides REQ-9, REQ-11.
- A-07 (eng-dffd): REQ-10 dashboard excludes Stale → "Yes, confirm"; decides REQ-10.

## Out of scope
- Notifying staff or patients when an appointment becomes Stale: not asked in the ticket.
- Changing existing Completed, Cancelled or No-show behaviour: not asked in the ticket.
- Back-filling: the job's first run marks every existing overdue Scheduled appointment; no separate
  migration of data was asked.

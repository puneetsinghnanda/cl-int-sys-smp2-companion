---
schema: stories.v1
run_id: run_01M8E7Q99RKS5X0BN20RNDJM8C
step: split
attempt: 1
skill: story-split@1.5.0
inputs:
  - path: artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/req.md
    sha256: f5e5e578aa75507ceb63ff21361a3ad8250648de451470e0318990ce245cbd09
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "A Scheduled appointment still Scheduled `24h after start` time is marked Stale."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/req.md#item:REQ-2"]
  - id: C2
    text: "The appointment_event row for marking Stale carries the actor `system`."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/req.md#item:REQ-9"]
  - id: C3
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/req.md#claim:C7"]
  - id: C4
    text: "A rescheduled Stale appointment is `Scheduled again` and counts for the booking overlap rules."
    cites: ["artifacts/HMS-34/run_01M8E7Q99RKS5X0BN20RNDJM8C/req.md#item:REQ-7"]
  - id: C5
    text: "`exclusion constraints` forbid overlapping Scheduled appointments per doctor and per patient."
    cites: ["context/architecture.md#double-booking"]
  - id: C6
    text: "A Cancelled appointment is called off `with a reason`."
    cites: ["context/glossary.md#cancelled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [S-1, S-2, S-3, S-4, S-5, S-6, S-7]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: A Scheduled appointment left open becomes Stale
    trace: [REQ-1, REQ-2, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment that starts at 10:00 on 1 December and is still Scheduled, when the time reaches 10:00 on 2 December (24h after start), then its state is Stale."
      - id: AC2
        text: "Given a Scheduled appointment that starts at 10:00 on 1 December, when it is checked at 09:59 on 2 December (before 24h after start), then its state is still Scheduled."
      - id: AC3
        text: "Given a Scheduled appointment that starts at 01:00 local on 3 December (Asia/Kolkata = UTC+05:30, so 19:30 UTC on 2 December), when it is checked at 00:30 local on 4 December (a new local day, but before 24h after start), then its state is still Scheduled."
      - id: AC4
        text: "Given a Cancelled appointment and a No-show appointment that both started at 10:00 on 1 December, when the time passes 10:00 on 2 December, then they stay Cancelled and No-show and neither is marked Stale."
  - id: S-2
    title: An appointment completed or rescheduled in time is not marked Stale
    trace: [REQ-3, REQ-4]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment that starts at 10:00 on 1 December and staff mark it Completed at 18:00 on 1 December, when the time passes 10:00 on 2 December, then its state stays Completed and it is not marked Stale."
      - id: AC2
        text: "Given a Scheduled appointment that starts at 10:00 on 1 December and staff reschedule it at 12:00 on 1 December to 10:00 on 5 December, when the time passes 10:00 on 2 December, then it is Scheduled at 10:00 on 5 December and not marked Stale."
  - id: S-3
    title: The change to Stale appears in appointment history as done by system
    trace: [REQ-8, REQ-9]
    acceptance_criteria:
      - id: AC1
        text: "Given an appointment that has just been marked Stale, when its appointment_event history is read, then it holds an entry for the change to Stale with its time."
      - id: AC2
        text: "Given an appointment that has just been marked Stale, when the appointment_event entry for that change is read, then its actor is system, not staff."
  - id: S-4
    title: Staff close a Stale appointment
    trace: [REQ-6]
    acceptance_criteria:
      - id: AC1
        text: "Given a Stale appointment, when staff mark it Completed, then its state is Completed."
      - id: AC2
        text: "Given a Stale appointment, when staff mark it No-show, then its state is No-show."
      - id: AC3
        text: "Given a Stale appointment, when staff cancel it with a reason, then its state is Cancelled and the reason is stored."
  - id: S-5
    title: Staff reschedule a Stale appointment and it is Scheduled again
    trace: [REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given a Stale appointment with doctor D, when staff reschedule it to a free slot of doctor D at 10:00 on 5 December, then its state is Scheduled at 10:00 on 5 December."
      - id: AC2
        text: "Given a Stale appointment rescheduled to 10:00 on 5 December with doctor D, when staff try to book another patient with doctor D at 10:00 on 5 December, then the booking is refused as an overlap."
  - id: S-6
    title: Staff see Stale appointments in the list and on the appointment page
    trace: [REQ-10, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given a Stale appointment, when staff open the appointment list, then that row shows the state Stale."
      - id: AC2
        text: "Given a Stale appointment, when staff open its appointment page, then the page shows the state Stale."
      - id: AC3
        text: "Given a Stale appointment that started at 10:00 on 1 December and a Scheduled appointment that started at 11:00 on 1 December (not yet 24h after start at the time of checking, 10:30 on 2 December), when staff filter the appointment list by Stale, then only the Stale appointment is listed."
  - id: S-7
    title: The glossary defines Stale
    trace: [REQ-12]
    acceptance_criteria:
      - id: AC1
        text: "Given the glossary after this change, when a reader looks up Stale, then the entry says it is a Scheduled appointment still open 24h after start, which staff can still close or reschedule."
---
# Stories: HMS-34

## Stories

### S-1 A Scheduled appointment left open becomes Stale
Traces: REQ-1, REQ-2, REQ-5.
- AC1: Given a Scheduled appointment that starts at 10:00 on 1 December and is still Scheduled, when the time reaches 10:00 on 2 December (24h after start), then its state is Stale.
- AC2: Given it starts at 10:00 on 1 December, when checked at 09:59 on 2 December (before 24h after start), then it is still Scheduled.
- AC3: Given it starts at 01:00 local on 3 December (Asia/Kolkata = UTC+05:30, so 19:30 UTC on 2 December), when checked at 00:30 local on 4 December (a new local day, but before 24h after start), then it is still Scheduled.
- AC4: Given a Cancelled and a No-show appointment that started at 10:00 on 1 December, when the time passes 10:00 on 2 December, then neither is marked Stale.

### S-2 An appointment completed or rescheduled in time is not marked Stale
Traces: REQ-3, REQ-4.
- AC1: Marked Completed at 18:00 on 1 December stays Completed after 10:00 on 2 December.
- AC2: Rescheduled at 12:00 on 1 December to 10:00 on 5 December stays Scheduled at its new time after 10:00 on 2 December.

### S-3 The change to Stale appears in appointment history as done by system
Traces: REQ-8, REQ-9.
- AC1: The appointment_event history holds an entry for the change to Stale with its time.
- AC2: That entry's actor is system, not staff.

### S-4 Staff close a Stale appointment
Traces: REQ-6.
- AC1: Staff mark a Stale appointment Completed.
- AC2: Staff mark a Stale appointment No-show.
- AC3: Staff cancel a Stale appointment with a reason; the reason is stored.

### S-5 Staff reschedule a Stale appointment and it is Scheduled again
Traces: REQ-7.
- AC1: Rescheduled to a free slot at 10:00 on 5 December, it is Scheduled at that time.
- AC2: Another booking with the same doctor at 10:00 on 5 December is refused as an overlap.

### S-6 Staff see Stale appointments in the list and on the appointment page
Traces: REQ-10, REQ-11.
- AC1: The appointment list row shows Stale.
- AC2: The appointment page shows Stale.
- AC3: Filtering by Stale at 10:30 on 2 December lists the Stale appointment that started at 10:00 on 1 December, not the Scheduled one that started at 11:00 on 1 December.

### S-7 The glossary defines Stale
Traces: REQ-12.
- AC1: The glossary entry for Stale says it is a Scheduled appointment still open 24h after start, which staff can still close or reschedule.

## Known
- S-1: from known REQ-1, REQ-2, REQ-5.
- S-2: from known REQ-3, REQ-4.
- S-3: from known REQ-8, REQ-9.
- S-4: from known REQ-6.
- S-5: from known REQ-7.
- S-6: from known REQ-10, REQ-11.
- S-7: from known REQ-12.

## Assumed
None.

## Unknown
None.

## Open questions
None.

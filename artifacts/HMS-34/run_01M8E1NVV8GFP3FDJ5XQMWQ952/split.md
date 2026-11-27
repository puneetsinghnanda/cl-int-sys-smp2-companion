---
schema: stories.v1
run_id: run_01M8E1NVV8GFP3FDJ5XQMWQ952
step: split
attempt: 1
skill: story-split@1.5.0
inputs:
  - path: artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/req.md
    sha256: 47389008525cbbf3efa944cfba39e9e05ef3931e2e9a26a9acf835126f435647
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "An appointment becomes Stale when it is still Scheduled `24h after start time`."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/req.md#item:REQ-2"]
  - id: C2
    text: "The Stale marking is done by a `Scheduled background job`."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/req.md#item:REQ-5"]
  - id: C3
    text: "The Stale marking is recorded in appointment_event with actor `system`."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/req.md#item:REQ-8"]
  - id: C4
    text: "The hospital time zone defaults to `Asia/Kolkata`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C5
    text: "A rescheduled Stale appointment is `Scheduled again`."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/req.md#item:REQ-13"]
  - id: C6
    text: "Completed, Cancelled and No-show appointments `never change`, so they are never marked Stale."
    cites: ["artifacts/HMS-34/run_01M8E1NVV8GFP3FDJ5XQMWQ952/req.md#item:REQ-7"]
  - id: C7
    text: "A cancellation reason exists exactly when the status is `CANCELLED`."
    cites: ["context/data-model.md#appointments"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-1, S-2, S-3, S-4, S-5]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: An overdue Scheduled appointment is marked Stale by the background job
    trace: [REQ-1, REQ-2, REQ-5, REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment starting at 10:00 UTC on 9 March that is neither marked Completed nor rescheduled, when the background job runs at 10:01 UTC on 10 March (24h after start time is 10:00 UTC on 10 March, so this run is after it), then the appointment's status is Stale."
      - id: AC2
        text: "Given a Scheduled appointment starting at 10:00 UTC on 9 March that is neither marked Completed nor rescheduled, when the background job runs at 09:59 UTC on 10 March (before 24h after start time, 10:00 UTC on 10 March), then the appointment is still Scheduled."
      - id: AC3
        text: "Given a Scheduled appointment starting at 01:30 Asia/Kolkata on 10 March (Asia/Kolkata = UTC+05:30, so 20:00 UTC on 9 March), when the background job runs at 19:00 UTC on 10 March (00:30 local on 11 March, after the local day of the start has ended but before 24h after start time, 20:00 UTC on 10 March), then the appointment is still Scheduled."
      - id: AC4
        text: "Given the appointment of AC1, when the background job marks it Stale, then an appointment_event row records the change to Stale with actor system."
  - id: S-2
    title: Appointments handled in time or already closed are not marked Stale
    trace: [REQ-3, REQ-4, REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given an appointment starting at 10:00 UTC on 9 March that staff marked Completed at 15:00 UTC on 9 March, when the background job runs at 10:01 UTC on 10 March, then its status is still COMPLETED and no change to Stale is recorded."
      - id: AC2
        text: "Given an appointment starting at 10:00 UTC on 9 March that staff rescheduled at 15:00 UTC on 9 March to 10:00 UTC on 12 March, when the background job runs at 10:01 UTC on 10 March, then it is still Scheduled at 10:00 UTC on 12 March."
      - id: AC3
        text: "Given an appointment starting at 10:00 UTC on 9 March that is CANCELLED, when the background job runs at 10:01 UTC on 10 March, then its status is still CANCELLED."
      - id: AC4
        text: "Given an appointment starting at 10:00 UTC on 9 March that is NO_SHOW, when the background job runs at 10:01 UTC on 10 March, then its status is still NO_SHOW."
  - id: S-3
    title: Staff close a Stale appointment
    trace: [REQ-6, REQ-10, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given a Stale appointment, when staff mark it Completed, then its status is COMPLETED."
      - id: AC2
        text: "Given a Stale appointment, when staff mark it No-show, then its status is NO_SHOW."
      - id: AC3
        text: "Given a Stale appointment, when staff cancel it with a reason, then its status is CANCELLED and it holds that reason."
  - id: S-4
    title: Staff reschedule a Stale appointment and it is Scheduled again
    trace: [REQ-12, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a Stale appointment, when staff reschedule it to 10:00 UTC on 20 March, then its status is Scheduled and it starts at 10:00 UTC on 20 March."
      - id: AC2
        text: "Given the appointment rescheduled in AC1, when the background job runs at 10:00 UTC on 15 March, then it is still Scheduled."
      - id: AC3
        text: "Given the appointment rescheduled in AC1 that is neither marked Completed nor rescheduled again, when the background job runs at 10:01 UTC on 21 March (24h after start time is 10:00 UTC on 21 March), then its status is Stale again."
  - id: S-5
    title: The glossary defines Stale
    trace: [REQ-9]
    acceptance_criteria:
      - id: AC1
        text: "Given the system glossary, when a reader looks up Stale, then it has an entry: a Scheduled appointment whose start time passed 24 hours ago without being marked Completed or rescheduled; it stays open."
---
# Stories: HMS-34

## Stories

### S-1 An overdue Scheduled appointment is marked Stale by the background job
Traces REQ-1, REQ-2, REQ-5, REQ-8.
- AC1: Given a Scheduled appointment starting at 10:00 UTC on 9 March that is neither marked Completed nor rescheduled, when the background job runs at 10:01 UTC on 10 March (after 24h after start time, 10:00 UTC on 10 March), then its status is Stale.
- AC2: Given the same appointment, when the job runs at 09:59 UTC on 10 March, then it is still Scheduled.
- AC3: Given a Scheduled appointment starting at 01:30 Asia/Kolkata on 10 March (20:00 UTC on 9 March), when the job runs at 19:00 UTC on 10 March (00:30 local on 11 March), then it is still Scheduled: the window is counted from the start instant, not the local day.
- AC4: Given the appointment of AC1, when the job marks it Stale, then an appointment_event row records the change with actor system.

### S-2 Appointments handled in time or already closed are not marked Stale
Traces REQ-3, REQ-4, REQ-7.
- AC1: Completed at 15:00 UTC on 9 March → still COMPLETED after the job runs at 10:01 UTC on 10 March, and no change to Stale is recorded.
- AC2: Rescheduled at 15:00 UTC on 9 March to 10:00 UTC on 12 March → still Scheduled at the new time after the job runs at 10:01 UTC on 10 March.
- AC3: CANCELLED → still CANCELLED after the job runs at 10:01 UTC on 10 March.
- AC4: NO_SHOW → still NO_SHOW after the job runs at 10:01 UTC on 10 March.

### S-3 Staff close a Stale appointment
Traces REQ-6, REQ-10, REQ-11.
- AC1: Stale, staff mark Completed → COMPLETED.
- AC2: Stale, staff mark No-show → NO_SHOW.
- AC3: Stale, staff cancel with a reason → CANCELLED with that reason.

### S-4 Staff reschedule a Stale appointment and it is Scheduled again
Traces REQ-12, REQ-13.
- AC1: Stale, rescheduled to 10:00 UTC on 20 March → Scheduled at 10:00 UTC on 20 March.
- AC2: The job at 10:00 UTC on 15 March leaves it Scheduled.
- AC3: Not closed or moved again, the job at 10:01 UTC on 21 March marks it Stale again.

### S-5 The glossary defines Stale
Traces REQ-9.
- AC1: The glossary has a Stale entry: a Scheduled appointment whose start time passed 24 hours ago without being marked Completed or rescheduled; it stays open.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-5, REQ-8.
- S-2: built from known REQ-3, REQ-4, REQ-7.
- S-3: built from known REQ-6, REQ-10, REQ-11.
- S-4: built from known REQ-12, REQ-13.
- S-5: built from known REQ-9.

## Assumed
None.

## Unknown
None.

## Open questions
None.

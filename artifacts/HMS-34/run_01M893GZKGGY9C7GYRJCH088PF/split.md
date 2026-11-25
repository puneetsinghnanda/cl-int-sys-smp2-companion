---
schema: stories.v1
run_id: run_01M893GZKGGY9C7GYRJCH088PF
step: split
attempt: 1
skill: story-split@1.5.0
inputs:
  - path: artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/req.md
    sha256: 36441e0509699b22a67bdb9f3ca7a1f511be79db2162ba7c24652d3871e33a4f
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "A Stale appointment is one not marked Completed or rescheduled by `24h` after its start time."
    cites: ["artifacts/HMS-34/run_01M893GZKGGY9C7GYRJCH088PF/req.md#item:REQ-2"]
  - id: C2
    text: "The web app shows an appointment status with the `StatusBadge` component."
    cites: ["context/generated/module-map.json#sym:sample-system:web/src/components/StatusBadge.tsx:StatusBadge"]
  - id: C3
    text: "The appointment list is the page `/appointments`."
    cites: ["context/generated/api-inventory.json#ui:/appointments"]
  - id: C4
    text: "Each appointment has its own page `/appointments/:id`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/:id"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: A past appointment left open is marked Stale automatically
    trace: [REQ-1, REQ-2, REQ-3]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment that starts at 10:00 on 3 December and is not marked Completed or rescheduled, when the background job runs at 10:01 on 4 December (one minute past 24h after its start time) with no staff action, then the appointment's state is Stale."
      - id: AC2
        text: "Given the same Scheduled appointment starting at 10:00 on 3 December, when the background job runs at 09:59 on 4 December (one minute before 24h after its start time), then the appointment is still Scheduled."
      - id: AC3
        text: "Given a Scheduled appointment that starts at 10:00 on 6 December, when the background job runs at 10:01 on 4 December, then the appointment is still Scheduled."
  - id: S-2
    title: An appointment closed or moved within the day is not marked Stale
    trace: [REQ-4, REQ-5, REQ-6]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment starting at 10:00 on 3 December that staff mark Completed at 09:59 on 4 December, when the background job runs at 10:01 on 4 December, then the appointment is Completed, not Stale."
      - id: AC2
        text: "Given a Scheduled appointment starting at 10:00 on 3 December that staff reschedule at 09:59 on 4 December to 10:00 on 6 December, when the background job runs at 10:01 on 4 December, then the appointment is Scheduled at 10:00 on 6 December, not Stale."
      - id: AC3
        text: "Given a Scheduled appointment starting at 10:00 on 3 December that staff cancel with a reason at 09:59 on 4 December, when the background job runs at 10:01 on 4 December, then the appointment is Cancelled, not Stale."
      - id: AC4
        text: "Given a Scheduled appointment starting at 10:00 on 3 December that staff mark No-show at 09:59 on 4 December, when the background job runs at 10:01 on 4 December, then the appointment is No-show, not Stale."
  - id: S-3
    title: Staff can still close a Stale appointment
    trace: [REQ-7, REQ-8, REQ-9]
    acceptance_criteria:
      - id: AC1
        text: "Given a Stale appointment, when staff mark it Completed, then the appointment is Completed."
      - id: AC2
        text: "Given a Stale appointment, when staff mark it No-show, then the appointment is No-show."
      - id: AC3
        text: "Given a Stale appointment, when staff cancel it with a reason, then the appointment is Cancelled and shows that reason."
  - id: S-4
    title: Staff can reschedule a Stale appointment
    trace: [REQ-10, REQ-15]
    acceptance_criteria:
      - id: AC1
        text: "Given a Stale appointment that started at 10:00 on 3 December, when staff reschedule it to a free slot at 10:00 on 6 December, then the appointment is Scheduled at 10:00 on 6 December."
      - id: AC2
        text: "Given that appointment rescheduled to 10:00 on 6 December, when the background job runs at 10:01 on 5 December, then the appointment is still Scheduled."
  - id: S-5
    title: Staff see Stale wherever a status is shown
    trace: [REQ-11, REQ-12, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a Stale appointment, when staff open the appointment list, then its status shows Stale."
      - id: AC2
        text: "Given a Stale appointment, when staff open that appointment's own page, then its status shows Stale."
      - id: AC3
        text: "Given one Stale and one Scheduled appointment, when staff filter the appointment list by status Stale, then only the Stale appointment is listed."
      - id: AC4
        text: "Given one Stale appointment, when staff open the dashboard, then it shows the same tiles as before and no tile for Stale appointments."
  - id: S-6
    title: The glossary defines Stale
    trace: [REQ-14]
    acceptance_criteria:
      - id: AC1
        text: "Given the glossary, when a reader looks up Stale, then its entry says a Scheduled appointment not marked Completed or rescheduled by 24h after its start time is marked Stale."
---
# Stories: HMS-34

## Stories

### S-1 A past appointment left open is marked Stale automatically
Traces REQ-1, REQ-2, REQ-3. The rule is C1 (`24h` after the start time).
- AC1: Given a Scheduled appointment that starts at 10:00 on 3 December and is not marked Completed or rescheduled, when the background job runs at 10:01 on 4 December (one minute past 24h after its start time) with no staff action, then the appointment's state is Stale.
- AC2: Given the same Scheduled appointment starting at 10:00 on 3 December, when the background job runs at 09:59 on 4 December (one minute before 24h after its start time), then the appointment is still Scheduled.
- AC3: Given a Scheduled appointment that starts at 10:00 on 6 December, when the background job runs at 10:01 on 4 December, then the appointment is still Scheduled.

### S-2 An appointment closed or moved within the day is not marked Stale
Traces REQ-4, REQ-5, REQ-6. Each case acts at 09:59 on 4 December, the latest moment inside the day.
- AC1: Given a Scheduled appointment starting at 10:00 on 3 December that staff mark Completed at 09:59 on 4 December, when the background job runs at 10:01 on 4 December, then the appointment is Completed, not Stale.
- AC2: Given a Scheduled appointment starting at 10:00 on 3 December that staff reschedule at 09:59 on 4 December to 10:00 on 6 December, when the background job runs at 10:01 on 4 December, then the appointment is Scheduled at 10:00 on 6 December, not Stale.
- AC3: Given a Scheduled appointment starting at 10:00 on 3 December that staff cancel with a reason at 09:59 on 4 December, when the background job runs at 10:01 on 4 December, then the appointment is Cancelled, not Stale.
- AC4: Given a Scheduled appointment starting at 10:00 on 3 December that staff mark No-show at 09:59 on 4 December, when the background job runs at 10:01 on 4 December, then the appointment is No-show, not Stale.

### S-3 Staff can still close a Stale appointment
Traces REQ-7, REQ-8, REQ-9.
- AC1: Given a Stale appointment, when staff mark it Completed, then the appointment is Completed.
- AC2: Given a Stale appointment, when staff mark it No-show, then the appointment is No-show.
- AC3: Given a Stale appointment, when staff cancel it with a reason, then the appointment is Cancelled and shows that reason.

### S-4 Staff can reschedule a Stale appointment
Traces REQ-10, REQ-15.
- AC1: Given a Stale appointment that started at 10:00 on 3 December, when staff reschedule it to a free slot at 10:00 on 6 December, then the appointment is Scheduled at 10:00 on 6 December.
- AC2: Given that appointment rescheduled to 10:00 on 6 December, when the background job runs at 10:01 on 5 December, then the appointment is still Scheduled.

### S-5 Staff see Stale wherever a status is shown
Traces REQ-11, REQ-12, REQ-13. A status is shown with the `StatusBadge` component (C2), on the list `/appointments` (C3) and on the appointment's own page `/appointments/:id` (C4).
- AC1: Given a Stale appointment, when staff open the appointment list, then its status shows Stale.
- AC2: Given a Stale appointment, when staff open that appointment's own page, then its status shows Stale.
- AC3: Given one Stale and one Scheduled appointment, when staff filter the appointment list by status Stale, then only the Stale appointment is listed.
- AC4: Given one Stale appointment, when staff open the dashboard, then it shows the same tiles as before and no tile for Stale appointments.

### S-6 The glossary defines Stale
Traces REQ-14.
- AC1: Given the glossary, when a reader looks up Stale, then its entry says a Scheduled appointment not marked Completed or rescheduled by 24h after its start time is marked Stale.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3.
- S-2: built from known REQ-4, REQ-5, REQ-6.
- S-3: built from known REQ-7, REQ-8, REQ-9.
- S-4: built from known REQ-10, REQ-15.
- S-5: built from known REQ-11, REQ-12, REQ-13.
- S-6: built from known REQ-14.

## Assumed
None.

## Unknown
None.

## Open questions
None.

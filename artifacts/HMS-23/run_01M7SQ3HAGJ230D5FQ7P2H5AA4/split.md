---
schema: stories.v1
run_id: run_01M7SQ3HAGJ230D5FQ7P2H5AA4
step: split
attempt: 3
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/req.md
    sha256: 0fbd5a70a16b61c0acd828a804affa01fc7aa1b37e6ae354918b533638b9179a
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Only `Active` doctors with an availability block on today's weekday are on duty."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/req.md#item:REQ-2", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-01"]
  - id: C2
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C3
    text: "An availability block is a weekly window keyed by `weekday` in hospital local time."
    cites: ["context/glossary.md#availability-block"]
  - id: C4
    text: "Booked and free counts follow the doctor's `day schedule` logic."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/req.md#item:REQ-4", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-02"]
  - id: C5
    text: "A doctor with no free slots left shows `Fully booked`."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/req.md#item:REQ-7", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-03"]
  - id: C6
    text: "The doctor's name links to the `day schedule` for today."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/req.md#item:REQ-8", "runs/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/ide-answers.md#a-04"]
  - id: C7
    text: "The next free slot is the earliest free slot that `starts after now`."
    cites: ["artifacts/HMS-23/run_01M7SQ3HAGJ230D5FQ7P2H5AA4/req.md#item:REQ-6"]
  - id: C8
    text: "A slot is free only when it is `in the future`."
    cites: ["context/intent-baseline.md#booking-rules"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See which doctors are on duty today
    trace: [REQ-1, REQ-2, REQ-3, REQ-10, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given Active doctor Dr Rao has an availability block on today's weekday and Active doctor Dr Sen has none today, when staff open the dashboard, then below the tiles a section \"On duty today\" shows one row for Dr Rao with name, department, booked slots, free slots and next free slot time, and no row for Dr Sen."
      - id: AC2
        text: "Given it is 00:30 on Tuesday in Asia/Kolkata (UTC+05:30, so 19:00 UTC on Monday, the local side after midnight) and Dr Rao is Active with availability only on Tuesday while Dr Sen is Active with availability only on Monday, when staff open the dashboard, then Dr Rao is listed and Dr Sen is not."
      - id: AC3
        text: "Given Inactive doctor Dr Iyer has an availability block on today's weekday and would have more free slots than any listed doctor, when staff open the dashboard, then Dr Iyer is not listed."
      - id: AC4
        text: "Given no Active doctor has an availability block on today's weekday, when staff open the dashboard, then the section shows \"No doctors on duty today\" and no rows."
  - id: S-2
    title: Booked, free and next free slot match the day schedule
    trace: [REQ-4, REQ-5, REQ-6]
    acceptance_criteria:
      - id: AC1
        text: "Given Dr Rao is on duty and today has one Scheduled, one Completed, one No-show and one Cancelled appointment with Dr Rao, when staff open the dashboard, then Dr Rao's booked and free counts equal the booked and free counts shown on Dr Rao's day schedule for today (so a dashboard that counts only Scheduled appointments fails whenever the day schedule also counts Completed or No-show)."
      - id: AC2
        text: "Given the time is 11:00 local and Dr Rao has an unbooked slot at 10:00 and an unbooked slot at 11:30, when staff open the dashboard, then the 10:00 slot is not counted as free and the next free slot shows 11:30."
      - id: AC3
        text: "Given the time is 11:00 local and Dr Rao's 11:30 slot is booked while the 12:00 slot is unbooked, when staff open the dashboard, then the next free slot shows 12:00, not 11:30."
      - id: AC4
        text: "Given the time is exactly 11:00:00 local and Dr Rao's only unbooked slots left today start at 11:00 and at 11:30, when staff open the dashboard, then Dr Rao's free count includes the 11:30 slot but not the 11:00 slot (a free slot must be in the future, so a slot starting exactly now is not free) and the next free slot shows 11:30."
  - id: S-3
    title: Doctor with no free slots left shows Fully booked
    trace: [REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given Dr Rao is on duty and every slot of Dr Rao's that starts after now is booked, when staff open the dashboard, then Dr Rao is listed with 0 free slots and \"Fully booked\" instead of a time."
      - id: AC2
        text: "Given Dr Rao is on duty and the time is after the start of Dr Rao's last slot today, with earlier slots left unbooked, when staff open the dashboard, then Dr Rao is still listed with 0 free slots and \"Fully booked\"."
  - id: S-4
    title: Doctors with the most free slots come first
    trace: [REQ-9]
    acceptance_criteria:
      - id: AC1
        text: "Given Dr Rao has fewer free slots left than Dr Sen and both are on duty, when staff open the dashboard, then Dr Sen's row is above Dr Rao's."
      - id: AC2
        text: "Given Dr Bhatt and Dr Anand are on duty with the same number of free slots, when staff open the dashboard, then Dr Anand's row is above Dr Bhatt's."
  - id: S-5
    title: Open a doctor's day schedule from the section
    trace: [REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given Dr Rao is listed in \"On duty today\", when staff select Dr Rao's name, then the doctor page /doctors/:id for Dr Rao opens showing the day schedule for today."
  - id: S-6
    title: The rest of the dashboard is unchanged
    trace: [REQ-12, REQ-13, REQ-14]
    acceptance_criteria:
      - id: AC1
        text: "Given the Period selector is set to Last 7 days, when staff view the dashboard, then the tiles show Last 7 days counts and the \"On duty today\" section still shows today's doctors and today's slot counts."
      - id: AC2
        text: "Given Dr Rao has contact details on record, when staff view Dr Rao's row, then no contact details are shown."
      - id: AC3
        text: "Given there are upcoming appointments, when staff open the dashboard, then the tiles and the next 10 appointments look and behave exactly as before the change."
      - id: AC4
        text: "Given there are no upcoming appointments, when staff open the dashboard, then the tiles and the appointments area look and behave exactly as before the change."
---
# Stories: HMS-23

## Stories

**S-1 See which doctors are on duty today** (REQ-1, REQ-2, REQ-3, REQ-10, REQ-11)
- AC1: Given Active doctor Dr Rao has an availability block on today's weekday and Active doctor Dr Sen has none today, when staff open the dashboard, then below the tiles a section "On duty today" shows one row for Dr Rao with name, department, booked slots, free slots and next free slot time, and no row for Dr Sen.
- AC2: Given it is 00:30 on Tuesday in Asia/Kolkata (UTC+05:30, so 19:00 UTC on Monday, the local side after midnight) and Dr Rao is Active with availability only on Tuesday while Dr Sen is Active with availability only on Monday, when staff open the dashboard, then Dr Rao is listed and Dr Sen is not.
- AC3: Given Inactive doctor Dr Iyer has an availability block on today's weekday and would have more free slots than any listed doctor, when staff open the dashboard, then Dr Iyer is not listed.
- AC4: Given no Active doctor has an availability block on today's weekday, when staff open the dashboard, then the section shows "No doctors on duty today" and no rows.

**S-2 Booked, free and next free slot match the day schedule** (REQ-4, REQ-5, REQ-6)
- AC1: Given Dr Rao is on duty and today has one Scheduled, one Completed, one No-show and one Cancelled appointment with Dr Rao, when staff open the dashboard, then Dr Rao's booked and free counts equal the booked and free counts shown on Dr Rao's day schedule for today (so a dashboard that counts only Scheduled appointments fails whenever the day schedule also counts Completed or No-show).
- AC2: Given the time is 11:00 local and Dr Rao has an unbooked slot at 10:00 and an unbooked slot at 11:30, when staff open the dashboard, then the 10:00 slot is not counted as free and the next free slot shows 11:30.
- AC3: Given the time is 11:00 local and Dr Rao's 11:30 slot is booked while the 12:00 slot is unbooked, when staff open the dashboard, then the next free slot shows 12:00, not 11:30.
- AC4: Given the time is exactly 11:00:00 local and Dr Rao's only unbooked slots left today start at 11:00 and at 11:30, when staff open the dashboard, then Dr Rao's free count includes the 11:30 slot but not the 11:00 slot (a free slot must be in the future, so a slot starting exactly now is not free) and the next free slot shows 11:30.

**S-3 Doctor with no free slots left shows Fully booked** (REQ-7)
- AC1: Given Dr Rao is on duty and every slot of Dr Rao's that starts after now is booked, when staff open the dashboard, then Dr Rao is listed with 0 free slots and "Fully booked" instead of a time.
- AC2: Given Dr Rao is on duty and the time is after the start of Dr Rao's last slot today, with earlier slots left unbooked, when staff open the dashboard, then Dr Rao is still listed with 0 free slots and "Fully booked".

**S-4 Doctors with the most free slots come first** (REQ-9)
- AC1: Given Dr Rao has fewer free slots left than Dr Sen and both are on duty, when staff open the dashboard, then Dr Sen's row is above Dr Rao's.
- AC2: Given Dr Bhatt and Dr Anand are on duty with the same number of free slots, when staff open the dashboard, then Dr Anand's row is above Dr Bhatt's.

**S-5 Open a doctor's day schedule from the section** (REQ-8)
- AC1: Given Dr Rao is listed in "On duty today", when staff select Dr Rao's name, then the doctor page /doctors/:id for Dr Rao opens showing the day schedule for today.

**S-6 The rest of the dashboard is unchanged** (REQ-12, REQ-13, REQ-14)
- AC1: Given the Period selector is set to Last 7 days, when staff view the dashboard, then the tiles show Last 7 days counts and the "On duty today" section still shows today's doctors and today's slot counts.
- AC2: Given Dr Rao has contact details on record, when staff view Dr Rao's row, then no contact details are shown.
- AC3: Given there are upcoming appointments, when staff open the dashboard, then the tiles and the next 10 appointments look and behave exactly as before the change.
- AC4: Given there are no upcoming appointments, when staff open the dashboard, then the tiles and the appointments area look and behave exactly as before the change.

## Known
- S-1: built from known REQ-1, REQ-2 (confirmed in A-01), REQ-3, REQ-10, REQ-11.
- S-2: built from known REQ-4 (decided in A-02), REQ-5, REQ-6. AC1 mixes appointment statuses. AC4 checks both the free count and the next free slot for a slot that starts exactly now, as the baseline booking rule requires (C8).
- S-3: built from known REQ-7 (confirmed in A-03).
- S-4: built from known REQ-9.
- S-5: built from known REQ-8 (confirmed in A-04).
- S-6: built from known REQ-12, REQ-13, REQ-14.

## Assumed
None.

## Unknown
None.

## Open questions
None.

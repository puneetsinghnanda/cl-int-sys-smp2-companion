---
schema: stories.v1
run_id: run_01M7Q1DS00W5S7BFVX9Y70NPZG
step: split
attempt: 2
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/req.md
    sha256: c5c9d980373a9426a9af4c3b4d72cca66f0eb9f6023b195e786aa8d084ba20f0
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
resolves:
  - id: F-1
    how: "S-3 AC1 now uses an appointment inside Last 7 days but not Today, so a period-filtered tile shows 0 under Today and fails."
claims:
  - id: C1
    text: "The new tile is labelled `Awaiting outcome`."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/req.md#item:REQ-1"]
  - id: C2
    text: "The tile counts Scheduled appointments whose `start time is before now`."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/req.md#item:REQ-2"]
  - id: C3
    text: "The count does not change with the `period selector`."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/req.md#item:REQ-7"]
  - id: C4
    text: "The tile links to the list of exactly the counted appointments (`DSH-8`)."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/req.md#item:REQ-8"]
  - id: C5
    text: "The list of the `next 10 upcoming` appointments is unchanged."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/req.md#item:REQ-11"]
  - id: C6
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["artifacts/HMS-21/run_01M7Q1DS00W5S7BFVX9Y70NPZG/req.md#claim:C8"]
  - id: C7
    text: "A period is Today, Last 7 days or `Last 30 days`, in whole hospital-local days ending today."
    cites: ["context/glossary.md#period"]
  - id: C8
    text: "`Medical history` is blood group, allergies, chronic conditions, current medications and notes."
    cites: ["context/glossary.md#medical-history"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See how many started appointments still await an outcome
    trace: [REQ-1, REQ-2, REQ-6, REQ-9]
    acceptance_criteria:
      - id: AC1
        text: "Given now is 10:00 local on 18 November, when staff open the dashboard, then a tile labelled Awaiting outcome is shown."
      - id: AC2
        text: "Given now is 10:00 local on 18 November, Scheduled A1 started at 09:59 local and Scheduled A2 starts at 10:01 local, when staff open the dashboard, then the Awaiting outcome tile counts A1 and not A2."
      - id: AC3
        text: "Given now is 10:00 local on 18 November, Scheduled A3 started at 00:30 local on 18 November (Asia/Kolkata = UTC+05:30, so 19:00 UTC on 17 November, after local midnight) and Scheduled A4 starts at 15:00 local today, when staff open the dashboard, then the tile counts A3 and not A4."
      - id: AC4
        text: "Given a Scheduled appointment started on 1 November and was never marked, when staff open the dashboard on 18 November, then the Awaiting outcome tile counts it."
      - id: AC5
        text: "Given every Scheduled appointment starts after now, when staff open the dashboard, then the Awaiting outcome tile shows 0."
  - id: S-2
    title: Closed appointments are never counted
    trace: [REQ-3, REQ-4, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given now is 10:00 local and the most recently started appointment, at 09:30 local today, is Completed, when staff open the dashboard, then the Awaiting outcome tile does not count it."
      - id: AC2
        text: "Given now is 10:00 local and an appointment at 09:45 local today was Cancelled, when staff open the dashboard, then the Awaiting outcome tile does not count it."
      - id: AC3
        text: "Given now is 10:00 local and an appointment at 09:15 local today is No-show, when staff open the dashboard, then the Awaiting outcome tile does not count it."
      - id: AC4
        text: "Given Scheduled A1 started at 09:00 local today and is counted, when staff mark A1 Completed and open the dashboard again, then the Awaiting outcome count no longer includes A1."
  - id: S-3
    title: The count ignores the period selector
    trace: [REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given now is 10:00 local on 18 November and the only Scheduled appointment started before now began at 09:00 local on 15 November, inside Last 7 days but not Today, when staff switch the period from Today to Last 7 days, then the tile shows 1 both times."
      - id: AC2
        text: "Given today is 18 November local and a Scheduled appointment started on 10 October, before Last 30 days begins on 20 October, when staff choose Last 30 days, then the Awaiting outcome tile still counts it."
  - id: S-4
    title: Open exactly the counted appointments from the tile
    trace: [REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given now is 10:00 local on 18 November, Scheduled A1 started at 09:59 local and Scheduled A2 starts at 10:01 local, when staff select the Awaiting outcome tile, then the list shows A1, not A2, and its total equals the tile's count."
      - id: AC2
        text: "Given Scheduled A3 started at 00:30 local on 18 November (19:00 UTC on 17 November) and Scheduled A5 started on 1 November, when staff select the Awaiting outcome tile, then the list shows both A3 and A5."
      - id: AC3
        text: "Given now is 10:00 local and an appointment at 09:30 local today is Completed, when staff select the Awaiting outcome tile, then the list does not show it."
  - id: S-5
    title: The rest of the dashboard stays as it is
    trace: [REQ-10, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment started before now exists, when staff open the dashboard, then every other tile shows the same count as before this change."
      - id: AC2
        text: "Given no Scheduled appointment started before now, when staff open the dashboard, then every other tile shows the same count as before this change."
      - id: AC3
        text: "Given 11 Scheduled appointments start after now, when staff open the dashboard, then the next upcoming list shows the 10 earliest and not the 11th, as before this change."
      - id: AC4
        text: "Given now is 10:00 local and Scheduled A1 started at 09:59 local, when staff open the dashboard, then the next upcoming list does not show A1, as before this change."
  - id: S-6
    title: No personal data on the dashboard or the linked list
    trace: [REQ-12, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given the patient of a counted appointment has a blood group, allergies, chronic conditions, current medications, notes and every contact detail the patient record holds, when staff open the dashboard, then neither the page nor its data response contains any of them."
      - id: AC2
        text: "Given the patient of a counted appointment has a blood group, allergies, chronic conditions, current medications, notes and every contact detail the patient record holds, when staff select the tile, then neither the list nor its data response contains any of them."
---
# Stories: HMS-21

## Resolves
- F-1: S-3 AC1 now uses an appointment inside Last 7 days but not Today, so a period-filtered tile shows 0 under Today and fails.

## Stories

### S-1 See how many started appointments still await an outcome
Traces: REQ-1, REQ-2, REQ-6, REQ-9.
- AC1: Given now is 10:00 local on 18 November, when staff open the dashboard, then a tile labelled Awaiting outcome is shown.
- AC2: Given now is 10:00 local on 18 November, Scheduled A1 started at 09:59 local and Scheduled A2 starts at 10:01 local, when staff open the dashboard, then the Awaiting outcome tile counts A1 and not A2.
- AC3: Given now is 10:00 local on 18 November, Scheduled A3 started at 00:30 local on 18 November (Asia/Kolkata = UTC+05:30, so 19:00 UTC on 17 November, after local midnight) and Scheduled A4 starts at 15:00 local today, when staff open the dashboard, then the tile counts A3 and not A4.
- AC4: Given a Scheduled appointment started on 1 November and was never marked, when staff open the dashboard on 18 November, then the Awaiting outcome tile counts it.
- AC5: Given every Scheduled appointment starts after now, when staff open the dashboard, then the Awaiting outcome tile shows 0.

### S-2 Closed appointments are never counted
Traces: REQ-3, REQ-4, REQ-5.
- AC1: Given now is 10:00 local and the most recently started appointment, at 09:30 local today, is Completed, when staff open the dashboard, then the Awaiting outcome tile does not count it.
- AC2: Given now is 10:00 local and an appointment at 09:45 local today was Cancelled, when staff open the dashboard, then the Awaiting outcome tile does not count it.
- AC3: Given now is 10:00 local and an appointment at 09:15 local today is No-show, when staff open the dashboard, then the Awaiting outcome tile does not count it.
- AC4: Given Scheduled A1 started at 09:00 local today and is counted, when staff mark A1 Completed and open the dashboard again, then the Awaiting outcome count no longer includes A1.

### S-3 The count ignores the period selector
Traces: REQ-7.
- AC1: Given now is 10:00 local on 18 November and the only Scheduled appointment started before now began at 09:00 local on 15 November, inside Last 7 days but not Today, when staff switch the period from Today to Last 7 days, then the tile shows 1 both times.
- AC2: Given today is 18 November local and a Scheduled appointment started on 10 October, before Last 30 days begins on 20 October, when staff choose Last 30 days, then the Awaiting outcome tile still counts it.

### S-4 Open exactly the counted appointments from the tile
Traces: REQ-8.
- AC1: Given now is 10:00 local on 18 November, Scheduled A1 started at 09:59 local and Scheduled A2 starts at 10:01 local, when staff select the Awaiting outcome tile, then the list shows A1, not A2, and its total equals the tile's count.
- AC2: Given Scheduled A3 started at 00:30 local on 18 November (19:00 UTC on 17 November) and Scheduled A5 started on 1 November, when staff select the Awaiting outcome tile, then the list shows both A3 and A5.
- AC3: Given now is 10:00 local and an appointment at 09:30 local today is Completed, when staff select the Awaiting outcome tile, then the list does not show it.

### S-5 The rest of the dashboard stays as it is
Traces: REQ-10, REQ-11.
- AC1: Given a Scheduled appointment started before now exists, when staff open the dashboard, then every other tile shows the same count as before this change.
- AC2: Given no Scheduled appointment started before now, when staff open the dashboard, then every other tile shows the same count as before this change.
- AC3: Given 11 Scheduled appointments start after now, when staff open the dashboard, then the next upcoming list shows the 10 earliest and not the 11th, as before this change.
- AC4: Given now is 10:00 local and Scheduled A1 started at 09:59 local, when staff open the dashboard, then the next upcoming list does not show A1, as before this change.

### S-6 No personal data on the dashboard or the linked list
Traces: REQ-12, REQ-13.
- AC1: Given the patient of a counted appointment has a blood group, allergies, chronic conditions, current medications, notes and every contact detail the patient record holds, when staff open the dashboard, then neither the page nor its data response contains any of them.
- AC2: Given the patient of a counted appointment has a blood group, allergies, chronic conditions, current medications, notes and every contact detail the patient record holds, when staff select the tile, then neither the list nor its data response contains any of them.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-6 and REQ-9. It tests the boundary at now on both sides (C2) and an instant whose local date differs from its UTC date in `Asia/Kolkata` (C6).
- S-2: built from known REQ-3, REQ-4 and REQ-5. Each excluded status is the most recent or most tempting started appointment.
- S-3: built from known REQ-7. One appointment sits inside Last 7 days but outside Today, and another before the first day of `Last 30 days` (C7). Both must stay counted (C3).
- S-4: built from known REQ-8. The list must match the count at the now boundary and across local dates (C4).
- S-5: built from known REQ-10 and REQ-11. The other tiles are checked with and without an awaiting appointment, and the `next 10 upcoming` list just over its limit (C5).
- S-6: built from known REQ-12 and REQ-13. Every `Medical history` field from the glossary (C8) and every contact detail the patient record holds are seeded.

## Assumed
None.

## Unknown
None.

## Open questions
None.

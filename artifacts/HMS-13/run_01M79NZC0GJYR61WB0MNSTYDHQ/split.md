---
schema: stories.v1
run_id: run_01M79NZC0GJYR61WB0MNSTYDHQ
step: split
attempt: 1
skill: story-split@1.2.0
inputs:
  - path: artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/req.md
    sha256: 4ac588db013a5e386b1768e14973edb892b7f1ec0225d0d53ffbbd470f7a8bd2
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Outside availability is decided by the doctor page's DOC-7 rule `withinAvailability`."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/req.md#item:REQ-2"]
  - id: C2
    text: "Only Scheduled appointments that have not started (`starts_at >= now`) get the outside-availability notice."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/req.md#item:REQ-3"]
  - id: C3
    text: "The doctor page's `50-appointment limit` is not copied to the appointment page."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/req.md#item:REQ-4"]
  - id: C4
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["artifacts/HMS-13/run_01M79NZC0GJYR61WB0MNSTYDHQ/req.md#claim:C6"]
  - id: C5
    text: "An availability block is a weekly window `in hospital local time`."
    cites: ["context/glossary.md#availability-block"]
  - id: C6
    text: "An Inactive doctor is kept for history but `not offered for new use`."
    cites: ["context/glossary.md#inactive"]
  - id: C7
    text: "A Scheduled appointment is one that is `booked and still open`."
    cites: ["context/glossary.md#scheduled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9]
known: [S-1, S-2, S-3, S-4, S-5]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See when a Scheduled appointment is outside its doctor's availability
    trace: [REQ-1, REQ-2, REQ-3, REQ-4]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment that has not started, whose doctor is Active and whose start and end lie in none of the doctor's availability blocks, so the doctor page flags it, when staff open its appointment page, then the notice \"This appointment is outside the doctor's current availability. Reschedule or cancel it.\" shows above the actions."
      - id: AC2
        text: "Given a Scheduled appointment that has not started, whose doctor is Active and whose start and end lie inside one of the doctor's availability blocks, so the doctor page does not flag it, when staff open its appointment page, then no outside-availability notice shows."
      - id: AC3
        text: "Given a Scheduled appointment of an Active doctor whose start time has already passed and whose start and end lie in none of the doctor's availability blocks, when staff open its appointment page, then no outside-availability notice shows."
      - id: AC4
        text: "Given an Active doctor whose 51st upcoming Scheduled appointment (one past the doctor page's 50-appointment limit) lies in none of the doctor's availability blocks, when staff open that appointment's page, then the outside-availability notice shows above the actions."
      - id: AC5
        text: "Given an Active doctor whose only availability block is Monday 19:00 to 20:00 hospital local time, and a Scheduled appointment from Tuesday 17 November 2026 01:00 to 01:30 local (Asia/Kolkata = UTC+05:30, so this is Monday 16 November 2026 19:30 to 20:00 UTC, the local side of midnight being Tuesday), when staff open its appointment page, then the outside-availability notice shows, because the local weekday is Tuesday and the doctor has no Tuesday block."
  - id: S-2
    title: See when a Scheduled appointment's doctor is Inactive
    trace: [REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment that has not started, lies inside one of its doctor's availability blocks and whose doctor is Inactive, when staff open its appointment page, then the notice \"The doctor is Inactive. Cancel this appointment or book the patient with another doctor.\" shows above the actions and no outside-availability notice shows."
      - id: AC2
        text: "Given a Scheduled appointment whose start time has already passed and whose doctor is Inactive, when staff open its appointment page, then the Inactive notice shows above the actions."
      - id: AC3
        text: "Given a Scheduled appointment that has not started, lies inside one of its doctor's availability blocks and whose doctor is Active, when staff open its appointment page, then no Inactive notice shows."
  - id: S-3
    title: See both notices, in order, when both apply
    trace: [REQ-6, REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment that has not started, lies in none of its doctor's availability blocks and whose doctor is Inactive, when staff open its appointment page, then both notices show above the actions, the outside-availability notice first and the Inactive notice second."
  - id: S-4
    title: Closed appointments show no notice
    trace: [REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given a Cancelled appointment whose start time has not passed, which lies in none of its doctor's availability blocks and whose doctor is Inactive, when staff open its appointment page, then neither notice shows."
      - id: AC2
        text: "Given a Completed appointment whose doctor is Inactive, when staff open its appointment page, then neither notice shows."
      - id: AC3
        text: "Given a No-show appointment whose doctor is Inactive, when staff open its appointment page, then neither notice shows."
  - id: S-5
    title: The rest of the appointment page is unchanged
    trace: [REQ-9]
    acceptance_criteria:
      - id: AC1
        text: "Given a Scheduled appointment that shows one or both notices, when staff open its appointment page, then when, doctor, patient, reason for visit, the actions and the history show as they did before this change."
      - id: AC2
        text: "Given a Scheduled appointment that shows no notice, when staff open its appointment page, then the page shows exactly what it showed before this change."
      - id: AC3
        text: "Given a Completed, Cancelled or No-show appointment, when staff open its appointment page, then the closed-appointment text and the history show as they did before this change."
---
# Stories: HMS-13

## Stories

### S-1 See when a Scheduled appointment is outside its doctor's availability
Traces REQ-1, REQ-2, REQ-3, REQ-4. The rule is `withinAvailability` (C1). It applies only to appointments that have not started (C2), with no 50-appointment cap (C3), and is evaluated in hospital local time (C4, C5).
- AC1: Given a Scheduled appointment that has not started, whose doctor is Active and whose start and end lie in none of the doctor's availability blocks (so the doctor page flags it), when staff open its appointment page, then "This appointment is outside the doctor's current availability. Reschedule or cancel it." shows above the actions.
- AC2: Given the same kind of appointment lying inside one availability block (so the doctor page does not flag it), when staff open its page, then no outside-availability notice shows.
- AC3: Given a Scheduled appointment of an Active doctor whose start time has passed and that lies outside all blocks, when staff open its page, then no outside-availability notice shows.
- AC4: Given an Active doctor whose 51st upcoming Scheduled appointment (one past the doctor page's 50-appointment limit) lies outside all blocks, when staff open that appointment's page, then the outside-availability notice shows above the actions.
- AC5: Given an Active doctor whose only block is Monday 19:00 to 20:00 local, and an appointment from Tuesday 17 November 2026 01:00 to 01:30 local (Asia/Kolkata = UTC+05:30, so Monday 16 November 2026 19:30 to 20:00 UTC; locally it is after midnight, on Tuesday), when staff open its page, then the outside-availability notice shows, because the local weekday is Tuesday and there is no Tuesday block.

### S-2 See when a Scheduled appointment's doctor is Inactive
Traces REQ-5. Inactive is defined in the glossary (C6).
- AC1: Given a Scheduled appointment that has not started, inside availability, whose doctor is Inactive, when staff open its page, then "The doctor is Inactive. Cancel this appointment or book the patient with another doctor." shows above the actions and no outside-availability notice shows.
- AC2: Given a Scheduled appointment whose start time has passed and whose doctor is Inactive, when staff open its page, then the Inactive notice shows above the actions.
- AC3: Given a Scheduled appointment that has not started, inside availability, whose doctor is Active, when staff open its page, then no Inactive notice shows.

### S-3 See both notices, in order, when both apply
Traces REQ-6, REQ-7.
- AC1: Given a Scheduled appointment that has not started, outside all of its doctor's blocks, whose doctor is Inactive, when staff open its page, then both notices show above the actions, the outside-availability notice first and the Inactive notice second.

### S-4 Closed appointments show no notice
Traces REQ-8. Each case is one that would show a notice if it were still Scheduled (C7).
- AC1: Given a Cancelled appointment whose start time has not passed, outside all blocks, whose doctor is Inactive, when staff open its page, then neither notice shows.
- AC2: Given a Completed appointment whose doctor is Inactive, when staff open its page, then neither notice shows.
- AC3: Given a No-show appointment whose doctor is Inactive, when staff open its page, then neither notice shows.

### S-5 The rest of the appointment page is unchanged
Traces REQ-9. There is one case per partition.
- AC1: Given a Scheduled appointment that shows one or both notices, when staff open its page, then when, doctor, patient, reason for visit, the actions and the history show as before this change.
- AC2: Given a Scheduled appointment that shows no notice, when staff open its page, then the page shows exactly what it showed before this change.
- AC3: Given a Completed, Cancelled or No-show appointment, when staff open its page, then the closed-appointment text and the history show as before this change.

## Known
- S-1: built from REQ-1, REQ-2, REQ-3 and REQ-4, all known in the intent.
- S-2: built from REQ-5, known in the intent.
- S-3: built from REQ-6 and REQ-7, both known in the intent.
- S-4: built from REQ-8, known in the intent.
- S-5: built from REQ-9, known in the intent.

## Assumed
None.

## Unknown
None. The intent has no unknowns or open questions.

## Open questions
None.

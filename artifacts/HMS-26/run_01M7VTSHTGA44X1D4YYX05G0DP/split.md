---
schema: stories.v1
run_id: run_01M7VTSHTGA44X1D4YYX05G0DP
step: split
attempt: 2
skill: story-split@1.4.0
inputs:
  - {path: artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/req.md, sha256: 9a1f1abb91af2282ee0e1cec2d11c2c78175ade5af0865cba4f1e1ec883e3ea7}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The display time zone is read from `useSettings().timeZone`, default Asia/Kolkata.", cites: ["runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-01"]}
  - {id: C2, text: "With no Scheduled appointment after now, next appointment shows `None booked`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/req.md#item:REQ-6"]}
  - {id: C3, text: "With no Completed appointment, last visit shows `No visits yet`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/req.md#item:REQ-10"]}
  - {id: C4, text: "A No-show count of `2 or more` is highlighted as a warning.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/req.md#item:REQ-13"]}
  - {id: C5, text: "Next appointment is the `earliest Scheduled` appointment that starts after now.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/req.md#item:REQ-3"]}
  - {id: C6, text: "Last visit is the `latest Completed` appointment.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/req.md#item:REQ-7"]}
  - {id: C7, text: "A No-show is a past appointment the patient `did not attend`.", cites: ["context/glossary.md#no-show"]}
  - {id: C8, text: "Medical history is blood group, `allergies`, chronic conditions, current medications and notes.", cites: ["context/glossary.md#medical-history"]}
  - {id: C9, text: "The display zone must not be hard-coded: `Do not hard-code Asia/Kolkata`.", cites: ["runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-01"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
resolves:
  - {id: F-1, how: "New story S-6 sets useSettings().timeZone to UTC, not the default, so a hard-coded Asia/Kolkata shows a different date."}
stories:
  - id: S-1
    title: See the next appointment
    trace: [REQ-3, REQ-4, REQ-5, REQ-6, REQ-9]
    acceptance_criteria:
      - {id: AC1, text: "Given now is 10:00 on 10 March hospital time and the patient has a Scheduled appointment that started at 09:00 on 10 March, a Scheduled appointment at 11:00 on 12 March with one doctor and a Scheduled appointment at 15:00 on 11 March with another doctor, when staff open the patient page, then Next appointment shows 11 March 15:00 and the second doctor's name, not the appointment that already started."}
      - {id: AC2, text: "Given the patient's earliest future appointment, at 10:00 on 11 March, is Cancelled and the next one, at 14:00 on 12 March, is Scheduled, when staff open the patient page, then Next appointment shows 12 March 14:00."}
      - {id: AC3, text: "Given Next appointment shows an appointment, when staff select its date and time, then that appointment's page opens."}
      - {id: AC4, text: "Given the patient has only Completed, Cancelled and No-show appointments and no Scheduled appointment after now, when staff open the patient page, then Next appointment shows None booked."}
      - {id: AC5, text: "Given useSettings().timeZone is Asia/Kolkata (UTC+05:30) and the next Scheduled appointment starts at 19:00 UTC on 9 March, when staff open the patient page, then Next appointment shows 10 March 00:30, since 19:00 UTC on 9 March is 00:30 local on 10 March, just after local midnight."}
  - id: S-2
    title: See the last visit
    trace: [REQ-7, REQ-8, REQ-9, REQ-10]
    acceptance_criteria:
      - {id: AC1, text: "Given the patient has a Completed appointment on 1 February with one doctor, a Completed appointment on 3 March with another doctor and a No-show on 5 March, when staff open the patient page, then Last visit shows 3 March and the second doctor's name, not the later No-show."}
      - {id: AC2, text: "Given Last visit shows a Completed appointment that started at 11:30 hospital time, when staff open the patient page, then Last visit shows its date and the doctor's name and no time of day."}
      - {id: AC3, text: "Given useSettings().timeZone is Asia/Kolkata (UTC+05:30) and the latest Completed appointment started at 20:00 UTC on 9 March, when staff open the patient page, then Last visit shows 10 March, since 20:00 UTC on 9 March is 01:30 local on 10 March, after local midnight."}
      - {id: AC4, text: "Given the patient has a Scheduled and a Cancelled appointment but no Completed appointment, when staff open the patient page, then Last visit shows No visits yet."}
  - id: S-3
    title: See the visit counts and the No-show warning
    trace: [REQ-2, REQ-11, REQ-12, REQ-13, REQ-14]
    acceptance_criteria:
      - {id: AC1, text: "Given the patient has a Completed appointment from the previous year, a Completed appointment last week, a Cancelled appointment dated next month, a No-show and a future Scheduled appointment, when staff open the patient page, then the summary shows Completed 2, Cancelled 1 and No-show 1, counting every date and not counting the Scheduled appointment."}
      - {id: AC2, text: "Given the patient has exactly one No-show, when staff open the patient page, then the No-show count shows 1 and is not highlighted as a warning."}
      - {id: AC3, text: "Given the patient has exactly two No-shows, when staff open the patient page, then the No-show count shows 2 and is highlighted as a warning."}
      - {id: AC4, text: "Given the patient has only Completed appointments, when staff open the patient page, then the Cancelled and No-show counts each show 0."}
      - {id: AC5, text: "Given the patient has no appointments at all, when staff open the patient page, then the summary shows None booked, No visits yet and three counts of 0."}
  - id: S-4
    title: Summary placement and privacy
    trace: [REQ-1, REQ-15, REQ-16, REQ-17]
    acceptance_criteria:
      - {id: AC1, text: "Given a patient with appointments, when staff open the patient page, then a Visits summary appears below the patient's name and above the patient's details."}
      - {id: AC2, text: "Given a patient with allergies and current medications recorded, when staff open the patient page, then the Visits summary shows none of the medical history values."}
      - {id: AC3, text: "Given a patient with a phone number and email recorded, when staff open the patient page, then the Visits summary shows neither the phone number nor the email."}
      - {id: AC4, text: "Given a patient with appointments, when staff open the dashboard, the appointments list and the page of that patient's doctor, then none of them shows a Visits summary."}
  - id: S-5
    title: Nothing else on the page or elsewhere changes
    trace: [REQ-18]
    acceptance_criteria:
      - {id: AC1, text: "Given a patient with both upcoming and past appointments, when staff open the patient page, then the details, the medical history and the appointment table (upcoming first, then past) show exactly as before the change."}
      - {id: AC2, text: "Given a patient with no appointments, when staff open the patient page, then the details, the medical history and the empty appointment table show exactly as before the change."}
      - {id: AC3, text: "Given any patient, when staff open the dashboard, the patient list and a doctor page, then each shows exactly as before the change."}
  - id: S-6
    title: Dates follow the hospital time zone setting
    trace: [REQ-9]
    acceptance_criteria:
      - {id: AC1, text: "Given useSettings().timeZone is set to UTC instead of the default Asia/Kolkata and the next Scheduled appointment starts at 19:00 UTC on 9 March, when staff open the patient page, then Next appointment shows 9 March 19:00, not 10 March 00:30 (the Asia/Kolkata reading, UTC+05:30)."}
      - {id: AC2, text: "Given useSettings().timeZone is set to UTC instead of the default Asia/Kolkata and the latest Completed appointment started at 20:00 UTC on 9 March, when staff open the patient page, then Last visit shows 9 March, not 10 March (the Asia/Kolkata reading, 01:30 local after midnight)."}
---
# Stories: HMS-26

## Stories

### S-1 See the next appointment
Traces: REQ-3, REQ-4, REQ-5, REQ-6, REQ-9.
- AC1: Given now is 10:00 on 10 March hospital time and the patient has a Scheduled appointment that started at 09:00 on 10 March, a Scheduled appointment at 11:00 on 12 March with one doctor and a Scheduled appointment at 15:00 on 11 March with another doctor, when staff open the patient page, then Next appointment shows 11 March 15:00 and the second doctor's name, not the appointment that already started.
- AC2: Given the patient's earliest future appointment, at 10:00 on 11 March, is Cancelled and the next one, at 14:00 on 12 March, is Scheduled, when staff open the patient page, then Next appointment shows 12 March 14:00.
- AC3: Given Next appointment shows an appointment, when staff select its date and time, then that appointment's page opens.
- AC4: Given the patient has only Completed, Cancelled and No-show appointments and no Scheduled appointment after now, when staff open the patient page, then Next appointment shows None booked.
- AC5: Given useSettings().timeZone is Asia/Kolkata (UTC+05:30) and the next Scheduled appointment starts at 19:00 UTC on 9 March, when staff open the patient page, then Next appointment shows 10 March 00:30, since 19:00 UTC on 9 March is 00:30 local on 10 March, just after local midnight.

### S-2 See the last visit
Traces: REQ-7, REQ-8, REQ-9, REQ-10.
- AC1: Given a Completed appointment on 1 February, a Completed one on 3 March with another doctor and a No-show on 5 March, then Last visit shows 3 March and that doctor's name, not the later No-show.
- AC2: Given the last visit started at 11:30 hospital time, then Last visit shows its date and the doctor's name and no time of day.
- AC3: Given Asia/Kolkata (UTC+05:30) from useSettings().timeZone and the latest Completed appointment at 20:00 UTC on 9 March, then Last visit shows 10 March (01:30 local, after local midnight).
- AC4: Given a Scheduled and a Cancelled appointment but no Completed one, then Last visit shows No visits yet.

### S-3 See the visit counts and the No-show warning
Traces: REQ-2, REQ-11, REQ-12, REQ-13, REQ-14.
- AC1: Completed from the previous year and last week, a Cancelled dated next month, a No-show and a future Scheduled → Completed 2, Cancelled 1, No-show 1; every date counts, Scheduled is not counted.
- AC2: exactly one No-show → count 1, not highlighted.
- AC3: exactly two No-shows → count 2, highlighted as a warning.
- AC4: only Completed appointments → Cancelled and No-show each show 0.
- AC5: no appointments → None booked, No visits yet and three counts of 0.

### S-4 Summary placement and privacy
Traces: REQ-1, REQ-15, REQ-16, REQ-17.
- AC1: the Visits summary is below the patient's name and above the details.
- AC2: allergies and current medications recorded → the summary shows no medical history value.
- AC3: phone number and email recorded → the summary shows neither.
- AC4: the dashboard, the appointments list and the doctor's page show no Visits summary.

### S-5 Nothing else on the page or elsewhere changes
Traces: REQ-18.
- AC1: with upcoming and past appointments, details, medical history and the appointment table (upcoming first, then past) are unchanged.
- AC2: with no appointments, details, medical history and the empty table are unchanged.
- AC3: the dashboard, the patient list and a doctor page are unchanged.

### S-6 Dates follow the hospital time zone setting
Traces: REQ-9.
- AC1: Given useSettings().timeZone is set to UTC instead of the default Asia/Kolkata and the next Scheduled appointment starts at 19:00 UTC on 9 March, then Next appointment shows 9 March 19:00, not 10 March 00:30 (the Asia/Kolkata reading).
- AC2: Given useSettings().timeZone is set to UTC and the latest Completed appointment started at 20:00 UTC on 9 March, then Last visit shows 9 March, not 10 March (the Asia/Kolkata reading).

Resolves F-1: New story S-6 sets useSettings().timeZone to UTC, not the default, so a hard-coded Asia/Kolkata shows a different date.

## Known
- S-1: from known REQ-3 to REQ-6 and REQ-9 (time zone decided in runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-01).
- S-2: from known REQ-7, REQ-8, REQ-9, REQ-10.
- S-3: from known REQ-2, REQ-11 to REQ-14.
- S-4: from known REQ-1, REQ-15, REQ-16, REQ-17.
- S-5: from known REQ-18.
- S-6: from known REQ-9 (zone read from Settings, not hard-coded, per runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-01).

## Assumed
None.

## Unknown
None.

## Open questions
None.

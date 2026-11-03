---
schema: stories.v1
run_id: run_01M6FXTGC048AXKA8X69ASS5QB
step: split
attempt: 1
skill: story-split@1.0.1
inputs:
  - path: artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/req.md
    sha256: 8cacd7572d8146137cd6151565f9b5e880d72194da94af616afa60fd16406ef8
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "A `No-show` is a past appointment the patient did not attend, closed instead of staying Scheduled."
    cites: ["context/glossary.md#no-show"]
  - id: C2
    text: "A dashboard `Tile` is one count with its label; selecting it opens the list behind the count."
    cites: ["context/glossary.md#tile"]
  - id: C3
    text: "The 30-day window and end of today are computed in `Asia/Kolkata`."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/req.md#requirements"]
  - id: C4
    text: "The count reuses the window and counting rule of the `dashboard` No-shows tile for Last 30 days."
    cites: ["artifacts/HMS-7/run_01M6FXTGC048AXKA8X69ASS5QB/req.md#requirements"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [S-1, S-2]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See a doctor's No-shows in the last 30 days on the doctor page
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor with No-show appointments whose start time falls in the window of the dashboard No-shows tile for Last 30 days, when front-desk staff open that doctor's page, then the doctor's details section shows a field labelled \"No-shows (last 30 days)\" with the number of those appointments."
      - id: AC2
        text: "Given a doctor with a No-show appointment starting before the window, an appointment starting after the end of today in Asia/Kolkata, and Scheduled, Completed and Cancelled appointments inside the window, when staff open the doctor's page, then none of those appointments is counted."
      - id: AC3
        text: "Given the same appointments, when staff open the doctor's page, then the count equals the number of that doctor's appointments that the dashboard No-shows tile's Last 30 days rule counts."
      - id: AC4
        text: "Given a doctor with no No-show appointments in the window, when staff open the doctor's page, then \"No-shows (last 30 days)\" shows 0."
      - id: AC5
        text: "Given any doctor, when staff open the doctor's page, then the details, weekly availability, upcoming appointments and day schedule are shown and behave as before."
  - id: S-2
    title: Open the No-show appointments behind the count
    trace: [REQ-6, REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given the doctor page shows a No-shows (last 30 days) count, when staff select the count, then the appointment list opens filtered to that doctor, status No-show and the same 30-day window."
      - id: AC2
        text: "Given the appointment list opened from the count, when staff look at it, then the number of appointments listed equals the count shown on the doctor page."
      - id: AC3
        text: "Given a doctor whose count is 0, when staff select the 0, then the appointment list opens with the same filters and shows no appointments."
---
# Stories: HMS-7

## Stories

### S-1 See a doctor's No-shows in the last 30 days on the doctor page
Traces: REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-8.
- AC1: Given a doctor with No-show appointments starting in the window of the dashboard No-shows tile for Last 30 days, when front-desk staff open that doctor's page, then the details section shows "No-shows (last 30 days)" with the number of those appointments.
- AC2: Given No-shows outside the window (before it, or after the end of today in Asia/Kolkata) and Scheduled, Completed or Cancelled appointments inside it, when staff open the page, then none of them is counted.
- AC3: Given the same appointments, when staff open the page, then the count equals what the dashboard No-shows tile's Last 30 days rule counts for that doctor.
- AC4: Given a doctor with no No-shows in the window, when staff open the page, then the field shows 0.
- AC5: Given any doctor, when staff open the page, then details, weekly availability, upcoming appointments and day schedule behave as before.

The window is defined once, by REQ-4 (the dashboard tile's rule, answer A-01); REQ-2 and REQ-3 describe it in words and are checked against that rule.

### S-2 Open the No-show appointments behind the count
Traces: REQ-6, REQ-7.
- AC1: Given a count is shown, when staff select it, then the appointment list opens filtered to that doctor, No-show and the same 30-day window.
- AC2: Given that list, then the number of appointments listed equals the count.
- AC3: Given a count of 0, when staff select it, then the list opens with the same filters and shows no appointments.

## Known
- S-1: built only from known REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-8.
- S-2: built only from known REQ-6, REQ-7.

## Assumed
None.

## Unknown
None. Every REQ in the intent is known and covered.

## Open questions
None.

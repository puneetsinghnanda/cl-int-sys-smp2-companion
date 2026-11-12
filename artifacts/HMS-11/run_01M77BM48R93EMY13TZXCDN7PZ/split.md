---
schema: stories.v1
run_id: run_01M77BM48R93EMY13TZXCDN7PZ
step: split
attempt: 2
skill: story-split@1.2.0
inputs:
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md
    sha256: 9020c22eea6b4e433f7a024deb321a42e4042b10165a1d67058dd3d9e7f1fc2c
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
resolves:
  - id: F-1
    how: "S-3 AC4 now uses null-slot names whose order changes with case, and S-3 AC5 checks the id tie-break for null-slot entries."
  - id: F-2
    how: "S-2 AC1 and AC2 now pin the boundary slot times on either side of the 08:00 local now."
  - id: F-3
    how: "S-1 AC4 returns a slot freed by a Cancelled appointment, and S-6 AC3 checks the day schedule with a Cancelled appointment is unchanged."
claims:
  - id: C1
    text: "The hospital time zone is `Asia/Kolkata`, a fixed IANA zone with no daylight saving (UTC+05:30), so local and UTC dates differ between local 00:00 and 05:29."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#claim:C1"]
  - id: C2
    text: "Doctor codes such as `D-0001` are display codes derived from row ids."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#claim:C5"]
  - id: C3
    text: "System lists normally answer `20 per page`; the new endpoint is unpaged."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#claim:C8"]
  - id: C4
    text: "The day schedule of one doctor and date is served from `api/src/routes/doctors.ts`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/req.md#claim:C3"]
  - id: C5
    text: "An Inactive `specialization` is kept for history but not offered for new use."
    cites: ["context/glossary.md#inactive"]
  - id: C6
    text: "When an appointment is Cancelled its slot becomes `free again`."
    cites: ["context/glossary.md#cancelled"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: Each Active doctor of a specialization with their earliest free slot
    trace: [REQ-1, REQ-3, REQ-4, REQ-6]
    acceptance_criteria:
      - id: AC1
        text: "Given Active doctor D of Cardiology whose first FREE slot of the window is 09:30 local on 2026-10-20, when staff call GET /api/specializations/:id/earliest-slots for Cardiology, then D's entry has id, code, full name, department name and that slot."
      - id: AC2
        text: "Given D's first FREE slot is 09:30 local on 2026-10-20, when staff call the endpoint, then the slot is date 2026-10-20, start 2026-10-20T04:00:00.000Z, label 09:30 (UTC+05:30, so 09:30 local is 04:00 UTC)."
      - id: AC3
        text: "Given D's first slot of the window is BOOKED and the next slot that date is FREE, when staff call the endpoint, then D's slot is the FREE one."
      - id: AC4
        text: "Given D's first slot of the window had an appointment that was Cancelled, so the day schedule shows it FREE again, when staff call the endpoint, then D's slot is that first slot."
      - id: AC5
        text: "Given an Inactive Cardiology doctor with an earlier FREE slot than any Active one, and an Active doctor of another specialization, when staff call the endpoint for Cardiology, then neither appears in items."
  - id: S-2
    title: The window is today to today+6 in the hospital time zone and skips started slots
    trace: [REQ-2, REQ-5, REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given now is 08:00 local on 2026-10-19 and D's only FREE slot is 11:30 local on 2026-10-25 (today+6, after 08:00), when staff call the endpoint, then D's slot is 11:30 on 2026-10-25."
      - id: AC2
        text: "Given now is 08:00 local on 2026-10-19 and D's only FREE slot is 07:30 local on 2026-10-26 (the day after today+6, before 08:00), when staff call the endpoint, then D's entry has slot: null."
      - id: AC3
        text: "Given now is 09:05 local on 2026-10-19 and D's 09:00 slot that day has no appointment (already started, so PAST), when staff call the endpoint, then D's slot is the next FREE slot after 09:05, never 09:00."
      - id: AC4
        text: "Given now is 2026-11-11T19:00:00Z (00:30 local on 12 Nov; UTC date still 11 Nov) and D's only FREE slots are on local 2026-11-18, when staff call the endpoint, then D's slot date is 2026-11-18."
  - id: S-3
    title: Entries are ordered by earliest slot, then by doctor name
    trace: [REQ-8, REQ-9, REQ-17]
    acceptance_criteria:
      - id: AC1
        text: "Given doctor A's earliest FREE slot is 10:00 local and doctor B's is 09:30 local on the same date, when staff call the endpoint, then B comes before A."
      - id: AC2
        text: "Given doctors \"dr. alpha\" and \"Dr. Beta\" with the same earliest slot start, when staff call the endpoint, then \"dr. alpha\" comes first (names compare ignoring case)."
      - id: AC3
        text: "Given two doctors with the same full name and the same earliest slot start, when staff call the endpoint, then the one with the lower id comes first."
      - id: AC4
        text: "Given \"dr. zed\" has a FREE slot and \"dr. amy\" and \"Dr. Bob\" have none in the window, when staff call the endpoint, then the order is dr. zed, dr. amy, Dr. Bob (a case-sensitive sort would put Dr. Bob before dr. amy)."
      - id: AC5
        text: "Given two doctors with the same full name and no FREE slot in the window, when staff call the endpoint, then both come last and the one with the lower id comes first."
  - id: S-4
    title: One unpaged list, with clear empty and error answers
    trace: [REQ-10, REQ-11, REQ-12, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a specialization with one more Active doctor than the 20 per page convention (a 21st doctor), when staff call the endpoint, then items holds all of them, including the 21st, with no page, page_size or total."
      - id: AC2
        text: "Given an Active specialization whose only doctor is Inactive, when staff call the endpoint, then the response is { items: [] }."
      - id: AC3
        text: "Given no specialization has the requested id, when staff call the endpoint, then it returns 404."
      - id: AC4
        text: "Given an Inactive specialization that has Active doctors, when staff call the endpoint for it, then it returns 400 with the message \"specialization is Inactive\"."
  - id: S-5
    title: No personal data leaks from the endpoint
    trace: [REQ-14, REQ-15]
    acceptance_criteria:
      - id: AC1
        text: "Given D has a phone and an email on record, when staff call the endpoint, then D's entry contains neither."
      - id: AC2
        text: "Given D's first window slot is BOOKED by a patient with allergies and medical notes, when staff call the endpoint, then the response has no patient name, allergies, notes or other medical history."
  - id: S-6
    title: Nothing else changes
    trace: [REQ-16]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor and date with a BOOKED appointment, when staff read that day schedule before and after the change, then both responses are the same."
      - id: AC2
        text: "Given a doctor and date with no appointment, when staff read that day schedule before and after the change, then both responses are the same."
      - id: AC3
        text: "Given a doctor and date with only a Cancelled appointment, when staff read that day schedule before and after the change, then both responses are the same."
      - id: AC4
        text: "Given a FREE slot returned by the endpoint, when staff book it through the existing booking endpoint, then booking succeeds under the existing rules."
---
# Stories: HMS-11

## Stories

### S-1 Each Active doctor of a specialization with their earliest free slot
Traces REQ-1, REQ-3, REQ-4, REQ-6.
- AC1: D's entry has id, code, full name, department name and the first FREE slot of the window.
- AC2: The slot at 09:30 local on 2026-10-20 is given as date 2026-10-20, start 2026-10-20T04:00:00.000Z and label 09:30 (UTC+05:30).
- AC3: A BOOKED first slot is skipped for the next FREE slot.
- AC4: A slot freed by a Cancelled appointment is FREE again and is returned.
- AC5: Inactive doctors, and doctors of other specializations, never appear, even with an earlier FREE slot.

### S-2 The window is today to today+6 in the hospital time zone and skips started slots
Traces REQ-2, REQ-5, REQ-7.
- AC1: At 08:00 local on 2026-10-19, a FREE slot at 11:30 on today+6 (2026-10-25) is returned.
- AC2: At 08:00 local on 2026-10-19, a FREE slot at 07:30 on the day after today+6 (2026-10-26) gives slot: null; a window ending at 08:00 that day would wrongly include it.
- AC3: At 09:05 local the started 09:00 slot is never returned.
- AC4: At 2026-11-11T19:00:00Z (00:30 local on 12 Nov) the window is counted from local 12 Nov, so FREE slots on 18 Nov are returned; a UTC-date window would miss them.

### S-3 Entries are ordered by earliest slot, then by doctor name
Traces REQ-8, REQ-9, REQ-17.
- AC1: An earlier slot comes first.
- AC2: On a tie, names compare ignoring case.
- AC3: Same name and same slot: the lower doctor id comes first.
- AC4: Entries with slot: null come last, ordered by name ignoring case (dr. amy before Dr. Bob).
- AC5: Null-slot entries with the same name: the lower doctor id comes first.

### S-4 One unpaged list, with clear empty and error answers
Traces REQ-10, REQ-11, REQ-12, REQ-13.
- AC1: With one more Active doctor than the 20 per page convention, all are returned, with no paging fields.
- AC2: A specialization whose only doctor is Inactive gives { items: [] }.
- AC3: An unknown id gives 404.
- AC4: An Inactive specialization gives 400 "specialization is Inactive".

### S-5 No personal data leaks from the endpoint
Traces REQ-14, REQ-15.
- AC1: No phone or email in entries.
- AC2: No patient name or medical history, even when the first slot is booked by a patient with history.

### S-6 Nothing else changes
Traces REQ-16.
- AC1: The day schedule for a date with a BOOKED appointment is unchanged.
- AC2: The day schedule for a date with no appointment is unchanged.
- AC3: The day schedule for a date with only a Cancelled appointment is unchanged.
- AC4: Booking a returned FREE slot works under the existing rules.

Resolved review findings:
- F-1: S-3 AC4 now uses null-slot names whose order changes with case, and S-3 AC5 checks the id tie-break for null-slot entries.
- F-2: S-2 AC1 and AC2 now pin the boundary slot times on either side of the 08:00 local now.
- F-3: S-1 AC4 returns a slot freed by a Cancelled appointment, and S-6 AC3 checks the day schedule with a Cancelled appointment is unchanged.

## Known
- S-1: from known REQ-1, REQ-3, REQ-4, REQ-6 (ticket acceptance criteria).
- S-2: from known REQ-2, REQ-5, REQ-7.
- S-3: from known REQ-8, REQ-9 and REQ-17 (confirmed in this run, A-01).
- S-4: from known REQ-10, REQ-11, REQ-12, REQ-13.
- S-5: from known REQ-14, REQ-15.
- S-6: from known REQ-16.

## Assumed
None.

## Unknown
None.

## Open questions
None.

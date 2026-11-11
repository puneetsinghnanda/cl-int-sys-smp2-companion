---
schema: stories.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
step: split
attempt: 2
skill: story-split@1.2.0
inputs:
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/req.md
    sha256: d91af6d2d33b27873ad261064f8454e09c4904922e83e8ea0b13ffb56bc031e8
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
resolves:
  - id: F-1
    how: "S-4 AC3 and AC4 now compare against the page as it was before this change and name no unsourced UI text."
claims:
  - id: C1
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/req.md#claim:C1"]
  - id: C2
    text: "A slot starts at the start of an `availability block` and steps by the doctor's slot length."
    cites: ["context/glossary.md#slot"]
  - id: C3
    text: "A `Cancelled` appointment's slot becomes free again."
    cites: ["context/glossary.md#cancelled"]
  - id: C4
    text: "Counts follow the `day schedule` states: a past slot with a non-cancelled appointment counts as booked, a past empty slot counts as neither."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/req.md#item:REQ-4", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-01"]
  - id: C5
    text: "The day link uses a `?date=` parameter on the doctor page."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/req.md#item:REQ-7", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-02"]
  - id: C6
    text: "The rest of the doctor page is `unchanged`."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/req.md#item:REQ-10"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: Front-desk staff see booked and free counts for the next 7 days
    trace: [REQ-1, REQ-2, REQ-3, REQ-4]
    acceptance_criteria:
      - id: AC1
        text: "Given an active doctor and the clock at 2026-11-11T19:00Z, which is 00:30 on 12 Nov in Asia/Kolkata (UTC+05:30, so after local midnight while the UTC date is still 11 Nov), when staff open the doctor page, then a section titled 'Next 7 days' lists one row per date from 12 Nov to 18 Nov (today to 6 days ahead), in date order, and no row for 11 Nov or 19 Nov."
      - id: AC2
        text: "Given an active doctor whose slots on 14 Nov are 09:00, 09:30 and 10:00 local, with a Scheduled appointment in the 09:30 slot, when staff open the doctor page, then the 14 Nov row shows its date, booked 1 and free 2, the same as the day schedule shows for 14 Nov."
      - id: AC3
        text: "Given today's slots are 09:00, 09:30 and 10:00 local, the 09:00 slot holds a Completed appointment, the 09:30 slot is empty, and the clock is 09:45 local, when staff open the doctor page, then today's row shows booked 1 and free 1 (the passed 09:00 slot with an appointment counts as booked; the passed empty 09:30 slot counts as neither)."
      - id: AC4
        text: "Given an active doctor whose 10:00 slot on 15 Nov holds only a Cancelled appointment, when staff open the doctor page, then the 15 Nov row counts that slot as free, not booked, matching the day schedule for 15 Nov."
  - id: S-2
    title: Days without slots, or with nothing free left, are shown correctly
    trace: [REQ-5, REQ-6]
    acceptance_criteria:
      - id: AC1
        text: "Given an active doctor with no availability block on the weekday of 16 Nov, when staff open the doctor page, then the 16 Nov row shows 'Not available' and no booked or free counts."
      - id: AC2
        text: "Given today's slots are 09:00 and 09:30 local, the 09:00 slot holds a No-show appointment, the 09:30 slot is empty, and the clock is 18:00 local, when staff open the doctor page, then today's row shows booked 1 and free 0, not 'Not available'."
      - id: AC3
        text: "Given today's slots are 09:00 and 09:30 local, both empty, and the clock is 18:00 local, when staff open the doctor page, then today's row shows booked 0 and free 0, not 'Not available', because the day has slots."
  - id: S-3
    title: Each day opens the day schedule for that date
    trace: [REQ-7, REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given the 'Next 7 days' section of doctor D's page lists a row for 2026-11-14, when staff select that row, then the browser goes to doctor D's page with ?date=2026-11-14 in the URL."
      - id: AC2
        text: "Given staff open doctor D's page with ?date=2026-11-14 in the URL, when the page loads, then the day schedule's date field shows 2026-11-14 and its slot list is the one for 14 Nov."
      - id: AC3
        text: "Given the clock is 2026-11-11T19:00Z (00:30 on 12 Nov in Asia/Kolkata), when staff open doctor D's page with no ?date= parameter, then the day schedule shows 12 Nov (local today), as it does now."
      - id: AC4
        text: "Given the 'Not available' row for 16 Nov, when staff select that row, then the doctor page opens with ?date=2026-11-16 and the day schedule for 16 Nov shows exactly what it shows today when 16 Nov is picked by hand."
  - id: S-4
    title: Inactive doctors show no section, and the rest of the page is unchanged
    trace: [REQ-9, REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given an Inactive doctor who has availability blocks and Scheduled appointments on dates within the next week, when staff open the doctor page, then no 'Next 7 days' section is shown."
      - id: AC2
        text: "Given an active doctor with upcoming appointments, when staff open the doctor page, then every section other than 'Next 7 days' shows the same content as on the build before this change, for the same doctor and clock."
      - id: AC3
        text: "Given an active doctor with no availability set and no upcoming appointments, when staff open the doctor page, then every section other than 'Next 7 days' shows the same content as on the build before this change, and every row in 'Next 7 days' shows 'Not available'."
      - id: AC4
        text: "Given an Inactive doctor with upcoming appointments, when staff open the doctor page, then every section and action on the page is the same as on the build before this change, for the same doctor and clock."
---
# Stories: HMS-9

Resolved review findings:
- F-1: S-4 AC3 and AC4 now compare against the page as it was before this change and name no unsourced UI text.

## Stories

### S-1 Front-desk staff see booked and free counts for the next 7 days
Traces REQ-1, REQ-2, REQ-3, REQ-4.
- AC1: The clock is 2026-11-11T19:00Z, which is 00:30 on 12 Nov in Asia/Kolkata (UTC+05:30). That is after local midnight while the UTC date is still 11 Nov. The section "Next 7 days" lists the rows 12 Nov to 18 Nov in order; there is no row for 11 Nov or 19 Nov.
- AC2: 14 Nov has slots at 09:00, 09:30 and 10:00, and 09:30 holds a Scheduled appointment. The row shows the date, booked 1 and free 2, the same as the day schedule.
- AC3: Today's 09:00 slot holds a Completed appointment, 09:30 is empty, and the clock is 09:45 local. The row shows booked 1 and free 1. The passed slot with an appointment counts as booked; the passed empty slot counts as neither (A-01).
- AC4: On 15 Nov the 10:00 slot holds only a Cancelled appointment. That slot counts as free, as in the day schedule.

### S-2 Days without slots, or with nothing free left, are shown correctly
Traces REQ-5, REQ-6.
- AC1: A day with no availability block shows "Not available" and no counts.
- AC2: Late today, a passed No-show slot plus a passed empty slot shows booked 1 and free 0, not "Not available" (A-04).
- AC3: Late today, with two passed empty slots, the row shows booked 0 and free 0, not "Not available", because the day has slots.

### S-3 Each day opens the day schedule for that date
Traces REQ-7, REQ-8.
- AC1: Selecting the 14 Nov row goes to the doctor page with `?date=2026-11-14` (A-02).
- AC2: Opening the doctor page with `?date=2026-11-14` shows 14 Nov in the day schedule.
- AC3: Without the parameter, the day schedule shows local today, which is 12 Nov at 00:30 IST while the UTC date is still 11 Nov.
- AC4: Selecting a "Not available" day opens its date, and the day schedule for that date looks exactly as it does when the date is picked by hand.

### S-4 Inactive doctors show no section, and the rest of the page is unchanged
Traces REQ-9, REQ-10.
- AC1: An Inactive doctor with availability and upcoming appointments shows no "Next 7 days" section.
- AC2: An active doctor with upcoming appointments: every other section matches the build before this change.
- AC3: An active doctor with no availability and no upcoming appointments: every other section matches the build before this change, and every new row shows "Not available".
- AC4: An Inactive doctor with upcoming appointments: every section and action matches the build before this change.

## Known
- S-1: built from known REQ-1 to REQ-4. The counting rule comes from A-01.
- S-2: built from known REQ-5 and REQ-6. REQ-6 was confirmed in A-04.
- S-3: built from known REQ-7 and REQ-8. The link shape comes from A-02.
- S-4: built from known REQ-9 and REQ-10.

## Assumed
None.

## Unknown
None. The intent has no unknown requirements or open questions.

## Open questions
None.

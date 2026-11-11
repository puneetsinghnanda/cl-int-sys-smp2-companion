---
schema: intent.v1
run_id: run_01M74D9VA0FDDS3DN77YFEXK7H
step: req
attempt: 1
skill: requirement-review@1.2.0
inputs:
  - path: artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/ticket.md
    sha256: dfe68fd254854a32c8908ac4c794bb8da72ee1d10ad9dab518ed0feb5e664a14
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C2
    text: "A slot starts at the start of an `availability block` and steps by the doctor's slot length."
    cites: ["context/glossary.md#slot"]
  - id: C3
    text: "An `Inactive` doctor is kept for history but not offered for new use."
    cites: ["context/glossary.md#inactive"]
  - id: C4
    text: "A slot is free only inside the doctor's availability, in the `future` and not overlapping a Scheduled appointment."
    cites: ["context/intent-baseline.md#booking-rules"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Next 7 days section on the doctor page
    type: functional
    status: known
    statement: "The doctor page has a section titled `Next 7 days`."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/ticket.md#L21"]
  - id: REQ-2
    title: One row per day, today to 6 days ahead, hospital time zone
    type: functional
    status: known
    statement: "The section has one row per day from today to `6 days ahead`, with days taken in the hospital time zone (Asia/Kolkata)."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/ticket.md#L21-L22", "context/intent-baseline.md#constraints"]
  - id: REQ-3
    title: Row content
    type: functional
    status: known
    statement: "Each row shows `the date, booked slots and free slots`."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/ticket.md#L22"]
  - id: REQ-4
    title: Counts follow the day schedule's slot states
    type: functional
    status: known
    statement: "Booked and free counts for a day equal the number of BOOKED and FREE slots the `day schedule` shows for that date. A past slot with a non-cancelled appointment counts as booked; a past slot without one counts as neither."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/ticket.md#L23", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-01"]
  - id: REQ-5
    title: Not available for a day without slots
    type: functional
    status: known
    statement: "A day on which the doctor has no slots shows `Not available` instead of counts."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/ticket.md#L25"]
  - id: REQ-6
    title: A day whose slots have all passed still shows counts
    type: functional
    status: known
    statement: "A day that has slots but no free slots left (for example late today) shows its booked and free counts, with free 0, not `Not available`."
    cites: ["runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-04", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-03"]
  - id: REQ-7
    title: Each day links to the day schedule for that date
    type: functional
    status: known
    statement: "Each row links to the doctor page with a `?date=` parameter set to that row's date (YYYY-MM-DD)."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/ticket.md#L26", "runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-02"]
  - id: REQ-8
    title: Day schedule opens on the date in the URL
    type: functional
    status: known
    statement: "When the doctor page URL carries a `?date=` parameter, the day schedule shows that date; without it, the day schedule still starts at today as it does now."
    cites: ["runs/run_01M74D9VA0FDDS3DN77YFEXK7H/ide-answers.md#a-02"]
  - id: REQ-9
    title: No section for an inactive doctor
    type: functional
    status: known
    statement: "An `inactive` doctor's page shows no Next 7 days section."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/ticket.md#L27"]
  - id: REQ-10
    title: Rest of the doctor page unchanged
    type: constraint
    status: known
    statement: "The rest of the doctor page is `unchanged`; the only change outside the new section is that the day schedule's start date can come from the URL."
    cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/ticket.md#L28"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "What is compatible with the current behaviour? What makes sense? Use that."
    requirements: [REQ-4]
    mismatch:
      text: "Read literally, the ticket leaves every already-passed slot today out of both counts. The user chose to follow the current day schedule instead, so a past slot with an appointment counts as booked and only an empty past slot counts as neither. This also keeps the ticket's first sentence (counted the same way as the day schedule)."
      cites: ["artifacts/HMS-9/run_01M74D9VA0FDDS3DN77YFEXK7H/ticket.md#L24"]
  - id: A-02
    kind: question
    about: Q-02
    answer: "URL ?date= param (proposed default)"
    requirements: [REQ-7, REQ-8]
  - id: A-03
    kind: assumption
    about: REQ-6
    answer: "Not sure. This relates to q1."
    requirements: [REQ-6]
  - id: A-04
    kind: assumption
    about: REQ-6
    answer: "Yes, confirm"
    requirements: [REQ-6]
glossary:
  - {term: Slot, found: true}
  - {term: Availability block, found: true}
  - {term: Inactive, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Cancelled, found: true}
  - {term: Day schedule, found: false}
---
# Requirements review: HMS-9

## Summary
HMS-9 adds a "Next 7 days" section to the doctor page. It has one row per day from today to 6 days ahead in hospital time, showing the day's booked and free slot counts and a link to that day's schedule. The main risk was a conflict inside the ticket: it says the counts match the day schedule, but also that passed slots count as neither, while the day schedule keeps a passed slot with an appointment as booked. The user resolved this in favour of the current day-schedule behaviour (A-01). The second risk is that the day schedule's date can't be opened from a link today; the user chose a `?date=` URL parameter (A-02), which is a small change to the existing day schedule.

Notes from the product code at the pinned SHA, for the impact and design steps (not cited, because these files are not declared inputs of this step):
- `api/src/services/doctorService.ts` `schedule` marks a slot BOOKED when an appointment overlaps it, and checks this before PAST. A past slot with an appointment therefore shows as booked.
- `api/src/repositories/appointmentRepo.ts` `occupyingBetween` counts every appointment that is not CANCELLED as taking a slot.
- `web/src/pages/DoctorDetailPage.tsx` shows "No availability on this day" when a day has no slots. The day schedule's date is component state that starts at `todayIn(timeZone)`; no URL parameter sets it.

## Requirements
- REQ-1 Next 7 days section on the doctor page (known): the doctor page has a section titled "Next 7 days".
- REQ-2 One row per day, today to 6 days ahead, hospital time zone (known).
- REQ-3 Row content (known): the date, booked slots and free slots.
- REQ-4 Counts follow the day schedule's slot states (known): BOOKED and FREE slots as the day schedule shows them; a past slot with a non-cancelled appointment counts as booked, and an empty past slot counts as neither.
- REQ-5 Not available for a day without slots (known): shows "Not available" instead of counts.
- REQ-6 A day whose slots have all passed still shows counts (known): free shows 0, not "Not available".
- REQ-7 Each day links to the day schedule for that date (known): the link is the doctor page with `?date=YYYY-MM-DD`.
- REQ-8 Day schedule opens on the date in the URL (known): with no parameter it starts at today, as now.
- REQ-9 No section for an inactive doctor (known).
- REQ-10 Rest of the doctor page unchanged (known, constraint).

## Known
- REQ-1, REQ-2, REQ-3: ticket AC 1 (L21-L22); the time zone is from the baseline constraints.
- REQ-4: ticket AC 2 (L23) and A-01.
- REQ-5: ticket AC 3 (L25); "no availability" uses the same test as the day schedule, which is that the day has no slots.
- REQ-6: the user confirmed it in A-04, after A-03 tied it to Q-01.
- REQ-7, REQ-8: ticket AC 4 (L26) and A-02.
- REQ-9: ticket AC 5 (L27).
- REQ-10: ticket AC 6 (L28).

## Assumed
None. The user confirmed every assumption.

## Unknown
None. Every question was answered.

## Open questions
None.

Answered in the IDE:
- A-01 (eng-3c51), Q-01, how passed slots today are counted: "What is compatible with the current behaviour? What makes sense? Use that." Applied as: match the day schedule, so a past slot with an appointment counts as booked and an empty past slot counts as neither. Decides REQ-4. Mismatch: read literally, the ticket's AC 2 second sentence (L24) leaves out every passed slot.
- A-02 (eng-3c51), Q-02, how the day link works: "URL ?date= param (proposed default)". Decides REQ-7 and REQ-8.
- A-03 (eng-3c51), REQ-6 assumption: "Not sure. This relates to q1." This tied REQ-6 to Q-01, so the question was asked again with the Q-01 outcome (A-04). REQ-6 rests on both answers.
- A-04 (eng-3c51), REQ-6 assumption, asked again: "Yes, confirm". Decides REQ-6.

## Out of scope
- The user story's aim of offering "the day with the most free slots" (L13-L14) is a goal, not a behaviour. No ranking or highlighting of the best day is required.
- The term "Day schedule" is not in the glossary. It refers to the existing APT-10 section of the doctor page (DoctorDetailPage.tsx). This doesn't block the work; the tech lead may want to add it to the glossary.

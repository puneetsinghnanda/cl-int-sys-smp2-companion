---
schema: stories.v1
run_id: run_01M8EC1C803452B4P6VTAB10M7
step: split
attempt: 1
skill: story-split@1.6.0
inputs:
  - {path: artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/req.md, sha256: 69e5f24b8ab6d28047031767176af38a9746fac0c500328b4336461cd9a40365}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "Appointments that occupy a slot exclude status `'CANCELLED'`; Scheduled, Completed and No-show appointments occupy it.", cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L117"]}
  - {id: C2, text: "An unbooked slot whose start is not after now is `PAST`, not free; a booked slot stays booked.", cites: ["sample-system:api/src/services/doctorService.ts#L161"]}
  - {id: C3, text: "Today is the current day in the hospital time zone, `Asia/Kolkata`.", cites: ["artifacts/HMS-23/run_01M8EC1C803452B4P6VTAB10M7/req.md#item:REQ-3"]}
  - {id: C4, text: "A doctor row stores a `phone`.", cites: ["context/generated/db-schema.json#column:doctor.phone"]}
  - {id: C5, text: "A doctor row stores an `email`.", cites: ["context/generated/db-schema.json#column:doctor.email"]}
  - {id: C6, text: "The doctor page opens its day schedule on a given day through a `?date=` link.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L115"]}
  - {id: C7, text: "The day schedule shows each slot's time as its `label`.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L168"]}
  - {id: C8, text: "Weekly `availability_block` rows hold a weekday and local wall-clock times.", cites: ["context/data-model.md#doctors"]}
  - {id: C9, text: "The doctor page appends `(Inactive)` to an Inactive department.", cites: ["sample-system:web/src/pages/DoctorDetailPage.tsx#L74"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [S-1, S-2, S-3, S-4, S-5, S-6, S-7]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: "See which doctors are on duty today"
    trace: [REQ-1, REQ-2, REQ-3, REQ-14, REQ-15]
    acceptance_criteria:
      - {id: AC1, text: "Given Active doctor A has an availability block for today's weekday and Active doctor B has none for today, when staff open the dashboard, then below the tiles a section titled \"On duty today\" lists doctor A and does not list doctor B."}
      - {id: AC2, text: "Given now is 04:00 local on Saturday 28 November 2026 (Asia/Kolkata = UTC+05:30, so 22:30 UTC on Friday 27 November; local 00:00 to 05:29 is still the previous UTC date), Active doctor A is available on Saturdays only and Active doctor B on Fridays only, when staff open the dashboard, then doctor A is listed and doctor B is not."}
      - {id: AC3, text: "Given doctor C is Inactive, has availability today and has more free slots today than any listed doctor, when staff open the dashboard, then doctor C is not listed."}
      - {id: AC4, text: "Given no Active doctor has availability today and Inactive doctor C has availability today, when staff open the dashboard, then the section shows \"No doctors on duty today\" and no rows."}
  - id: S-2
    title: "See who each on-duty doctor is, without contact details"
    trace: [REQ-4, REQ-5, REQ-6, REQ-17]
    acceptance_criteria:
      - {id: AC1, text: "Given Active doctor A in Active department X is on duty today, when staff open the dashboard, then doctor A's row shows doctor A's name and department X's name with no marker."}
      - {id: AC2, text: "Given Active doctor A is on duty today and department X is Inactive, when staff open the dashboard, then doctor A's row shows the department as \"X (Inactive)\"."}
      - {id: AC3, text: "Given doctor A is on duty today and has a phone and an email stored, when staff open the dashboard, then doctor A's row shows neither the phone nor the email."}
  - id: S-3
    title: "See booked and free slots today, counted like the day schedule"
    trace: [REQ-7, REQ-8]
    acceptance_criteria:
      - {id: AC1, text: "Given doctor A's slots today start at 09:00, 09:30, 10:00, 10:30 and 11:00 local, the 09:00 slot has a Completed appointment, 09:30 a No-show, 10:30 a Scheduled one, 11:00 only a Cancelled one, 10:00 none, and now is 09:45 local, when staff open the dashboard, then doctor A's row shows booked 3 and free 2."}
      - {id: AC2, text: "Given doctor A's slots today start at 09:00 and 10:00 local, neither is booked, and now is 09:45 local, when staff open the dashboard, then doctor A's row shows booked 0 and free 1, because the passed 09:00 slot is neither booked nor free."}
      - {id: AC3, text: "Given any on-duty doctor at any time today, when staff compare the row with that doctor's day schedule for today, then the row's booked count equals the number of booked slots and its free count equals the number of free (\"Free: book\") slots in the day schedule."}
  - id: S-4
    title: "See when each doctor's next free slot is"
    trace: [REQ-9, REQ-12, REQ-19]
    acceptance_criteria:
      - {id: AC1, text: "Given doctor A's slots today start at 09:00, 09:30 and 10:00 local, none is booked, and now is exactly 09:30 local, when staff open the dashboard, then doctor A's next free slot shows 10:00, the same label the day schedule shows for that slot, because a slot starting at now has not started after now."}
      - {id: AC2, text: "Given doctor A's 09:00 slot is free, the 10:00 slot is booked and the 11:00 slot is free, and now is 08:00 local, when staff open the dashboard, then doctor A's next free slot shows 09:00."}
      - {id: AC3, text: "Given doctor A's slots after now are all booked, when staff open the dashboard, then doctor A's row shows \"Fully booked\" instead of a time."}
      - {id: AC4, text: "Given doctor A's last slot today starts at 16:30 local, none of doctor A's slots today is booked, and now is 17:00 local, when staff open the dashboard, then doctor A is still listed, with free 0 and \"Fully booked\"."}
  - id: S-5
    title: "Doctors with the most room come first"
    trace: [REQ-10, REQ-11]
    acceptance_criteria:
      - {id: AC1, text: "Given on-duty doctors A, B and C have 1, 4 and 2 free slots left today, when staff open the dashboard, then the rows are ordered B, C, A."}
      - {id: AC2, text: "Given on-duty doctors named \"Alpha\" and \"Beta\" both have 3 free slots left, and \"Beta\" was added to the system before \"Alpha\", when staff open the dashboard, then \"Alpha\" is listed before \"Beta\"."}
      - {id: AC3, text: "Given on-duty doctor A shows \"Fully booked\" and doctor B has 1 free slot left, when staff open the dashboard, then doctor B is listed before doctor A."}
  - id: S-6
    title: "Open a doctor's schedule for today from the list"
    trace: [REQ-13]
    acceptance_criteria:
      - {id: AC1, text: "Given doctor A is listed, when staff select doctor A's name, then doctor A's page opens with its day schedule on today's date."}
      - {id: AC2, text: "Given now is 04:00 local on Saturday 28 November 2026 (Asia/Kolkata = UTC+05:30, so 22:30 UTC on Friday 27 November) and doctor A is listed, when staff select doctor A's name, then the day schedule shows Saturday 28 November, not 27 November."}
  - id: S-7
    title: "The section does not change the rest of the dashboard"
    trace: [REQ-16, REQ-18]
    acceptance_criteria:
      - {id: AC1, text: "Given the section lists doctors with their counts while the Period is Today, when staff change the Period to Last 30 days, then the section shows the same rows, counts and next free slots."}
      - {id: AC2, text: "Given there are upcoming Scheduled appointments, when staff open the dashboard, then the tiles, their links and the Next appointments list show the same values as before this change."}
      - {id: AC3, text: "Given there are no upcoming Scheduled appointments, when staff open the dashboard, then the tiles and the Next appointments list show the same values as before this change."}
---
# Stories: HMS-23

## Stories

### S-1 See which doctors are on duty today
Traces REQ-1, REQ-2, REQ-3, REQ-14, REQ-15. On duty means an Active doctor with an `availability_block` for today's weekday in local time (C8). Today is the hospital day in `Asia/Kolkata` (C3).
- AC1: Given Active doctor A has an availability block for today's weekday and Active doctor B has none for today, when staff open the dashboard, then below the tiles a section titled "On duty today" lists doctor A and not doctor B.
- AC2: Given now is 04:00 local on Saturday 28 November 2026 (22:30 UTC on Friday 27 November), doctor A is available on Saturdays only and doctor B on Fridays only, when staff open the dashboard, then doctor A is listed and doctor B is not.
- AC3: Given Inactive doctor C has availability today and more free slots than any listed doctor, when staff open the dashboard, then doctor C is not listed.
- AC4: Given no Active doctor has availability today and only Inactive doctor C does, when staff open the dashboard, then the section shows "No doctors on duty today".

### S-2 See who each on-duty doctor is, without contact details
Traces REQ-4, REQ-5, REQ-6, REQ-17. A doctor row stores a `phone` and an `email` (C4, C5); neither may appear (DSH-9). The Inactive marker matches the doctor page (C9).
- AC1: Given doctor A in Active department X is on duty, then the row shows doctor A's name and "X".
- AC2: Given department X is Inactive, then the row shows "X (Inactive)".
- AC3: Given doctor A has a phone and an email stored, then the row shows neither.

### S-3 See booked and free slots today, counted like the day schedule
Traces REQ-7, REQ-8. Slots held by Scheduled, Completed or No-show appointments are booked. A slot with only a Cancelled appointment is not booked (C1). An unbooked slot that has started is PAST, not free (C2).
- AC1: Given slots at 09:00, 09:30, 10:00, 10:30, 11:00, with 09:00 Completed, 09:30 No-show, 10:30 Scheduled, 11:00 Cancelled only, 10:00 empty, and now 09:45, then the row shows booked 3 and free 2.
- AC2: Given unbooked slots at 09:00 and 10:00 and now 09:45, then the row shows booked 0 and free 1.
- AC3: Given any on-duty doctor, then the booked and free counts equal the booked and "Free: book" slots of that doctor's day schedule for today.

### S-4 See when each doctor's next free slot is
Traces REQ-9, REQ-12, REQ-19. The time is shown as the slot `label` the day schedule shows (C7), the default stated in the requirements review.
- AC1: Given unbooked slots at 09:00, 09:30, 10:00 and now exactly 09:30, then the next free slot shows 10:00.
- AC2: Given 09:00 free, 10:00 booked, 11:00 free and now 08:00, then the next free slot shows 09:00.
- AC3: Given every slot after now is booked, then the row shows "Fully booked" instead of a time.
- AC4: Given the last slot today starts at 16:30, nothing is booked and now is 17:00, then doctor A stays listed with free 0 and "Fully booked".

### S-5 Doctors with the most room come first
Traces REQ-10, REQ-11.
- AC1: Given 1, 4 and 2 free slots for A, B and C, then the order is B, C, A.
- AC2: Given "Alpha" and "Beta" both have 3 free slots and "Beta" was added first, then "Alpha" comes before "Beta".
- AC3: Given A is "Fully booked" and B has 1 free slot, then B comes before A.

### S-6 Open a doctor's schedule for today from the list
Traces REQ-13. The doctor page already opens a given day through a `?date=` link (C6).
- AC1: Given doctor A is listed, when staff select the name, then doctor A's page opens with the day schedule on today's date.
- AC2: Given now is 04:00 local on Saturday 28 November 2026 (22:30 UTC on 27 November), when staff select the name, then the day schedule shows 28 November.

### S-7 The section does not change the rest of the dashboard
Traces REQ-16, REQ-18.
- AC1: Given the Period is Today, when staff change it to Last 30 days, then the section's rows, counts and next free slots stay the same.
- AC2: Given upcoming Scheduled appointments exist, then the tiles, their links and the Next appointments list are the same as before.
- AC3: Given no upcoming Scheduled appointments exist, then the tiles and the Next appointments list are the same as before.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3, REQ-14 and REQ-15.
- S-2: built from known REQ-4, REQ-5, REQ-6 and REQ-17; REQ-6 was decided in the IDE (runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-02).
- S-3: built from known REQ-7 and REQ-8.
- S-4: built from known REQ-9, REQ-12 and REQ-19; REQ-12 and REQ-19 were decided in the IDE (runs/run_01M8EC1C803452B4P6VTAB10M7/ide-answers.md#a-01).
- S-5: built from known REQ-10 and REQ-11.
- S-6: built from known REQ-13.
- S-7: built from known REQ-16 and REQ-18.

## Assumed
None.

## Unknown
None.

## Open questions
None.

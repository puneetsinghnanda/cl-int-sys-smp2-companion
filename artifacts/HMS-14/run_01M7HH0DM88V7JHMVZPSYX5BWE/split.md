---
schema: stories.v1
run_id: run_01M7HH0DM88V7JHMVZPSYX5BWE
step: split
attempt: 2
skill: story-split@1.2.0
inputs:
  - path: artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md
    sha256: 7f7f379c0fac62230826a6d88863ac42c7c9dd2da60c5db6afedef2d3e5ebcdb
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "Cancelled and No-show appointment pages show a `Book again` button."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-1", "artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-2"]
  - id: C2
    text: "The booking date opened by Book again is today in `Asia/Kolkata`."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-7"]
  - id: C3
    text: "The reason for visit keeps its `300-character` limit."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-16"]
  - id: C4
    text: "An Inactive doctor or patient is kept for history but `not offered for new use`."
    cites: ["context/glossary.md#inactive"]
  - id: C5
    text: "When both doctor and patient are Inactive the disabled button shows both reasons, as answered `Show both (proposed default)`."
    cites: ["runs/run_01M7HH0DM88V7JHMVZPSYX5BWE/ide-answers.md#a-01"]
  - id: C6
    text: "Booking again keeps the free-slot rule `APT-2` unchanged."
    cites: ["artifacts/HMS-14/run_01M7HH0DM88V7JHMVZPSYX5BWE/req.md#item:REQ-12"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18, REQ-19]
known: [S-1, S-2, S-3, S-4, S-5, S-6]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See "Book again" only on Cancelled and No-show appointments
    trace: [REQ-1, REQ-2, REQ-3]
    acceptance_criteria:
      - id: AC1
        text: 'Given a Cancelled appointment with an Active doctor and patient, when staff open its appointment page, then a "Book again" button is shown.'
      - id: AC2
        text: 'Given a No-show appointment with an Active doctor and patient, when staff open its appointment page, then a "Book again" button is shown.'
      - id: AC3
        text: 'Given a Scheduled appointment, when staff open its appointment page, then no "Book again" button is shown.'
      - id: AC4
        text: 'Given a Completed appointment, when staff open its appointment page, then no "Book again" button is shown.'
  - id: S-2
    title: Book again opens the booking page with doctor, patient and today chosen
    trace: [REQ-4, REQ-5, REQ-7, REQ-8]
    acceptance_criteria:
      - id: AC1
        text: 'Given a Cancelled appointment of doctor D and patient P, when staff select "Book again", then the book appointment page opens with doctor D and patient P chosen.'
      - id: AC2
        text: 'Given a No-show appointment, when staff select "Book again" at 00:30 Asia/Kolkata on 17 November (Asia/Kolkata = UTC+05:30, so this is 19:00 UTC on 16 November, the local side of midnight), then the chosen date is 17 November, not 16 November.'
      - id: AC3
        text: 'Given a Cancelled appointment whose original start was on a later date than today, when staff select "Book again", then the chosen date is today in Asia/Kolkata, not the original appointment''s date.'
      - id: AC4
        text: 'Given any Cancelled or No-show appointment, when staff select "Book again", then the book appointment page opens with no slot chosen.'
  - id: S-3
    title: Reason for visit is carried over and stays editable
    trace: [REQ-6, REQ-15, REQ-16, REQ-18, REQ-19]
    acceptance_criteria:
      - id: AC1
        text: 'Given a No-show appointment with reason for visit "Follow-up", when staff select "Book again", then the reason for visit field holds "Follow-up".'
      - id: AC2
        text: 'Given a Cancelled appointment with reason for visit "Follow-up" and cancellation reason "Doctor on leave", when staff select "Book again", then the reason for visit field holds "Follow-up" and does not contain "Doctor on leave".'
      - id: AC3
        text: 'Given a Cancelled appointment with no reason for visit, when staff select "Book again", then the reason for visit field is empty.'
      - id: AC4
        text: 'Given the page opened by "Book again" with reason "Follow-up", when staff change it to "Review results" and book a free slot, then the new appointment''s reason for visit is "Review results".'
      - id: AC5
        text: 'Given the page opened by "Book again", when staff enter a reason for visit with a 301st character (one past the 300-character limit), then the page rejects it exactly as it does today.'
  - id: S-4
    title: Book again is disabled with a reason when doctor or patient is Inactive
    trace: [REQ-9, REQ-10, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: 'Given a Cancelled appointment whose doctor is now Inactive and patient is Active, when staff open its appointment page, then "Book again" is disabled and says "Doctor is inactive".'
      - id: AC2
        text: 'Given a No-show appointment whose patient is now Inactive and doctor is Active, when staff open its appointment page, then "Book again" is disabled and says "Patient is inactive".'
      - id: AC3
        text: 'Given a Cancelled appointment whose doctor and patient are both now Inactive, when staff open its appointment page, then "Book again" is disabled and shows both "Doctor is inactive" and "Patient is inactive".'
  - id: S-5
    title: Booking again follows existing rules and leaves the original untouched
    trace: [REQ-12, REQ-13, REQ-14]
    acceptance_criteria:
      - id: AC1
        text: 'Given the page opened by "Book again" and a slot that another booking takes after the page loaded, when staff book that slot, then the booking is refused with "slot no longer available" (APT-4).'
      - id: AC2
        text: 'Given the page opened by "Book again" and a patient who already has a Scheduled appointment overlapping the chosen slot, when staff book it, then the booking is refused as today (APT-3).'
      - id: AC3
        text: 'Given the page opened by "Book again" from appointment A, when staff book a free slot, then a new appointment is created with its own ID, different from A''s ID.'
      - id: AC4
        text: 'Given a Cancelled appointment A with its history, when staff book again from it successfully, then A keeps its status, cancellation reason and history entries exactly as before (APT-8, APT-11).'
      - id: AC5
        text: 'Given the page opened by "Book again" for doctor D today, where one of D''s slots today already holds a Scheduled appointment, when staff view the slots for today, then that booked slot is not offered as free and only free slots can be booked, as today (APT-2).'
  - id: S-6
    title: The appointment and book appointment pages work as today
    trace: [REQ-17]
    acceptance_criteria:
      - id: AC1
        text: 'Given a Scheduled appointment, when staff open its appointment page, then reschedule, cancel and mark Completed or No-show are offered as today.'
      - id: AC2
        text: 'Given the book appointment page opened from a patient page link (doctor, patient, date and slot from the address, no reason), when staff book, then it behaves as today and the reason field starts empty.'
      - id: AC3
        text: 'Given the book appointment page opened with nothing in its address, when staff choose doctor, patient, date and slot and book, then it behaves as today.'
---
# Stories: HMS-14

## Stories

### S-1 See "Book again" only on Cancelled and No-show appointments
Traces REQ-1, REQ-2, REQ-3.
- AC1: Given a Cancelled appointment with an Active doctor and patient, when staff open its appointment page, then a "Book again" button is shown.
- AC2: Given a No-show appointment with an Active doctor and patient, when staff open its appointment page, then a "Book again" button is shown.
- AC3: Given a Scheduled appointment, when staff open its appointment page, then no "Book again" button is shown.
- AC4: Given a Completed appointment, when staff open its appointment page, then no "Book again" button is shown.

### S-2 Book again opens the booking page with doctor, patient and today chosen
Traces REQ-4, REQ-5, REQ-7, REQ-8.
- AC1: Given a Cancelled appointment of doctor D and patient P, when staff select "Book again", then the book appointment page opens with doctor D and patient P chosen.
- AC2: Given a No-show appointment, when staff select "Book again" at 00:30 Asia/Kolkata on 17 November (Asia/Kolkata = UTC+05:30, so this is 19:00 UTC on 16 November, the local side of midnight), then the chosen date is 17 November, not 16 November.
- AC3: Given a Cancelled appointment whose original start was on a later date than today, when staff select "Book again", then the chosen date is today in Asia/Kolkata, not the original appointment's date.
- AC4: Given any Cancelled or No-show appointment, when staff select "Book again", then the book appointment page opens with no slot chosen.

### S-3 Reason for visit is carried over and stays editable
Traces REQ-6, REQ-15, REQ-16, REQ-18, REQ-19.
- AC1: Given a No-show appointment with reason for visit "Follow-up", when staff select "Book again", then the reason for visit field holds "Follow-up".
- AC2: Given a Cancelled appointment with reason for visit "Follow-up" and cancellation reason "Doctor on leave", when staff select "Book again", then the reason for visit field holds "Follow-up" and does not contain "Doctor on leave".
- AC3: Given a Cancelled appointment with no reason for visit, when staff select "Book again", then the reason for visit field is empty.
- AC4: Given the page opened by "Book again" with reason "Follow-up", when staff change it to "Review results" and book a free slot, then the new appointment's reason for visit is "Review results".
- AC5: Given the page opened by "Book again", when staff enter a reason for visit with a 301st character (one past the 300-character limit), then the page rejects it exactly as it does today.

### S-4 Book again is disabled with a reason when doctor or patient is Inactive
Traces REQ-9, REQ-10, REQ-11.
- AC1: Given a Cancelled appointment whose doctor is now Inactive and patient is Active, when staff open its appointment page, then "Book again" is disabled and says "Doctor is inactive".
- AC2: Given a No-show appointment whose patient is now Inactive and doctor is Active, when staff open its appointment page, then "Book again" is disabled and says "Patient is inactive".
- AC3: Given a Cancelled appointment whose doctor and patient are both now Inactive, when staff open its appointment page, then "Book again" is disabled and shows both "Doctor is inactive" and "Patient is inactive".
- The enabled case with an Active doctor and patient is covered by S-1 AC1.

### S-5 Booking again follows existing rules and leaves the original untouched
Traces REQ-12, REQ-13, REQ-14.
- AC1: Given the page opened by "Book again" and a slot that another booking takes after the page loaded, when staff book that slot, then the booking is refused with "slot no longer available" (APT-4).
- AC2: Given the page opened by "Book again" and a patient who already has a Scheduled appointment overlapping the chosen slot, when staff book it, then the booking is refused as today (APT-3).
- AC3: Given the page opened by "Book again" from appointment A, when staff book a free slot, then a new appointment is created with its own ID, different from A's ID.
- AC4: Given a Cancelled appointment A with its history, when staff book again from it successfully, then A keeps its status, cancellation reason and history entries exactly as before (APT-8, APT-11).
- AC5: Given the page opened by "Book again" for doctor D today, where one of D's slots today already holds a Scheduled appointment, when staff view the slots for today, then that booked slot is not offered as free and only free slots can be booked, as today (APT-2).

### S-6 The appointment and book appointment pages work as today
Traces REQ-17.
- AC1: Given a Scheduled appointment, when staff open its appointment page, then reschedule, cancel and mark Completed or No-show are offered as today.
- AC2: Given the book appointment page opened from a patient page link (doctor, patient, date and slot from the address, no reason), when staff book, then it behaves as today and the reason field starts empty.
- AC3: Given the book appointment page opened with nothing in its address, when staff choose doctor, patient, date and slot and book, then it behaves as today.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3 (ticket acceptance criterion 1).
- S-2: built from known REQ-4, REQ-5, REQ-7, REQ-8 (ticket acceptance criterion 2).
- S-3: built from known REQ-6, REQ-15, REQ-16, REQ-18, REQ-19 (ticket criteria 2 and 5, answers A-02 and A-03).
- S-4: built from known REQ-9, REQ-10, REQ-11 (ticket criterion 3, answer A-01).
- S-5: built from known REQ-12, REQ-13, REQ-14 (ticket criterion 4); AC5 covers the APT-2 free-slot rule.
- S-6: built from known REQ-17 (ticket criterion 6).

## Assumed
None

## Unknown
None

## Open questions
None

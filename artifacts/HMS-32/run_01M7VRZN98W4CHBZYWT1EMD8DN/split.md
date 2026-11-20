---
schema: stories.v1
run_id: run_01M7VRZN98W4CHBZYWT1EMD8DN
step: split
attempt: 2
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/req.md
    sha256: 090d7caec2892d7d704168997202146560d5e2a0ecf4bd50423664f4dcd23d2b
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
resolves:
  - id: F-1
    how: "S-3 adds AC4 for a slot that has already started and AC5 for a day outside availability, and C6 states the full rule."
claims:
  - id: C1
    text: "150000 paise shows as `₹1,500.00`."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/req.md#item:REQ-2"]
  - id: C2
    text: "A fee of 0 paise shows as `₹0.00`."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/req.md#item:REQ-5"]
  - id: C3
    text: "Paise are one hundredth of a `rupee`, so a rupee amount is the paise amount divided by 100."
    cites: ["context/glossary.md#paise"]
  - id: C4
    text: "A doctor filled in from the `URL` counts as chosen."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/req.md#item:REQ-10"]
  - id: C5
    text: "When the fee `cannot be loaded`, no fee is shown and staff can still book."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/req.md#item:REQ-9"]
  - id: C6
    text: "A slot is free only inside the doctor's `availability`, in the future and not overlapping a Scheduled appointment."
    cites: ["artifacts/HMS-32/run_01M7VRZN98W4CHBZYWT1EMD8DN/req.md#claim:C5"]
  - id: C7
    text: "Medical history includes a patient's `allergies`."
    cites: ["context/glossary.md#medical-history"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See the chosen doctor's consultation fee while booking
    trace: [REQ-1, REQ-2, REQ-4, REQ-5, REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor whose fee is 150000 paise, when staff choose that doctor in the Doctor field of the book appointment page, then the page shows \"Consultation fee\" with ₹1,500.00."
      - id: AC2
        text: "Given the book appointment page opened with no doctor chosen, when the page has loaded, then no fee and no \"Consultation fee\" label are shown."
      - id: AC3
        text: "Given a doctor whose fee is 0 paise, when staff choose that doctor, then the page shows \"Consultation fee\" with ₹0.00."
      - id: AC4
        text: "Given the page opened with a doctor whose fee is 150000 paise already filled in from the URL, when the page has loaded and staff have picked nothing, then the page shows \"Consultation fee\" with ₹1,500.00."
      - id: AC5
        text: "Given a doctor whose fee is 10000000 paise (divided by 100 that is one lakh rupees, which Indian digit grouping writes as ₹1,00,000.00, not ₹100,000.00), when staff choose that doctor, then the page shows ₹1,00,000.00."
  - id: S-2
    title: The fee follows the doctor the staff have chosen
    trace: [REQ-3, REQ-9]
    acceptance_criteria:
      - id: AC1
        text: "Given doctor A with fee 150000 paise is chosen and ₹1,500.00 is shown, when staff choose doctor B with fee 50000 paise (divided by 100 that is ₹500.00), then the page shows ₹500.00, no longer shows ₹1,500.00, and the page did not reload."
      - id: AC2
        text: "Given the fee of a doctor cannot be loaded, when staff choose that doctor, then no fee is shown and staff can still choose a patient and a free slot and book."
      - id: AC3
        text: "Given doctor A with fee 150000 paise is chosen and ₹1,500.00 is shown, when staff choose doctor B whose fee cannot be loaded, then no fee is shown, so ₹1,500.00 of doctor A does not stay on the page."
  - id: S-3
    title: The fee is display-only and booking works as before
    trace: [REQ-6, REQ-7, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor is chosen and the fee is shown, when staff try to edit the fee, then the fee is plain text with no input, so it cannot be changed on this page."
      - id: AC2
        text: "Given a doctor, a patient, a free slot and a reason are chosen and the fee is shown, when staff press Book, then the booking request is the same as one sent before this change and carries no fee."
      - id: AC3
        text: "Given a chosen doctor's day where one slot is already taken by a Scheduled appointment, when staff look at that day's slots with the fee shown, then the taken slot is not offered as free, as before."
      - id: AC4
        text: "Given today is chosen and one of the chosen doctor's slots today has already started, when staff look at today's slots with the fee shown, then that started slot is not offered as free, as before."
      - id: AC5
        text: "Given a date on a weekday when the chosen doctor has no availability, when staff look at that day's slots with the fee shown, then no slot is offered as free, as before."
  - id: S-4
    title: The rest of the page stays as it was and shows no private data
    trace: [REQ-8, REQ-12, REQ-13, REQ-14]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor with free slots on the chosen date is chosen and the fee is shown, when staff look at the slot list, then it lists the same free slots as before this change."
      - id: AC2
        text: "Given a doctor with no free slots on the chosen date is chosen and the fee is shown, when staff look at the slot list, then it shows the same result as before this change."
      - id: AC3
        text: "Given a doctor is chosen and the fee is shown, when staff look at the rest of the page, then every other field, label and button is present and works as before this change."
      - id: AC4
        text: "Given a patient whose allergies are recorded is chosen together with a doctor whose fee is shown, when staff look at the page, then none of the patient's medical history appears."
      - id: AC5
        text: "Given a doctor and a patient who both have a phone number and an email recorded are chosen and the fee is shown, when staff look at the page, then no phone number or email of either appears."
---
# Stories: HMS-32

Resolves F-1: S-3 adds AC4 for a slot that has already started and AC5 for a day outside availability, and C6 states the full rule.

## Stories

### S-1 See the chosen doctor's consultation fee while booking
Traces REQ-1, REQ-2, REQ-4, REQ-5, REQ-10.
- AC1: Given a doctor whose fee is 150000 paise, when staff choose that doctor in the Doctor field of the book appointment page, then the page shows "Consultation fee" with ₹1,500.00 (C1).
- AC2: Given the book appointment page opened with no doctor chosen, when the page has loaded, then no fee and no "Consultation fee" label are shown.
- AC3: Given a doctor whose fee is 0 paise, when staff choose that doctor, then the page shows "Consultation fee" with ₹0.00 (C2).
- AC4: Given the page opened with a doctor whose fee is 150000 paise already filled in from the URL, when the page has loaded and staff have picked nothing, then the page shows "Consultation fee" with ₹1,500.00 (C4).
- AC5: Given a doctor whose fee is 10000000 paise (divided by 100 that is one lakh rupees, which Indian digit grouping writes as ₹1,00,000.00, not ₹100,000.00), when staff choose that doctor, then the page shows ₹1,00,000.00 (C3).

### S-2 The fee follows the doctor the staff have chosen
Traces REQ-3, REQ-9.
- AC1: Given doctor A with fee 150000 paise is chosen and ₹1,500.00 is shown, when staff choose doctor B with fee 50000 paise (divided by 100 that is ₹500.00), then the page shows ₹500.00, no longer shows ₹1,500.00, and the page did not reload.
- AC2: Given the fee of a doctor cannot be loaded, when staff choose that doctor, then no fee is shown and staff can still choose a patient and a free slot and book (C5).
- AC3: Given doctor A with fee 150000 paise is chosen and ₹1,500.00 is shown, when staff choose doctor B whose fee cannot be loaded, then no fee is shown, so ₹1,500.00 of doctor A does not stay on the page.

### S-3 The fee is display-only and booking works as before
Traces REQ-6, REQ-7, REQ-11.
- AC1: Given a doctor is chosen and the fee is shown, when staff try to edit the fee, then the fee is plain text with no input, so it cannot be changed on this page.
- AC2: Given a doctor, a patient, a free slot and a reason are chosen and the fee is shown, when staff press Book, then the booking request is the same as one sent before this change and carries no fee.
- AC3: Given a chosen doctor's day where one slot is already taken by a Scheduled appointment, when staff look at that day's slots with the fee shown, then the taken slot is not offered as free, as before (C6).
- AC4: Given today is chosen and one of the chosen doctor's slots today has already started, when staff look at today's slots with the fee shown, then that started slot is not offered as free, as before (C6).
- AC5: Given a date on a weekday when the chosen doctor has no availability, when staff look at that day's slots with the fee shown, then no slot is offered as free, as before (C6).

### S-4 The rest of the page stays as it was and shows no private data
Traces REQ-8, REQ-12, REQ-13, REQ-14.
- AC1: Given a doctor with free slots on the chosen date is chosen and the fee is shown, when staff look at the slot list, then it lists the same free slots as before this change.
- AC2: Given a doctor with no free slots on the chosen date is chosen and the fee is shown, when staff look at the slot list, then it shows the same result as before this change.
- AC3: Given a doctor is chosen and the fee is shown, when staff look at the rest of the page, then every other field, label and button is present and works as before this change.
- AC4: Given a patient whose allergies are recorded is chosen together with a doctor whose fee is shown, when staff look at the page, then none of the patient's medical history appears (C7).
- AC5: Given a doctor and a patient who both have a phone number and an email recorded are chosen and the fee is shown, when staff look at the page, then no phone number or email of either appears.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-4, REQ-5 and REQ-10 (REQ-10 was decided in the req step, runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-01).
- S-2: built from known REQ-3 and REQ-9 (REQ-9 was confirmed in the req step, runs/run_01M7VRZN98W4CHBZYWT1EMD8DN/ide-answers.md#a-02).
- S-3: built from known REQ-6, REQ-7 and REQ-11.
- S-4: built from known REQ-8, REQ-12, REQ-13 and REQ-14.

## Assumed
None.

## Unknown
None. Every requirement of the intent is known and traced by a story.

## Open questions
None.

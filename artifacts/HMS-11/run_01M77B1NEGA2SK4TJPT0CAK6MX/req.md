---
schema: intent.v1
run_id: run_01M77B1NEGA2SK4TJPT0CAK6MX
step: req
attempt: 1
skill: requirement-review@1.2.0
inputs:
  - path: artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md
    sha256: 5829fb8121047fc5b1238a74036e31bd9b3c48cbfdae449365b50f275f8f61ea
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
    text: "A slot is free only if it is `in the future`, inside availability and not overlapping a Scheduled appointment."
    cites: ["context/intent-baseline.md#booking-rules"]
  - id: C5
    text: "System lists answer `20 per page`."
    cites: ["context/conventions.md#lists"]
  - id: C6
    text: "Changing the doctor on the book appointment page clears only the chosen slot: `setStart(undefined)`."
    cites: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L82"]
  - id: C7
    text: "The book appointment page is the web route `/appointments/new`."
    cites: ["context/generated/api-inventory.json#ui:/appointments/new"]
  - id: C8
    text: "The day schedule of one doctor and date is served by `api/src/routes/doctors.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: C9
    text: "Specializations and departments are staff-maintained reference lists: `specialization`."
    cites: ["context/data-model.md#reference-lists"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-15, REQ-16, REQ-17]
assumed: []
unknown: [U-1]
requirements:
  - id: REQ-1
    title: Earliest free slot section
    type: functional
    status: known
    statement: "The book appointment page has a section `Earliest free slot`."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L22"]
  - id: REQ-2
    title: Specialization picker lists Active specializations only
    type: functional
    status: known
    statement: "The section has a specialization picker that lists only `Active specializations`; specialization means the staff-maintained reference list."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L22-L23", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-04"]
  - id: REQ-3
    title: List Active doctors of the specialization
    type: functional
    status: known
    statement: "After a specialization is chosen, the section lists each `Active doctor` with that specialization."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L24"]
  - id: REQ-4
    title: Earliest free slot within today to today+6
    type: functional
    status: known
    statement: "For each listed doctor the section shows their earliest free slot from now until the end of the 6th day after today, in the hospital time zone (`Today to today+6`)."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L25", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-01", "context/intent-baseline.md#constraints"]
  - id: REQ-5
    title: Row content
    type: functional
    status: known
    statement: "Each row shows `doctor name, department, date and start time`; department means the staff-maintained reference list."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L26", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-04"]
  - id: REQ-6
    title: Ordering
    type: functional
    status: known
    statement: "The list is ordered by the earliest free slot, `earliest first; ties are ordered by doctor name`."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L27"]
  - id: REQ-7
    title: Free means free on the day schedule
    type: functional
    status: known
    statement: "A slot counts as free exactly when the `day schedule shows it as free` for that doctor and date (APT-2, APT-10)."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L28-L29", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-04"]
  - id: REQ-8
    title: Started slots never shown
    type: functional
    status: known
    statement: "Slots that have `already started` are never shown."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L29"]
  - id: REQ-9
    title: Doctor with no free slot
    type: functional
    status: known
    statement: "A doctor with no free slot in the window is listed last with `No free slot in the next 7 days`."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L30"]
  - id: REQ-10
    title: Specialization with no Active doctors
    type: functional
    status: known
    statement: "A specialization with no Active doctors shows `No active doctors`."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L31"]
  - id: REQ-11
    title: Choosing a slot fills the form
    type: functional
    status: known
    statement: "Choosing a listed slot `fills the booking form` with that doctor, date and slot."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L32"]
  - id: REQ-12
    title: Staff still choose patient and confirm
    type: functional
    status: known
    statement: "After choosing a listed slot, staff still `choose the patient and confirm` the booking."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L33"]
  - id: REQ-13
    title: Existing booking rules apply
    type: constraint
    status: known
    statement: "Booking from a listed slot follows the existing booking rules, including \"slot no longer available\" when someone else took it first (`APT-4`)."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L34"]
  - id: REQ-14
    title: Patient and reason when a listed slot is chosen
    type: functional
    status: unknown
    statement: "Open (U-1, Q-03): whether choosing a listed slot keeps any patient and reason for visit already entered, or clears them; staff still `choose the patient and confirm`."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L33", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-03"]
  - id: REQ-15
    title: Rest of the page unchanged
    type: constraint
    status: known
    statement: "The `rest of the book appointment page` is unchanged."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L35"]
  - id: REQ-16
    title: Manual doctor and date booking unchanged
    type: constraint
    status: known
    statement: "`booking a chosen doctor and date as today` is unchanged."
    cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L35"]
  - id: REQ-17
    title: No paging
    type: non-functional
    status: known
    statement: "The earliest-free-slot list shows every Active doctor of the specialization on one list with no paging (`All doctors, no paging`)."
    cites: ["runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-02", "artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L24"]
unknowns:
  - id: U-1
    text: "Whether choosing a listed slot keeps a patient and reason already entered on the form (REQ-14)."
    question: Q-03
questions:
  - id: Q-03
    blocking: false
    ask: product owner
    text: "REQ-14: when staff choose a listed earliest slot, should the patient and reason for visit already entered stay, or be cleared?"
    why: "It decides whether picking a slot can lose data staff already entered; the engineer asked was not sure (A-03)."
    evidence: ["sample-system:web/src/pages/BookAppointmentPage.tsx#L82", "artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L33", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-03"]
    options: ["Keep the patient and reason (proposed default)", "Clear the patient"]
    default: "Keep the patient and reason; replace only the doctor, date and slot, as the doctor picker does today."
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Today to today+6"
    requirements: [REQ-4]
    mismatch:
      text: "The ticket says from now until the end of the 7th day after today (today+7, 8 calendar days); the user decided today to today+6, which matches the 7-day message."
      cites: ["artifacts/HMS-11/run_01M77B1NEGA2SK4TJPT0CAK6MX/ticket.md#L25"]
  - id: A-02
    kind: question
    about: Q-02
    answer: "All doctors, no paging (proposed default)"
    requirements: [REQ-17]
    mismatch:
      text: "The list convention pages at 20 per page; the user decided the earliest-free-slot list shows all doctors with no paging."
      cites: ["context/conventions.md#lists"]
  - id: A-03
    kind: assumption
    about: REQ-14
    answer: "not sure"
    requirements: []
  - id: A-04
    kind: question
    about: Q-04
    answer: "Use existing meanings (proposed default)"
    requirements: [REQ-2, REQ-5, REQ-7]
glossary:
  - {term: Front-desk staff, found: true}
  - {term: Slot, found: true}
  - {term: Inactive, found: true}
  - {term: Specialization, found: false}
  - {term: Department, found: false}
  - {term: Day schedule, found: false}
---
# Requirements review: HMS-11

## Summary
HMS-11 adds an "Earliest free slot" section to the book appointment page (`/appointments/new`). Staff pick an Active specialization, see each Active doctor of it with their earliest free slot in the window, and pick a slot to fill the booking form. The user narrowed the window to today through today+6 (A-01), which differs from the ticket's "7th day after today". "Free" must reuse the APT-10 day-schedule rule so the two views never disagree. One question stays open: whether choosing a listed slot keeps a patient already entered (Q-03, non-blocking). Booking from the list must still hit the existing concurrency guard (APT-4).

## Requirements
- REQ-1 Earliest free slot section (known): the book appointment page has a section "Earliest free slot".
- REQ-2 Specialization picker (known): the picker lists only Active specializations.
- REQ-3 Active doctors listed (known): after a specialization is chosen, each Active doctor of it is listed.
- REQ-4 Window (known): each doctor's earliest free slot is searched from now until the end of today+6, hospital time zone.
- REQ-5 Row content (known): doctor name, department, date and start time.
- REQ-6 Ordering (known): by slot, earliest first; ties by doctor name.
- REQ-7 Free rule (known): a slot is free exactly when the day schedule shows it free (APT-2, APT-10).
- REQ-8 Started slots (known): slots that have already started are never shown.
- REQ-9 No free slot (known): such a doctor is listed last with "No free slot in the next 7 days".
- REQ-10 No doctors (known): a specialization with no Active doctors shows "No active doctors".
- REQ-11 Fill form (known): choosing a listed slot fills the form with that doctor, date and slot.
- REQ-12 Patient and confirm (known): staff still choose the patient and confirm.
- REQ-13 Booking rules (known): booking follows the existing rules, including "slot no longer available" (APT-4).
- REQ-14 Patient and reason on choosing a slot (unknown): keep or clear a patient and reason already entered; open as Q-03.
- REQ-15 Rest of page (known): the rest of the book appointment page is unchanged.
- REQ-16 Manual booking (known): booking a chosen doctor and date works as today.
- REQ-17 No paging (known): the list shows all Active doctors of the specialization, unpaged.

## Known
- REQ-1, REQ-3, REQ-6, REQ-8 to REQ-13, REQ-15, REQ-16: stated in the ticket acceptance criteria (L22–L35).
- REQ-2, REQ-5, REQ-7: stated in the ticket; the meanings of Specialization, Department and Day schedule were confirmed in the IDE (A-04).
- REQ-4: ticket L25, with the window decided in the IDE (A-01); the hospital zone is `Asia/Kolkata` (C1).
- REQ-17: decided in the IDE (A-02).

## Assumed
None.

## Unknown
- U-1 (REQ-14): does choosing a listed slot keep a patient and reason already entered? The user was not sure (A-03); the current doctor picker clears only the slot (C6). See Q-03.

## Open questions
- Q-03 (non-blocking, product owner): when staff choose a listed slot, should the patient and reason already entered stay or be cleared? Proposed default: keep them.

Answered in the IDE:
- A-01 (eng-3fd6): Q-01, which window applies? "Today to today+6". Decides REQ-4. Mismatch: ticket L25 says until the end of the 7th day after today (today+7).
- A-02 (eng-3fd6): Q-02, should the list page at 20? "All doctors, no paging (proposed default)". Decides REQ-17. Mismatch: conventions#lists pages at 20.
- A-03 (eng-3fd6): REQ-14, keep patient and reason? "not sure". Decides nothing; Q-03 stays open.
- A-04 (eng-3fd6): Q-04, use the existing meanings of Specialization, Department and Day schedule? "Use existing meanings (proposed default)". Decides REQ-2, REQ-5, REQ-7.

## Out of scope
- How the earliest slot is computed (new endpoint vs reusing `GET /api/doctors/:id/slots`) is a design decision, not a requirement.
- The date and time display format follows the page's existing slot labels; no new format is asked for.
- Adding glossary entries for Specialization, Department and Day schedule is left to the glossary owner.
- No instruction-like text and no credentials or personal data were found in the inputs.

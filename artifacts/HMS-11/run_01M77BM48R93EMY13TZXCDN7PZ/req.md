---
schema: intent.v1
run_id: run_01M77BM48R93EMY13TZXCDN7PZ
step: req
attempt: 2
skill: requirement-review@1.2.0
inputs:
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md
    sha256: ae2a8335d832f0c845dd6fd2f9749cc54adb31073facb239b3b98ba79e5aecb6
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
  - id: C3
    text: "The day schedule of one doctor and date is served from `api/src/routes/doctors.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots"]
  - id: C4
    text: "Specializations already have their own router in `api/src/routes/specializations.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/specializations"]
  - id: C5
    text: "Doctor codes such as `D-0001` are display codes derived from row ids, not stored."
    cites: ["context/data-model.md#codes"]
  - id: C6
    text: "Invalid input answers 400 with a `ValidationError` body; unknown ids answer 404."
    cites: ["context/conventions.md#errors"]
  - id: C7
    text: "`Medical history` appears only on the patient's own page, never in lists."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C8
    text: "System lists normally answer `20 per page`; this endpoint is a stated exception."
    cites: ["context/conventions.md#lists"]
  - id: C9
    text: "An Inactive `specialization` is kept for history but not offered for new use."
    cites: ["context/glossary.md#inactive"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Earliest-slots endpoint per specialization
    type: functional
    status: known
    statement: "A new endpoint `GET /api/specializations/:id/earliest-slots` returns one entry per Active doctor with that specialization; specialization means the staff-maintained reference list."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L21-L22", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-04"]
  - id: REQ-2
    title: Search window today to today+6
    type: functional
    status: known
    statement: "The window runs from now until the end of the 6th day after today in the hospital time zone (`today to today+6`)."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L23-L24", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-01", "context/intent-baseline.md#constraints"]
  - id: REQ-3
    title: Doctor fields of an entry
    type: functional
    status: known
    statement: "Each entry has the doctor's id, code, full name and `department name`; department means the staff-maintained reference list."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L25", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-04"]
  - id: REQ-4
    title: Slot fields of an entry
    type: functional
    status: known
    statement: "The earliest free slot is given as date (YYYY-MM-DD, hospital time zone), start (`UTC ISO instant`) and label (HH:MM)."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L25-L26"]
  - id: REQ-5
    title: Null slot when none is free
    type: functional
    status: known
    statement: "An entry has `slot: null` when the doctor has no free slot in the window."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L27"]
  - id: REQ-6
    title: Free means FREE on the day schedule
    type: functional
    status: known
    statement: "A slot counts as free exactly when the day schedule `shows it as FREE` for that doctor and date (APT-2, APT-10)."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L28-L29", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-04"]
  - id: REQ-7
    title: Started slots never returned
    type: functional
    status: known
    statement: "Slots that have `already started` are never returned."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L29"]
  - id: REQ-8
    title: Order of entries with a slot
    type: functional
    status: known
    statement: "Entries are ordered by slot start, `earliest first; ties by doctor name`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L30"]
  - id: REQ-9
    title: Entries without a slot come last
    type: functional
    status: known
    statement: "Entries with a null slot `come last, ordered by doctor name`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L30-L31"]
  - id: REQ-10
    title: Unpaged items response
    type: functional
    status: known
    statement: "The response is `{ items: [...] }` with every Active doctor of the specialization and `no paging`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L32", "runs/run_01M77B1NEGA2SK4TJPT0CAK6MX/ide-answers.md#a-02"]
  - id: REQ-11
    title: No Active doctors
    type: functional
    status: known
    statement: "A specialization with no Active doctors returns `{ items: [] }`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L33"]
  - id: REQ-12
    title: Unknown specialization
    type: functional
    status: known
    statement: "An unknown specialization id `returns 404`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L34"]
  - id: REQ-13
    title: Inactive specialization
    type: functional
    status: known
    statement: "An Inactive specialization returns 400 with the message `specialization is Inactive`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L34-L35"]
  - id: REQ-14
    title: No contact details
    type: constraint
    status: known
    statement: "Entries carry `no contact details`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L36"]
  - id: REQ-15
    title: No medical history
    type: constraint
    status: known
    statement: "Entries carry `no medical history`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L36"]
  - id: REQ-16
    title: Nothing else changes
    type: constraint
    status: known
    statement: "`No other endpoint, page or booking rule changes`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L37"]
  - id: REQ-17
    title: Name comparison and final tie-break
    type: functional
    status: known
    statement: "Wherever entries are ordered by doctor name, names compare ignoring case, with `doctor id as the final tie-break`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/ticket.md#L30-L31", "runs/run_01M77BM48R93EMY13TZXCDN7PZ/ide-answers.md#a-01"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: assumption
    about: REQ-17
    answer: "Yes, confirm"
    requirements: [REQ-17]
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
HMS-11 adds one read-only endpoint, `GET /api/specializations/:id/earliest-slots`. For each Active doctor of a specialization it returns the earliest free slot in the 7 hospital-local days from today to today+6. "Free" must be exactly the FREE state of the existing day schedule, so the two never disagree; reusing that rule rather than re-deriving it is the main risk. The response is unpaged, which departs from the 20-per-page list convention (C8), and it must not leak contact details or medical history. The booking page section is a later story.

## Requirements
- REQ-1 Earliest-slots endpoint (known): `GET /api/specializations/:id/earliest-slots` returns one entry per Active doctor of the specialization.
- REQ-2 Window (known): from now until the end of today+6, hospital time zone.
- REQ-3 Doctor fields (known): id, code, full name, department name.
- REQ-4 Slot fields (known): date (YYYY-MM-DD, hospital zone), start (UTC ISO instant), label (HH:MM).
- REQ-5 Null slot (known): `slot: null` when no free slot is in the window.
- REQ-6 Free rule (known): free exactly when the day schedule shows the slot as FREE.
- REQ-7 Started slots (known): never returned.
- REQ-8 Order (known): by slot start, earliest first; ties by doctor name.
- REQ-9 Null slots last (known): ordered by doctor name.
- REQ-10 Response (known): `{ items: [...] }`, every Active doctor, no paging.
- REQ-11 No Active doctors (known): `{ items: [] }`.
- REQ-12 Unknown id (known): 404.
- REQ-13 Inactive specialization (known): 400 "specialization is Inactive".
- REQ-14 No contact details (known).
- REQ-15 No medical history (known).
- REQ-16 Nothing else changes (known).
- REQ-17 Name comparison (known): wherever entries are ordered by doctor name, names compare ignoring case, with doctor id as the final tie-break.

## Known
- REQ-1 to REQ-16: stated in the ticket acceptance criteria (L21–L37).
- REQ-2 and REQ-10 also match decisions from the cancelled run of this ticket (A-01 window, A-02 no paging). That A-01 was asked against older ticket wording, which the current ticket now matches.
- REQ-1, REQ-3 and REQ-6 use the existing meanings of Specialization, Department and Day schedule, confirmed in the cancelled run (A-04).
- REQ-17: the assumption was confirmed in the IDE (A-01 of this run).

## Assumed
None.

## Unknown
None.

## Open questions
None.

Answered in the IDE:
- A-01 (this run): REQ-17, do names compare ignoring case with doctor id as the final tie-break? "Yes, confirm". Decides REQ-17. No mismatch; the ticket leaves this unstated.
- Reused from the cancelled run run_01M77B1NEGA2SK4TJPT0CAK6MX: A-01 (window today to today+6), A-02 (no paging), A-04 (existing meanings of Specialization, Department, Day schedule).

## Out of scope
- The booking page "Earliest free slot" section: the ticket defers it to a later story (L17–L18). Prior answer A-06 in the cancelled run chose "API + booking page section"; the ticket was rewritten afterwards to the API only.
- Which permission the route declares, and whether the rule is shared with the day schedule in code, are design decisions.
- Glossary entries for Specialization, Department and Day schedule are left to the glossary owner.
- No instruction-like text and no credentials or personal data were found in the inputs.

---
schema: intent.v1
run_id: run_01M5HF2TJRVWS776C5G07JE7NH
step: req
attempt: 1
skill: requirement-review@1.0.0
inputs:
  - path: artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md
    sha256: 62142aacdf72be00b65396974255ad0d284be0f7c2b9404c54b0523e52a50c82
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 9bfd556283382d3d3835bf3b12514acc4f6941dc24f21cba0c8eaa4cd658a36c
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
claims:
  - id: C1
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C2
    text: "Medical history never appears in `lists`."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C3
    text: "A `Scheduled` appointment is booked and still open."
    cites: ["context/glossary.md#scheduled"]
  - id: C4
    text: "Medical history includes `allergies` and other clinical fields of one patient."
    cites: ["context/glossary.md#medical-history"]
  - id: C5
    text: "An Inactive doctor or patient is kept for `history`."
    cites: ["context/glossary.md#inactive"]
  - id: C6
    text: "Completed, Cancelled and No-show appointments `never change`."
    cites: ["context/intent-baseline.md#closed-appointments"]
  - id: C7
    text: "A `No-show` is a past appointment the patient did not attend."
    cites: ["context/glossary.md#no-show"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: [REQ-13]
unknown: [U-1, U-2, U-3, U-4, U-5, U-6, U-7, U-8]
requirements:
  - id: REQ-1
    title: Next appointment column
    type: functional
    status: known
    statement: "The patient list has a column titled `Next appointment`."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L16"]
  - id: REQ-2
    title: Which appointment is shown
    type: functional
    status: known
    statement: "The column shows the patient's `earliest` Scheduled appointment whose start is after the current time."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L17", "artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L18"]
  - id: REQ-3
    title: Start date and time shown
    type: functional
    status: known
    statement: "The column shows the appointment's `start date and time`."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L17"]
  - id: REQ-4
    title: Hospital time zone
    type: constraint
    status: known
    statement: "The start date and time are shown in the hospital time zone, `Asia/Kolkata`."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L17", "context/intent-baseline.md#constraints"]
  - id: REQ-5
    title: Doctor's name shown
    type: functional
    status: known
    statement: "The column shows the `doctor's name` of that appointment."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L17"]
  - id: REQ-6
    title: Closed appointments excluded
    type: functional
    status: known
    statement: "Appointments with status Cancelled, Completed or `No-show` are never shown in the column."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L19"]
  - id: REQ-7
    title: No upcoming appointment
    type: functional
    status: known
    statement: "A patient with no upcoming Scheduled appointment shows `None` in the column."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L20"]
  - id: REQ-8
    title: Link to appointment
    type: functional
    status: known
    statement: "The shown value links to that appointment's `page`."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L21"]
  - id: REQ-9
    title: Page size unchanged
    type: constraint
    status: known
    statement: "The patient list stays at `20 per page`."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L22"]
  - id: REQ-10
    title: Search unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its current `search` behaviour."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L22"]
  - id: REQ-11
    title: Sort order unchanged
    type: constraint
    status: known
    statement: "The patient list keeps its current `sort order`."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L22"]
  - id: REQ-12
    title: No medical history in the list
    type: constraint
    status: known
    statement: "No `medical history` appears in the patient list."
    cites: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L23", "context/intent-baseline.md#data-protection"]
  - id: REQ-13
    title: Inactive doctor still named
    type: functional
    status: assumed
    statement: "If the doctor of the next appointment is Inactive, the column still shows that doctor's name."
    basis: "Inactive records are kept for `history`, and the ticket asks for the doctor's name without exception."
    cites: ["context/glossary.md#inactive", "artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L17"]
unknowns:
  - id: U-1
    text: "The display format of the start date and time is not given."
    question: Q-01
  - id: U-2
    text: "The inputs do not confirm that an appointment page exists to link to."
    question: Q-02
  - id: U-3
    text: "The ticket cites PRD PAT-8, which is not among the inputs."
    question: Q-03
  - id: U-4
    text: "Whether an Inactive doctor's name is shown is not stated (REQ-13 assumes yes)."
    question: Q-04
  - id: U-5
    text: "Glossary has no entry for Cancelled."
    question: Q-05
  - id: U-6
    text: "Glossary has no entry for Completed."
    question: Q-05
  - id: U-7
    text: "Glossary has no entry for patient list."
    question: Q-05
  - id: U-8
    text: "Glossary has no entry for front-desk staff."
    question: Q-05
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "What date and time format should the Next appointment column use?"
    why: "Acceptance tests need an exact expected string."
    evidence: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L17"]
    options: ["Reuse the format already used elsewhere in the app", "Specify a new format"]
    default: "Reuse the app's existing date-time format, in Asia/Kolkata."
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is there an existing appointment page to link to, or must one be built?"
    why: "If no page exists, REQ-8 grows the scope."
    evidence: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L21"]
    options: ["Link to the existing appointment page", "Build an appointment page in this ticket"]
    default: "Link to the existing appointment page; impact analysis confirms it."
  - id: Q-03
    blocking: false
    ask: product owner
    text: "Does PRD PAT-8 add anything beyond keeping medical history out of lists?"
    why: "REQ-12 relies on the baseline's data-protection rule instead of PAT-8."
    evidence: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L23", "context/intent-baseline.md#data-protection"]
    options: ["No, the baseline rule is enough", "Yes, share PAT-8"]
    default: "The baseline data-protection rule is enough."
  - id: Q-04
    blocking: false
    ask: product owner
    text: "Should the column show the doctor's name when the doctor is Inactive?"
    why: "Upcoming appointments may remain with a doctor made Inactive."
    evidence: ["context/glossary.md#inactive"]
    options: ["Show the name", "Show the name marked Inactive"]
    default: "Show the name."
  - id: Q-05
    blocking: false
    ask: tech lead
    text: "Should Cancelled, Completed, patient list and front-desk staff be added to the glossary?"
    why: "These terms are used by the ticket but not defined."
    evidence: ["artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L19", "artifacts/HMS-1/run_01M5HF2TJRVWS776C5G07JE7NH/ticket.md#L12"]
    options: ["Add them", "Leave as plain English"]
    default: "Use the plain meanings; Cancelled and Completed are appointment statuses."
glossary:
  - {term: Scheduled, found: true}
  - {term: No-show, found: true}
  - {term: Medical history, found: true}
  - {term: Inactive, found: true}
  - {term: Cancelled, found: false}
  - {term: Completed, found: false}
  - {term: patient list, found: false}
  - {term: front-desk staff, found: false}
---
# Requirements review: HMS-1

## Summary
HMS-1 asks for a "Next appointment" column on the patient list. It shows the date, time and doctor of each patient's earliest upcoming Scheduled appointment and links to that appointment. The list's page size, search and sort must not change, and medical history must stay out of the list. The main risks are an unspecified date-time format, the assumption that an appointment page already exists, and the PAT-8 reference, which is not among the inputs. Nothing blocks design.

## Requirements
- REQ-1 Next appointment column — known — The patient list has a `Next appointment` column.
- REQ-2 Which appointment is shown — known — The column shows the earliest Scheduled appointment that starts after now.
- REQ-3 Start date and time shown — known — The column shows the start date and time.
- REQ-4 Hospital time zone — known — Times are shown in `Asia/Kolkata`.
- REQ-5 Doctor's name shown — known — The column shows the doctor's name.
- REQ-6 Closed appointments excluded — known — Cancelled, Completed and No-show appointments are never shown.
- REQ-7 No upcoming appointment — known — The column shows `None` when the patient has no upcoming Scheduled appointment.
- REQ-8 Link to appointment — known — The value links to the appointment's page.
- REQ-9 Page size unchanged — known — The list stays at 20 per page.
- REQ-10 Search unchanged — known — The list keeps its current search.
- REQ-11 Sort order unchanged — known — The list keeps its current sort order.
- REQ-12 No medical history in the list — known — No medical history appears in the list.
- REQ-13 Inactive doctor still named — assumed — An Inactive doctor's name is still shown.

## Known
- REQ-1: ticket AC1 (L16).
- REQ-2: ticket AC2 (L17–L18).
- REQ-3, REQ-5: ticket AC2 (L17).
- REQ-4: ticket AC2 (L17); the baseline constraints set the zone to Asia/Kolkata.
- REQ-6: ticket AC3 (L19).
- REQ-7: ticket AC4 (L20).
- REQ-8: ticket AC5 (L21).
- REQ-9, REQ-10, REQ-11: ticket AC6 (L22).
- REQ-12: ticket AC7 (L23); the baseline data-protection rule keeps medical history out of lists.

## Assumed
- REQ-13: an Inactive doctor's name is still shown. Basis: the glossary says Inactive records are kept for history, and the ticket asks for the doctor's name without exception.

## Unknown
- U-1: the date-time display format (Q-01).
- U-2: whether an appointment page exists (Q-02).
- U-3: the content of PRD PAT-8 (Q-03).
- U-4: how to show an Inactive doctor's name (Q-04).
- U-5: Cancelled is not in the glossary (Q-05).
- U-6: Completed is not in the glossary (Q-05).
- U-7: patient list is not in the glossary (Q-05).
- U-8: front-desk staff is not in the glossary (Q-05).

## Open questions
- Q-01 (non-blocking, product owner): which date-time format? Default: the app's existing format, in Asia/Kolkata.
- Q-02 (non-blocking, tech lead): does an appointment page exist? Default: link to the existing page.
- Q-03 (non-blocking, product owner): does PAT-8 add anything? Default: the baseline rule is enough.
- Q-04 (non-blocking, product owner): show an Inactive doctor's name? Default: yes.
- Q-05 (non-blocking, tech lead): add the missing terms to the glossary? Default: use their plain meanings.

## Out of scope
- The user-story line (L12–L13) gives the motivation and adds no separate behaviour.
- Appointments in the past and appointments of other patients: the ticket asks only for each patient's own next upcoming appointment.

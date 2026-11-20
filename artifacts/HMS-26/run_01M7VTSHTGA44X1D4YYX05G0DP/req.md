---
schema: intent.v1
run_id: run_01M7VTSHTGA44X1D4YYX05G0DP
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - {path: artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md, sha256: 27c3e604dfd1da7098ee505272c558db15af2e5dde1729d2d810704abb6e3db5}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The baseline fixes the hospital time zone as `Asia/Kolkata`.", cites: ["context/intent-baseline.md#constraints"]}
  - {id: C2, text: "Appointment start and end times are stored in `UTC`.", cites: ["context/data-model.md#appointments"]}
  - {id: C3, text: "An appointment status is one of SCHEDULED, COMPLETED, CANCELLED or `NO_SHOW`.", cites: ["context/data-model.md#appointments"]}
  - {id: C4, text: "`Medical history` appears only on the patient's own page, never in lists, the dashboard or logs.", cites: ["context/intent-baseline.md#data-protection"]}
  - {id: C5, text: "A patient row holds identity, `contact details` and the basic medical history.", cites: ["context/data-model.md#patients"]}
  - {id: C6, text: "A No-show is a past appointment the patient `did not attend`.", cites: ["context/glossary.md#no-show"]}
  - {id: C7, text: "A Completed appointment is one the patient `attended`, closed once its start time has passed.", cites: ["context/glossary.md#completed"]}
  - {id: C8, text: "In the MVP every user acts as `staff`.", cites: ["context/glossary.md#front-desk-staff"]}
  - {id: C9, text: "Dates and times on the summary use the hospital time zone read from `useSettings().timeZone`, not a hard-coded zone.", cites: ["runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-01"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17, REQ-18]
assumed: []
unknown: []
requirements:
  - {id: REQ-1, title: Summary placement, type: functional, status: known, statement: "The patient page shows a `Visits` summary below the patient's name and above the details.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L22"]}
  - {id: REQ-2, title: Summary contents, type: functional, status: known, statement: "The summary shows the `next appointment`, the last visit, and the number of Completed, Cancelled and No-show appointments.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L22-L23"]}
  - {id: REQ-3, title: Next appointment selection, type: functional, status: known, statement: "Next appointment is the `earliest Scheduled` appointment of the patient that starts after now in the hospital time zone.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L24"]}
  - {id: REQ-4, title: Next appointment display, type: functional, status: known, statement: "Next appointment is shown as its date and time and the `doctor's name`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L25"]}
  - {id: REQ-5, title: Next appointment link, type: functional, status: known, statement: "The next appointment's date and time `link to that appointment's page`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L25"]}
  - {id: REQ-6, title: No next appointment, type: functional, status: known, statement: "With no Scheduled appointment starting after now, next appointment shows `None booked`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L26"]}
  - {id: REQ-7, title: Last visit selection, type: functional, status: known, statement: "Last visit is the `latest Completed` appointment of the patient.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L27"]}
  - {id: REQ-8, title: Last visit display, type: functional, status: known, statement: "Last visit is `shown as its date and the doctor's name`, without a time.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L27"]}
  - {id: REQ-9, title: Display time zone, type: constraint, status: known, statement: "The next appointment's date and time and the last visit's date are shown in the hospital time zone read from Settings (`useSettings().timeZone`, default Asia/Kolkata), as the doctor and booking pages do; the zone is not hard-coded.", cites: ["runs/run_01M7VTSHTGA44X1D4YYX05G0DP/ide-answers.md#a-01", "context/intent-baseline.md#constraints"]}
  - {id: REQ-10, title: No last visit, type: functional, status: known, statement: "With no Completed appointment, last visit shows `No visits yet`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L28"]}
  - {id: REQ-11, title: Count scope, type: functional, status: known, statement: "The Completed, Cancelled and No-show counts cover all of the patient's appointments, `of any date`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L29"]}
  - {id: REQ-12, title: Zero counts, type: functional, status: known, statement: "`A count of 0 is shown as 0`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L29"]}
  - {id: REQ-13, title: No-show warning, type: functional, status: known, statement: "When the No-show count is `2 or more`, it is highlighted as a warning.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L30"]}
  - {id: REQ-14, title: Patient without appointments, type: functional, status: known, statement: "A patient with no appointments shows None booked, No visits yet and `three counts of 0`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L31"]}
  - {id: REQ-15, title: No medical history in summary, type: constraint, status: known, statement: "The summary shows `no medical history`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L32", "context/intent-baseline.md#data-protection"]}
  - {id: REQ-16, title: No contact details in summary, type: constraint, status: known, statement: "The summary shows `no contact details`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L32"]}
  - {id: REQ-17, title: Only on the patient's own page, type: constraint, status: known, statement: "The summary appears only on the `patient's own page` (PAT-8), on no other page.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L32-L33"]}
  - {id: REQ-18, title: Nothing else changes, type: constraint, status: known, statement: "The rest of the patient page, and `every other page, is unchanged`.", cites: ["artifacts/HMS-26/run_01M7VTSHTGA44X1D4YYX05G0DP/ticket.md#L34"]}
unknowns: []
questions: []
answers:
  - id: A-01
    kind: assumption
    about: REQ-9
    answer: "Yes, the hospital time zone, but read it from Settings (useSettings().timeZone, default Asia/Kolkata, PRD NFR-2), as the doctor and booking pages do. Do not hard-code Asia/Kolkata."
    requirements: [REQ-9]
    mismatch: {text: "The baseline states the hospital time zone as Asia/Kolkata; the user decided the zone is read from Settings (useSettings().timeZone) with Asia/Kolkata only as the default, and must not be hard-coded.", cites: ["context/intent-baseline.md#constraints"]}
glossary:
  - {term: Scheduled, found: true}
  - {term: Completed, found: true}
  - {term: Cancelled, found: true}
  - {term: No-show, found: true}
  - {term: Front-desk staff, found: true}
  - {term: Medical history, found: true}
---
# Requirements review: HMS-26

## Summary
HMS-26 asks for a "Visits" summary at the top of the patient page so front-desk staff can see the
next appointment, the last visit and the Completed, Cancelled and No-show counts without scanning the
appointment table. The ticket fixes the selection rules, empty-state texts, the No-show warning
threshold of 2 and the placement. The main risks are time-zone handling (appointments are stored in
UTC; the user confirmed the hospital zone comes from Settings, not a hard-coded value) and keeping
medical history and contact details out of the summary.

## Requirements
- REQ-1 Summary placement (known): a "Visits" summary below the patient's name and above the details.
- REQ-2 Summary contents (known): next appointment, last visit, Completed/Cancelled/No-show counts.
- REQ-3 Next appointment selection (known): earliest Scheduled appointment starting after now, hospital time zone.
- REQ-4 Next appointment display (known): date and time plus the doctor's name.
- REQ-5 Next appointment link (known): the date and time link to that appointment's page.
- REQ-6 No next appointment (known): shows "None booked".
- REQ-7 Last visit selection (known): latest Completed appointment.
- REQ-8 Last visit display (known): its date and the doctor's name, without a time.
- REQ-9 Display time zone (known): hospital zone from Settings (useSettings().timeZone, default Asia/Kolkata); not hard-coded.
- REQ-10 No last visit (known): shows "No visits yet".
- REQ-11 Count scope (known): counts cover all of the patient's appointments, of any date.
- REQ-12 Zero counts (known): a count of 0 is shown as 0.
- REQ-13 No-show warning (known): a No-show count of 2 or more is highlighted as a warning.
- REQ-14 Patient without appointments (known): "None booked", "No visits yet" and three counts of 0.
- REQ-15 No medical history in summary (known).
- REQ-16 No contact details in summary (known).
- REQ-17 Only on the patient's own page (known, PAT-8).
- REQ-18 Nothing else changes (known): rest of the patient page and every other page unchanged.

## Known
- REQ-1 to REQ-8, REQ-10 to REQ-18: stated in the ticket acceptance criteria (lines 22 to 34).
- REQ-15 also matches the baseline data-protection rule that medical history leaves only for the patient's own page.
- REQ-9: decided by the user in the IDE (A-01).

## Assumed
None. The one assumption (REQ-9) was put to the user and corrected in A-01.

## Unknown
None.

## Open questions
None open.

Answered in the IDE:
- A-01 (eng-dffd): REQ-9, "show dates and times in the hospital time zone Asia/Kolkata?" Answer: yes,
  the hospital time zone, but read it from Settings (useSettings().timeZone, default Asia/Kolkata,
  PRD NFR-2), as the doctor and booking pages do; do not hard-code Asia/Kolkata. Decides REQ-9.
  Mismatch: the baseline states the zone as Asia/Kolkata; the user decided it is read from Settings
  with Asia/Kolkata as the default only.

## Out of scope
- The visual style of the No-show warning (REQ-13): a design choice for the design step.
- A Scheduled count: the ticket lists only Completed, Cancelled and No-show counts.
- The contents of PRD PAT-3 and PAT-8: referenced by the ticket but not among this step's inputs.

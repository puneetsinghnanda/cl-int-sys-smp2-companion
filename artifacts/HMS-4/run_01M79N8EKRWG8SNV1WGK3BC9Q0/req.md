---
schema: intent.v1
run_id: run_01M79N8EKRWG8SNV1WGK3BC9Q0
step: req
attempt: 1
skill: requirement-review@1.3.0
inputs:
  - {path: artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md, sha256: ac432f944c4c5387496f348a1fdb5720225a1c5d05aa7c38a9b629dd20e59bfa}
  - {path: context/intent-baseline.md, sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33}
claims:
  - {id: C1, text: "The patient list gets a new column `Last visit`.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L16"]}
  - {id: C2, text: "The column shows the date of the patient's most recent `Completed` appointment.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L17"]}
  - {id: C3, text: "`Cancelled, No-show and Scheduled` appointments are never shown in the column.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L18"]}
  - {id: C4, text: "A patient with no Completed appointment shows `\"-\"`.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L19"]}
  - {id: C5, text: "The list stays `20 per page`.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L20"]}
  - {id: C6, text: "The list keeps its `current search` and sort order.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L20"]}
  - {id: C7, text: "The Next `appointment column is unchanged`.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L21"]}
  - {id: C8, text: "`No medical history` appears in the list.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L22"]}
  - {id: C9, text: "The hospital time zone is `Asia/Kolkata`.", cites: ["context/intent-baseline.md#constraints"]}
  - {id: C10, text: "Medical history appears only on the patient's own page, `never in lists`.", cites: ["context/intent-baseline.md#data-protection"]}
  - {id: C11, text: "A Completed appointment is closed by staff `once its start time has passed`.", cites: ["context/glossary.md#completed"]}
  - {id: C12, text: "The patient list already shows each patient's `next appointment (HMS-1)`.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L14"]}
  - {id: C13, text: "In an earlier HMS-4 run the user chose `Latest start, start date` as the Last visit value.", cites: ["runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-01"]}
  - {id: C14, text: "In an earlier HMS-4 run the user chose to format Last visit `Same as Next appt, with time`.", cites: ["runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-02"]}
  - {id: C15, text: "In an earlier HMS-4 run the user chose that Last visit is `Not sortable`.", cites: ["runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-03"]}
  - {id: C16, text: "In an earlier HMS-4 run the user chose to `Only show the date`, with no highlight or filter.", cites: ["runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-04"]}
  - {id: C17, text: "In an earlier HMS-4 run the user confirmed the patient list is the existing paged list: `Yes, that list`.", cites: ["runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-03"]}
  - {id: C18, text: "Front-desk staff act as `staff` in the MVP.", cites: ["context/glossary.md#front-desk-staff"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12]
assumed: []
unknown: []
requirements:
  - {id: REQ-1, title: "Last visit column", type: functional, status: known, statement: "The existing paged patient list shows a column `Last visit`.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L16", "runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-03"]}
  - {id: REQ-2, title: "Value source", type: functional, status: known, statement: "Last visit is taken from the patient's `Completed` appointment with the latest start time.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L17", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-01"]}
  - {id: REQ-3, title: "Hospital time zone", type: constraint, status: known, statement: "Last visit is shown in the hospital time zone `Asia/Kolkata`.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L17", "context/intent-baseline.md#constraints"]}
  - {id: REQ-4, title: "Display format", type: functional, status: known, statement: "Last visit shows the appointment's start date with its time, formatted the same as Next appointment (`Same as Next appt, with time`).", cites: ["runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-02", "artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L17"]}
  - {id: REQ-5, title: "Other statuses excluded", type: functional, status: known, statement: "`Cancelled, No-show and Scheduled` appointments are never shown in the Last visit column.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L18"]}
  - {id: REQ-6, title: "No Completed appointment", type: functional, status: known, statement: "A patient with no Completed appointment shows `\"-\"` in the Last visit column.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L19"]}
  - {id: REQ-7, title: "Page size unchanged", type: constraint, status: known, statement: "The patient list stays `20 per page`.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L20"]}
  - {id: REQ-8, title: "Search unchanged", type: constraint, status: known, statement: "The patient list keeps its `current search` behaviour.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L20"]}
  - {id: REQ-9, title: "Sort unchanged, column not sortable", type: constraint, status: known, statement: "The patient list keeps its current sort order and the Last visit column is `Not sortable`.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L20", "runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-03"]}
  - {id: REQ-10, title: "Next appointment unchanged", type: constraint, status: known, statement: "The Next `appointment column is unchanged`.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L21"]}
  - {id: REQ-11, title: "No medical history", type: constraint, status: known, statement: "`No medical history` appears in the patient list.", cites: ["artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L22", "context/intent-baseline.md#data-protection"]}
  - {id: REQ-12, title: "Display only", type: constraint, status: known, statement: "Long-unseen patients are not highlighted or filterable; the list will `Only show the date`.", cites: ["runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-04", "artifacts/HMS-4/run_01M79N8EKRWG8SNV1WGK3BC9Q0/ticket.md#L12"]}
unknowns: []
questions: []
answers: []
glossary:
  - {term: "Patient list", found: false}
  - {term: "Last visit", found: false}
  - {term: "Next appointment", found: false}
  - {term: "Completed", found: true}
  - {term: "Cancelled", found: true}
  - {term: "No-show", found: true}
  - {term: "Scheduled", found: true}
  - {term: "Front-desk staff", found: true}
  - {term: "Medical history", found: true}
---
# Requirements review: HMS-4

## Summary
HMS-4 adds a `Last visit` column to the existing paged patient list (C1, C17). The column shows when the patient's most recent `Completed` appointment took place (C2), in hospital time `Asia/Kolkata` (C9). The page size, search, sort order and Next appointment column stay as they are (C5, C6, C7). Answers from earlier HMS-4 runs settle every open point. The main risk is that one of those answers asks for a date with a time, while the ticket says "the date" (see the mismatch below); also, the context pack is 4 commits behind sample-system (E-CTX-10), which matters for later steps.

## Requirements
- REQ-1 Last visit column (known): the existing paged patient list shows a column `Last visit`.
- REQ-2 Value source (known): Last visit is taken from the Completed appointment with the latest start time.
- REQ-3 Hospital time zone (known): shown in `Asia/Kolkata`.
- REQ-4 Display format (known): start date with its time, formatted the same as Next appointment.
- REQ-5 Other statuses excluded (known): Cancelled, No-show and Scheduled appointments are never shown.
- REQ-6 No Completed appointment (known): the column shows `"-"`.
- REQ-7 Page size unchanged (known): 20 per page.
- REQ-8 Search unchanged (known): current search is kept.
- REQ-9 Sort unchanged, column not sortable (known).
- REQ-10 Next appointment unchanged (known).
- REQ-11 No medical history (known): nothing from medical history appears in the list.
- REQ-12 Display only (known): long-unseen patients are not highlighted or filterable.

## Known
- REQ-1: ticket L16; earlier answer confirms it is the existing paged list.
- REQ-2: ticket L17; earlier answer chose latest start. This agrees with Completed being closed `once its start time has passed` (C11).
- REQ-3: ticket L17 and baseline constraints.
- REQ-4: earlier answer `Same as Next appt, with time`.
- REQ-5, REQ-6: ticket L18 and L19.
- REQ-7, REQ-8, REQ-9, REQ-10: ticket L20 and L21; earlier answer makes the column not sortable.
- REQ-11: ticket L22 and baseline data protection.
- REQ-12: earlier answer `Only show the date`.

## Assumed
None.

## Unknown
None. "Patient list", "Last visit" and "Next appointment" are not in the glossary. Earlier answers define them (C13, C17), and the ticket defines Last visit at L17.

## Open questions
None open. No questions were asked in the IDE in this run because answers from earlier HMS-4 runs were reused:
- runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-01: "Latest start, start date". Decides REQ-2.
- runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-02: "Same as Next appt, with time". Decides REQ-4. Mismatch: ticket L17 says "the date"; the user decided to show the date with its time, like Next appointment.
- runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-03: "Not sortable". Decides REQ-9.
- runs/run_01M6GRSJH8H42FGV284MDTDAZY/ide-answers.md#a-04: "Only show the date". Decides REQ-12.
- runs/run_01M6GDYQX07XECMVT06FY0Z6YF/ide-answers.md#a-03: "Yes, that list". Decides REQ-1.

## Out of scope
- Highlighting or filtering long-unseen patients (user story L12-L13): excluded by REQ-12.
- Sorting by Last visit: excluded by REQ-9.
- No instruction-like text, credentials or personal data were found in the inputs.

---
schema: intent.v1
run_id: run_01M89BPMN0XE2ZPMFG17PNG5CP
step: req
attempt: 1
skill: requirement-review@1.4.0
inputs:
  - path: artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md
    sha256: f14f019cc591c5a50bda38602f0e4eb78a9f696182aadd92784ab25bb2a6d61f
  - path: context/intent-baseline.md
    sha256: 9bfd821aeb2fb9524863e346707e049e4fcfb949a6e55b90223a79194946a215
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The patient list today shows its patients `20 per page`, newest registration first, with search and a status filter."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L19"]
  - id: C2
    text: "Lists in the system answer `{ items, page, page_size, total }`, 20 per page."
    cites: ["context/conventions.md#lists"]
  - id: C3
    text: "An appointment's status is one of SCHEDULED, `COMPLETED`, CANCELLED or NO_SHOW."
    cites: ["context/data-model.md#appointments"]
  - id: C4
    text: "A Completed appointment is a past appointment the patient `attended`."
    cites: ["context/glossary.md#completed"]
  - id: C5
    text: "Appointment times are stored as [starts_at, ends_at) in `UTC`."
    cites: ["context/data-model.md#appointments"]
  - id: C6
    text: "The hospital time zone is `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints"]
  - id: C7
    text: "Medical history appears only on the patient's own page, `never in lists`, the dashboard or logs."
    cites: ["context/intent-baseline.md#data-protection"]
  - id: C8
    text: "Medical history is the patient's `Blood group`, allergies, chronic conditions, current medications and notes."
    cites: ["context/glossary.md#medical-history"]
  - id: C9
    text: "The patient list is served by GET /api/patients in `api/src/routes/patients.ts`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/patients"]
  - id: C10
    text: "The patient list page is the web route `/patients`."
    cites: ["context/generated/api-inventory.json#ui:/patients"]
  - id: C11
    text: "Today staff find a patient's last visit by opening the patient's page and reading the `appointment table`."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L20"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13]
assumed: []
unknown: []
requirements:
  - id: REQ-1
    title: Last visit column on the patient list
    type: functional
    status: known
    statement: "The patient list has a `Last visit` column placed directly after the \"Next appointment\" column."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L23"]
  - id: REQ-2
    title: Last visit is the latest Completed appointment
    type: functional
    status: known
    statement: "A patient's last visit is their latest appointment with status `COMPLETED`; Scheduled, Cancelled and No-show appointments do not count as visits."
    cites: ["context/data-model.md#appointments", "context/glossary.md#completed", "artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L23-L24", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-01"]
  - id: REQ-3
    title: Last visit shown as a hospital-local date
    type: functional
    status: known
    statement: "The Last visit column shows the date only (no time) of the last visit, as the hospital-local date in `Asia/Kolkata`."
    cites: ["context/intent-baseline.md#constraints", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-04"]
  - id: REQ-4
    title: Last visit shows the doctor's name
    type: functional
    status: known
    statement: "Next to the date, the Last visit column shows the `doctor's name` of the last-visit appointment."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L24"]
  - id: REQ-5
    title: Never-visited patients show Never
    type: functional
    status: known
    statement: "A patient with no COMPLETED appointment shows `Never` in the Last visit column."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L25", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-01"]
  - id: REQ-6
    title: Not seen for a year filter control
    type: functional
    status: known
    statement: "The patient list has a `Not seen for a year` filter placed next to the status filter."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L26"]
  - id: REQ-7
    title: Filter keeps patients last seen more than 12 months ago
    type: functional
    status: known
    statement: "When the filter is on, the list shows only patients whose last visit was `more than 12 months ago`: the last visit's hospital-local date (Asia/Kolkata) is before the same calendar date 12 months before today."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L26-L27", "context/intent-baseline.md#constraints", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-03"]
  - id: REQ-8
    title: Filter excludes never-visited patients
    type: functional
    status: known
    statement: "When the filter is on, patients who show `Never` (no COMPLETED appointment) are not listed."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L25-L27", "runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-02"]
  - id: REQ-9
    title: Filter combines with search
    type: functional
    status: known
    statement: "The Not seen for a year filter applies together with the list's `search`: both narrow the same result."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L28"]
  - id: REQ-10
    title: Filter combines with the status filter
    type: functional
    status: known
    statement: "The Not seen for a year filter applies together with the `status filter`: both narrow the same result."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L28"]
  - id: REQ-11
    title: Filter works with paging
    type: functional
    status: known
    statement: "With the filter on, the list still pages at `20 per page` and its total counts only the filtered patients."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L28", "context/conventions.md#lists"]
  - id: REQ-12
    title: No medical history in the column or the filter
    type: constraint
    status: known
    statement: "The Last visit column and the filter show no `medical history` (PAT-8)."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L29", "context/intent-baseline.md#data-protection"]
  - id: REQ-13
    title: Everything else unchanged
    type: constraint
    status: known
    statement: "The rest of the patient list, and every other page, is `unchanged`."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/ticket.md#L30"]
unknowns: []
questions: []
answers:
  - id: A-01
    kind: question
    about: Q-01
    answer: "Latest Completed (proposed default)"
    requirements: [REQ-2, REQ-5]
  - id: A-02
    kind: question
    about: Q-02
    answer: "No, exclude (proposed default)"
    requirements: [REQ-8]
  - id: A-03
    kind: question
    about: Q-03
    answer: "12 calendar months, IST (proposed default)"
    requirements: [REQ-7]
  - id: A-04
    kind: assumption
    about: REQ-3
    answer: "Yes, confirm"
    requirements: [REQ-3]
glossary:
  - {term: Front-desk staff, found: true}
  - {term: Completed, found: true}
  - {term: Medical history, found: true}
  - {term: Last visit, found: false}
---
# Requirements review: HMS-27

## Summary
HMS-27 adds a "Last visit" column (date and doctor) to the patient list and a "Not seen for a year" filter, so front-desk staff can find patients due a check-up without opening each patient's page (ticket L14-L16). The glossary does not define a visit. A-01 settles it: a visit is a COMPLETED appointment, so "Never" means there is no Completed appointment. The filter uses 12 calendar months on hospital-local dates (A-03), leaves out never-visited patients (A-02), and works together with search, the status filter and paging. The main risks are time-zone handling at the 12-month boundary, since appointments are stored in UTC (C5, C6), and keeping medical history out of the list query (C7).

## Requirements
- REQ-1 Last visit column on the patient list — known — The patient list has a `Last visit` column placed directly after the "Next appointment" column.
- REQ-2 Last visit is the latest Completed appointment — known — A patient's last visit is their latest appointment with status `COMPLETED`; Scheduled, Cancelled and No-show appointments do not count as visits.
- REQ-3 Last visit shown as a hospital-local date — known — The column shows the date only (no time), as the hospital-local date in `Asia/Kolkata`.
- REQ-4 Last visit shows the doctor's name — known — Next to the date, the column shows the `doctor's name` of the last-visit appointment.
- REQ-5 Never-visited patients show Never — known — A patient with no COMPLETED appointment shows `Never`.
- REQ-6 Not seen for a year filter control — known — The patient list has a `Not seen for a year` filter next to the status filter.
- REQ-7 Filter keeps patients last seen more than 12 months ago — known — With the filter on, only patients whose last visit's hospital-local date is before the same calendar date 12 months before today are listed.
- REQ-8 Filter excludes never-visited patients — known — With the filter on, patients who show `Never` are not listed.
- REQ-9 Filter combines with search — known — The filter and `search` narrow the same result.
- REQ-10 Filter combines with the status filter — known — The filter and the `status filter` narrow the same result.
- REQ-11 Filter works with paging — known — With the filter on, the list still pages at `20 per page`, and the total counts only filtered patients.
- REQ-12 No medical history in the column or the filter — known — The column and the filter show no `medical history` (PAT-8).
- REQ-13 Everything else unchanged — known — The rest of the patient list, and every other page, is `unchanged`.

## Known
- REQ-1: ticket acceptance criterion 1 (L23).
- REQ-2: the visit definition, decided by A-01 and grounded in the glossary entry Completed and the appointment statuses (C3, C4).
- REQ-3: assumption confirmed by A-04, based on the hospital time zone (C6).
- REQ-4: ticket acceptance criterion 1 (L24).
- REQ-5: ticket acceptance criterion 2 (L25), with "visited" defined by A-01.
- REQ-6: ticket acceptance criterion 3 (L26).
- REQ-7: ticket acceptance criterion 3 (L26-L27), with the measure decided by A-03.
- REQ-8: decided by A-02, which matches acceptance criterion 3 read literally.
- REQ-9, REQ-10, REQ-11: ticket acceptance criterion 4 (L28) and the lists convention (C2).
- REQ-12: ticket acceptance criterion 5 (L29) and the data-protection baseline (C7, C8).
- REQ-13: ticket acceptance criterion 6 (L30).

## Assumed
None. The user confirmed the one assumption (REQ-3, A-04).

## Unknown
None. All questions were answered in the IDE.

## Open questions
None open.

Answered in the IDE:
- A-01 (Q-01, answered by eng-dffd): which appointment counts as the last visit → "Latest Completed (proposed default)". Decides REQ-2 and REQ-5. No mismatch with the ticket or baseline.
- A-02 (Q-02, answered by eng-dffd): include never-visited patients under the filter? → "No, exclude (proposed default)". Decides REQ-8. No mismatch; this is the literal reading of ticket L26-L27.
- A-03 (Q-03, answered by eng-dffd): how to measure "more than 12 months ago" → "12 calendar months, IST (proposed default)". Decides REQ-7. No mismatch.
- A-04 (REQ-3 assumption, answered by eng-dffd): date only, hospital-local in Asia/Kolkata → "Yes, confirm". Decides REQ-3. No mismatch.

## Out of scope
- Calling patients for a check-up (ticket L15-L16) is the staff outcome the feature supports, not a system behaviour.
- Sorting the list by last visit was not asked for; the list keeps its newest-registration-first order (C1, REQ-13).
- The patient page's appointment table (C11) is not changed.
- "Last visit" is not in the glossary (found: false). A-01 defines it for this run; adding a glossary entry is left to the context owner.
- No instruction-like text, credentials or personal data were found in the inputs.

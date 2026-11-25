---
schema: stories.v1
run_id: run_01M89BPMN0XE2ZPMFG17PNG5CP
step: split
attempt: 1
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/req.md
    sha256: 447d72cf313d2a06ab87a4c709878bfd9f923c28907519db3dd6997a343ec267
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "A patient's last visit is their latest appointment with status `COMPLETED`."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/req.md#item:REQ-2"]
  - id: C2
    text: "The user chose `Latest Completed` as the definition of the last visit."
    cites: ["runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-01"]
  - id: C3
    text: "A Completed appointment is one the patient `attended`."
    cites: ["context/glossary.md#completed"]
  - id: C4
    text: "The Last visit date is the hospital-local date in `Asia/Kolkata`."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/req.md#item:REQ-3"]
  - id: C5
    text: "Appointment times are stored in `UTC`."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/req.md#claim:C5"]
  - id: C6
    text: "The 12-month rule counts `12 calendar months` in hospital-local days."
    cites: ["runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-03"]
  - id: C7
    text: "Never-visited patients are excluded from the filter: the user answered `No, exclude`."
    cites: ["runs/run_01M89BPMN0XE2ZPMFG17PNG5CP/ide-answers.md#a-02"]
  - id: C8
    text: "The patient list pages at `20 per page`."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/req.md#item:REQ-11"]
  - id: C9
    text: "The patient list is ordered `newest registration first`."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/req.md#claim:C1"]
  - id: C10
    text: "The list response carries a `total` count."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/req.md#item:REQ-14"]
  - id: C11
    text: "Medical history is `Blood group`, allergies, chronic conditions, current medications and notes."
    cites: ["context/glossary.md#medical-history"]
  - id: C12
    text: "The patient list is served from `api/src/routes/patients.ts`."
    cites: ["artifacts/HMS-27/run_01M89BPMN0XE2ZPMFG17PNG5CP/req.md#claim:C9"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See each patient's last visit on the patient list
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given the patient list is open, when staff look at the column headers, then a \"Last visit\" column appears directly after the \"Next appointment\" column."
      - id: AC2
        text: "Given a patient with a COMPLETED appointment with Dr A on 10 March 2026 and a later NO_SHOW appointment with Dr B on 1 October 2026, when staff open the patient list, then the patient's Last visit shows 10 March 2026 and Dr A, not the later No-show."
      - id: AC3
        text: "Given a patient whose only COMPLETED appointment starts at 19:00 UTC on 24 November 2025 (Asia/Kolkata = UTC+05:30, so 00:30 local on 25 November 2025, just after local midnight), when staff open the patient list, then the Last visit shows the date 25 November 2025 with no time of day."
      - id: AC4
        text: "Given a patient with a COMPLETED appointment with Dr A in January 2026 and a later COMPLETED appointment with Dr B in June 2026, when staff open the patient list, then the Last visit shows Dr B's name next to the June 2026 date."
      - id: AC5
        text: "Given a patient who has only a CANCELLED appointment and a future SCHEDULED appointment and no COMPLETED appointment, when staff open the patient list, then the Last visit column shows \"Never\"."
  - id: S-2
    title: Filter the patient list to patients not seen for a year
    trace: [REQ-6, REQ-7, REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given the patient list is open, when staff look at the filters, then a \"Not seen for a year\" filter appears next to the status filter."
      - id: AC2
        text: "Given today is 25 November 2026 in Asia/Kolkata (12 calendar months earlier is 25 November 2025) and patient P1's last COMPLETED appointment is at 10:00 local on 24 November 2025, when staff turn the filter on, then P1 is listed."
      - id: AC3
        text: "Given today is 25 November 2026 in Asia/Kolkata and patient P2's last COMPLETED appointment starts at 19:00 UTC on 24 November 2025 (UTC+05:30, so 00:30 local on 25 November 2025, after local midnight), when staff turn the filter on, then P2 is not listed, because its local date is not before 25 November 2025."
      - id: AC4
        text: "Given today is 25 November 2026 and patient P3 has a COMPLETED appointment on 1 January 2025 and a later NO_SHOW appointment on 1 October 2026, when staff turn the filter on, then P3 is listed, because the No-show is not a visit."
      - id: AC5
        text: "Given a patient who shows \"Never\" (no COMPLETED appointment, registered long ago), when staff turn the filter on, then that patient is not listed."
  - id: S-3
    title: Use the not-seen filter with search, status and paging
    trace: [REQ-9, REQ-10, REQ-11, REQ-14]
    acceptance_criteria:
      - id: AC1
        text: "Given two patients not seen for a year, only one of whose name matches the search text, when staff turn the filter on and search for that text, then only the matching patient is listed."
      - id: AC2
        text: "Given two patients not seen for a year, one of them Inactive, when staff turn the filter on and set the status filter to Inactive, then only the Inactive patient is listed."
      - id: AC3
        text: "Given 21 patients not seen for a year, when staff turn the filter on, then page 1 lists 20 of them (20 per page) and page 2 lists the 21st."
      - id: AC4
        text: "Given 25 patients of whom 21 are not seen for a year, when staff turn the filter on, then the list total is 21, not 25."
  - id: S-4
    title: Keep medical history out and leave the rest of the list unchanged
    trace: [REQ-12, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a patient with a blood group, allergies and current medications recorded and a last visit more than a year ago, when staff open the patient list with the filter on, then neither the page nor the GET /api/patients response shows any of that medical history."
      - id: AC2
        text: "Given a patient with a future SCHEDULED appointment, when staff open the patient list with the filter off, then the Next appointment column shows that appointment as it did before the change."
      - id: AC3
        text: "Given a patient with no future SCHEDULED appointment, when staff open the patient list with the filter off, then the Next appointment column shows the same empty value as before the change."
      - id: AC4
        text: "Given more patients than fit on one page, when staff open the patient list with the filter off, then all patients are listed newest registration first at 20 per page, as before the change."
      - id: AC5
        text: "Given any patient, when staff open that patient's own page, then the page, including its appointment table, is unchanged."
---
# Stories: HMS-27

## Stories

### S-1 See each patient's last visit on the patient list
Traces REQ-1, REQ-2, REQ-3, REQ-4, REQ-5.
- AC1: Given the patient list is open, when staff look at the column headers, then a "Last visit" column appears directly after the "Next appointment" column.
- AC2: Given a patient with a COMPLETED appointment with Dr A on 10 March 2026 and a later NO_SHOW appointment with Dr B on 1 October 2026, when staff open the patient list, then the patient's Last visit shows 10 March 2026 and Dr A, not the later No-show.
- AC3: Given a patient whose only COMPLETED appointment starts at 19:00 UTC on 24 November 2025 (Asia/Kolkata = UTC+05:30, so 00:30 local on 25 November 2025, just after local midnight), when staff open the patient list, then the Last visit shows the date 25 November 2025 with no time of day.
- AC4: Given a patient with a COMPLETED appointment with Dr A in January 2026 and a later COMPLETED appointment with Dr B in June 2026, when staff open the patient list, then the Last visit shows Dr B's name next to the June 2026 date.
- AC5: Given a patient who has only a CANCELLED appointment and a future SCHEDULED appointment and no COMPLETED appointment, when staff open the patient list, then the Last visit column shows "Never".

### S-2 Filter the patient list to patients not seen for a year
Traces REQ-6, REQ-7, REQ-8.
- AC1: Given the patient list is open, when staff look at the filters, then a "Not seen for a year" filter appears next to the status filter.
- AC2: Given today is 25 November 2026 in Asia/Kolkata (12 calendar months earlier is 25 November 2025) and patient P1's last COMPLETED appointment is at 10:00 local on 24 November 2025, when staff turn the filter on, then P1 is listed.
- AC3: Given today is 25 November 2026 in Asia/Kolkata and patient P2's last COMPLETED appointment starts at 19:00 UTC on 24 November 2025 (UTC+05:30, so 00:30 local on 25 November 2025, after local midnight), when staff turn the filter on, then P2 is not listed, because its local date is not before 25 November 2025.
- AC4: Given today is 25 November 2026 and patient P3 has a COMPLETED appointment on 1 January 2025 and a later NO_SHOW appointment on 1 October 2026, when staff turn the filter on, then P3 is listed, because the No-show is not a visit.
- AC5: Given a patient who shows "Never" (no COMPLETED appointment, registered long ago), when staff turn the filter on, then that patient is not listed.

### S-3 Use the not-seen filter with search, status and paging
Traces REQ-9, REQ-10, REQ-11, REQ-14.
- AC1: Given two patients not seen for a year, only one of whose name matches the search text, when staff turn the filter on and search for that text, then only the matching patient is listed.
- AC2: Given two patients not seen for a year, one of them Inactive, when staff turn the filter on and set the status filter to Inactive, then only the Inactive patient is listed.
- AC3: Given 21 patients not seen for a year, when staff turn the filter on, then page 1 lists 20 of them (20 per page) and page 2 lists the 21st.
- AC4: Given 25 patients of whom 21 are not seen for a year, when staff turn the filter on, then the list total is 21, not 25.

### S-4 Keep medical history out and leave the rest of the list unchanged
Traces REQ-12, REQ-13.
- AC1: Given a patient with a blood group, allergies and current medications recorded and a last visit more than a year ago, when staff open the patient list with the filter on, then neither the page nor the GET /api/patients response shows any of that medical history.
- AC2: Given a patient with a future SCHEDULED appointment, when staff open the patient list with the filter off, then the Next appointment column shows that appointment as it did before the change.
- AC3: Given a patient with no future SCHEDULED appointment, when staff open the patient list with the filter off, then the Next appointment column shows the same empty value as before the change.
- AC4: Given more patients than fit on one page, when staff open the patient list with the filter off, then all patients are listed newest registration first at 20 per page, as before the change.
- AC5: Given any patient, when staff open that patient's own page, then the page, including its appointment table, is unchanged.

## Known
- S-1: built only from known REQ-1 to REQ-5. The visit definition comes from A-01 (C1, C2) and the local date from REQ-3 (C4, C5).
- S-2: built only from known REQ-6 to REQ-8. The 12-month rule comes from A-03 (C6) and the never-visited exclusion from A-02 (C7).
- S-3: built only from known REQ-9, REQ-10, REQ-11 and REQ-14. Paging and the total follow the lists convention (C8, C10).
- S-4: built only from known REQ-12 and REQ-13. Medical history is as the glossary defines it (C11), and the list order is unchanged (C9).

## Assumed
None.

## Unknown
None. The intent has no unknown requirements or open questions.

## Open questions
None.

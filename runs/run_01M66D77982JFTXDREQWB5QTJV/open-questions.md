---
schema: open-questions.v1
run_id: run_01M66D77982JFTXDREQWB5QTJV
ticket: HMS-4
workflow: brownfield-change@1.3.0
step: secrev@1.0.1
created_at: '2026-10-30T13:04:02Z'
status: open
---

# Open questions — HMS-4 (split step)

Answer each question below the line `answer:`. Keep the ids. Then run:
`/truminds:resume run_01M66D77982JFTXDREQWB5QTJV`

## Q-01 · non-blocking · ask: product owner
**Question:** Which timestamp defines the Last visit date, the appointment start time or the time staff marked it Completed?
**Why it matters:** Staff may close an appointment after it took place, which can change the date shown; S-1 assumes start time (REQ-13, REQ-15).
**Evidence:** artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md#L115, context/glossary.md#completed
**Options:**
  A. appointment start time
  B. time marked Completed
**Proposed default:** appointment start time
answer:
answered_by:
decision:

## Q-02 · non-blocking · ask: product owner
**Question:** What date format should the Last visit column use?
**Why it matters:** S-1 AC4 assumes the Next appointment column's format (REQ-14).
**Evidence:** artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md#L122
**Options:**
  A. same format as the Next appointment column
  B. a new format
**Proposed default:** same format as the Next appointment column
answer:
answered_by:
decision:

## Q-03 · non-blocking · ask: product owner
**Question:** Should the Last visit column be sortable or searchable?
**Why it matters:** No story covers sorting or searching by Last visit; S-3 keeps the current search and sort order.
**Evidence:** artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/req.md#L97
**Options:**
  A. display only
  B. sortable
  C. sortable and filterable
**Proposed default:** display only
answer:
answered_by:
decision:

## Q-04 · non-blocking · ask: tech lead
**Question:** Should Last visit, patient list and next appointment be added to the glossary?
**Why it matters:** Agreed definitions keep tests and later steps consistent.
**Evidence:** context/glossary.md#glossary
**Options:**
  A. add all three
  B. leave as is
**Proposed default:** add all three; Last visit as defined in REQ-2
answer:
answered_by:
decision:

## Q-05 · non-blocking · ask: product owner · round 2
**Question:** Is Last visit based on the appointment start time (current default) or on the time staff marked it Completed? The second option would bring appointment_event into the read path.
**Proposed default:** none
answer:
answered_by:
decision:

## Q-06 · non-blocking · ask: tech lead · round 2
**Question:** Should the conversion of the Last visit instant to the hospital date happen in the API or in the web page?
**Why it matters:** Instants are stored in UTC and the hospital zone is configured in the API; converting in one place avoids dates that differ from the Next appointment column.
**Evidence:** context/architecture.md#time-and-slots
**Options:**
  A. API returns the instant and the web formats it like Next appointment
  B. API returns a hospital-zone date
**Proposed default:** API returns the instant and the web formats it like Next appointment
answer:
answered_by:
decision:

## Q-07 · non-blocking · ask: tech lead · round 2
**Question:** Does any web page other than the patient list call GET /api/patients (for example a patient picker when booking)?
**Why it matters:** The list response gains a field; other callers would receive it too. The facts list the route but not its callers.
**Evidence:** context/generated/api-inventory.json#http:GET:/api/patients, context/architecture.md#components
**Options:**
  A. only the patient list
  B. other callers exist and must ignore the new field
**Proposed default:** other callers ignore the new field; it is additive
answer:
answered_by:
decision:

## Q-08 · non-blocking · ask: product owner · round 3
**Question:** Should the Last visit cell show only the calendar date in the hospital zone (REQ-2), or the same date and time that startsAt shows for Next appointment (REQ-14)?
**Proposed default:** none
answer:
answered_by:
decision:

## Q-09 · non-blocking · ask: tech lead · round 3
**Question:** What should the new list field be called?
**Why it matters:** The name is not in any input; the web page and tests depend on it.
**Evidence:** context/conventions.md#lists, context/generated/api-inventory.json#http:GET:/api/patients
**Options:**
  A. last_visit_at
  B. lastVisitAt
**Proposed default:** last_visit_at, matching the snake_case of appointment starts_at
answer:
answered_by:
decision:

## Q-10 · non-blocking · ask: tech lead · round 3
**Question:** Does any web page other than the patient list read GET /api/patients?
**Why it matters:** Those callers receive the new field; it is additive, so they should be unaffected.
**Evidence:** context/generated/api-inventory.json#http:GET:/api/patients, context/architecture.md#components
**Options:**
  A. only the patient list
  B. other callers exist and ignore the new field
**Proposed default:** other callers exist and ignore the new field
answer:
answered_by:
decision:

## Q-11 · non-blocking · ask: tech lead · round 4
**Question:** In the MVP every request is staff with every permission granted; is it accepted that the new visit history on the list has the same exposure?
**Why it matters:** The Last visit column adds visit history to a list that has no real sign-in yet; spoofing and privilege threats rely on the future sign-in.
**Evidence:** context/architecture.md#identity
**Options:**
  A. accept for MVP
  B. block until sign-in exists
**Proposed default:** accept for MVP; the list already shows Next appointment with the same exposure
answer:
answered_by:
decision:

## Q-12 · non-blocking · ask: product owner · round 5
**Question:** Where should the Last visit column sit in the patient list?
**Why it matters:** The ticket names the column but not its position; it was placed before Next appointment so the Next appointment column stays the last cell, as existing tests and users expect.
**Evidence:** artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/split.md#L74
**Options:**
  A. before Next appointment
  B. after Next appointment
**Proposed default:** before Next appointment
answer:
answered_by:
decision:

## Q-13 · non-blocking · ask: product owner · round 5
**Question:** Should the Last visit cell show only the date, or the same date and time as Next appointment?
**Why it matters:** The startsAt helper shows date and time; REQ-2 says date while REQ-14 says the same format as Next appointment. The code follows REQ-14.
**Evidence:** artifacts/HMS-4/run_01M66D77982JFTXDREQWB5QTJV/split.md#L80
**Options:**
  A. same date and time as Next appointment
  B. date only
**Proposed default:** same date and time as Next appointment
answer:
answered_by:
decision:

## Q-14 · non-blocking · ask: tech lead · round 6
**Question:** Does the verifier container provide PostgreSQL 16 or HMS_TEST_DATABASE_URL, so the API tests run instead of being skipped?
**Why it matters:** All API tests, including the HMS-4 ones, skip without a database; a passing H3 would then not prove the last-visit query.
**Evidence:** context/test-strategy.md#database-for-tests
**Options:**
  A. verifier provides PostgreSQL
  B. API tests run only locally and in CI
**Proposed default:** API tests run only locally and in CI; run npm test with PostgreSQL before merging
answer:
answered_by:
decision:

## Q-15 · non-blocking · ask: tech lead · round 6
**Question:** Does the search "patient one" match only fixture patient 1?
**Why it matters:** The REQ-8/REQ-9 test assumes it does, mirroring the existing HMS-1 search for "patient two".
**Evidence:** context/generated/test-layout.json#test:sample-system:api/test/patients-next-appointment.test.ts
**Options:**
  A. yes
  B. no, use another search term
**Proposed default:** yes
answer:
answered_by:
decision:

## Q-16 · non-blocking · ask: tech lead · round 7
**Question:** Is it accepted for the MVP that every caller of GET /api/patients now also sees each patient's last visit date?
**Why it matters:** In the MVP every request is staff with every permission, so the new field has no access control beyond the existing list.
**Evidence:** context/architecture.md#identity
**Options:**
  A. accept for MVP
  B. gate behind sign-in before release
**Proposed default:** accept for MVP; same exposure as the existing Next appointment field
answer:
answered_by:
decision:

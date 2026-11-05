# IDE answers: run_01M6GDYQX07XECMVT06FY0Z6YF

Written by the tsl post-tool hook from AskUserQuestion. Do not edit: artifacts cite these
entries as the user's own answers.

## A-01
- step: req
- attempt: 1
- asked_at: 2026-11-03T10:08:32Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_01MNj46D3QPZgJJWd7NBsq9r
- question: Q-01: The ticket says "Last visit" shows the date of the most recent Completed appointment, but it doesn't say which date. Which one should it be?
- options: Start date (proposed default) | Date it was closed
- answer: Start date (proposed default)

## A-02
- step: req
- attempt: 1
- asked_at: 2026-11-03T10:08:32Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_01MNj46D3QPZgJJWd7NBsq9r
- question: Q-02: The ticket says the list keeps its current sort order. Should staff also be able to sort by the new "Last visit" column?
- options: Not sortable (proposed default) | Sortable
- answer: Not sortable (proposed default)

## A-03
- step: req
- attempt: 1
- asked_at: 2026-11-03T10:08:32Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_01MNj46D3QPZgJJWd7NBsq9r
- question: Q-03: Neither "Patient list" nor "Last visit" is in the glossary. Is "patient list" the existing paged patients list that already shows Next appointment (HMS-1)?
- options: Yes, that list (proposed default) | Also other lists
- answer: Yes, that list (proposed default)

## A-04
- step: req
- attempt: 1
- asked_at: 2026-11-03T10:08:32Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_01MNj46D3QPZgJJWd7NBsq9r
- question: Q-04: The ticket doesn't give a date format for "Last visit". Which format should it use?
- options: Same as Next appointment (proposed default) | Date only, ISO
- answer: Same as Next appointment (proposed default)

## A-05
- step: impact
- attempt: 1
- asked_at: 2026-11-03T10:32:27Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_014GEtUiDzzSQUnabW8bPoeQ
- question: Q-01: Where should the patient list get each patient's Last visit? The facts show one list route, `GET /api/patients` (api/src/routes/patients.ts).
- options: Add to GET /api/patients (proposed default) | New API route
- answer: Add to GET /api/patients (proposed default)

## A-06
- step: impact
- attempt: 1
- asked_at: 2026-11-03T10:32:27Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_014GEtUiDzzSQUnabW8bPoeQ
- question: Q-02: Does this need a database change? `table:appointment` already has `index:appointment_patient_start_idx`, which covers patient and start time.
- options: No migration (proposed default) | New index or column
- answer: No migration (proposed default)

## A-07
- step: impact
- attempt: 1
- asked_at: 2026-11-03T10:32:27Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_014GEtUiDzzSQUnabW8bPoeQ
- question: Q-03: The generated facts have no call edges. Which web pages call `GET /api/patients`?
- options: PatientsPage + pickers (proposed default) | Only PatientsPage
- answer: PatientsPage + pickers (proposed default)

## A-08
- step: impact
- attempt: 1
- asked_at: 2026-11-03T10:32:27Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_014GEtUiDzzSQUnabW8bPoeQ
- question: Q-04: tsl warned E-CTX-10: the context pack is 2 commits and 10 files behind sample-system. How should impact analysis handle that?
- options: Proceed, re-check in implement (proposed default) | Stop and refresh the pack
- answer: Proceed, re-check in implement (proposed default)

## A-09
- step: impact
- attempt: 1
- asked_at: 2026-11-03T10:34:20Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_01QNdVnA762DSE7GW1LQDAdP
- question: C1: I assumed the UI route `ui:/patients` (web/src/App.tsx line 30) is rendered by `PatientsPage` (web/src/pages/PatientsPage.tsx), and that this page gets the new Last visit column. Is that right?
- options: Yes, confirm | A different component
- answer: Yes, confirm

## A-10
- step: impact
- attempt: 1
- asked_at: 2026-11-03T10:34:20Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_01QNdVnA762DSE7GW1LQDAdP
- question: C2: I assumed the API works out the local date of the last visit with the hospital time zone (`HOSPITAL_TZ`, default Asia/Kolkata) and the helpers in api/src/lib/time.ts such as `localDay`. Is that right?
- options: Yes, confirm | Web formats it
- answer: Yes, confirm

## A-11
- step: impact
- attempt: 1
- asked_at: 2026-11-03T10:34:20Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_01QNdVnA762DSE7GW1LQDAdP
- question: C3: I assumed the Next appointment column is formatted with the web helper `dateTime` in web/src/lib/format.ts, so Last visit reuses the same helper. Is that right?
- options: Yes, confirm | Formatted inline
- answer: Yes, confirm

## A-12
- step: impact
- attempt: 2
- asked_at: 2026-11-03T10:40:54Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_01PRTpB65s54EmiKkrDr7Vaz
- question: Q-05: The reviewer found a conflict. C2 (the API works out a local date) and C3 (reuse `dateTime`, which takes an instant and also prints hour:minute) can't both hold. How should Last visit go from the API to the page?
- options: Instant + date-only helper (proposed default) | API local day + date helper | Instant + dateTime as-is
- answer: Instant + date-only helper (proposed default)

## A-13
- step: impact
- attempt: 2
- asked_at: 2026-11-03T10:40:54Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_01PRTpB65s54EmiKkrDr7Vaz
- question: C14: I assumed `PatientService` (api/src/services/patientService.ts) only passes the new lastVisit field through from PatientRepo to the route, with no new rule. Is that right?
- options: Yes, confirm | Service computes it
- answer: Yes, confirm

## A-14
- step: design
- attempt: 1
- asked_at: 2026-11-03T10:54:20Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_015dKvHWKXAGqBBaVCmLaevw
- question: Q-01: The stories check the Last visit date, so the field needs an exact name and format. What should each item in `GET /api/patients` carry? List responses use snake_case such as `page_size`.
- options: last_visit_at, ISO UTC (proposed default) | lastVisitAt, ISO UTC
- answer: lastVisitAt, ISO UTC

## A-15
- step: design
- attempt: 1
- asked_at: 2026-11-03T10:54:20Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_015dKvHWKXAGqBBaVCmLaevw
- question: Q-02: What should the new date-only helper next to `dateTime` in web/src/lib/format.ts be called?
- options: date(iso, timeZone) (proposed default) | dateOnly(iso, timeZone)
- answer: dateOnly(iso, timeZone)

## A-16
- step: design
- attempt: 1
- asked_at: 2026-11-03T10:54:20Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_015dKvHWKXAGqBBaVCmLaevw
- question: C1: I assumed PatientsPage gets the hospital time zone from `useSettings` (web/src/hooks/useSettings.tsx, backed by GET /api/settings), the same way it formats Next appointment. Is that right?
- options: Yes, confirm | Another source
- answer: Yes, confirm

## A-17
- step: design
- attempt: 1
- asked_at: 2026-11-03T10:54:20Z
- answered_by: gaurav-saini-truminds
- tool_use_id: toolu_015dKvHWKXAGqBBaVCmLaevw
- question: C2: The facts don't show where the web's patient list item type is declared. I assumed implement finds and extends it, in PatientsPage.tsx or next to `api` in web/src/api/client.ts. Is that right?
- options: Yes, confirm | In web/src/api/client.ts
- answer: Yes, confirm

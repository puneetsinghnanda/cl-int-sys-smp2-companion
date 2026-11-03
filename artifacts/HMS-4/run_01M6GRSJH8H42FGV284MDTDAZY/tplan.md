---
schema: test-plan.v1
run_id: run_01M6GRSJH8H42FGV284MDTDAZY
step: tplan
attempt: 1
skill: test-plan@1.1.0
inputs:
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/split.md
  sha256: 1097c5a30f050a771deca3a06a8eb429ba26a1b31dd0aada1164d17b95a31518
- path: artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/design.md
  sha256: f9caf706ba92848aaa8be4ce155d2505c138a5e39292e0c5527eedec728526bd
- path: context/test-strategy.md
  sha256: e7465d17d8814cd71fe160b96a859f1ba1414836ad71ef48b9aaeb166259eee8
- path: context/generated/test-layout.json
  sha256: fd4b5d51b15f9aba1d03b5f6a0350e4e4849d862118fb3a3bda7abfa5127e179
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
- id: C1
  text: "API tests use Vitest and `Supertest` against a throwaway PostgreSQL 16 with a fixed clock."
  cites: ["context/test-strategy.md#layers"]
- id: C2
  text: "Web tests use Vitest with jsdom and `Testing Library`."
  cites: ["context/test-strategy.md#layers"]
- id: C3
  text: "No page is tested end to end in a `browser`."
  cites: ["context/test-strategy.md#gaps"]
- id: C4
  text: "The design adds `lastVisitAt` to each list item, null when there is no Completed appointment."
  cites: ["artifacts/HMS-4/run_01M6GRSJH8H42FGV284MDTDAZY/design.md#claim:C13"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [TP-1, TP-2, TP-3, TP-4, TP-5, TP-6, TP-7, TP-8, TP-9, TP-10, TP-11, TP-12, TP-13, TP-14, TP-15, TP-16, TP-17]
assumed: []
unknown: []
questions: []
tests:
- {id: TP-1, title: "Latest Completed start wins, whatever the closing order", covers: [S-1/AC1], level: integration}
- {id: TP-2, title: "19:30 UTC on 9 March shows as 10 March 01:00 Asia/Kolkata", covers: [S-1/AC2], level: unit}
- {id: TP-3, title: "18:15 UTC on 10 March shows as 10 March 23:45", covers: [S-1/AC3], level: unit}
- {id: TP-4, title: "Last visit uses the same text as Next appointment for the same instant", covers: [S-1/AC4], level: unit}
- {id: TP-5, title: "Old and recent visits render alike, with no highlight or new filter", covers: [S-1/AC5], level: unit}
- {id: TP-6, title: "No-show, Cancelled and Scheduled appointments after the Completed one are ignored", covers: [S-2/AC1, S-2/AC2, S-2/AC3], level: integration}
- {id: TP-7, title: "Only non-Completed appointments gives null in the API", covers: [S-2/AC4], level: integration}
- {id: TP-8, title: "No appointments gives null in the API and '-' on the page", covers: [S-2/AC5], level: integration}
- {id: TP-9, title: "21 patients: 20 on page one, the 21st alone on page two, each once", covers: [S-3/AC1], level: integration}
- {id: TP-10, title: "Search returns each matching patient once with its Last visit", covers: [S-3/AC2], level: integration}
- {id: TP-11, title: "Rows stay newest first, not ordered by Last visit", covers: [S-3/AC3], level: integration}
- {id: TP-12, title: "Last visit header has no sort control and clicking it changes nothing", covers: [S-3/AC4], level: unit}
- {id: TP-13, title: "Next appointment cell unchanged with an upcoming appointment", covers: [S-4/AC1], level: unit}
- {id: TP-14, title: "Next appointment unchanged with no upcoming appointment; the Completed one shows only under Last visit", covers: [S-4/AC2], level: integration}
- {id: TP-15, title: "List items carry no medical history fields or values", covers: [S-5/AC1], level: integration}
- {id: TP-16, title: "Negative: an invalid list filter is still rejected with a field error", covers: [S-3/AC2], level: integration}
- {id: TP-17, title: "Negative: a null lastVisitAt renders '-' and an invalid hospital zone falls back to Asia/Kolkata", covers: [S-2/AC5, S-1/AC2], level: unit}
---
# Test plan: HMS-4

## Scope
In scope: the `lastVisitAt` field on `GET /api/patients` (C4) and the Last visit column on the patient list page, plus regressions of paging, search, order, Next appointment and the no-medical-history rule.
- API behaviour is tested at the integration level, with Vitest and Supertest on a throwaway database and a fixed clock (C1).
- Page rendering is tested at the unit level, with Vitest, jsdom and Testing Library (C2).

Out of scope: browser end-to-end tests, because the system has no browser test layer (C3). No criterion needs one.

Existing files reused: `api/test/patients-last-visit.test.ts`, `api/test/patients.test.ts` (PAT-8 key set), `api/test/patients-next-appointment.test.ts` and `web/src/__tests__/PatientsPage.test.tsx`.

## Tests
- TP-1 (integration): S-1/AC1. Two Completed visits, the later one closed first; lastVisitAt is the later start.
- TP-2 (unit): S-1/AC2. The cell shows 10 Mar 2026 and 01:00, never 09 Mar or 19:30.
- TP-3 (unit): S-1/AC3. The cell shows 10 Mar 2026 and 23:45.
- TP-4 (unit): S-1/AC4. The Last visit text equals the date-time part of Next appointment for the same instant.
- TP-5 (unit): S-1/AC5. A row with a visit a year old and a row with a visit yesterday have the same cell attributes, and the page has no new filter control.
- TP-6 (integration): S-2/AC1, S-2/AC2, S-2/AC3. Later No-show, Cancelled and still-Scheduled appointments do not change lastVisitAt.
- TP-7 (integration): S-2/AC4. A patient with only non-Completed appointments gets null.
- TP-8 (integration): S-2/AC5. A patient with no appointments gets null. The unit test for "-" is TP-17.
- TP-9 (integration): S-3/AC1. With 21 patients, page one holds 20 rows, page two holds only the 21st, and every id appears once. The 1st and 20th rows have several appointments.
- TP-10 (integration): S-3/AC2. A search returns each matching patient once, with its value.
- TP-11 (integration): S-3/AC3. The order stays newest first, whatever the visit dates.
- TP-12 (unit): S-3/AC4. The header has no button, link or aria-sort. Clicking it leaves the row order and the fetch calls unchanged.
- TP-13 (unit): S-4/AC1. The existing HMS-2 test of the Next appointment link and doctor, now aimed at the second-to-last cell.
- TP-14 (integration): S-4/AC2. A Completed appointment and no upcoming one: next_appointment is null and lastVisitAt holds the visit.
- TP-15 (integration): S-5/AC1. The PAT-8 key set includes lastVisitAt and no medical-history key. The list JSON contains none of the patient's blood group, allergies, conditions, medications or notes.
- TP-16 (integration, negative): S-3/AC2. An invalid status filter still returns 400 with a field error.
- TP-17 (unit, negative and boundary): S-2/AC5, S-1/AC2. A null value renders "-". An invalid hospital zone setting falls back to Asia/Kolkata, as Next appointment does.

## Known
- TP-1, TP-6 to TP-11, TP-14 to TP-16: integration tests through the list route against a real database (C1).
- TP-2 to TP-5, TP-12, TP-13, TP-17: unit tests of the page, which are the cheapest level that proves rendering (C2).

## Assumed
None.

## Unknown
None.

## Open questions
None.

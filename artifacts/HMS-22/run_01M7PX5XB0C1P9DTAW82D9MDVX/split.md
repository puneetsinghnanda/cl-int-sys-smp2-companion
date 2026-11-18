---
schema: stories.v1
run_id: run_01M7PX5XB0C1P9DTAW82D9MDVX
step: split
attempt: 2
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/req.md
    sha256: cce290751712fe7cab482e213a87e79b5617807ebafe66bc494ea520b72f724a
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
resolves:
  - id: F-1
    how: "S-2 AC2 and AC3 now test the first local day of Last 7 days and Last 30 days on both sides of midnight."
  - id: F-2
    how: "S-3 AC1 now checks the list filter on both sides of local midnight in Asia/Kolkata."
  - id: F-3
    how: "S-4 AC4 now covers the doctor tiles, and the personal-data criteria moved to new story S-5."
claims:
  - id: C1
    text: "The new dashboard tile is labelled `New patients`."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/req.md#item:REQ-1"]
  - id: C2
    text: "The tile counts patients by their `created_at` in the selected period."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/req.md#item:REQ-2"]
  - id: C3
    text: "Period days are hospital-local days in `Asia/Kolkata`."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/req.md#item:REQ-3"]
  - id: C4
    text: "A period is Today, Last 7 days or `Last 30 days`, in whole hospital-local days ending today."
    cites: ["context/glossary.md#period"]
  - id: C5
    text: "The list opened from the tile uses `All statuses from tile`."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/req.md#item:REQ-7"]
  - id: C6
    text: "The filtered patient list stays at `20 per page`."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/req.md#item:REQ-8"]
  - id: C7
    text: "A search already typed on the list is `cleared` when the list is opened from the tile."
    cites: ["artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/req.md#item:REQ-9"]
  - id: C8
    text: "`Medical history` is blood group, allergies, chronic conditions, current medications and notes."
    cites: ["context/glossary.md#medical-history"]
  - id: C9
    text: "The dashboard has tiles that count patients, `doctors` and appointments."
    cites: ["context/glossary.md#dashboard"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [S-1, S-2, S-3, S-4, S-5]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See how many patients were registered in the period
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given today is 18 November in Asia/Kolkata and the period is Today, when staff open the dashboard, then a tile labelled New patients is shown."
      - id: AC2
        text: "Given the period is Today on 18 November and patient P1 has created_at 2026-11-17T19:00Z (Asia/Kolkata = UTC+05:30, so 00:30 local on 18 November, after local midnight) and patient P2 has created_at 2026-11-17T18:00Z (23:30 local on 17 November, before local midnight), when staff open the dashboard, then the New patients tile counts P1 and not P2."
      - id: AC3
        text: "Given the period is Today and two patients were created today, one Active and one Inactive, when staff open the dashboard, then the New patients tile counts both of them."
      - id: AC4
        text: "Given the period is Today and patients exist but none has a created_at on today's local date, when staff open the dashboard, then the New patients tile shows 0."
  - id: S-2
    title: The count follows the period selector
    trace: [REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given the period is Today, when staff change the period to Last 7 days, then the New patients count updates the same way the Completed and Cancelled tiles do."
      - id: AC2
        text: "Given today is 18 November local, P3 has created_at 2026-11-11T18:40Z (Asia/Kolkata = UTC+05:30, so 00:10 local 12 November, after midnight) and P4 2026-11-11T18:20Z (23:50 local 11 November, before midnight), when staff pick Last 7 days (local 12 to 18 November), then the count includes P3, not P4."
      - id: AC3
        text: "Given today is 18 November local, P5 has created_at 2026-10-19T18:40Z (Asia/Kolkata = UTC+05:30, so 00:10 local 20 October, after midnight) and P6 2026-10-19T18:20Z (23:50 local 19 October, before midnight), when staff pick Last 30 days (local 20 October to 18 November), then the count includes P5, not P6."
      - id: AC4
        text: "Given the period is Last 30 days, when staff change the period back to Today, then the New patients count shows only patients created on today's local date."
  - id: S-3
    title: Open the patients registered in the period from the tile
    trace: [REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given the period is Today on 18 November, P1 has created_at 2026-11-17T19:00Z (Asia/Kolkata = UTC+05:30, so 00:30 local 18 November, after midnight) and P2 2026-11-17T18:00Z (23:30 local 17 November, before midnight), when staff select the New patients tile, then the list includes P1, not P2, and its total equals the tile's count."
      - id: AC2
        text: "Given the patient list was last set to show only Active patients and an Inactive patient was created today, when staff select the New patients tile with the period Today, then the list shows all statuses and includes the Inactive patient."
      - id: AC3
        text: "Given staff typed a search on the patient list that matches only one of today's new patients, when they select the New patients tile with the period Today, then the search is cleared and the list shows every patient the tile counts."
      - id: AC4
        text: "Given 21 patients were created today, when staff select the New patients tile with the period Today, then the first page shows 20 patients and the 21st patient is on the second page."
      - id: AC5
        text: "Given staff chose a sort order on the patient list other than its default, when they select the New patients tile, then the filtered list keeps that sort order."
  - id: S-4
    title: The rest of the dashboard stays as it is
    trace: [REQ-12, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given an Inactive patient was created today, when staff open the dashboard, then the Total patients tile shows the same count as before this change."
      - id: AC2
        text: "Given no patient was created in the period, when staff open the dashboard, then the Total patients tile shows the same count as before this change."
      - id: AC3
        text: "Given Completed and Cancelled appointments and an upcoming Scheduled appointment exist, when staff open the dashboard, then the Completed and Cancelled tiles and the next upcoming appointments are the same as before this change."
      - id: AC4
        text: "Given Active and Inactive doctors exist and a patient was created today, when staff open the dashboard, then the tiles that count doctors show the same counts as before this change."
  - id: S-5
    title: The dashboard stays free of personal data
    trace: [REQ-14, REQ-15]
    acceptance_criteria:
      - id: AC1
        text: "Given the patient created most recently today has allergies and a current medication recorded, when staff open the dashboard, then neither the dashboard page nor its data response contains any medical history field."
      - id: AC2
        text: "Given the patient created most recently today has a phone number and an emergency contact recorded, when staff open the dashboard, then neither the dashboard page nor its data response contains any contact details."
---
# Stories: HMS-22

## Resolves
- F-1: S-2 AC2 and AC3 now test the first local day of Last 7 days and Last 30 days on both sides of midnight.
- F-2: S-3 AC1 now checks the list filter on both sides of local midnight in Asia/Kolkata.
- F-3: S-4 AC4 now covers the doctor tiles, and the personal-data criteria moved to new story S-5.

## Stories

### S-1 See how many patients were registered in the period
Traces: REQ-1, REQ-2, REQ-3, REQ-4, REQ-11.
- AC1: Given today is 18 November in Asia/Kolkata and the period is Today, when staff open the dashboard, then a tile labelled New patients is shown.
- AC2: Given the period is Today on 18 November and patient P1 has created_at 2026-11-17T19:00Z (Asia/Kolkata = UTC+05:30, so 00:30 local on 18 November, after local midnight) and patient P2 has created_at 2026-11-17T18:00Z (23:30 local on 17 November, before local midnight), when staff open the dashboard, then the New patients tile counts P1 and not P2.
- AC3: Given the period is Today and two patients were created today, one Active and one Inactive, when staff open the dashboard, then the New patients tile counts both of them.
- AC4: Given the period is Today and patients exist but none has a created_at on today's local date, when staff open the dashboard, then the New patients tile shows 0.

### S-2 The count follows the period selector
Traces: REQ-5.
- AC1: Given the period is Today, when staff change the period to Last 7 days, then the New patients count updates the same way the Completed and Cancelled tiles do.
- AC2: Given today is 18 November local, P3 has created_at 2026-11-11T18:40Z (Asia/Kolkata = UTC+05:30, so 00:10 local 12 November, after midnight) and P4 2026-11-11T18:20Z (23:50 local 11 November, before midnight), when staff pick Last 7 days (local 12 to 18 November), then the count includes P3, not P4.
- AC3: Given today is 18 November local, P5 has created_at 2026-10-19T18:40Z (Asia/Kolkata = UTC+05:30, so 00:10 local 20 October, after midnight) and P6 2026-10-19T18:20Z (23:50 local 19 October, before midnight), when staff pick Last 30 days (local 20 October to 18 November), then the count includes P5, not P6.
- AC4: Given the period is Last 30 days, when staff change the period back to Today, then the New patients count shows only patients created on today's local date.

### S-3 Open the patients registered in the period from the tile
Traces: REQ-6, REQ-7, REQ-8, REQ-9, REQ-10.
- AC1: Given the period is Today on 18 November, P1 has created_at 2026-11-17T19:00Z (Asia/Kolkata = UTC+05:30, so 00:30 local 18 November, after midnight) and P2 2026-11-17T18:00Z (23:30 local 17 November, before midnight), when staff select the New patients tile, then the list includes P1, not P2, and its total equals the tile's count.
- AC2: Given the patient list was last set to show only Active patients and an Inactive patient was created today, when staff select the New patients tile with the period Today, then the list shows all statuses and includes the Inactive patient.
- AC3: Given staff typed a search on the patient list that matches only one of today's new patients, when they select the New patients tile with the period Today, then the search is cleared and the list shows every patient the tile counts.
- AC4: Given 21 patients were created today, when staff select the New patients tile with the period Today, then the first page shows 20 patients and the 21st patient is on the second page.
- AC5: Given staff chose a sort order on the patient list other than its default, when they select the New patients tile, then the filtered list keeps that sort order.

### S-4 The rest of the dashboard stays as it is
Traces: REQ-12, REQ-13.
- AC1: Given an Inactive patient was created today, when staff open the dashboard, then the Total patients tile shows the same count as before this change.
- AC2: Given no patient was created in the period, when staff open the dashboard, then the Total patients tile shows the same count as before this change.
- AC3: Given Completed and Cancelled appointments and an upcoming Scheduled appointment exist, when staff open the dashboard, then the Completed and Cancelled tiles and the next upcoming appointments are the same as before this change.
- AC4: Given Active and Inactive doctors exist and a patient was created today, when staff open the dashboard, then the tiles that count doctors show the same counts as before this change.

### S-5 The dashboard stays free of personal data
Traces: REQ-14, REQ-15.
- AC1: Given the patient created most recently today has allergies and a current medication recorded, when staff open the dashboard, then neither the dashboard page nor its data response contains any medical history field.
- AC2: Given the patient created most recently today has a phone number and an emergency contact recorded, when staff open the dashboard, then neither the dashboard page nor its data response contains any contact details.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3, REQ-4 and REQ-11. "Registered" means `created_at` (C2), and the period uses `Asia/Kolkata` days (C3).
- S-2: built from known REQ-5; period options and the whole-local-days rule come from the glossary (C4).
- S-3: built from known REQ-6 to REQ-10. The list uses all statuses (C5), clears a typed search (C7) and keeps `20 per page` (C6).
- S-4: built from known REQ-12 and REQ-13. The dashboard also counts doctors (C9).
- S-5: built from known REQ-14 and REQ-15. Medical history fields come from the glossary (C8).

## Assumed
None.

## Unknown
None.

## Open questions
None.

---
schema: stories.v1
run_id: run_01M7PX5XB0C1P9DTAW82D9MDVX
step: split
attempt: 1
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-22/run_01M7PX5XB0C1P9DTAW82D9MDVX/req.md
    sha256: cce290751712fe7cab482e213a87e79b5617807ebafe66bc494ea520b72f724a
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
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
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [S-1, S-2, S-3, S-4]
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
        text: "Given today is 18 November local, patient P3 was created on 15 November local and the period is Today, when staff change the period to Last 7 days, then the New patients count goes up to include P3 without reloading the page, the same way the Completed and Cancelled tiles update."
      - id: AC2
        text: "Given today is 18 November local, patient P4 was created on 10 October local (before the first day of Last 30 days, 20 October), and the period is Last 7 days, when staff change the period to Last 30 days, then the New patients count does not include P4."
      - id: AC3
        text: "Given the period is Last 30 days, when staff change the period back to Today, then the New patients count shows only patients created on today's local date."
  - id: S-3
    title: Open the patients registered in the period from the tile
    trace: [REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given the period is Last 7 days and patients exist that were created before that period, when staff select the New patients tile, then the patient list opens filtered to the patients whose created_at falls in that period, and its total equals the tile's count."
      - id: AC2
        text: "Given the patient list was last set to show only Active patients and an Inactive patient was created today, when staff select the New patients tile with the period Today, then the list shows all statuses and includes the Inactive patient."
      - id: AC3
        text: "Given staff typed a search on the patient list that matches only one of today's new patients, when they select the New patients tile with the period Today, then the search is cleared and the list shows every patient the tile counts."
      - id: AC4
        text: "Given 21 patients were created today, when staff select the New patients tile with the period Today, then the first page shows 20 patients and the 21st patient is on the second page."
      - id: AC5
        text: "Given staff chose a sort order on the patient list other than its default, when they select the New patients tile, then the filtered list keeps that sort order."
  - id: S-4
    title: The rest of the dashboard stays as it is and stays free of personal data
    trace: [REQ-12, REQ-13, REQ-14, REQ-15]
    acceptance_criteria:
      - id: AC1
        text: "Given an Inactive patient was created today, when staff open the dashboard, then the Total patients tile shows the same count as before this change."
      - id: AC2
        text: "Given no patient was created in the period, when staff open the dashboard, then the Total patients tile shows the same count as before this change."
      - id: AC3
        text: "Given Completed and Cancelled appointments and an upcoming Scheduled appointment exist, when staff open the dashboard, then the Completed and Cancelled tiles and the next upcoming appointments are the same as before this change."
      - id: AC4
        text: "Given the patient created most recently today has allergies and a current medication recorded, when staff open the dashboard, then neither the dashboard page nor its data response contains any medical history field."
      - id: AC5
        text: "Given the patient created most recently today has a phone number and an emergency contact recorded, when staff open the dashboard, then neither the dashboard page nor its data response contains any contact details."
---
# Stories: HMS-22

## Stories

### S-1 See how many patients were registered in the period
Traces: REQ-1, REQ-2, REQ-3, REQ-4, REQ-11.
- AC1: Given today is 18 November in Asia/Kolkata and the period is Today, when staff open the dashboard, then a tile labelled New patients is shown.
- AC2: Given the period is Today on 18 November and patient P1 has created_at 2026-11-17T19:00Z (Asia/Kolkata = UTC+05:30, so 00:30 local on 18 November, after local midnight) and patient P2 has created_at 2026-11-17T18:00Z (23:30 local on 17 November, before local midnight), when staff open the dashboard, then the New patients tile counts P1 and not P2.
- AC3: Given the period is Today and two patients were created today, one Active and one Inactive, when staff open the dashboard, then the New patients tile counts both of them.
- AC4: Given the period is Today and patients exist but none has a created_at on today's local date, when staff open the dashboard, then the New patients tile shows 0.

### S-2 The count follows the period selector
Traces: REQ-5.
- AC1: Given today is 18 November local, patient P3 was created on 15 November local and the period is Today, when staff change the period to Last 7 days, then the New patients count goes up to include P3 without reloading the page, the same way the Completed and Cancelled tiles update.
- AC2: Given today is 18 November local, patient P4 was created on 10 October local (before the first day of Last 30 days, 20 October), and the period is Last 7 days, when staff change the period to Last 30 days, then the New patients count does not include P4.
- AC3: Given the period is Last 30 days, when staff change the period back to Today, then the New patients count shows only patients created on today's local date.

### S-3 Open the patients registered in the period from the tile
Traces: REQ-6, REQ-7, REQ-8, REQ-9, REQ-10.
- AC1: Given the period is Last 7 days and patients exist that were created before that period, when staff select the New patients tile, then the patient list opens filtered to the patients whose created_at falls in that period, and its total equals the tile's count.
- AC2: Given the patient list was last set to show only Active patients and an Inactive patient was created today, when staff select the New patients tile with the period Today, then the list shows all statuses and includes the Inactive patient.
- AC3: Given staff typed a search on the patient list that matches only one of today's new patients, when they select the New patients tile with the period Today, then the search is cleared and the list shows every patient the tile counts.
- AC4: Given 21 patients were created today, when staff select the New patients tile with the period Today, then the first page shows 20 patients and the 21st patient is on the second page.
- AC5: Given staff chose a sort order on the patient list other than its default, when they select the New patients tile, then the filtered list keeps that sort order.

### S-4 The rest of the dashboard stays as it is and stays free of personal data
Traces: REQ-12, REQ-13, REQ-14, REQ-15.
- AC1: Given an Inactive patient was created today, when staff open the dashboard, then the Total patients tile shows the same count as before this change.
- AC2: Given no patient was created in the period, when staff open the dashboard, then the Total patients tile shows the same count as before this change.
- AC3: Given Completed and Cancelled appointments and an upcoming Scheduled appointment exist, when staff open the dashboard, then the Completed and Cancelled tiles and the next upcoming appointments are the same as before this change.
- AC4: Given the patient created most recently today has allergies and a current medication recorded, when staff open the dashboard, then neither the dashboard page nor its data response contains any medical history field.
- AC5: Given the patient created most recently today has a phone number and an emergency contact recorded, when staff open the dashboard, then neither the dashboard page nor its data response contains any contact details.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3, REQ-4 and REQ-11. "Registered" means `created_at` (C2), and the period uses `Asia/Kolkata` days (C3).
- S-2: built from known REQ-5; period options from the glossary (C4).
- S-3: built from known REQ-6 to REQ-10. The list uses all statuses (C5), clears a typed search (C7) and keeps `20 per page` (C6).
- S-4: built from known REQ-12 to REQ-15. Medical history fields come from the glossary (C8).

## Assumed
None.

## Unknown
None.

## Open questions
None.

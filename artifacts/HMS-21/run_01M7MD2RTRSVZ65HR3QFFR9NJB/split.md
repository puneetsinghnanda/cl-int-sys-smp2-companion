---
schema: stories.v1
run_id: run_01M7MD2RTRSVZ65HR3QFFR9NJB
step: split
attempt: 2
skill: story-split@1.3.0
inputs:
  - {path: artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md, sha256: cdf917be0f8dfa8f1b3a5b08603bd0a50957b5f840e47e335b5c6a52a24591fc}
  - {path: context/glossary.md, sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
claims:
  - {id: C1, text: "The dashboard has a tile `Awaiting outcome`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-1"]}
  - {id: C2, text: "The tile counts Scheduled appointments whose `start time is before now`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-2"]}
  - {id: C3, text: "`Completed, Cancelled and No-show` appointments are never counted.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-3"]}
  - {id: C4, text: "Scheduled appointments that have not `started yet` are never counted.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-4"]}
  - {id: C5, text: "The count does not change with the `period selector`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-5"]}
  - {id: C6, text: "The tile links to the appointment list showing `exactly the appointments it counts`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-6"]}
  - {id: C7, text: "With no such appointments the tile `shows 0`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-7"]}
  - {id: C8, text: "The `other tiles` of the dashboard are unchanged.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-8"]}
  - {id: C9, text: "The list of the `next 10 upcoming appointments` is unchanged.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-9"]}
  - {id: C10, text: "`No medical history` appears on the tile or the dashboard.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-10"]}
  - {id: C11, text: "No `contact details` appear on the tile or the dashboard.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-11"]}
  - {id: C12, text: "The Period choices are Today, Last 7 days or `Last 30 days`.", cites: ["context/glossary.md#period"]}
  - {id: C13, text: "Selecting a tile `opens the list behind the count`.", cites: ["context/glossary.md#tile"]}
  - {id: C14, text: "Medical history appears `never in lists, the dashboard or logs`.", cites: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#claim:C7"]}
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "May contact details (phone, email) appear in the appointment list that the Awaiting outcome tile opens, or should the no-contact-details rule also cover that list?"
    why: "REQ-11 forbids contact details on the tile and the dashboard only; the list the tile opens is not named, so S-4 AC5 checks the tile and dashboard only."
    evidence: ["artifacts/HMS-21/run_01M7MD2RTRSVZ65HR3QFFR9NJB/req.md#item:REQ-11", "context/glossary.md#tile"]
    options: ["Tile and dashboard only, as REQ-11 states", "Also the appointment list the tile opens"]
    default: "Tile and dashboard only, as REQ-11 states"
resolves:
  - {id: F-1, how: "S-1 AC3 now adds a Scheduled appointment that starts at exactly 10:00 when it is 10:00, and it is not counted."}
  - {id: F-2, how: "The contact-details question is now this artifact's own non-blocking question Q-01 for the product owner."}
stories:
  - id: S-1
    title: "See how many past appointments are still awaiting an outcome"
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-7]
    acceptance_criteria:
      - {id: AC1, text: "Given it is 10:00 hospital time and two Scheduled appointments started today at 09:00 and 09:30, when staff open the dashboard, then a tile labelled Awaiting outcome shows 2."}
      - {id: AC2, text: "Given it is 10:00 and one Scheduled appointment started at 09:00, while a Completed one started at 09:15, a No-show one at 09:30 and a Cancelled one at 09:45 (the most recent of all), when staff open the dashboard, then the Awaiting outcome tile shows 1."}
      - {id: AC3, text: "Given it is exactly 10:00, one Scheduled appointment started at 09:00, another Scheduled appointment starts at exactly 10:00 and a third starts later today at 10:30, when staff open the dashboard, then the Awaiting outcome tile shows 1, because a start at 10:00 is not before now and the 10:30 one has not started yet."}
      - {id: AC4, text: "Given it is 10:00 and the only Scheduled appointments start at 10:30 today and tomorrow, and all earlier appointments are Completed, Cancelled or No-show, when staff open the dashboard, then the Awaiting outcome tile shows 0."}
      - {id: AC5, text: "Given the Awaiting outcome tile shows 2, when staff mark one of the two appointments Completed and open the dashboard again, then the tile shows 1."}
  - id: S-2
    title: "The Awaiting outcome count ignores the period"
    trace: [REQ-5]
    acceptance_criteria:
      - {id: AC1, text: "Given today is 19 October 2026 at 10:00, one Scheduled appointment started on 1 August 2026 (outside Last 30 days) and another started today at 09:00, when staff switch the period between Today, Last 7 days and Last 30 days, then the Awaiting outcome tile shows 2 for each period."}
  - id: S-3
    title: "Open the appointments behind the Awaiting outcome count"
    trace: [REQ-6]
    acceptance_criteria:
      - {id: AC1, text: "Given it is 10:00, two Scheduled appointments started at 09:00 and 09:30, a Scheduled one starts at 10:30 and a Completed one started at 09:15, when staff select the Awaiting outcome tile, then the appointment list shows exactly the 09:00 and 09:30 appointments."}
      - {id: AC2, text: "Given today is 19 October 2026, the period is Today and one Scheduled appointment started on 1 August 2026, when staff select the Awaiting outcome tile, then the list includes the 1 August appointment, because the list applies no period."}
      - {id: AC3, text: "Given the Awaiting outcome tile shows 0, when staff select it, then the appointment list shows no appointments."}
  - id: S-4
    title: "The rest of the dashboard is unchanged and shows no private data"
    trace: [REQ-8, REQ-9, REQ-10, REQ-11]
    acceptance_criteria:
      - {id: AC1, text: "Given it is 10:00, a Scheduled appointment started at 09:00 and another Scheduled appointment starts at 10:30, when staff open the dashboard with each period, then every tile other than Awaiting outcome shows the same count as before the change."}
      - {id: AC2, text: "Given the same appointments, when staff open the dashboard, then the next 10 upcoming appointments list shows the 10:30 appointment and not the 09:00 one, exactly as before the change."}
      - {id: AC3, text: "Given no Scheduled appointment starts after now, when staff open the dashboard, then the upcoming appointments area shows the same content as before the change."}
      - {id: AC4, text: "Given a counted appointment belongs to a patient with allergies and notes recorded, when staff open the dashboard and then the list behind the Awaiting outcome tile, then no medical history appears on either."}
      - {id: AC5, text: "Given a counted appointment belongs to a patient with a phone number and email recorded, when staff open the dashboard, then no contact details appear on the tile or anywhere on the dashboard."}
---
# Stories: HMS-21

## Stories
**S-1 See how many past appointments are still awaiting an outcome** (REQ-1, REQ-2, REQ-3, REQ-4, REQ-7)
- AC1: Given it is 10:00 hospital time and two Scheduled appointments started today at 09:00 and 09:30, when staff open the dashboard, then a tile labelled Awaiting outcome shows 2.
- AC2: Given it is 10:00 and one Scheduled appointment started at 09:00, while a Completed one started at 09:15, a No-show one at 09:30 and a Cancelled one at 09:45 (the most recent of all), when staff open the dashboard, then the Awaiting outcome tile shows 1.
- AC3: Given it is exactly 10:00, one Scheduled appointment started at 09:00, another Scheduled appointment starts at exactly 10:00 and a third starts later today at 10:30, when staff open the dashboard, then the Awaiting outcome tile shows 1, because a start at 10:00 is not before now and the 10:30 one has not started yet.
- AC4: Given it is 10:00 and the only Scheduled appointments start at 10:30 today and tomorrow, and all earlier appointments are Completed, Cancelled or No-show, when staff open the dashboard, then the Awaiting outcome tile shows 0.
- AC5: Given the Awaiting outcome tile shows 2, when staff mark one of the two appointments Completed and open the dashboard again, then the tile shows 1.

**S-2 The Awaiting outcome count ignores the period** (REQ-5)
- AC1: Given today is 19 October 2026 at 10:00, one Scheduled appointment started on 1 August 2026 (outside Last 30 days) and another started today at 09:00, when staff switch the period between Today, Last 7 days and Last 30 days, then the Awaiting outcome tile shows 2 for each period.

**S-3 Open the appointments behind the Awaiting outcome count** (REQ-6)
- AC1: Given it is 10:00, two Scheduled appointments started at 09:00 and 09:30, a Scheduled one starts at 10:30 and a Completed one started at 09:15, when staff select the Awaiting outcome tile, then the appointment list shows exactly the 09:00 and 09:30 appointments.
- AC2: Given today is 19 October 2026, the period is Today and one Scheduled appointment started on 1 August 2026, when staff select the Awaiting outcome tile, then the list includes the 1 August appointment, because the list applies no period.
- AC3: Given the Awaiting outcome tile shows 0, when staff select it, then the appointment list shows no appointments.

**S-4 The rest of the dashboard is unchanged and shows no private data** (REQ-8, REQ-9, REQ-10, REQ-11)
- AC1: Given it is 10:00, a Scheduled appointment started at 09:00 and another Scheduled appointment starts at 10:30, when staff open the dashboard with each period, then every tile other than Awaiting outcome shows the same count as before the change.
- AC2: Given the same appointments, when staff open the dashboard, then the next 10 upcoming appointments list shows the 10:30 appointment and not the 09:00 one, exactly as before the change.
- AC3: Given no Scheduled appointment starts after now, when staff open the dashboard, then the upcoming appointments area shows the same content as before the change.
- AC4: Given a counted appointment belongs to a patient with allergies and notes recorded, when staff open the dashboard and then the list behind the Awaiting outcome tile, then no medical history appears on either.
- AC5: Given a counted appointment belongs to a patient with a phone number and email recorded, when staff open the dashboard, then no contact details appear on the tile or anywhere on the dashboard.

Times are hospital-local clock times on one day unless a date is given. The count compares start instants with now, so no AC needs a local date that differs from its UTC date.

Retry fixes:
- F-1: S-1 AC3 now adds a Scheduled appointment that starts at exactly 10:00 when it is 10:00, and it is not counted.
- F-2: The contact-details question is now this artifact's own non-blocking question Q-01 for the product owner.
- I also moved the extra sentence out of the body copy of S-4 AC4, so both copies match; its rationale is under Known.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3, REQ-4 and REQ-7 (C1 to C4, C7). AC2 puts the Cancelled appointment latest, so a count that takes the most recent status still fails it. AC3 puts one Scheduled appointment at exactly now and one later the same day, so a count that uses "at or before now", or a date instead of the start time, fails it.
- S-2: built from known REQ-5 (C5). The 1 August appointment lies outside Last 30 days (C12), so a count that applied the period would differ by period.
- S-3: built from known REQ-6 (C6, C13). AC2 checks the list applies no period, matching the count.
- S-4: built from known REQ-8 to REQ-11 (C8 to C11). AC1 to AC3 cover the partitions with and without an upcoming appointment. AC4 extends the medical-history check to the list the tile opens, because medical history never appears in lists or on the dashboard (C14). AC5 checks the tile and dashboard, as REQ-11 states; Q-01 asks whether it should also cover the list.

## Assumed
None.

## Unknown
None. Every known REQ is traced by a story, and the intent has no unknowns.

## Open questions
- Q-01 (non-blocking, product owner): may contact details (phone, email) appear in the appointment list the Awaiting outcome tile opens, or should the no-contact-details rule also cover that list? Default: tile and dashboard only, as REQ-11 states. Until it is answered, S-4 AC5 checks the tile and dashboard only.

---
schema: stories.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
step: split
attempt: 2
skill: story-split@1.0.1
inputs:
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md
    sha256: e04911f106dc8dc1dd0ed9c0dfdc17bb61a93e72095ba3a354e1a994ab185ffb
  - path: context/glossary.md
    sha256: 7623345ad8bad33b8a14a713b452edb2a36834e8b1c2228b76f3aa154034f16e
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
resolves:
  - id: F-1
    how: "S-1 AC4 now states an observable outcome: when every appointment starting today belongs to one doctor, that doctor's Today count equals the dashboard's Appointments today tile value. It avoids summing across doctors, which the intent review (req a1 F-1) flagged as depending on unstated scope."
claims:
  - id: C1
    text: "A Cancelled appointment is one `called off` before it took place."
    cites: ["context/glossary.md#cancelled"]
  - id: C2
    text: "Selecting a dashboard tile `opens the list behind the count`."
    cites: ["context/glossary.md#tile"]
  - id: C3
    text: "Front-desk staff book appointments; in the MVP every user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [S-1, S-2]
assumed: [S-3]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
  - {id: U-3, blocking: false}
  - {id: U-4, blocking: false}
  - {id: U-5, blocking: false}
  - {id: U-6, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: product owner
    text: "Does 'walk-in' imply any new booking behaviour, or is it only the motivation for the Today column?"
    why: "A walk-in flow would need its own story."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L135"]
    options: ["Motivation only; no new behaviour", "New walk-in booking flow (separate ticket)"]
    default: "Motivation only; no new behaviour"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is the 'doctors list' the existing doctors list page described by PRD DOC-2?"
    why: "S-1 and S-3 change that screen."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L143"]
    options: ["Existing doctors list (PRD DOC-2)", "Another view"]
    default: "Existing doctors list (PRD DOC-2)"
  - id: Q-03
    blocking: false
    ask: tech lead
    text: "Does the existing appointment list support filtering by doctor and by a single day? If not, may this change add those filters?"
    why: "S-2 needs a link target that applies both filters."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L151"]
    options: ["Filters exist; link to them", "Add the missing filter(s) in this change"]
    default: "Add the missing filter(s) in this change"
  - id: Q-04
    blocking: false
    ask: tech lead
    text: "Confirm 'today' is the calendar day in the hospital time zone Asia/Kolkata."
    why: "Day boundaries decide which appointments S-1 counts."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L159"]
    options: ["Asia/Kolkata calendar day", "Server or browser local day"]
    default: "Asia/Kolkata calendar day"
  - id: Q-05
    blocking: false
    ask: tech lead
    text: "Does the dashboard's 'Appointments today' tile count every status except Cancelled? If the tile differs, which rule wins?"
    why: "S-1 AC3 and AC4 must agree."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L167"]
    options: ["Tile already matches; reuse its rule", "Tile differs; follow the ticket and raise a separate fix"]
    default: "Tile differs; follow the ticket and raise a separate fix"
  - id: Q-06
    blocking: false
    ask: product owner
    text: "Should a count of 0 also be a link to the (empty) filtered appointment list?"
    why: "Decides whether S-2 applies to rows showing 0."
    evidence: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/req.md#L175"]
    options: ["Every count links, including 0", "0 is plain text"]
    default: "Every count links, including 0"
stories:
  - id: S-1
    title: See each doctor's appointment count for today
    trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given front-desk staff open the doctors list, when the list loads, then it shows a column labelled \"Today\"."
      - id: AC2
        text: "Given a doctor has appointments whose start falls on the current day in Asia/Kolkata and others on other days, when the doctors list loads, then that doctor's Today cell shows the number of the current-day appointments only."
      - id: AC3
        text: "Given a doctor has appointments starting today in Asia/Kolkata in several statuses including Cancelled, when the doctors list loads, then every appointment except the Cancelled ones is counted."
      - id: AC4
        text: "Given every appointment starting today in Asia/Kolkata belongs to one doctor, in several statuses including Cancelled, when staff open the doctors list and the dashboard, then that doctor's Today count equals the dashboard's \"Appointments today\" tile value."
      - id: AC5
        text: "Given a doctor with no counted appointments today, when the doctors list loads, then that doctor's Today cell shows 0."
  - id: S-2
    title: Open a doctor's appointments for today from the count
    trace: [REQ-6, REQ-7]
    acceptance_criteria:
      - id: AC1
        text: "Given the doctors list shows a Today count for a doctor, when staff select the count, then the appointment list opens filtered to that doctor."
      - id: AC2
        text: "Given the doctors list shows a Today count for a doctor, when staff select the count, then the appointment list opened is filtered to today in Asia/Kolkata."
  - id: S-3
    title: Doctors list keeps its current behaviour
    trace: [REQ-8, REQ-9, REQ-10, REQ-11]
    acceptance_criteria:
      - id: AC1
        text: "Given more than 20 doctors, when staff open the doctors list, then it shows 20 per page."
      - id: AC2
        text: "Given several doctors, when staff open the doctors list, then the rows are sorted by name."
      - id: AC3
        text: "Given staff apply the list's current filters, when the list reloads, then the same doctors are shown as before this change."
      - id: AC4
        text: "Given staff use the list's current search, when the list reloads, then the same doctors are shown as before this change."
      - id: AC5
        text: "Given the doctors list loads, when staff view a row, then code, name, specialization, department, fee and status are still shown alongside Today."
---
# Stories: HMS-6

## Stories

**S-1 See each doctor's appointment count for today**, tracing REQ-1, REQ-2, REQ-3, REQ-4, REQ-5.
- AC1: Given front-desk staff open the doctors list, when the list loads, then it shows a column labelled "Today".
- AC2: Given a doctor has appointments whose start falls on the current day in Asia/Kolkata and others on other days, when the doctors list loads, then that doctor's Today cell shows the number of the current-day appointments only.
- AC3: Given a doctor has appointments starting today in Asia/Kolkata in several statuses including Cancelled, when the doctors list loads, then every appointment except the Cancelled ones is counted.
- AC4: Given every appointment starting today in Asia/Kolkata belongs to one doctor, in several statuses including Cancelled, when staff open the doctors list and the dashboard, then that doctor's Today count equals the dashboard's "Appointments today" tile value.
- AC5: Given a doctor with no counted appointments today, when the doctors list loads, then that doctor's Today cell shows 0.

**S-2 Open a doctor's appointments for today from the count**, tracing REQ-6, REQ-7.
- AC1: Given the doctors list shows a Today count for a doctor, when staff select the count, then the appointment list opens filtered to that doctor.
- AC2: Given the doctors list shows a Today count for a doctor, when staff select the count, then the appointment list opened is filtered to today in Asia/Kolkata.

**S-3 Doctors list keeps its current behaviour**, tracing REQ-8, REQ-9, REQ-10, REQ-11.
- AC1: Given more than 20 doctors, when staff open the doctors list, then it shows 20 per page.
- AC2: Given several doctors, when staff open the doctors list, then the rows are sorted by name.
- AC3: Given staff apply the list's current filters, when the list reloads, then the same doctors are shown as before this change.
- AC4: Given staff use the list's current search, when the list reloads, then the same doctors are shown as before this change.
- AC5: Given the doctors list loads, when staff view a row, then code, name, specialization, department, fee and status are still shown alongside Today.

## Resolved feedback
- F-1: S-1 AC4 now compares numbers a tester can see (one doctor's Today count against the dashboard tile) instead of naming an internal rule.

## Known
- S-1: built only from known REQ-1 to REQ-5 (ticket acceptance criteria 1 to 3).
- S-2: built only from known REQ-6 and REQ-7 (ticket acceptance criterion 4).

## Assumed
- S-3: relies on assumed REQ-11 (existing columns kept), whose basis is that the ticket lists them as current and asks only to add one column; REQ-8 to REQ-10 are known.

## Unknown
- U-1 (non-blocking): "walk-in" not in the glossary → Q-01.
- U-2 (non-blocking): "doctors list" not in the glossary → Q-02.
- U-3 (non-blocking): whether the appointment list can filter by doctor and day → Q-03; affects S-2.
- U-4 (non-blocking): "hospital time zone" not in the glossary → Q-04; affects S-1 and S-2.
- U-5 (non-blocking): the tile's counting rule is not confirmed → Q-05; affects S-1 AC3 and AC4.
- U-6 (non-blocking): whether a 0 count links → Q-06; S-2 applies to every count under the Q-06 default.

## Open questions
- Q-01 (non-blocking, product owner): does "walk-in" imply new behaviour? Default: motivation only.
- Q-02 (non-blocking, tech lead): is this the existing DOC-2 doctors list? Default: yes.
- Q-03 (non-blocking, tech lead): can the appointment list filter by doctor and day? Default: add the missing filter(s) in this change.
- Q-04 (non-blocking, tech lead): is "today" the Asia/Kolkata calendar day? Default: yes.
- Q-05 (non-blocking, tech lead): does the tile exclude only Cancelled? Default: if it differs, follow the ticket and raise a separate fix.
- Q-06 (non-blocking, product owner): does a 0 count link? Default: every count links, including 0.

---
schema: stories.v1
run_id: run_01M81PTAY0WBBGAGVYF6QV4PCW
step: split
attempt: 1
skill: story-split@1.4.0
inputs:
  - path: artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/req.md
    sha256: 35b81b83af2e9a6dfa955d70288cccd1ced2f5904e2cd3b03a6218dd1e9703c7
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "The doctor list (`DOC-2`) shows code, name, specialization, department, fee and status."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/req.md#claim:C1"]
  - id: C2
    text: "The doctor list already has a `Today` column after Status."
    cites: ["artifacts/HMS-28/run_01M81PTAY0WBBGAGVYF6QV4PCW/req.md#claim:C7"]
  - id: C3
    text: "An availability block is a weekly window with a `weekday`, start time and end time."
    cites: ["context/glossary.md#availability-block"]
  - id: C4
    text: "An Inactive doctor is `kept for history` but not offered for new use."
    cites: ["context/glossary.md#inactive"]
  - id: C5
    text: "In the MVP every front-desk user acts as `staff`."
    cites: ["context/glossary.md#front-desk-staff"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15]
known: [S-1, S-2, S-3, S-4, S-5]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: See which weekdays each doctor works on the doctor list
    trace: [REQ-1, REQ-2, REQ-3, REQ-11, REQ-13]
    acceptance_criteria:
      - id: AC1
        text: "Given a doctor whose availability blocks were entered for Saturday, then Monday, then Wednesday, when front-desk staff open the doctor list, then that doctor's Works on column shows \"Mon, Wed, Sat\" (days in the order Mon, Tue, Wed, Thu, Fri, Sat, Sun, not in entry order)."
      - id: AC2
        text: "Given a doctor with two availability blocks on Tuesday (one in the morning, one in the afternoon) and none on other days, when front-desk staff open the doctor list, then that doctor's Works on column shows \"Tue\" once."
      - id: AC3
        text: "Given a doctor with no availability blocks, when front-desk staff open the doctor list, then that doctor's Works on column shows \"No availability\"."
      - id: AC4
        text: "Given an Inactive doctor with an availability block on Sunday, when front-desk staff open the doctor list with status \"Any status\", then that doctor's Works on column shows \"Sun\"."
      - id: AC5
        text: "Given the doctor list is open, when front-desk staff look at the column headers, then the Works on column is after Status and before Today."
  - id: S-2
    title: Filter the doctor list to one weekday
    trace: [REQ-4, REQ-5, REQ-6, REQ-14]
    acceptance_criteria:
      - id: AC1
        text: "Given front-desk staff open the doctor list without choosing a weekday, when they look at the Works on filter, then it shows \"Any day\" and the list includes a doctor with no availability blocks."
      - id: AC2
        text: "Given the doctor list is open, when front-desk staff open the Works on filter, then its options read \"Any day\", \"Monday\", \"Tuesday\", \"Wednesday\", \"Thursday\", \"Friday\", \"Saturday\", \"Sunday\"."
      - id: AC3
        text: "Given doctor A has an availability block on Saturday and doctor B has blocks only on Friday, when front-desk staff choose \"Saturday\" in the Works on filter, then doctor A is listed and doctor B is not."
      - id: AC4
        text: "Given doctor C has no availability blocks, when front-desk staff choose \"Saturday\" in the Works on filter, then doctor C is not listed."
      - id: AC5
        text: "Given front-desk staff chose \"Saturday\" in the Works on filter, when they choose \"Any day\" again, then doctor B (blocks only on Friday) and doctor C (no blocks) are listed again."
  - id: S-3
    title: Combine the weekday filter with search, filters and paging
    trace: [REQ-7, REQ-8, REQ-9, REQ-11, REQ-12]
    acceptance_criteria:
      - id: AC1
        text: "Given doctor D in Cardiology and doctor E in another department both have an availability block on Saturday, when front-desk staff choose \"Saturday\" and department Cardiology, then doctor D is listed and doctor E is not."
      - id: AC2
        text: "Given two doctors with an availability block on Saturday, only one of whose names matches the search text, when front-desk staff search that text and choose \"Saturday\", then only the matching doctor is listed."
      - id: AC3
        text: "Given an Active doctor and an Inactive doctor both have an availability block on Saturday, when front-desk staff choose \"Saturday\" and status Active, then only the Active doctor is listed; with status \"Any status\" both are listed."
      - id: AC4
        text: "Given a 21st doctor by name has an availability block on Saturday (one more than fit on a page at 20 per page), when front-desk staff choose \"Saturday\", then page 1 shows the first 20 such doctors sorted by name and page 2 shows the 21st."
      - id: AC5
        text: "Given the whole doctor list fills more than one page but only three doctors have an availability block on Saturday, when front-desk staff choose \"Saturday\", then the total shows three doctors and there is a single page."
  - id: S-4
    title: Keep the chosen weekday in the page address
    trace: [REQ-10]
    acceptance_criteria:
      - id: AC1
        text: "Given front-desk staff chose \"Saturday\" in the Works on filter, when they copy the page address and open it in a new tab, then the Works on filter shows \"Saturday\" and the same doctors are listed."
      - id: AC2
        text: "Given front-desk staff chose \"Saturday\" and opened a doctor's own page from the list, when they go back, then the Works on filter still shows \"Saturday\" and the list is still filtered to Saturday."
  - id: S-5
    title: The rest of the doctor list keeps working as before
    trace: [REQ-12, REQ-15]
    acceptance_criteria:
      - id: AC1
        text: "Given the Works on filter is \"Any day\", when front-desk staff choose status Inactive, then exactly the Inactive doctors are listed, the same as before the change."
      - id: AC2
        text: "Given a doctor with appointments today, when front-desk staff open the doctor list, then that doctor's Code, Name, Specialization, Department, Fee, Status and Today values are the same as before the change."
      - id: AC3
        text: "Given a doctor with no appointments today, when front-desk staff open the doctor list, then that doctor's Code, Name, Specialization, Department, Fee, Status and Today values are the same as before the change."
---
# Stories: HMS-28

## Stories

### S-1 See which weekdays each doctor works on the doctor list
Traces REQ-1, REQ-2, REQ-3, REQ-11, REQ-13.
- AC1: Given a doctor whose availability blocks were entered for Saturday, then Monday, then Wednesday, when front-desk staff open the doctor list, then that doctor's Works on column shows "Mon, Wed, Sat" (days in the order Mon, Tue, Wed, Thu, Fri, Sat, Sun, not in entry order).
- AC2: Given a doctor with two availability blocks on Tuesday (one in the morning, one in the afternoon) and none on other days, when front-desk staff open the doctor list, then that doctor's Works on column shows "Tue" once.
- AC3: Given a doctor with no availability blocks, when front-desk staff open the doctor list, then that doctor's Works on column shows "No availability".
- AC4: Given an Inactive doctor with an availability block on Sunday, when front-desk staff open the doctor list with status "Any status", then that doctor's Works on column shows "Sun".
- AC5: Given the doctor list is open, when front-desk staff look at the column headers, then the Works on column is after Status and before Today.

### S-2 Filter the doctor list to one weekday
Traces REQ-4, REQ-5, REQ-6, REQ-14.
- AC1: Given front-desk staff open the doctor list without choosing a weekday, when they look at the Works on filter, then it shows "Any day" and the list includes a doctor with no availability blocks.
- AC2: Given the doctor list is open, when front-desk staff open the Works on filter, then its options read "Any day", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday".
- AC3: Given doctor A has an availability block on Saturday and doctor B has blocks only on Friday, when front-desk staff choose "Saturday" in the Works on filter, then doctor A is listed and doctor B is not.
- AC4: Given doctor C has no availability blocks, when front-desk staff choose "Saturday" in the Works on filter, then doctor C is not listed.
- AC5: Given front-desk staff chose "Saturday" in the Works on filter, when they choose "Any day" again, then doctor B (blocks only on Friday) and doctor C (no blocks) are listed again.

### S-3 Combine the weekday filter with search, filters and paging
Traces REQ-7, REQ-8, REQ-9, REQ-11, REQ-12.
- AC1: Given doctor D in Cardiology and doctor E in another department both have an availability block on Saturday, when front-desk staff choose "Saturday" and department Cardiology, then doctor D is listed and doctor E is not.
- AC2: Given two doctors with an availability block on Saturday, only one of whose names matches the search text, when front-desk staff search that text and choose "Saturday", then only the matching doctor is listed.
- AC3: Given an Active doctor and an Inactive doctor both have an availability block on Saturday, when front-desk staff choose "Saturday" and status Active, then only the Active doctor is listed; with status "Any status" both are listed.
- AC4: Given a 21st doctor by name has an availability block on Saturday (one more than fit on a page at 20 per page), when front-desk staff choose "Saturday", then page 1 shows the first 20 such doctors sorted by name and page 2 shows the 21st.
- AC5: Given the whole doctor list fills more than one page but only three doctors have an availability block on Saturday, when front-desk staff choose "Saturday", then the total shows three doctors and there is a single page.

### S-4 Keep the chosen weekday in the page address
Traces REQ-10.
- AC1: Given front-desk staff chose "Saturday" in the Works on filter, when they copy the page address and open it in a new tab, then the Works on filter shows "Saturday" and the same doctors are listed.
- AC2: Given front-desk staff chose "Saturday" and opened a doctor's own page from the list, when they go back, then the Works on filter still shows "Saturday" and the list is still filtered to Saturday.

### S-5 The rest of the doctor list keeps working as before
Traces REQ-12, REQ-15.
- AC1: Given the Works on filter is "Any day", when front-desk staff choose status Inactive, then exactly the Inactive doctors are listed, the same as before the change.
- AC2: Given a doctor with appointments today, when front-desk staff open the doctor list, then that doctor's Code, Name, Specialization, Department, Fee, Status and Today values are the same as before the change.
- AC3: Given a doctor with no appointments today, when front-desk staff open the doctor list, then that doctor's Code, Name, Specialization, Department, Fee, Status and Today values are the same as before the change.

## Known
- S-1: built from known REQ-1, REQ-2, REQ-3, REQ-11 (ticket) and REQ-13 (decided in A-01).
- S-2: built from known REQ-4, REQ-5, REQ-6 (ticket) and REQ-14 (confirmed in A-02).
- S-3: built from known REQ-7, REQ-8, REQ-9, REQ-11 and REQ-12 (ticket).
- S-4: built from known REQ-10 (ticket).
- S-5: built from known REQ-12 and REQ-15 (ticket); the existing columns come from C1 and C2.

## Assumed
None

## Unknown
None

## Open questions
None

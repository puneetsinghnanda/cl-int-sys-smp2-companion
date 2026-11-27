---
schema: stories.v1
run_id: run_01M8DTDH90WA96KAFT09500VSZ
step: split
attempt: 1
skill: story-split@1.5.0
inputs:
  - path: artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md
    sha256: ff03e26e49a1e532c308fc010cae551de526a9e156225092dc2de150e408fe5b
  - path: context/glossary.md
    sha256: 430a90e14c50f969f7f757089c61cf254eb21596ed6e60abb8bf59286822b9ff
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
claims:
  - id: C1
    text: "REQ-1: the number in the `Doctors` column of a department links to the doctor list filtered to that department."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#item:REQ-1"]
  - id: C2
    text: "REQ-2: the number in the Doctors column of a `specialization` links to the doctor list filtered to that specialization."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#item:REQ-2"]
  - id: C3
    text: "REQ-3: the linked doctor list leaves the status filter at `Any status`, so it shows the same number of doctors as the Settings page count."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#item:REQ-3"]
  - id: C4
    text: "REQ-4: a count of 0 is shown as `plain text, not a link`."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#item:REQ-4"]
  - id: C5
    text: "REQ-5: the link works the same for `Active and Inactive` entries."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#item:REQ-5"]
  - id: C6
    text: "REQ-6: the link's accessible name follows the pattern `3 doctors in Cardiology` for both kinds, with \"1 doctor in <name>\" for a count of 1."
    cites:
      - "artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#item:REQ-6"
      - "runs/run_01M8DTDH90WA96KAFT09500VSZ/ide-answers.md#a-01"
  - id: C7
    text: "REQ-7: the rest of the `Settings page` is unchanged."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#item:REQ-7"]
  - id: C8
    text: "REQ-8: the `doctor list` is unchanged."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#item:REQ-8"]
  - id: C9
    text: "An entry used by any doctor `cannot be` deleted (REF-2)."
    cites: ["artifacts/HMS-29/run_01M8DTDH90WA96KAFT09500VSZ/req.md#claim:C2"]
  - id: C10
    text: "An `Inactive` department or specialization is kept for history but not offered for new use."
    cites: ["context/glossary.md#inactive"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8]
known: [S-1, S-2, S-3, S-4]
assumed: []
unknown: []
questions: []
stories:
  - id: S-1
    title: Open the doctors of a department from its Settings count
    trace: [REQ-1, REQ-3, REQ-4, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given the department Cardiology is used by 3 doctors and another department is used by other doctors, when staff select the Cardiology count in the Doctors column on the Settings page, then the doctor list opens with the Department filter set to Cardiology and lists only those 3 doctors."
      - id: AC2
        text: "Given the department Cardiology is used by 3 doctors, one of them Inactive, when staff select the Cardiology count on the Settings page, then the doctor list shows the status filter at \"Any status\" and lists all 3 doctors, the Inactive one included, matching the count."
      - id: AC3
        text: "Given a department used by no doctor, when staff view the Settings page, then its Doctors column shows 0 as plain text, not a link."
      - id: AC4
        text: "Given an Inactive department used by 3 doctors, when staff select its count on the Settings page, then the doctor list opens filtered to that department with the status filter at \"Any status\" and lists those 3 doctors, the same as for an Active department."
  - id: S-2
    title: Open the doctors of a specialization from its Settings count
    trace: [REQ-2, REQ-3, REQ-4, REQ-5]
    acceptance_criteria:
      - id: AC1
        text: "Given a specialization used by 3 doctors and another specialization used by other doctors, when staff select the first specialization's count in the Doctors column on the Settings page, then the doctor list opens with the Specialization filter set to that specialization and lists only those 3 doctors."
      - id: AC2
        text: "Given a specialization used by 3 doctors, one of them Inactive, when staff select its count on the Settings page, then the doctor list shows the status filter at \"Any status\" and lists all 3 doctors, the Inactive one included, matching the count."
      - id: AC3
        text: "Given a specialization used by no doctor, when staff view the Settings page, then its Doctors column shows 0 as plain text, not a link."
      - id: AC4
        text: "Given an Inactive specialization used by 3 doctors, when staff select its count on the Settings page, then the doctor list opens filtered to that specialization with the status filter at \"Any status\" and lists those 3 doctors, the same as for an Active specialization."
  - id: S-3
    title: The count link says what it opens
    trace: [REQ-6]
    acceptance_criteria:
      - id: AC1
        text: "Given the department Cardiology is used by 3 doctors, when a screen reader reads its count link on the Settings page, then the link's accessible name is \"3 doctors in Cardiology\"."
      - id: AC2
        text: "Given a specialization used by 3 doctors, when a screen reader reads its count link on the Settings page, then the link's accessible name is \"3 doctors in <the specialization's name>\", in the same pattern as a department."
      - id: AC3
        text: "Given a department used by exactly 1 doctor, when a screen reader reads its count link on the Settings page, then the link's accessible name is \"1 doctor in <the department's name>\", with \"doctor\" singular."
  - id: S-4
    title: The rest of the Settings page and the doctor list stay as they are
    trace: [REQ-7, REQ-8]
    acceptance_criteria:
      - id: AC1
        text: "Given an Active department used by 3 doctors, when staff view the Settings page, then its row still shows its name, its status and the Rename and Deactivate actions, and no Delete action because an entry in use cannot be deleted."
      - id: AC2
        text: "Given an Inactive specialization used by no doctor, when staff view the Settings page, then its row still shows its name, its status and the Rename, Activate and Delete actions, and the add form below the table is unchanged."
      - id: AC3
        text: "Given staff open the doctor list from the navigation, not from a Settings link, when the list loads, then the Department, Specialization and status filters show \"All departments\", \"All specializations\" and \"Any status\" and the list shows every doctor as before."
      - id: AC4
        text: "Given staff opened the doctor list from the Cardiology count link, when they change the Department filter to \"All departments\", then the list shows every doctor, the same as selecting that filter by hand."
---
# Stories: HMS-29

## Stories

**S-1 Open the doctors of a department from its Settings count.** Traces REQ-1, REQ-3, REQ-4, REQ-5.
- AC1: Given the department Cardiology is used by 3 doctors and another department is used by other doctors, when staff select the Cardiology count in the Doctors column on the Settings page, then the doctor list opens with the Department filter set to Cardiology and lists only those 3 doctors.
- AC2: Given the department Cardiology is used by 3 doctors, one of them Inactive, when staff select the Cardiology count on the Settings page, then the doctor list shows the status filter at "Any status" and lists all 3 doctors, the Inactive one included, matching the count.
- AC3: Given a department used by no doctor, when staff view the Settings page, then its Doctors column shows 0 as plain text, not a link.
- AC4: Given an Inactive department used by 3 doctors, when staff select its count on the Settings page, then the doctor list opens filtered to that department with the status filter at "Any status" and lists those 3 doctors, the same as for an Active department.

**S-2 Open the doctors of a specialization from its Settings count.** Traces REQ-2, REQ-3, REQ-4, REQ-5.
- AC1: Given a specialization used by 3 doctors and another specialization used by other doctors, when staff select the first specialization's count in the Doctors column on the Settings page, then the doctor list opens with the Specialization filter set to that specialization and lists only those 3 doctors.
- AC2: Given a specialization used by 3 doctors, one of them Inactive, when staff select its count on the Settings page, then the doctor list shows the status filter at "Any status" and lists all 3 doctors, the Inactive one included, matching the count.
- AC3: Given a specialization used by no doctor, when staff view the Settings page, then its Doctors column shows 0 as plain text, not a link.
- AC4: Given an Inactive specialization used by 3 doctors, when staff select its count on the Settings page, then the doctor list opens filtered to that specialization with the status filter at "Any status" and lists those 3 doctors, the same as for an Active specialization.

**S-3 The count link says what it opens.** Traces REQ-6.
- AC1: Given the department Cardiology is used by 3 doctors, when a screen reader reads its count link on the Settings page, then the link's accessible name is "3 doctors in Cardiology".
- AC2: Given a specialization used by 3 doctors, when a screen reader reads its count link on the Settings page, then the link's accessible name is "3 doctors in <the specialization's name>", in the same pattern as a department.
- AC3: Given a department used by exactly 1 doctor, when a screen reader reads its count link on the Settings page, then the link's accessible name is "1 doctor in <the department's name>", with "doctor" singular.

**S-4 The rest of the Settings page and the doctor list stay as they are.** Traces REQ-7, REQ-8.
- AC1: Given an Active department used by 3 doctors, when staff view the Settings page, then its row still shows its name, its status and the Rename and Deactivate actions, and no Delete action because an entry in use cannot be deleted.
- AC2: Given an Inactive specialization used by no doctor, when staff view the Settings page, then its row still shows its name, its status and the Rename, Activate and Delete actions, and the add form below the table is unchanged.
- AC3: Given staff open the doctor list from the navigation, not from a Settings link, when the list loads, then the Department, Specialization and status filters show "All departments", "All specializations" and "Any status" and the list shows every doctor as before.
- AC4: Given staff opened the doctor list from the Cardiology count link, when they change the Department filter to "All departments", then the list shows every doctor, the same as selecting that filter by hand.

## Known
- S-1: built only from known REQ-1, REQ-3, REQ-4 and REQ-5 (C1, C3, C4, C5).
- S-2: built only from known REQ-2, REQ-3, REQ-4 and REQ-5 (C2, C3, C4, C5).
- S-3: built only from known REQ-6, whose wording A-01 decided (C6).
- S-4: built only from known REQ-7 and REQ-8 (C7, C8). It covers both partitions: entries in use, which cannot be deleted (C9), and unused entries, plus Active and Inactive entries (C10).

## Assumed
None.

## Unknown
None.

## Open questions
None.

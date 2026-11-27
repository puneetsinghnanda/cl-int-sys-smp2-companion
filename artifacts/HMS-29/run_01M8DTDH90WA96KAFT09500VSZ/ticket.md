---
schema: ticket-snapshot.v1
run_id: run_01M8DTDH90WA96KAFT09500VSZ
ticket: HMS-29
source: file
fetched_at: '2026-11-27T06:17:24Z'
title: Open the doctors who use a department or specialization from the Settings page
issue_type: Story
source_sha256: 13b7007aa21f8980e02769e82395aa2c8f2eea971173ba0efd26195c386bb656
---
# HMS-29 Open the doctors who use a department or specialization from the Settings page

As front-desk staff, I want to open the list of doctors who use a department or specialization
straight from the Settings page, so that I can see who is affected before I rename an entry or set
it Inactive.

The Settings page lists departments and specializations with their name, the number of doctors who
use them, their status and the actions (PRD REF-1 to REF-4). An entry used by any doctor cannot be
deleted (REF-2). The number of doctors is plain text, so to see which doctors they are, staff have
to go to the doctor list and set the filter by hand (DOC-2).

Acceptance criteria:
1. On the Settings page, the number in the Doctors column of a department is a link to the doctor
   list filtered to that department; for a specialization, to the doctor list filtered to that
   specialization.
2. The linked doctor list shows the same number of doctors as the Settings page count: doctors of
   any status, so the status filter is left at "Any status".
3. A count of 0 is shown as plain text, not a link.
4. The link works the same for Active and Inactive entries.
5. The link's accessible name says what it opens, for example "3 doctors in Cardiology".
6. The rest of the Settings page and the doctor list are unchanged.

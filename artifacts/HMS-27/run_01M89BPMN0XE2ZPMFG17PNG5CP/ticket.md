---
schema: ticket-snapshot.v1
run_id: run_01M89BPMN0XE2ZPMFG17PNG5CP
ticket: HMS-27
source: file
fetched_at: '2026-11-25T12:43:16Z'
title: Show each patient's last visit on the patient list, and find patients not seen
  for a year
issue_type: Story
source_sha256: c93ac401e0827e092feefb4876b055aff1f53c5fef164fd9e1ba14c08eaf9489
---
# HMS-27 Show each patient's last visit on the patient list, and find patients not seen for a year

As front-desk staff, I want to see on the patient list when each patient last visited, and to list
the patients who have not been seen for over a year, so that I can call them for a check-up
without opening every patient's page.

The patient list (PRD PAT-2) shows each patient's code, name, age, gender, phone, status and next
appointment, 20 per page, newest registration first, with search and a status filter. To find when
a patient last came in, staff have to open the patient's page and read the appointment table.

Acceptance criteria:
1. The patient list has a "Last visit" column after "Next appointment", showing the date of the
   patient's last visit and the doctor's name.
2. A patient who has never visited shows "Never" in the column.
3. The list has a "Not seen for a year" filter next to the status filter. When it is on, the list
   shows only the patients whose last visit was more than 12 months ago.
4. The filter works together with search and the status filter, and with paging (20 per page).
5. The column and the filter show no medical history (PAT-8).
6. The rest of the patient list, and every other page, is unchanged.

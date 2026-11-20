---
schema: ticket-snapshot.v1
run_id: run_01M7W20GEGAG07YJ0WPHSC463Z
ticket: HMS-33
source: file
fetched_at: '2026-11-20T08:43:46Z'
title: Show the cancellation reason on the appointment list
issue_type: Story
source_sha256: be3d65341464f5c29979c574b000427a2faba52f0af6c2a99a6ded492a599641
---
# HMS-33 Show the cancellation reason on the appointment list
As front-desk staff, I want to see why an appointment was cancelled straight from the appointment
list, so that I can answer a patient or doctor without opening each appointment.
A cancelled appointment always has a reason of up to 300 characters, entered when it is cancelled;
today it is shown only on the appointment's own page.
Acceptance criteria:
1. The appointment list has a column "Cancellation reason" after the Status column.
2. For a Cancelled appointment the column shows its cancellation reason.
3. For Scheduled, Completed and No-show appointments the column shows "-".
4. A reason longer than 60 characters shows its first 60 characters followed by "…"; the full
   reason is still shown on the appointment's own page.
5. The column follows the list's status filter, date range and paging; filtering by Cancelled
   shows a reason on every row.
6. The dashboard, the doctor page and the patient page, which show appointments in the same
   kind of table, are unchanged and get no new column.
7. Opening, cancelling and booking appointments work as before.
8. No medical history and no contact details are added to the list.

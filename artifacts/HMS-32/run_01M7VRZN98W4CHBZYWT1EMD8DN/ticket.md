---
schema: ticket-snapshot.v1
run_id: run_01M7VRZN98W4CHBZYWT1EMD8DN
ticket: HMS-32
source: file
fetched_at: '2026-11-20T06:06:01Z'
title: Show the doctor's consultation fee on the book appointment page
issue_type: Story
source_sha256: 3e87c0faf83c892767c25de88fe35208b7778ffa9bda78ef61a9166b0874f68c
---
# HMS-32 Show the doctor's consultation fee on the book appointment page
As front-desk staff, I want to see the doctor's consultation fee while I book an appointment, so
that I can tell the patient what the visit will cost before I confirm it.
Each doctor already has a fee, stored and sent as integer paise; the hospital currency is INR.
Acceptance criteria:
1. On the book appointment page, once a doctor is chosen, the page shows "Consultation fee"
   with that doctor's fee.
2. The fee is shown in rupees with the ₹ sign, Indian digit grouping and two decimals, for
   example 150000 paise shows as ₹1,500.00.
3. Choosing a different doctor updates the fee straight away, without reloading the page.
4. Before a doctor is chosen, no fee is shown.
5. A fee of 0 paise shows as ₹0.00.
6. The fee is shown only; staff cannot change it on this page, and the booking request sent
   when staff confirm is unchanged.
7. The slot list, booking rules and the rest of the page are unchanged.
8. The change adds no medical history and no contact details to the page.

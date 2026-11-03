---
schema: context-section.v1
section: glossary
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc}
---

# Glossary {#glossary}

## Availability block {#availability-block}
A weekly window when a doctor sees patients: weekday, start time, end time, in hospital local time.

## Slot {#slot}
One bookable interval of a doctor's slot length, starting at the start of an availability block.

## Scheduled {#scheduled}
An appointment that is booked and still open. Only Scheduled appointments can change.

## No-show {#no-show}
A past appointment the patient did not attend, closed instead of staying Scheduled.

## Completed {#completed}
A past appointment the patient attended, closed by staff once its start time has passed.

## Cancelled {#cancelled}
An appointment called off before it took place, with a reason; its slot becomes free again.

## Dashboard {#dashboard}
The start page: tiles that count patients, doctors and appointments, each linking to the list it
counts, plus the next upcoming appointments.

## Period {#period}
A run of whole hospital-local days ending today: Today, Last 7 days or Last 30 days. The dashboard
tiles and the doctor page's No-shows count use the same rule.

## Tile {#tile}
One count on the dashboard, with its label; selecting it opens the list behind the count.

## Front-desk staff {#front-desk-staff}
The hospital staff who register patients and book, move and close appointments; in the MVP every
user acts as `staff`.

## Inactive {#inactive}
A doctor, patient, department or specialization kept for history but not offered for new use.

## Medical history {#medical-history}
Blood group, allergies, chronic conditions, current medications and notes of one patient.

## Actor {#actor}
Who made a change. In the MVP always `staff`; recorded in created_by, updated_by and appointment history.

## Paise {#paise}
One hundredth of a rupee. Fees are stored and sent as integer paise.

---
schema: context-section.v1
section: glossary
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: 415369398bdad3122ac084a00766a01a3034f900}
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

## Inactive {#inactive}
A doctor, patient, department or specialization kept for history but not offered for new use.

## Medical history {#medical-history}
Blood group, allergies, chronic conditions, current medications and notes of one patient.

## Actor {#actor}
Who made a change. In the MVP always `staff`; recorded in created_by, updated_by and appointment history.

## Paise {#paise}
One hundredth of a rupee. Fees are stored and sent as integer paise.

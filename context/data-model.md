---
schema: context-section.v1
section: data-model
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: 415369398bdad3122ac084a00766a01a3034f900}
---

# Data model {#data-model}

## Reference lists {#reference-lists}
`department` and `specialization` are short lists staff maintain. Names are unique ignoring case.
An entry used by a doctor is set Inactive instead of deleted.
Sources: context/generated/db-schema.json#table:department, context/generated/db-schema.json#table:specialization

## Doctors {#doctors}
A doctor has one specialization, one department, a fee in integer paise, a slot length and weekly
`availability_block` rows (weekday 1 = Monday, local wall-clock times, no overlaps per weekday).
Sources: context/generated/db-schema.json#table:doctor, context/generated/db-schema.json#table:availability_block

## Patients {#patients}
A patient row holds identity, contact details and the basic medical history (blood group, allergies,
chronic conditions, current medications, notes). Medical history leaves the database only for the
patient's own page.
Sources: context/generated/db-schema.json#table:patient, sample-system:api/src/repositories/patientRepo.ts#L29

## Appointments {#appointments}
An appointment links one doctor and one patient for [starts_at, ends_at) in UTC. Status is SCHEDULED,
COMPLETED, CANCELLED or NO_SHOW; a cancellation reason exists exactly when CANCELLED.
`appointment_event` records every change with time and actor.
Sources: context/generated/db-schema.json#table:appointment, context/generated/db-schema.json#table:appointment_event, sample-system:db/migrations/20261016T090300__create_appointments.sql#L17

## Codes {#codes}
D-0001, P-000001 and A-000001 are display codes derived from row ids; they are not stored.
Sources: sample-system:api/src/lib/codes.ts#L7-L9

## Data rules {#data-rules}
Every table carries created_by and updated_by from the request actor. Seed data is invented:
example.com emails and phone numbers in an unassigned +91 00000 range.
Sources: sample-system:docs/PRD.md#L253

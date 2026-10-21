---
schema: context-section.v1
section: intent-baseline
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: 415369398bdad3122ac084a00766a01a3034f900}
---

# Intent baseline {#intent-baseline}

## Goals {#goals}
Staff manage doctors, patients and appointments end to end. Booking rules prevent double-booking and
appointments outside availability. The dashboard shows the day in one screen. Sign-in can be added
without touching every route.
Sources: sample-system:docs/PRD.md#L25-L29

## Booking rules {#booking-rules}
A slot is free only inside the doctor's availability, in the future and not overlapping a Scheduled
appointment. A patient never has two overlapping Scheduled appointments. Concurrent bookings of one
slot leave exactly one.
Sources: sample-system:docs/PRD.md#L148-L150

## Closed appointments {#closed-appointments}
Completed and No-show are allowed only after the start time. Completed, Cancelled and No-show
appointments never change.
Sources: sample-system:docs/PRD.md#L153-L154

## Data protection {#data-protection}
Medical history appears only on the patient's own page, never in lists, the dashboard or logs.
Doctors and patients with appointments are made Inactive, never deleted.
Sources: sample-system:docs/PRD.md#L113, sample-system:docs/PRD.md#L77

## Constraints {#constraints}
Hospital time zone Asia/Kolkata, currency INR. No secrets in git; the database URL comes from the
environment. Seed data is invented.
Sources: sample-system:docs/PRD.md#L248, sample-system:docs/PRD.md#L251

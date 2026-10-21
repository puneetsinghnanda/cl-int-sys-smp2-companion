---
schema: context-section.v1
section: system-baseline
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: 415369398bdad3122ac084a00766a01a3034f900}
---

# System baseline {#system-baseline}

## Purpose {#purpose}
A hospital front desk manages doctors, patients and their appointments and sees the day at a glance.
It is also the shared product the team extends with AI Skill Library runs.
Sources: sample-system:docs/PRD.md#L10-L17

## Modules {#modules}
Three npm workspaces: `db` (SQL migrations, migration runner, seed, test harness), `api` (Express HTTP
API) and `web` (React single-page app). One process serves both the API and the built web app in the
shared environment.
Sources: sample-system:package.json#L10-L14, sample-system:api/src/app.ts#L36-L43

## Key flows {#key-flows}
Booking: the web app reads a doctor's free slots for a day, then posts the chosen slot.
Closing: a Scheduled appointment is rescheduled, cancelled with a reason, or marked Completed or No-show.
Dashboard: one request returns all six counts and the next ten appointments.
Sources: context/generated/api-inventory.json#http:GET:/api/doctors/:id/slots, context/generated/api-inventory.json#http:POST:/api/appointments, context/generated/api-inventory.json#http:GET:/api/dashboard

## Environments {#environments}
Every merge to `main` deploys to one Render web service backed by the `main` branch of the Neon
project `hms`. A deploy applies new migrations, seeds only an empty database, then starts.
Sources: sample-system:render.yaml#L12-L14

## Open points {#open-points}
- There is no sign-in yet; the shared URL is open and holds invented data only (PRD AUTH-5).

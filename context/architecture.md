---
schema: context-section.v1
section: architecture
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: 415369398bdad3122ac084a00766a01a3034f900}
---

# Architecture {#architecture}

## Components {#components}
The API is layered: routes parse input and declare a permission, services hold the rules and
transactions, repositories hold the SQL. Each resource has its own router mounted under `/api`.
The web app talks to the API only through `web/src/api/client.ts`.
Sources: sample-system:api/src/app.ts#L24-L33, sample-system:web/src/api/client.ts#L47

## Identity {#identity}
Every API request passes through `identify`, which sets the actor; routes call `requirePermission`.
In the MVP every request is the actor `staff` and every permission is granted. Sign-in replaces these
two functions only.
Sources: sample-system:api/src/auth/identity.ts#L25-L45

## Time and slots {#time-and-slots}
Instants are stored in UTC. Availability, slots and "today" use the hospital time zone
(`HOSPITAL_TZ`, default Asia/Kolkata). A slot starts at the start of an availability block and steps
by the doctor's slot length; it must end inside the block.
Sources: sample-system:api/src/config.ts#L33-L34, sample-system:api/src/lib/slots.ts#L10-L26

## Double-booking {#double-booking}
The database, not the service, is the final guard: exclusion constraints forbid overlapping Scheduled
appointments per doctor and per patient. The service turns those violations into 409 responses.
Sources: sample-system:db/migrations/20261016T090300__create_appointments.sql#L19-L26, sample-system:api/src/services/appointmentService.ts#L153-L163

## Deployment shape {#deployment-shape}
Render builds all workspaces, then runs migrate, seed-if-empty and the API, which also serves
`web/dist`. Health check: `GET /api/health`.
Sources: sample-system:render.yaml#L12-L14

## Open points {#open-points}
- No background jobs exist yet. Reminders or clean-ups would be the first.

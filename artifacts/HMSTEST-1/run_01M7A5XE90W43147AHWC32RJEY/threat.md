---
schema: threat-model.v1
run_id: run_01M7A5XE90W43147AHWC32RJEY
step: threat
attempt: 4
skill: threat-model@1.1.0
inputs:
  - {path: context/architecture.md, sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a}
  - {path: context/data-model.md, sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55}
  - {path: context/generated/api-inventory.json, sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9}
pack_sha: {sample-system: 7681f87749883d71abb4011d118366ca1242e1cc}
resolves:
  - {id: H2, how: "No design exists for a security-review run, so every architecture component is modelled instead."}
claims:
  - {id: C1, text: "Each resource has its own router mounted under `/api`.", cites: ["context/architecture.md#components"]}
  - {id: C2, text: "The web app talks to the API only through `web/src/api/client.ts`.", cites: ["context/architecture.md#components"]}
  - {id: C3, text: "Every API request passes through `identify`, which sets the actor.", cites: ["context/architecture.md#identity"]}
  - {id: C4, text: "Routes call `requirePermission`, but in the MVP every permission is granted.", cites: ["context/architecture.md#identity"]}
  - {id: C5, text: "In the MVP every request is the actor `staff`.", cites: ["context/architecture.md#identity"]}
  - {id: C6, text: "The database guards double-booking with `exclusion constraints`.", cites: ["context/architecture.md#double-booking"]}
  - {id: C7, text: "`appointment_event` records every appointment change with time and actor.", cites: ["context/data-model.md#appointments"]}
  - {id: C8, text: "Every table carries `created_by` and updated_by from the request actor.", cites: ["context/data-model.md#data-rules"]}
  - {id: C9, text: "A patient row holds `allergies`, chronic conditions and current medications.", cites: ["context/data-model.md#patients"]}
  - {id: C10, text: "Medical history leaves the database only for the `patient's own page`.", cites: ["context/data-model.md#patients"]}
  - {id: C11, text: "Patients can be deleted through a route in `api/src/routes/patients.ts`.", cites: ["context/generated/api-inventory.json#http:DELETE:/api/patients/:id"]}
  - {id: C12, text: "The API also serves `web/dist` on Render.", cites: ["context/architecture.md#deployment-shape"]}
  - {id: C13, text: "Render runs migrate and `seed-if-empty` before the API.", cites: ["context/architecture.md#deployment-shape"]}
  - {id: C14, text: "Repositories hold the `SQL`; services hold the rules and transactions.", cites: ["context/architecture.md#components"]}
  - {id: C15, text: "`No background jobs` exist yet.", cites: ["context/architecture.md#open-points"]}
  - {id: C16, text: "`Sign-in` is planned to replace identify and requirePermission only; no sign-in route exists yet.", cites: ["context/architecture.md#identity"]}
trace: []
known: [T-1, T-2, T-3, T-4, T-5, T-6, T-7, T-9, T-10, T-11, T-12, T-13, T-14]
assumed: [A-1]
unknown:
  - {id: U-1, blocking: false}
  - {id: U-2, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Is real patient data allowed in any deployment before sign-in replaces `identify` and `requirePermission`?"
    why: "Until then every caller is the actor staff with every permission, so patient data is reachable by anyone who can reach the API."
    evidence: ["context/architecture.md#identity", "context/data-model.md#patients"]
    options: ["No real patient data until sign-in ships", "Allowed behind a network restriction"]
    default: "No real patient data until sign-in ships"
  - id: Q-02
    blocking: false
    ask: tech lead
    text: "Is there any request rate or body size limit in front of the API?"
    why: "The context pack names no limit, so denial-of-service threats on the API cannot be assessed."
    evidence: ["context/architecture.md#components", "context/architecture.md#deployment-shape", "context/generated/api-inventory.json#http:GET:/api/patients"]
    options: ["Add a limit in the API", "Rely on a platform limit", "Accept the risk for the MVP"]
    default: "Add a limit in the API"
threats:
  - {id: T-1, component: Identity, stride: spoofing, threat: "Any caller is treated as the actor staff, so a caller cannot be told apart from real staff (C3, C5).", mitigation: "New control for the implement step: sign-in replacing identify, as the architecture plans (C3)."}
  - {id: T-2, component: Identity, stride: elevation_of_privilege, threat: "requirePermission grants every permission, so any caller can perform any action, including deletes (C4, C11).", mitigation: "New control for the implement step: real permission checks in requirePermission once sign-in exists (C4)."}
  - {id: T-3, component: API routes, stride: information_disclosure, threat: "Patient routes under /api expose identity, contact details and medical history to any caller (C1, C9).", mitigation: "Existing control: medical history leaves the database only for the patient's own page (C10); new control: authentication from T-1."}
  - {id: T-4, component: API routes, stride: denial_of_service, threat: "Unauthenticated list and slot routes can be called without a known limit (U-2).", mitigation: "New control for the implement step: request rate and size limits, pending Q-02."}
  - {id: T-5, component: API routes, stride: tampering, threat: "Malformed or hostile input reaches services if a route does not parse it fully (C14).", mitigation: "Existing control: routes parse input before services run (C1, C14); keep parsing on every new route."}
  - {id: T-6, component: Services, stride: tampering, threat: "Concurrent bookings could create overlapping appointments for a doctor or patient.", mitigation: "Existing control: database exclusion constraints, turned into 409 responses by the service (C6)."}
  - {id: T-7, component: Services, stride: repudiation, threat: "A changed or cancelled appointment cannot be tied to a real person because every actor is staff (C5).", mitigation: "Existing control: appointment_event and created_by/updated_by record the actor (C7, C8); becomes meaningful only with sign-in (T-1)."}
  - {id: T-8, component: Repositories, stride: tampering, threat: "SQL built from request input could change queries in the database (C14).", mitigation: "Assumed control A-1: parameterised queries in repositories; verify in secure-code-review."}
  - {id: T-9, component: Repositories, stride: information_disclosure, threat: "A repository query returning medical history to list views would leak it beyond the patient page (C10).", mitigation: "Existing control: medical history is returned only for the patient's own page (C10)."}
  - {id: T-10, component: Web app, stride: information_disclosure, threat: "Patient data shown in the browser is visible to anyone using the shared staff session (C5).", mitigation: "New control from T-1: sign-in; the single API path through web/src/api/client.ts is where credentials attach (C2)."}
  - {id: T-11, component: Web app, stride: tampering, threat: "A browser page calling the API outside the client module would bypass shared handling (C2).", mitigation: "Existing control: the web app talks to the API only through web/src/api/client.ts (C2)."}
  - {id: T-12, component: Deployment, stride: tampering, threat: "seed-if-empty could load invented seed data into a production database that happens to be empty (C13).", mitigation: "Existing control: seed data is invented (example.com emails, unassigned phone range) per data rules; new control: disable seeding in production, pending Q-01."}
  - {id: T-13, component: Deployment, stride: information_disclosure, threat: "The public service serves both web/dist and the API from one origin, so the API is exposed wherever the web app is (C12).", mitigation: "New control from T-1: authentication on the API; until then, Q-01 decides whether real data may be deployed."}
  - {id: T-14, component: Identity, stride: spoofing, threat: "Once sign-in exists, repeated login attempts without a rate limit let a caller guess staff credentials (C16).", mitigation: "New control for the implement step: rate limiting on the sign-in route, with the limit value set by the tech lead (no value exists in the inputs; see Q-02)."}
---
# Threat model: HMSTEST-1

No design exists for a security-review run, so every architecture component is modelled instead.

Attempt 4 follows the approver's change request at gate G-escalate-threat-a3: T-14 adds the login rate-limiting threat; T-1 keeps its wording unchanged.

## Components
- **API routes**: one router per resource mounted under `/api` (C1); handles patients, doctors, appointments, departments, specializations (C11).
- **Identity**: `identify` and `requirePermission` (C3, C4); every request is the actor `staff` (C5). Sign-in is planned to replace both (C16).
- **Services**: rules and transactions (C14), including appointment state changes and double-booking handling (C6, C7).
- **Repositories**: the SQL over patient, doctor and appointment tables (C14); patient rows hold medical history (C9).
- **Web app**: talks to the API only through `web/src/api/client.ts` (C2).
- **Deployment**: Render runs migrate, `seed-if-empty` and the API, which serves `web/dist` (C12, C13). No background jobs exist (C15).

## Threats
- T-1 Identity, spoofing: any caller is the actor staff. Mitigation: sign-in (new).
- T-2 Identity, elevation of privilege: every permission granted. Mitigation: real permission checks (new).
- T-3 API routes, information disclosure: patient data open to any caller. Mitigation: history only on the patient's own page (existing) plus T-1.
- T-4 API routes, denial of service: no known limit. Mitigation: rate and size limits (new, Q-02).
- T-5 API routes, tampering: unparsed input. Mitigation: routes parse input (existing).
- T-6 Services, tampering: overlapping bookings. Mitigation: exclusion constraints and 409 (existing).
- T-7 Services, repudiation: actor is always staff. Mitigation: appointment_event and created_by/updated_by (existing) plus T-1.
- T-8 Repositories, tampering: SQL built from input. Mitigation: parameterised queries (assumed A-1).
- T-9 Repositories, information disclosure: history leaking to lists. Mitigation: history only on the patient's own page (existing).
- T-10 Web app, information disclosure: shared staff session. Mitigation: sign-in via the client module (new).
- T-11 Web app, tampering: calls bypassing the client. Mitigation: single client module (existing).
- T-12 Deployment, tampering: seed data loaded into an empty production database. Mitigation: invented seed data (existing); disable seeding in production (new, Q-01).
- T-13 Deployment, information disclosure: API exposed with the web app. Mitigation: authentication (new, Q-01).
- T-14 Identity, spoofing: unlimited login attempts allow credential guessing once sign-in exists. Mitigation: rate limiting on the sign-in route (new; limit value to be set by the tech lead, Q-02).

## Known
- T-1, T-2, T-3, T-4, T-5, T-6, T-7, T-9, T-10, T-11, T-12, T-13, T-14: each threat and its control rest on cited claims C1 to C16 from the architecture, data-model and api-inventory sections.

## Assumed
- A-1: repositories use parameterised queries (supports T-8). Basis: repositories hold the SQL (C14); not checked against the code in this step, to be confirmed by secure-code-review.

## Unknown
- U-1: whether real patient data may be deployed before sign-in (Q-01).
- U-2: whether any request rate or body size limit exists, including on the future sign-in route (Q-02).

## Open questions
- Q-01 (non-blocking, tech lead): Is real patient data allowed in any deployment before sign-in replaces `identify` and `requirePermission`?
- Q-02 (non-blocking, tech lead): Is there any request rate or body size limit in front of the API?

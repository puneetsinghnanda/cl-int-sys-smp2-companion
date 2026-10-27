---
schema: threat-model.v1
run_id: run_01M5XZYC101HCFE34KWJ49C90V
step: threat
attempt: 2
skill: threat-model@1.0.1
inputs:
  - path: artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md
    sha256: 889e67e42166431aed6f8a95542c06b93ebc626f15e7f1c13cac95d491a6be1b
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: 8dfdbde2e1d64359cb29b8f710a2a467d5e19b383d90def7c856e7738d4ca104
pack_sha:
  sample-system: 415369398bdad3122ac084a00766a01a3034f900
resolves:
  - {id: F-1, how: "T-1, T-4 and Known no longer say the service returns counts only: the new noShows field is a count read from the appointment table alone (C10), while the existing response already carries `next`, up to 10 upcoming appointments with patient and doctor names (C15), which this ticket does not change."}
  - {id: F-2, how: "T-9 now rests on the list's own row shape: appointment summaries carry codes, names, times and status, and no contact details (C16, C17); T-9 moved from assumed to known, with access still under Q-01."}
claims:
  - id: C1
    text: "In the MVP every request is the actor `staff` and every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Routes declare a permission by calling `requirePermission`."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "Sign-in replaces the two identity functions `only`."
    cites: ["context/architecture.md#identity"]
  - id: C4
    text: "The API runs on `Render` and also serves the web app."
    cites: ["context/architecture.md#deployment-shape"]
  - id: C5
    text: "A patient row holds identity, `contact details` and the basic medical history."
    cites: ["context/data-model.md#patients"]
  - id: C6
    text: "`appointment_event` records every appointment change with time and actor."
    cites: ["context/data-model.md#appointments"]
  - id: C7
    text: "Seed data is invented, with `example.com` emails."
    cites: ["context/data-model.md#data-rules"]
  - id: C8
    text: "The dashboard is served by the route `/api/dashboard`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/dashboard"]
  - id: C9
    text: "The appointment list is served by the route `/api/appointments`."
    cites: ["context/generated/api-inventory.json#http:GET:/api/appointments"]
  - id: C10
    text: "The No-show count reads only the appointment table with status `NO_SHOW`."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L117"]
  - id: C11
    text: "The tile links to `/appointments` with status NO_SHOW and the period's from and to."
    cites: ["artifacts/HMS-3/run_01M5XZYC101HCFE34KWJ49C90V/design.md#L121"]
  - id: C12
    text: "The period is chosen from `PERIODS` only."
    cites: ["sample-system:api/src/services/dashboardService.ts#L36"]
  - id: C13
    text: "The counts run as one statement with bound parameters `$4` and `$2` for the bounds."
    cites: ["sample-system:api/src/services/dashboardService.ts#L51"]
  - id: C14
    text: "The dashboard's counting statement reads no `patient` columns beyond the Active count."
    cites: ["sample-system:api/src/services/dashboardService.ts#L46"]
  - id: C15
    text: "The dashboard response also lists the next upcoming appointments, `LIMIT 10`."
    cites: ["sample-system:api/src/services/dashboardService.ts#L57"]
  - id: C16
    text: "Appointment summaries select the doctor and patient `full_name` and the appointment's times and status."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L22"]
  - id: C17
    text: "Each summary returns the patient as id, code and `fullName` only."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L33"]
  - id: C18
    text: "The web app talks to the API only through `client.ts`."
    cites: ["context/architecture.md#components"]
  - id: C19
    text: "The `Tile` component renders a value and a label inside a link."
    cites: ["sample-system:web/src/pages/DashboardPage.tsx#L64"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10]
known: [T-1, T-2, T-3, T-4, T-6, T-7, T-8, T-9, T-10, T-11]
assumed: [T-5]
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "Before release, is the deployment reachable only by hospital staff, or will GET /api/dashboard and GET /api/appointments be behind sign-in?"
    why: "In the MVP every request is staff and every permission is granted (C1), and the API runs on Render and serves the web app (C4), so it may be publicly reachable; the new tile adds a count and a link to an existing list, not a new kind of data."
    evidence: ["context/architecture.md#identity", "context/architecture.md#deployment-shape", "context/generated/api-inventory.json#http:GET:/api/appointments"]
    options: ["Staff-only network", "Add sign-in before release", "Accept for the MVP"]
    default: "Treat the deployment as possibly public; this ticket does not change the trust boundary, so record the risk and keep the existing requirePermission calls so sign-in can restrict both routes later."
threats:
  - id: T-5
    component: dashboardRouter
    stride: elevation_of_privilege
    threat: "Anyone who can reach the API can read the dashboard, now including the No-show count."
    mitigation: "No effective control in the MVP: every request is staff and every permission is granted (C1); the deployment may be public (C4). The route keeps its requirePermission call (C2) so sign-in can restrict it later (C3). Access before release is decided under Q-01. No new control in this ticket."
  - id: T-9
    component: Appointment list (status and from/to filters)
    stride: information_disclosure
    threat: "The tile's link opens a list of No-show appointments with patient names to anyone who can reach the list."
    mitigation: "No new data: the list already serves any status, including No-show, under the same MVP identity model (C1, C9). Its rows carry codes, names, times and status, and no contact details or medical history (C16, C17), although the patient row holds them (C5). Access before release falls under Q-01. No new control."
  - id: T-1
    component: DashboardService no-show count
    stride: information_disclosure
    threat: "The new count could read or return patient data beyond a number."
    mitigation: "The new sub-select reads only the appointment table by status and start time (C10) and returns a number. The counting statement reads no patient columns beyond the Active count (C14). The response already carries patient and doctor names in its list of next appointments (C15); this ticket does not change that list. No new control."
  - id: T-2
    component: DashboardService no-show count
    stride: tampering
    threat: "A crafted period value could alter the count query."
    mitigation: "Existing controls: the period is accepted only from PERIODS (C12) and the bounds are passed as bound parameters (C13). New control for implement: keep the NO_SHOW status as a fixed literal and reuse the same bound parameters; build no SQL from request input."
  - id: T-3
    component: DashboardService no-show count
    stride: denial_of_service
    threat: "One more count on every dashboard load adds database work."
    mitigation: "The count has the same shape and bounds as the existing Completed and Cancelled counts in the same statement (C13), so it costs one more count of the same kind. No new control."
  - id: T-4
    component: Dashboard totals type (api)
    stride: information_disclosure
    threat: "Widening the response could add fields that leak data."
    mitigation: "The only added field is the number of No-show appointments (C10); no names or contact details are added. The existing next-appointments list (C15) is unchanged. No new control."
  - id: T-6
    component: Dashboard totals type (web)
    stride: tampering
    threat: "The web app trusts the count it receives from the API."
    mitigation: "The web app talks to the API only through its client (C18); the value is shown as a number and drives no action. No new control."
  - id: T-7
    component: DashboardPage No-shows tile
    stride: tampering
    threat: "The tile's link could be built from untrusted text and lead somewhere unexpected."
    mitigation: "The link is built from fixed values: the path /appointments, the literal status NO_SHOW and the period dates the API returns (C11), with the page's existing query helper as the Cancelled tile does. New control for implement: build no part of the link from free text."
  - id: T-8
    component: Tile
    stride: information_disclosure
    threat: "The tile could show more than a label and a number."
    mitigation: "Tile renders a value and a label inside a link only (C19); the value is a count. No new control."
  - id: T-10
    component: API dashboard test
    stride: information_disclosure
    threat: "New test cases could use real patient data."
    mitigation: "Existing rule: seed and test data are invented, with example.com emails (C7). New control for implement: build No-show cases from invented patients and doctors only."
  - id: T-11
    component: Web DashboardPage test
    stride: information_disclosure
    threat: "The web test fixture could hold real data."
    mitigation: "Existing rule: data is invented (C7); the fixture needs only counts and dates. New control for implement: use counts and dates only, no names."
---
# Threat model: HMS-3

## Components
- DashboardService no-show count (api): reads the appointment table by status and start time; returns a number (C10).
- Dashboard totals type (api): the response's totals, now with noShows, a number; the response also carries the next appointments with names (C15), unchanged.
- dashboardRouter (api): serves GET /api/dashboard (C8) under the MVP identity model (C1, C2).
- Dashboard totals type (web): the same number on the web side.
- DashboardPage No-shows tile (web): shows the number and links to the No-show list for the period (C11).
- Tile (web): a value and a label inside a link (C19).
- Appointment list (status and from/to filters) (api): GET /api/appointments (C9); rows with codes, names, times and status (C16, C17).
- API dashboard test and Web DashboardPage test: invented data only (C7).

## Threats
Ordered by risk: the two with no effective control first.
- T-5 (elevation of privilege, dashboardRouter): no access control in the MVP; requirePermission kept for sign-in (C1, C2, C3); Q-01.
- T-9 (information disclosure, Appointment list): existing list, existing access; rows hold names, no contact details (C16, C17); Q-01.
- T-1 (information disclosure, DashboardService no-show count): the new count reads the appointment table only (C10, C14); the existing names in `next` are unchanged (C15).
- T-2 (tampering, DashboardService no-show count): period from PERIODS, bound parameters; implement keeps NO_SHOW literal (C12, C13).
- T-3 (denial of service, DashboardService no-show count): one more count of the same kind (C13).
- T-4 (information disclosure, Dashboard totals type (api)): one added number.
- T-6 (tampering, Dashboard totals type (web)): number shown only (C18).
- T-7 (tampering, DashboardPage No-shows tile): link from fixed values; implement builds none from free text (C11).
- T-8 (information disclosure, Tile): value, label and link only (C19).
- T-10 (information disclosure, API dashboard test): invented data (C7).
- T-11 (information disclosure, Web DashboardPage test): counts and dates only (C7).

## Known
- T-1, T-4: the new field is a count from the appointment table alone (C10, C14); the response's existing names (C15) are not changed here.
- T-2: the period is checked against PERIODS and bounds are bound parameters (C12, C13).
- T-3: same count shape as the existing ones (C13).
- T-6, T-7, T-8: the value and link come from the API and fixed values (C11, C18, C19).
- T-9: the list's rows hold codes, names, times and status only (C16, C17).
- T-10, T-11: data rules require invented data (C7).

## Assumed
- T-5: the deployment may be publicly reachable. Basis: the API runs on Render and serves the web app (C4), and every request is staff (C1); the actual network exposure is not in the inputs (Q-01).

## Unknown
- U-1 (non-blocking): who can reach the API before release (Q-01).

## Open questions
- Q-01 (non-blocking, tech lead): staff-only network, sign-in before release, or accept for the MVP? Default: treat as possibly public; this ticket does not change the trust boundary.

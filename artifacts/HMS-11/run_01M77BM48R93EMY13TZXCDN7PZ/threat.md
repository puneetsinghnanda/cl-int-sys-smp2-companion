---
schema: threat-model.v1
run_id: run_01M77BM48R93EMY13TZXCDN7PZ
step: threat
attempt: 1
skill: threat-model@1.1.0
inputs:
  - path: artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md
    sha256: 5bbb2ba02b9021065e0ec9ff8410030043417595f497f433b47f9e00817a8237
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
  - path: context/data-model.md
    sha256: 2386ae151d54a73cdbe4fd9e0b69f8e8539c2870f5e52c12e5ed0191cfa95e55
  - path: context/generated/api-inventory.json
    sha256: ba024ab1b4b4151d26ce1afa77bd2dab1cec645ea737ce37f98e0e8674f795e9
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
claims:
  - id: C1
    text: "Routes declare their permission with `requirePermission`; in the MVP every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Every request passes through `identify`, which sets the actor; in the MVP the actor is always staff."
    cites: ["context/architecture.md#identity"]
  - id: C3
    text: "The database, through `exclusion constraints`, is the final guard against overlapping Scheduled appointments."
    cites: ["context/architecture.md#double-booking"]
  - id: C4
    text: "Slots and today use the configured hospital zone `HOSPITAL_TZ`."
    cites: ["context/architecture.md#time-and-slots"]
  - id: C5
    text: "Layering keeps SQL in the data layer: `repositories hold the SQL`."
    cites: ["context/architecture.md#components"]
  - id: C6
    text: "`Medical history` leaves the database only for the patient's own page."
    cites: ["context/data-model.md#patients"]
  - id: C7
    text: "Every appointment change is recorded in `appointment_event` with time and actor; a read-only route writes none."
    cites: ["context/data-model.md#appointments"]
  - id: C8
    text: "Each response entry is flat, with `departmentName` as a string."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C18"]
  - id: C9
    text: "The new route requires `appointments:read`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C19"]
  - id: C10
    text: "The specialization's existence and status are read with `refStatus`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C6"]
  - id: C11
    text: "Path ids are validated by `parseId` before any query."
    cites: ["sample-system:api/src/lib/codes.ts#L21"]
  - id: C12
    text: "Unknown ids answer 404 through `notFound`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C12"]
  - id: C13
    text: "The search window is bounded to `today to today+6`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C21"]
  - id: C14
    text: "The response has `no paging`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C22"]
  - id: C15
    text: "The route is new and does not replace any listed route: `earliest-slots`."
    cites: ["artifacts/HMS-11/run_01M77BM48R93EMY13TZXCDN7PZ/design.md#claim:C17"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14, REQ-15, REQ-16, REQ-17]
known: [T-1, T-2, T-3, T-4, T-5, T-6, T-7, T-8, T-9, T-10, T-11, T-12, T-13]
assumed: []
unknown: []
questions: []
threats:
  - id: T-1
    component: "specializationsRouter: GET /:id/earliest-slots"
    stride: spoofing
    threat: "Anyone who can reach the shared URL can call the route as staff, because the MVP has no sign-in and identify sets every request's actor to staff (C2)."
    mitigation: "Existing, accepted MVP control: identify and requirePermission are the single place sign-in will plug in (C1, C2). The route must declare requirePermission like every other route so that it is covered when sign-in lands. Implement must not bypass identify."
  - id: T-2
    component: "specializationsRouter: GET /:id/earliest-slots"
    stride: elevation_of_privilege
    threat: "If the route declared a weaker permission, or none, a future reference-only role could read appointment occupancy it should not see."
    mitigation: "Declare requirePermission('appointments:read') on the route, the same as the day schedule (C9, C1). Implement must add it; a test checks the route goes through requirePermission."
  - id: T-3
    component: "specializationsRouter: GET /:id/earliest-slots"
    stride: tampering
    threat: "A malformed or hostile :id path value could reach the query layer and change what is read."
    mitigation: "Existing control: parseId rejects non-integer ids with 400 before any query (C11). The repositories use parameter placeholders, never string-built values (C5). Implement must pass the parsed id only."
  - id: T-4
    component: "DoctorService.earliestSlots"
    stride: information_disclosure
    threat: "Returning internal objects (doctor rows or appointment summaries) instead of mapped entries would leak doctor phone and email, patient names or other personal data."
    mitigation: "New control that implement must build: map every entry field by field to the flat EarliestSlotEntry (C8). Never spread or return doctor rows or appointment summaries. A test asserts the exact key set of each entry."
  - id: T-5
    component: "DoctorService.earliestSlots"
    stride: denial_of_service
    threat: "The response is unpaged and the work grows with the number of Active doctors (per-doctor availability and appointment reads), so a large specialization makes one request expensive."
    mitigation: "Partly existing: the window is bounded to 7 days (C13), and each doctor's appointments are read once for the whole window, not per day. Accepted residual risk for the MVP, because the response is unpaged by decision (C14). Implement must not add per-day or per-slot queries."
  - id: T-6
    component: "DoctorService.earliestSlots"
    stride: repudiation
    threat: "Reads of slot occupancy leave no audit trail, so nobody can later show who looked up which specialization."
    mitigation: "Accepted. The route is read-only and changes nothing, so no appointment_event row applies (C7). The existing request log records the method and route; no new control is needed."
  - id: T-7
    component: "DoctorRepo.activeBySpecialization"
    stride: tampering
    threat: "A new query built by string concatenation could let input change the SQL."
    mitigation: "Existing pattern, which implement must follow: SQL lives in the repository with $n parameters (C5). The specialization id is passed as a parameter, and the status filter is a fixed literal."
  - id: T-8
    component: "DoctorRepo.activeBySpecialization"
    stride: information_disclosure
    threat: "Selecting the full doctor row brings phone and email into the service, raising the chance they reach the response."
    mitigation: "Implement should select only the needed columns (id, full name, department name, slot length). In any case the service maps only the entry fields (T-4, C8)."
  - id: T-9
    component: "DoctorRepo.refStatus and availability"
    stride: information_disclosure
    threat: "Different answers for unknown and Inactive specializations reveal whether an id exists."
    mitigation: "Accepted. Specializations are a staff-maintained reference list that GET /api/specializations already lists in full. The 404 (C12) and 400 answers are required by REQ-12 and REQ-13 (C10)."
  - id: T-10
    component: "AppointmentRepo.occupyingBetween"
    stride: tampering
    threat: "A slot shown as free can be booked by someone else before staff book it, so acting on a stale answer could produce a double booking."
    mitigation: "Existing control: the database exclusion constraints are the final guard. A second booking of the slot fails with 409 'slot no longer available' (C3). The endpoint is read-only and never books."
  - id: T-11
    component: "EarliestSlotEntry type in api/src/types.ts"
    stride: information_disclosure
    threat: "Widening the response type later (for example to reuse DoctorSummary) would silently add contact fields to every entry."
    mitigation: "New control that implement must build: declare EarliestSlotEntry with only id, code, fullName, departmentName and slot (C8). Do not extend DoctorSummary or AppointmentSummary. The key-set test from T-4 guards against drift."
  - id: T-12
    component: "Slot, time and error helpers"
    stride: tampering
    threat: "Computing days in the wrong zone would shift the window and could show slots outside today to today+6."
    mitigation: "Existing control: the helpers use the configured HOSPITAL_TZ (C4), and the window is today to today+6 (C13). Tests cover a local date that differs from the UTC date."
  - id: T-13
    component: "Web app"
    stride: information_disclosure
    threat: "The web app does not change, but the new route is reachable from the same open shared URL as the rest of the API."
    mitigation: "Accepted MVP risk, the same as every existing route (C1, C2). No web change is made, and the payload carries no contact details or medical history (C6, C8)."
---
# Threat model: HMS-11

## Components
- **specializationsRouter: GET /:id/earliest-slots**: handles the path id and the permission check.
- **DoctorService.earliestSlots**: reads doctors, availability and occupying appointments, which include patient-linked rows; returns doctor id, code, name, department name and one slot.
- **DoctorRepo.activeBySpecialization**: reads doctor rows, which also hold phone and email.
- **DoctorRepo.refStatus and availability**: reads specialization status and weekly blocks.
- **AppointmentRepo.occupyingBetween**: reads non-Cancelled appointments, which are joined to patient names.
- **EarliestSlotEntry type in api/src/types.ts**: the response shape.
- **Slot, time and error helpers**: hospital-zone dates and error answers.
- **Web app**: unchanged.

## Threats
- T-1 (spoofing, router): no sign-in in the MVP. Mitigation: the existing identify and requirePermission seam (C1, C2), and the route must declare a permission.
- T-2 (elevation of privilege, router): a missing or weak permission. Mitigation: `appointments:read`, which implement must add (C9).
- T-3 (tampering, router): a hostile path id. Mitigation: parseId rejects it with 400, and repositories use parameters (C11, C5).
- T-4 (information disclosure, service): internal rows or summaries leaking. Mitigation: a new field-by-field mapping, with a key-set test.
- T-5 (denial of service, service): unpaged, per-doctor work. Mitigation: the 7-day bound and one appointments read per doctor; residual risk accepted (C13, C14).
- T-6 (repudiation, service): no audit of reads. Accepted, because the route is read-only (C7).
- T-7 (tampering, new repo method): SQL built from strings. Mitigation: the existing parameterised-repository pattern (C5).
- T-8 (information disclosure, new repo method): selecting contact columns. Mitigation: select only the needed columns, and map in the service.
- T-9 (information disclosure, refStatus): 404 versus 400 reveals whether an id exists. Accepted, because the list is public to staff (C10, C12).
- T-10 (tampering, occupyingBetween): acting on a stale free slot. Mitigation: the existing DB exclusion constraints and 409 (C3).
- T-11 (information disclosure, type): the type widening later. Mitigation: a dedicated narrow type and the key-set test (C8).
- T-12 (tampering, helpers): computing days in the wrong zone. Mitigation: HOSPITAL_TZ and the window tests (C4, C13).
- T-13 (information disclosure, web app): the open shared URL. Accepted MVP risk; the payload holds no contact details or medical history (C6, C8).

New controls implement must build: the requirePermission('appointments:read') declaration (T-2), the explicit field mapping and narrow type with a key-set test (T-4, T-11), and a column-limited, parameterised query (T-7, T-8).

## Known
- T-1 to T-13: each is grounded in a cited architecture section, data-model section, design claim or product code line (C1 to C15).

## Assumed
None.

## Unknown
None.

## Open questions
None.

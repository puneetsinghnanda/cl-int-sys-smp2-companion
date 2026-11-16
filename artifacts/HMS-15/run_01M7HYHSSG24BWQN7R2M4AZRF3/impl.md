---
schema: change.v1
run_id: run_01M7HYHSSG24BWQN7R2M4AZRF3
step: impl
attempt: 1
skill: implement@1.2.0
inputs:
  - path: artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/design.md
    sha256: c9a1ede4306bc083606d25524f9f23a42bd6bc2c7879ad56be7df79f2ab386f4
  - path: artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/split.md
    sha256: 37e1c1b4a8f6927e7237a6d3526934d298e5ce3a870e6da718f067bf13f3507d
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 4158c8fcd239c22cb8f1029dd11f61d8839867ff8088bd9ab76197e2db96db4b
pack_sha:
  sample-system: 7681f87749883d71abb4011d118366ca1242e1cc
code:
  - repo: sample-system
    base_sha: 7681f87749883d71abb4011d118366ca1242e1cc
    head_sha: 7f074fb32f05198b8507ba46acb09ceaae451afb
    diff_sha256: c67395543d40400b2c3a9199763c057ccd05188578a42a63e3e0e9347ec918a8
    files:
      - api/src/repositories/patientRepo.ts
      - api/src/services/patientService.ts
      - api/src/types.ts
      - web/src/api/types.ts
      - web/src/pages/PatientDetailPage.tsx
claims:
  - id: C1
    text: "The repository gains `nextAppointment(id, now)`, which runs the shared next-appointment SQL for one patient."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/design.md#item:CO-1"]
  - id: C2
    text: "The list keeps its single `LEFT JOIN LATERAL` query per page, now built from the shared fragment."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/design.md#claim:C25"]
  - id: C3
    text: "The service reads `this.clock()` once and passes that instant to both appointments and nextAppointment."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/design.md#item:CO-2"]
  - id: C4
    text: "The API type PatientDetail gains `next_appointment: NextAppointment | null`."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/design.md#item:CO-3"]
  - id: C5
    text: "The web type PatientDetail mirrors `next_appointment: NextAppointment | null`."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/design.md#item:CO-4"]
  - id: C6
    text: "The patient page shows the line just before `AppointmentTable`."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/design.md#item:CO-5"]
  - id: C7
    text: "The date and time are printed with `dateTime` and the useSettings time zone, as in the table."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/design.md#item:CO-6"]
  - id: C8
    text: "Only the date and time text is the link (`Date and time (proposed default)`)."
    cites: ["runs/run_01M7HYHSSG24BWQN7R2M4AZRF3/ide-answers.md#a-04"]
  - id: C9
    text: "The doctor's name follows the date and time, as the design's `doctor's name follows as plain text` says; the code joins them with the word \"with\"."
    cites: ["artifacts/HMS-15/run_01M7HYHSSG24BWQN7R2M4AZRF3/design.md#L200"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [CO-1, CO-2, CO-3, CO-4, CO-5, CO-6, CO-7]
assumed: [AS-1]
unknown: []
questions: []
summary: "GET/POST/PATCH patient responses now carry next_appointment, chosen by the HMS-1 rule through one shared SQL fragment; the patient page shows it above the Appointments table, with the date/time linking to the appointment, or 'No upcoming appointment'."
---
# Change: HMS-15

## Summary
The changes implement the approved design:
- CO-1: `PatientRepo` now builds the next-appointment rule (status 'SCHEDULED', `starts_at > now`, ordered by `starts_at, id`, limit 1) in one private `nextAppointmentSql` fragment.
  - `list` still runs a single `LEFT JOIN LATERAL` query per page built from that fragment.
  - The new `nextAppointment(id, now)` runs the same fragment for one patient.
  - `get` now returns `Omit<PatientDetail, 'appointments' | 'next_appointment'>`.
- CO-2: `PatientService.get` reads `now = this.clock()` once and uses it for both `appointments` and `next_appointment`. Create and update return through `get`, so EP-1, EP-2 and EP-3 all carry the field.
- CO-3 and CO-4: both `PatientDetail` types gain `next_appointment: NextAppointment | null`.
- CO-5: `PatientDetailPage` shows the line between the "Appointments" heading and `AppointmentTable`.
  - With an appointment: "Next appointment:" then a link to `/appointments/{id}` whose text is `dateTime(starts_at, timeZone)`, then "with {doctor_name}".
  - Without one: "No upcoming appointment".
- CO-6: the existing `dateTime` and `useSettings` are reused.
- CO-7: `AppointmentTable` and the Book appointment button are not changed.

No tests were added: the design asks for no regression tests, and the unit-test step owns them. Existing API tests compare the patient body with `toMatchObject`, so the extra field does not break them.

## Changes
- `api/src/repositories/patientRepo.ts`: the shared `nextAppointmentSql` fragment, the list uses it, the new `nextAppointment`, and the `get` return type omits `next_appointment` (CO-1).
- `api/src/services/patientService.ts`: one `now` instant, and `next_appointment` added to the detail response (CO-2).
- `api/src/types.ts`: `PatientDetail.next_appointment` (CO-3).
- `web/src/api/types.ts`: `PatientDetail.next_appointment` (CO-4).
- `web/src/pages/PatientDetailPage.tsx`: the Next appointment line and the empty state, using `dateTime` and `useSettings` (CO-5, CO-6).

## Known
- CO-1 to CO-7: built as the design specifies (C1 to C8); the link element follows A-04.

## Assumed
- AS-1 (claim C9): the design says the doctor's name follows the date and time as plain text but gives no separator. The code writes "with" between them, for example "Next appointment: 15 Mar 2027, 10:00 with <doctor>". Trade-off: it reads naturally, but it is a word the ticket does not state. Changing it is a one-word edit.

## Unknown
None.

## Open questions
None.

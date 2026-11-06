---
schema: change.v1
run_id: run_01M6QZ4JKGSK55APYQTFP14YQD
step: impl
attempt: 1
skill: implement@1.1.0
inputs:
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/design.md
    sha256: 42358b2515450494074ee4e85508c7791b3c755e3e822e4ffc47d3c48b0c96a7
  - path: artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/split.md
    sha256: 27a56fb6c5c77620388a68804cade76e555a471888785e875f98064fab7c3d95
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/generated/module-map.json
    sha256: 07674e27e969e7fd56aba8a2a1419c1f597c1b4a9c490b574a56b82ff6571114
pack_sha:
  sample-system: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
code:
  - repo: sample-system
    base_sha: c2709e164850ab7e177e4bccfc42a5dd9b961fdc
    head_sha: dde15588741cb3055cf14e2cbe60f2730bb488d3
    diff_sha256: 83f99d94faf5d450a74fe6da21caea7437e3d0e5d7f7e541c2792aee878a70d0
    files:
      - api/src/repositories/appointmentRepo.ts
      - api/src/services/appointmentService.ts
      - web/src/__tests__/DoctorsPage.test.tsx
      - web/src/pages/AppointmentsPage.tsx
      - web/src/pages/DoctorsPage.tsx
claims:
  - id: C1
    text: "The service's status check used `APPOINTMENT_STATUSES`; it now also accepts NOT_CANCELLED, and other values still fail validation."
    cites: ["sample-system:api/src/services/appointmentService.ts#L35"]
  - id: C2
    text: "Fields.oneOf rejects a value outside its list with `must be one of`, giving the 400 ValidationError the conventions require."
    cites: ["sample-system:api/src/lib/validate.ts#L65"]
  - id: C3
    text: "The repository filter declared `status?: AppointmentStatus`; it widens to include NOT_CANCELLED."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L43"]
  - id: C4
    text: "The NOT_CANCELLED branch reuses the Today count's fixed condition `status <> 'CANCELLED'` and takes no request value."
    cites: ["sample-system:api/src/repositories/appointmentRepo.ts#L129"]
  - id: C5
    text: "The existing Picker clears with `onChange(undefined, '')`, which the list page maps to removing doctorId only."
    cites: ["sample-system:web/src/components/Picker.tsx#L28"]
  - id: C6
    text: "Picker renders labels as React text (`{selectedLabel}`), never as HTML."
    cites: ["sample-system:web/src/components/Picker.tsx#L27"]
  - id: C7
    text: "The query helper writes keys in insertion order with `URLSearchParams`, so the Today href is doctorId, status, from, to."
    cites: ["sample-system:web/src/api/client.ts#L41"]
  - id: C8
    text: "The design's doctor label is `${d.fullName} (${d.code}) · ${d.specialization.name}`."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/design.md#claim:C4"]
  - id: C9
    text: "Clear All is shown `Only while at least one filter is set`."
    cites: ["artifacts/HMS-8/run_01M6QZ4JKGSK55APYQTFP14YQD/design.md#claim:C14"]
  - id: C10
    text: "The set helper copies the current parameters with `new URLSearchParams(params)`, so changing the doctor keeps the other filters."
    cites: ["sample-system:web/src/pages/AppointmentsPage.tsx#L18"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11, REQ-12, REQ-13, REQ-14]
assumed: []
unknown: []
questions: []
summary: "The appointment list gets an always-visible doctor Picker showing the doctor's name (CO-1, CO-2), an Any except Cancelled status option (NOT_CANCELLED) and a Clear All button. The doctors list's Today link adds status=NOT_CANCELLED (CO-3), and its test is updated (CO-4). GET /api/appointments accepts NOT_CANCELLED (CO-5) and filters it with the Today count's fixed condition (CO-6)."
---
# Change: HMS-8

## Summary
This change implements design components CO-1 to CO-6. CO-7 and CO-8 are left untouched, as the design marks them existing.

**API (CO-5, CO-6).**
- `AppointmentService.list` accepts `NOT_CANCELLED` next to the four stored statuses. Any other value still fails `oneOf` with a 400 (C1, C2).
- `AppointmentFilter.status` widens to `AppointmentStatus | 'NOT_CANCELLED'` (C3).
- `AppointmentRepo.list` pushes the fixed condition `a.status <> 'CANCELLED'` for NOT_CANCELLED, the same rule the Today count uses (C4). Every other status stays a bound parameter, so nothing from the request enters the SQL text (threat T-10). The order and page size are unchanged.

**Web (CO-1 to CO-4).**
- AppointmentsPage always shows the existing Picker for the doctor (A-02).
  - It searches GET /api/doctors with no status, so Inactive doctors are offered (A-01).
  - A doctorId from the URL is turned into a number. Its label comes from GET /api/doctors/:id, in the design's label format (C8), and is shown as plain text (C6, threat T-2).
  - Picking a doctor sets doctorId through the existing set helper, which keeps the patient, status and dates and resets the page (C10). Picker's Change action clears doctorId only (C5).
- The status select gains "Any except Cancelled" with value `NOT_CANCELLED` (A-04).
- "Clear doctor / patient filter" is replaced by "Clear All". It removes every parameter and is shown while doctorId, patientId, status, from or to is set (A-03, C9).
- DoctorsPage's Today link becomes `query({ doctorId: d.id, status: 'NOT_CANCELLED', from: today, to: today })`, using the fixed literal (threat T-6).
- The HMS-6 Today link test now asserts the exact href with `status=NOT_CANCELLED` (C7, threat T-7).

## Changes
- `api/src/services/appointmentService.ts`: CO-5. Status may also be `NOT_CANCELLED`.
- `api/src/repositories/appointmentRepo.ts`: CO-6. `AppointmentFilter.status` widens, and the NOT_CANCELLED branch adds the fixed condition.
- `web/src/pages/AppointmentsPage.tsx`: CO-1, using CO-2 (Picker, unchanged). It adds the doctor Picker and label, the Any except Cancelled option and Clear All.
- `web/src/pages/DoctorsPage.tsx`: CO-3. The Today link adds `status: 'NOT_CANCELLED'`.
- `web/src/__tests__/DoctorsPage.test.tsx`: CO-4. Test "HMS-6 REQ-6, REQ-7, HMS-8 REQ-9: the count links to that doctor's appointments today, except Cancelled" expects `/appointments?doctorId=1&status=NOT_CANCELLED&from=…&to=…`.

## Known
- REQ-1, REQ-2, REQ-3, REQ-14: the Picker and label in AppointmentsPage (A-01, A-02, C5, C6, C8).
- REQ-4, REQ-5: the set helper keeps the other filters (C10).
- REQ-6: Clear All (A-03, C9).
- REQ-7, REQ-8: the NOT_CANCELLED option, service and repository (A-04, C1–C4).
- REQ-9, REQ-10: the Today link and its test (C7), plus the same exclusion rule as the count (C4).
- REQ-11, REQ-12, REQ-13: the DoctorDetailPage and DashboardPage links, the order and the page size are not edited.

## Assumed
None.

## Unknown
None.

## Open questions
None.

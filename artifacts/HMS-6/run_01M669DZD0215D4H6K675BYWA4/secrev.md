---
schema: secure-review.v1
run_id: run_01M669DZD0215D4H6K675BYWA4
step: secrev
attempt: 1
skill: secure-code-review@1.0.1
inputs:
  - path: artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/impl.md
    sha256: 3ef6f5eb86e6950f21bdde784f9d6b7f9bc618f94ce7e9364aaa33a0358e5834
  - path: context/conventions.md
    sha256: dc53d94ec34eb193eeb06fdd0db19eb86a24c29f7d3c434e7c212d61038ca852
  - path: context/architecture.md
    sha256: 9faab093d10933a46f6473b25c80728f8745e597100e650cd2537bbafb2b595a
pack_sha:
  sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33
claims:
  - id: C1
    text: "Routes declare a permission with `requirePermission`; in the MVP every permission is granted."
    cites: ["context/architecture.md#identity"]
  - id: C2
    text: "Invalid input is a 400 with one message per field (`ValidationError`)."
    cites: ["context/conventions.md#errors"]
  - id: C3
    text: "Logs carry method, route and error message only, never `request bodies`."
    cites: ["context/conventions.md#logging-and-data"]
  - id: C4
    text: "Repositories hold the `SQL`; services hold the rules."
    cites: ["context/architecture.md#components"]
  - id: C5
    text: "The change adds `countTodayByDoctor` with bound parameters and an early return for an empty id list."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/impl.md#L89"]
  - id: C6
    text: "`with_today` is parsed with the existing `Fields.oneOf`; anything else is a 400."
    cites: ["artifacts/HMS-6/run_01M669DZD0215D4H6K675BYWA4/impl.md#L90"]
trace: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
known: [REQ-1, REQ-2, REQ-3, REQ-4, REQ-5, REQ-6, REQ-7, REQ-8, REQ-9, REQ-10, REQ-11]
assumed: [S-1]
unknown:
  - {id: U-1, blocking: false}
questions:
  - id: Q-01
    blocking: false
    ask: tech lead
    text: "GET /api/doctors is guarded by doctors:read, but with_today=true now returns per-doctor appointment counts. Should with_today=true also require appointments:read once sign-in replaces the MVP actor?"
    why: "Today every permission is granted, so nothing leaks now; after sign-in, a role with doctors:read but not appointments:read would see appointment volume per doctor."
    evidence: ["context/architecture.md#identity"]
    options: ["Also require appointments:read for with_today=true", "Counts are not sensitive; doctors:read is enough"]
    default: "Also require appointments:read for with_today=true"
scans:
  - kind: sast
    report_sha256: 6a3c62b6afcdcca2dfdeeac507a52749aaf9126b20508dfb5c8b6eddd2f1feca
  - kind: secrets
    report_sha256: 5a1cea4400bf5f18180b6d2eff35db1be83f6e03aa19603fd628c0484b8d90a3
findings:
  - id: SF-1
    severity: minor
    text: "with_today=true adds appointment counts to a route guarded only by doctors:read; once roles exist, appointment data is visible without appointments:read. Fix direction: check appointments:read before computing today_count."
    cites: ["sample-system:api/src/services/doctorService.ts#L57"]
---
# Secure code review: HMS-6

## Summary
Overall risk is low. The change adds one read-only count query and one display column; it adds no route, no write path, no dependency and no logging. The query uses bound parameters only (C5), the new flag is validated with the existing field validator and fails with a 400 (C2, C6), and the web link is built from a numeric id and a date through the client's encoding `query()` helper and rendered as text by React. The SAST and secrets scans from the verifier are attached by hash unchanged. The most important finding is SF-1: appointment counts are served behind the doctors read permission rather than the appointments read permission, which only matters once sign-in replaces the MVP actor (C1).

## Findings
- SF-1 (minor): `with_today=true` returns per-doctor appointment counts on GET /api/doctors, which declares only `doctors:read`; after sign-in a role without `appointments:read` would see appointment volume. Fix direction: require `appointments:read` for the flag. Cite: sample-system:api/src/services/doctorService.ts#L57.

Checked with no finding:
- Input validation: `with_today` accepts only `true`/`false` via `Fields.oneOf` (sample-system:api/src/services/doctorService.ts#L35); other filters are unchanged.
- Injection: `countTodayByDoctor` passes ids and bounds as `$1..$3` with `ANY($1::int[])`; no string building (sample-system:api/src/repositories/appointmentRepo.ts#L127-L131).
- Denial of service: the count is limited to the ids of the current page (page size from config) and returns early for an empty list (sample-system:api/src/repositories/appointmentRepo.ts#L126).
- Output encoding: the link target is built with `query()` from a numeric id and a date; the count renders as a React text node (sample-system:web/src/pages/DoctorsPage.tsx#L86).
- Error leakage and logging: no new error messages or log lines (C3).
- Dependencies: no package changes in the diff.

## Known
- REQ-1 to REQ-11: the change serving these requirements was read at head in all six changed files; no finding blocks any of them.

## Assumed
- S-1: assumed no exposure exists today, because in the MVP every request is the `staff` actor and every permission is granted (C1, context/architecture.md#identity). SF-1 is therefore minor.

## Unknown
- U-1 (non-blocking): whether `with_today=true` should also need `appointments:read` once sign-in exists → Q-01.

## Open questions
- Q-01 (non-blocking, tech lead): should `with_today=true` also require `appointments:read` after sign-in? Default: yes, require it.

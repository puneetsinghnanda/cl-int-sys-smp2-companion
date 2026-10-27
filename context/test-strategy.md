---
schema: context-section.v1
section: test-strategy
owner: eng-db6b
origin: human
status: reviewed
verified_at: {sample-system: bd98ff08b83f8762c5e5c073ba5fadef88169b33}
---

# Test strategy {#test-strategy}

## Layers {#layers}
API tests use Vitest and Supertest against a throwaway PostgreSQL 16 with a fixed clock. Web tests use
Vitest with jsdom and Testing Library. The db workspace tests the seed and the migrations.
Sources: sample-system:api/vitest.config.mts#L9-L10, sample-system:web/vitest.config.mts#L10

## Requirement IDs {#requirement-ids}
Every PRD requirement has at least one test whose name starts with its ID (DOC-1, APT-4, ...).
New requirements get new IDs and tests.
Sources: sample-system:docs/PRD.md#L250

## Database for tests {#database-for-tests}
The harness starts a temporary cluster from PostgreSQL 16 binaries or uses `HMS_TEST_DATABASE_URL`.
Without either, database tests are skipped. Tests never touch Neon.
Sources: sample-system:db/testing/pg-harness.ts#L10-L11

## How to run {#how-to-run}
`npm run lint`, `npm run typecheck` and `npm test` at the repository root; CI runs the same on every
pull request.
Sources: sample-system:package.json#L17-L19

## Gaps {#gaps}
- Web tests cover the dashboard tiles and links, the patient list's Next appointment column, the slot
  picker and formatting; other pages have no web tests, and no page is tested end to end in a browser.
- The verifier skips coverage (the project has no coverage script), so no run reports coverage.
Sources: sample-system:web/src/__tests__/DashboardPage.test.tsx#L1, sample-system:web/src/__tests__/PatientsPage.test.tsx#L1

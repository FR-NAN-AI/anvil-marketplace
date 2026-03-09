---
---

# Testing (TDD)

Follow a test-first workflow when feasible:

## Red
- Write the smallest failing test that represents the requirement.
- Prefer user-visible outcomes over implementation details.

## Green
- Implement the simplest code to make the test pass.
- Avoid over-engineering during the green step.

## Refactor
- Clean up names, extract helpers, and remove duplication.
- Keep tests readable and fast.

## Coverage Expectations
- Each acceptance criterion should map to at least one test.
- Cover boundary conditions and error paths.
- Include integration tests when behavior crosses modules or services.

## Test File Naming
- Mirror production paths and names (e.g., `foo/bar.ts` → `foo/bar.test.ts`).
- Keep unit tests close to the module when the repo standard allows it.
- Use descriptive test names that read like requirements.

## Mocking Strategy
- Mock only external boundaries (network, filesystem, time, third-party APIs).
- Avoid mocking the unit under test or its core collaborators.
- Prefer real implementations for pure functions.
- Keep mocks minimal and reset state between tests.

## Test Data Management
- Use small, explicit fixtures instead of large snapshots.
- Centralize shared fixtures in a `test/fixtures` directory.
- Prefer factory helpers for generating valid objects.
- Avoid random data unless seeded and deterministic.

## CI Integration
- Ensure new tests run in the default CI pipeline.
- Keep tests fast; isolate slow tests in a separate suite.
- Document new test commands in README when they add requirements.

## Test Hygiene
- No flaky tests; stabilize or remove.
- Keep fixtures small and focused.
- Prefer deterministic data over randomization.

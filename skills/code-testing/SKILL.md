---
name: code-testing
description: Use to design, add, or improve automated tests and to verify test results before submission. Covers generated-code tests, regression cases, weak existing suites, and changed behavior. Derive cases from requirements and contracts; cover relevant boundaries, errors, and state changes; prove tests can detect defects; and run the repository's required suite. Use the project's framework. TDD during implementation belongs to code-implementation; broad PR review belongs to code-review.
metadata:
  short-description: Design effective tests and verify required suites
---

# Code Testing

Design tests that demonstrate observable requirements and detect realistic defects. Test quality is not measured by test count or coverage percentage alone: each test should have an independent expected result and a clear contract.

## Choose the Task Mode

- **New behavior or fix:** derive tests from the requirement and regression risk. During implementation, coordinate with `code-implementation` for the red-green-refactor cycle.
- **Add tests to existing code:** first characterize the current contract, then target uncovered behavior without changing production code unless requested.
- **Improve a weak suite:** identify what important behavior the existing tests fail to distinguish; revise only tests that provide stronger evidence.
- **Pre-submission verification:** run the repository-required test sequence, including relevant integration or end-to-end tests.

Do not impose a fixed test pyramid or an arbitrary test ratio. Do not turn every test into an end-to-end test.

## Workflow

1. **Establish context and baseline:** Read project instructions, test configuration, adjacent tests, and CI. Confirm the framework, version, discovery rules, fixtures, and commands. For test changes, establish the diff including untracked files; for test-addition work, identify the target module. When feasible, run relevant existing tests first and distinguish pre-existing failures from new ones.
2. **Derive cases from contracts:** Map each rule to a scenario/input, expected result and side effects, test layer, and test location. Keep a short mapping in the work log for simple tasks; use the repository's test-plan location for complex tasks. Prioritize by risk. See [references/test-design.md](references/test-design.md).
3. **Write discriminating tests:** Reuse the project's assertions and fixtures. Keep the real subject under test involved; replace only uncontrollable boundaries. Ensure async work is awaited, failures reach the test runner, and expected values are independent. See [references/test-quality.md](references/test-quality.md).
4. **Verify the tests themselves:** For new behavior or fixes, use TDD to check the failure reason. Existing-behavior test additions do not need to be deliberately red first. For important assertions, prove a plausible defect fails in an isolated environment, compare behavior before and after a fix, or use focused mutation testing. Restore all mutations and rerun tests. See `test-quality.md`.
5. **Run from narrow to broad:** Run the new tests, then the affected module, then the repository-required pre-submission suite and applicable integration/E2E tests. Inspect actual collection counts, exit codes, failures, skips, and retries rather than relying on a green summary. See [references/execution.md](references/execution.md).
6. **Attribute and report:** Distinguish implementation defects, test defects, environment problems, and pre-existing failures. Change expectations only when a test contradicts the contract. Do not hide product regressions by removing assertions, accepting snapshots wholesale, skipping tests, or adding retries.

## Pre-Submission Completion Criteria

- Important acceptance behavior maps to tests; relevant boundaries, errors, and side effects are covered, and uncovered risks are explicit.
- New tests are discovered and executed. Critical tests have evidence that they catch defects; any sensitivity checks not performed are disclosed.
- The repository-required local pre-submission suite has run; applicable integration/E2E tests have run, and failures or unexpected skips are not hidden.
- After the last change to code, tests, fixtures, or configuration, relevant checks were rerun. Lint and build do not replace tests.
- If required tests fail, the environment is missing, or the suite did not finish, state that the test gate is not yet passed. List known failures with evidence; only project policy and an authorized decision can grant an exception.

## Output

Report concisely, scaled to task complexity:

1. Test scope, contract sources, and the cases added or changed with the risks they cover.
2. Evidence of test effectiveness, such as before/after failure or mutation checks, plus anything not performed and why.
3. Commands, working directory or tested revision, exit codes, test counts, failures, skips, and retries.
4. Implementation issues, environment blockers, uncovered risk, and whether the required test gate passed.

Local success does not mean remote CI passed; a green suite does not prove the program has no defects. See `references/execution.md` for reporting examples.

## Responsibility Boundaries

- This skill decides what to test, how to write effective tests, and how to run and explain them. It can be used independently for adding tests.
- `code-implementation` owns the red-green-refactor loop, debugging, and production changes made during implementation.
- `code-review` owns comprehensive review of a change; passing tests are only one kind of evidence.
- `github-engineering-workflow` owns commits, PRs, CI gates, and delivery. A green suite does not authorize a commit or release by itself.

## Red Flags

- Computing expected output by calling the subject under test again or copying production conditions into the test.
- Checking only “not empty,” “does not throw,” or “mock was called” without verifying the required result.
- Mocking the subject's logic or every integration boundary while claiming the real path was tested.
- Covering only the happy path and ignoring whether rejected requests still write data or repeated requests duplicate side effects.
- Using fixed sleeps, shared test accounts, execution-order dependencies, or automatic retries to hide instability.
- Claiming pre-submission tests passed after running zero tests, collecting without executing, running only the new file, or skipping required suites.
- Changing product expectations, accepting snapshots wholesale, disabling failure checks, or optimizing only for coverage and test count.

## References

Read only what is needed:

- [references/test-design.md](references/test-design.md): contract mapping, boundary/state cases, test-layer selection, and property-based tests.
- [references/test-quality.md](references/test-quality.md): assertions, test doubles, fixtures, sensitivity checks, and runnable counterexamples.
- [references/execution.md](references/execution.md): pre-submission runs, flaky-test attribution, web tests, and evidence format.
- [references/sources.md](references/sources.md): official skills and guidance plus integration choices; read when tracing or updating the approach.

# Pre-Submission Runs and Evidence

## Find the Repository's Actual Commands

Read project instructions, CI workflows, test configuration, and entry points such as `package.json` or `Makefile`. Confirm framework/version, discovery paths, working directory, runtime, and service dependencies. Derive commands from repository evidence; do not assume every project runs `npm test` or `pytest`.

List the required suites for this change: fast tests, full unit tests, relevant integration/contract tests, critical E2E, and existing pre-submission gates. Note which can run locally and which run only remotely. A test skill does not make unconfigured performance/load tests mandatory for every change.

If the project has no test foundation, use its language and existing dependencies to select the smallest runnable approach. No tests does not mean passed. Do not install global tools, change CI, or connect external services without authorization.

## Execution Order

1. When the environment permits, run relevant existing tests first and record pre-existing failures.
2. Run added/changed cases and confirm they are collected; then run the module suite to expose shared-state problems.
3. Run the complete required pre-submission suite and applicable integration/E2E tests. Do not substitute changed/failed-test filters for a required full gate.
4. After production code, fixture, or configuration changes, rerun affected checks. The final candidate must have results from the complete required suite; do not combine green results from different revisions.
5. Read exit codes and full summaries. Check for unexpected zero-test runs, `only`/`focus`, newly added skip/xfail/todo, filters, retries, and runner configuration. Collection-only, a pending watch, background work still running, or a pipeline that swallows failures is not completion.

For a long-running suite, report progress, but do not downgrade to “only run the new tests.” If required suites cannot finish due to environment or cost constraints, list what remains and why; retain the blocker in the conclusion.

Running the full suite does not require testing every platform combination on every Commit; follow the project's matrix and report local and CI results separately. Without remote evidence, do not say CI passed.

## Attribute Failures

| Category | Next step |
| --- | --- |
| Implementation violates an explicit contract | Keep a minimal reproduction and actual/expected result. If authorized to fix it, follow implementation workflow; otherwise report the defect rather than changing assertions to accommodate it. |
| Test defect | Use the contract/evidence to fix fixtures, selectors, assertions, or async waiting, then rerun. |
| Environment issue | Name the missing service, dependency, permission, or version. Complete safe, authorized setup or report not-run/blocking. |
| Pre-existing failure | Compare with evidence from the same environment and version before the change. Without a baseline, do not claim it is unrelated or report the suite as all green. |
| Contract conflicts with observation, and it is unclear which is stale | Verify the undisputed behavior and ask the user to clarify the specific rule and observation; do not change business policy unilaterally. |

For flaky tests, investigate isolation, ordering, shared resources, async races, time, and randomness. Preserve the first failure and retry result. Use an existing repeat/random-order option only after confirming the plugin and parameter exist. Record owner, reason, and tracking item for isolation/quarantine according to repository policy; do not silently skip a test and claim the gate passed.

## Additional Constraints for Web Tests

- Derive locators from a real page; prefer role, label, or established test ID. Do not guess the DOM.
- Use framework auto-waiting and retryable assertions. Wait for a business result or explicit event; fixed sleeps and `networkidle` are not default readiness checks.
- Recorded clicks/fills are only steps. Add result assertions derived from the contract; current page content is not automatically the correct expected result.
- Isolate sessions and data between tests, including server-side data. Creating a browser context alone does not clear the backend database.
- Preserve traces, console errors, and failure screenshots when useful. They aid diagnosis but do not replace result assertions. Stop only processes you started, restore your own test data, and do not stop shared services.

## Evidence Report Example

This is a format, not a real test result. A short task may use a few lines; do not create a file unless needed.

```text
Scope / contract: <target module or diff baseline; requirement/API doc location>
Cases: <test file and behavior; boundaries, failures, required side effects>
Effectiveness: <failure before defect fix/pass after, or actual mutation and failure reason; disclose if not run>

Candidate revision: <commit and worktree changes, or diff record before/after tests>
Directory: <actual working directory>
Command: <full command; necessary environment notes, no credentials>
Result: <exit code; pass/fail/skip/retry counts; duration/report path if available>
Required suites: <list passed/failed/not run for each>

Findings and gaps: <implementation defect, environment blocker, unverified path>
Conclusion: <required tests passed / test gate not yet passed>
```

This conclusion describes test evidence only. It does not replace code review or authorize Commit, Push, or Merge.

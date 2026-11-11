# Pre-Delivery Verification

Test design, test effectiveness, and required test-suite execution belong to `code-testing`. This guide covers the broader delivery gate, including builds, lint, and documentation.

Rule:

```text
Without fresh verification evidence, do not claim “complete,” “passed,” or “fixed.”
```

If you did not run a command during this task, do not say it passed.

## Five-Step Verification Gate

Before claiming any status:

1. **Identify:** Which command proves this claim?
2. **Run:** Execute the complete command freshly.
3. **Read:** Inspect the complete output, exit code, and failure count.
4. **Check:** Does the output actually support the claim? If not, report the state and evidence honestly; if so, report the conclusion with evidence.
5. **Conclude:** Skipping any step means it was not verified.

## Common False Evidence

| Claim | Required evidence | Not sufficient |
| --- | --- | --- |
| Tests pass | Test command reports zero failures | It passed last time; it “should pass.” |
| Lint is clean | Linter reports zero errors | Partial inspection or inference. |
| Formatting is complete | Formatter reports zero changes | Remembering that it ran or visual alignment. |
| Build succeeds | Build command exits 0 | Lint passes or logs look plausible. |
| Bug is fixed | A test reproducing the symptom passes | Code changed and seems fixed. |
| Regression test is effective | Test passes with fix, fails without fix, passes again after restoring it | One passing run. |
| Acceptance is met | Each criterion is checked explicitly | Entire suite is green. |
| A subagent completed work | VCS diff confirms the actual change | The agent claims it succeeded. |

## Stop Signs

- “Should,” “probably,” “looks like,” or “seems to” in a completion claim.
- Saying “done,” “complete,” or “perfect” without running verification.
- Preparing a Commit, push, or PR before verification.
- Bypassing pre-commit hooks with `--no-verify` or ignoring failed checks. Investigate and fix a failed gate; do not bypass it.
- Trusting an agent's or tool's success claim without evidence.
- Running only part of verification (lint does not prove a build works).
- Deciding “just this once” to skip verification.

## Evidence Examples

- `pytest` → `34 passed` → “34/34 tests passed.”
- `npm run build` → exit 0 → “Build passed.”
- Regression test: write → pass with fix → remove fix and confirm failure → restore fix and pass again.
- Acceptance: reread the requirements → create a checklist → verify each criterion → report gaps or completion.

## Delivery Checklist

Check every applicable item before claiming completion:

- [ ] Read the full diff: no `print`/`console.log`/debugger, commented-out code, temporary files, or unrelated changes.
- [ ] Full test suite passes with zero failures/errors.
- [ ] Formatter ran and left no uncommitted formatting changes.
- [ ] Linter ran with zero errors.
- [ ] Build succeeded (exit 0).
- [ ] Each acceptance criterion was checked.
- [ ] Documentation is synchronized: update affected README/API docs/changelog. A code/documentation mismatch means delivery is incomplete.
- [ ] No `--no-verify` bypass; hooks and checks are green.

## When Verification Is Required

Verify before any claim that work is done, passed, or fixed, including equivalent wording; and before committing, opening a PR, marking a task complete, moving to the next task, or handing work to another agent.

This rule applies to any wording that implies success, not only these exact terms.

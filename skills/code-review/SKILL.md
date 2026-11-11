---
name: code-review
description: Use when reviewing code changes, including pull requests, branches, uncommitted work, generated code, or review feedback. Pin the diff baseline first, assess both repository-standard compliance and requirement compliance, then provide prioritized findings, verification evidence, and a verdict.
metadata:
  short-description: Review code against standards and requirements
---

# Code Review

Review changes for correctness, maintainability, and fit with the repository and request. Findings must be actionable and evidence-based; do not turn personal preference into a defect.

## Establish the Review Context

Before reviewing, determine:

- The exact target: PR, branch, commit range, or working tree.
- The comparison base and head; include untracked files when reviewing a working tree.
- The task description, acceptance criteria, and repository-specific instructions.
- Whether you are doing a self-review or reviewing someone else's change.
- The relevant tests and checks, and which have actually been run.

If the diff or task is not available, state the limitation instead of inferring a review target. Do not review unrelated changes as if they belonged to the target.

## Review on Two Axes

### Repository Standards

- Follow `AGENTS.md`, contribution guidance, established architecture, naming, formatting, and error-handling conventions.
- Reuse existing helpers and patterns unless there is a concrete reason not to.
- Check that dependencies, generated files, migrations, configuration, and documentation follow repository policy.

### Requirement Compliance

- Map each acceptance criterion to changed behavior and evidence.
- Check normal, boundary, failure, and state-transition behavior relevant to the change.
- Trace affected callers and contracts rather than reviewing changed lines in isolation.
- Identify missing tests or tests that do not distinguish correct behavior from a plausible defect.

For production consequences such as data loss, service outages, rollout order, or rollback hazards, use `production-risk-review`; do not duplicate its release-risk gate here.

## Finding Quality

Report issues that are actionable and likely to change the decision. Order findings by severity. For each one, include:

1. Severity and concise title.
2. File and line evidence.
3. The condition that triggers the problem.
4. User or system impact.
5. A practical correction.

Do not report speculative issues without a concrete path. Ask a question when evidence is incomplete, and label non-blocking preferences as suggestions rather than defects.

## Verdict

Use the repository's review states when available. Otherwise state one of:

- **Request changes:** at least one confirmed blocker or material correctness issue remains.
- **Comment:** there are useful non-blocking questions or improvements.
- **Approve:** no material issue was found within the reviewed scope.

Never imply that an unrun test, CI check, or external review passed. Separate findings from verification gaps and assumptions.

## Output

Lead with findings, ordered by severity and anchored to file/line references. Then state open questions or assumptions, checks actually performed and their results, and a brief summary only if useful. If no issue is found, say so clearly and describe remaining test gaps or residual risk.

## Red Flags

- Reviewing a moving or unspecified diff without recording its base and head.
- Treating a passing build as proof of requirement compliance.
- Reporting style preferences as correctness findings.
- Guessing runtime behavior without tracing the relevant code path or contract.
- Overlooking untracked files, migrations, configuration, or generated changes in scope.
- Saying “looks good” while omitting unrun checks, uncertainty, or relevant risk.

---
name: code-implementation
description: Use when turning a requirement into correct, verifiable, mergeable code, including features, bug fixes, refactors, and behavior changes. Covers clarification, implementation planning, TDD, systematic debugging, and pre-delivery verification; hand off to code-review for independent review. Classify throwaway versus maintained work before choosing TDD rigor. Do not write production code before observing a failing test in maintained work, and do not claim completion without fresh verification evidence.
metadata:
  short-description: Turn requirements into correct, verifiable code
---

# Code Implementation

Turn an unclear request into code with a clear purpose, verifiable behavior, and a path to the main branch. The goal is not merely to finish the code; it is to finish with evidence that it does the right thing, is covered by tests, and leaves the main branch usable.

This skill owns the TDD rhythm during implementation. Use `code-testing` for test design, adding tests to existing code, assertion quality, and required pre-commit test execution; do not duplicate its detailed test-design guidance here.

## Core Principles

1. Think before acting: do not code until the requirement is clear and the plan is concrete.
2. Rule: write a failing test before production code (red-green-refactor). Without seeing it fail, you do not know what it verifies.
3. Implement the minimum: follow YAGNI and write only enough to satisfy the test; avoid opportunistic changes the tests do not require.
4. Never claim “complete” or “passing” without fresh verification evidence. Evidence means the check was just run and its output supports the claim.
5. Fix root causes: before changing a bug, find all callers of the function and fix the shared layer once instead of patching every caller. Do not guess, stack patches, or treat symptoms.
6. Make the plan challengeable: question your assumptions after writing it until none remain unexamined.
7. Implementation makes the change; `code-review` independently judges it. Do not act as both implementer and approver.
8. Climb the ladder before writing code. Ask whether the behavior needs to exist, already exists in the repository, is available in the standard library, is native to the platform, is covered by an installed dependency, or can be solved in one line. Write new code only if none applies. The best code is sometimes code never written.
9. Treat the plan as the execution authority. When reality conflicts with it, determine whether the plan or the code is wrong. Record each departure as a Ruling instead of making undocumented decisions.

## Classify the Work

Determine the code's lifecycle before choosing TDD rigor:

- **Throwaway** (one-off script, prototype, or exploration): it will not be maintained. TDD may be lighter, but correctness checks are still required: provide a reproducible way to run it and an expected result. The result must be trustworthy and must not be mistakenly merged into the main branch.
- **Maintained** (feature, bug fix, refactor, or behavior change entering the main branch): use full red-green-refactor. Every behavior change starts with a failing test.

Default to maintained when code will be merged, depended on by others, or maintained over time. Use throwaway only when it will be run temporarily and discarded without being committed. If uncertain, use maintained.

### Rigor Levels

| Level | Use when | Behavior |
| --- | --- | --- |
| **lite** | Exploration or prototype; only prove the path works | Provide the simplest implementation and mention one lazier alternative for the user to choose. |
| **full** (default) | Maintained code or work entering the main branch | Climb the ladder, use red-green-refactor, and keep the diff minimal. |
| **ultra** | Simplifying a refactor or deleting more than adding | Delete first, challenge the requirement, and minimize what remains. |

The level changes how aggressively to reduce scope, not the requirement to verify. Even `lite` needs a reproducible run and expected output.

## Workflow

1. **Clarify the requirement:** Establish what must happen, how acceptance will be measured, and what is explicitly out of scope. Ask the requester when this is unclear; do not code against ambiguity. See `references/clarify.md`.
2. **Climb the ladder:** For each requirement, check the seven alternatives and stop at the first adequate one. Briefly explain what will not be written and why an existing platform or standard-library option is sufficient. See `references/ladder.md`.
3. **Write an implementation plan:** Split the work into independently verifiable tasks, each with its test loop and commit-sized intent. Name files, interfaces, code changes, and verification commands, then challenge the plan. See `references/planning.md`.
4. **Implement each task with red-green-refactor:** Write a failing test, confirm the failure, add the minimum implementation, confirm it passes, then commit when authorized. Do not skip this for maintained work. Characterize legacy behavior before changing untested code. See `references/tdd.md`.
   Continue through authorized work without requesting approval at every step. Track progress and decisions in the execution log; pause only when required authority or information is missing. See `references/planning.md`.
5. **Debug in four phases:** Investigate the root cause, analyze the pattern, form and test hypotheses, then fix and run regression checks. See `references/debugging.md`.
6. **Verify before delivery:** Run the required tests, build, and lint; read the output; and check every acceptance criterion before claiming completion. See `references/verification.md`.
7. **Hand off for review:** Summarize the change and evidence, then hand the branch or PR to `code-review` for independent review. Do not approve your own work.

## Output

Concisely report: (1) the clarified requirement and acceptance criteria; (2) the implementation task plan; (3) each task's red-green evidence (failure reason and passing result); (4) full verification commands and results; (5) issues found during self-check and their fixes; and (6) items for `code-review`. Never describe a check that was not run as passing.

## Red Flags

- Starting implementation design without climbing the ladder; the code may not need to exist.
- Coding before the requirement is clear, or using “it works” as the only acceptance criterion.
- Writing code without a plan, or leaving placeholders such as “TODO” or “handle errors appropriately” in the plan.
- Writing production code before tests in maintained work.
- Saying tests “should pass” without running them.
- Changing several things at once without making each step independently verifiable.
- Changing a bug without reproducing it or identifying its root cause, or trying many fixes at once.
- Silently changing course when reality conflicts with the plan; record a Ruling for each departure.
- Editing untested legacy code without first adding a characterization test.
- Delivering debug leftovers (`print`, `console.log`, commented-out code) or bypassing checks with `--no-verify`.
- Declaring the work problem-free without handing it to `code-review`.

## References

Read only what the task needs:

- `references/clarify.md`: requirement questions and design-tree challenge.
- `references/ladder.md`: the seven-step ladder to check before writing code.
- `references/planning.md`: plan structure, no-placeholder rule, self-checks, execution log, Rulings, and stop conditions.
- `references/tdd.md`: red-green-refactor, test examples, legacy-code strategy, and counterexamples.
- `references/debugging.md`: four-phase debugging, instrumentation, bisection, and layered verification.
- `references/verification.md`: pre-delivery gates, diff self-review, and common false evidence.

# Systematic Debugging

Rule:

```text
No root-cause investigation, no fix. Treating a symptom is failure.
```

## When to Use It

Use this for any technical problem: failing tests, production bugs, unexpected behavior, performance issues, build failures, or integration problems. It is especially important when rushed, when the fix seems obvious, after several attempts, when a previous fix did not work, or when you do not fully understand the problem. These are exactly the moments when guess-and-patch is most likely to mislead you.

Do not skip it because the problem “looks simple” or time is short. A systematic approach is faster than repeated guessing.

## Four Phases

Finish each phase before proceeding.

### Phase 1: Investigate the Root Cause (Before Editing)

Observe first, then act. Before changing code, use runtime evidence to answer where the problem is. Capture a screenshot, inspect logs and console output, or examine DOM state. Let evidence form the hypothesis; do not jump directly to a fix based on instinct.

1. **Read the complete error:** the full message, warnings, stack trace, line, file, and code often contain the answer.
2. **Reproduce consistently:** identify the trigger and whether it always occurs. If it cannot be reproduced, gather more evidence before guessing.
3. **Inspect recent changes:** review `git diff`, recent Commits, new dependencies, configuration changes, and environmental differences. If the introducing Commit is unknown, use `git bisect` to halve the history search. For a code region, use comments/toggles and binary isolation instead of guessing line by line.
4. **Instrument multi-component systems:** capture data at each boundary to locate which layer first changes it, then investigate that layer.
5. **Trace data backward:** where did the bad value originate, and who called the function with it? Record the value at each layer until you reach the source. Fix the source, not the symptom. If manual tracing stalls, add temporary instrumentation before the suspicious operation (key variables and a stack such as `new Error().stack`), run once, then remove it as soon as the cause is located.

### Phase 2: Analyze the Pattern (Find the Difference)

1. Find a comparable path in the codebase that works.
2. Read the full reference implementation line by line rather than skimming.
3. List every difference between the working and failing paths; do not dismiss one as unimportant without evidence.
4. Identify dependencies, components, configuration, environment, and assumptions.

### Phase 3: Hypothesize and Verify

1. State one hypothesis at a time: “I believe X is the root cause because Y.”
2. Make the smallest change that can test it; change one variable at a time.
3. Verify before continuing. If it works, go to Phase 4. If it does not, form a new hypothesis instead of layering patches onto the old one.

### Phase 4: Fix and Regress

1. Fix the root cause. Find all callers of the function/module and confirm every path passes through the fix. Fix the shared layer once rather than only the path where the issue appeared.
2. Add or update tests and run the red-green cycle (see `tdd.md`).
3. **Validate at trust boundaries:** for bugs caused by invalid data, check whether other call paths can bypass the fix. Reject invalid external input and protect required business invariants. Add checks only at independently meaningful boundaries, not mechanically at every layer. Use regression tests to prove the paths covered; do not claim layered validation eliminates all related bugs.
4. Run the full relevant suite and checks, confirm no regression, and remove all temporary instrumentation.

## Common Counterexamples

| Behavior | Why it is wrong |
| --- | --- |
| Editing before reading the full error | The answer may already be in the error details. |
| Fixing without reproducing | You may be fixing a different bug. |
| Trying three fixes at once | You cannot tell which one worked. |
| Patching a symptom | The root cause remains and another path can still fail. |
| “I'll just change it and see” | That is chance, not debugging. |
| Skipping regression tests | The old bug may return in another form. |

## The Only Way Out When Stuck

Stop adding patches. Return to Phase 1 and gather evidence again, or ask the requester/another engineer for help with the hypotheses already ruled out and the evidence collected. Do not keep guessing.

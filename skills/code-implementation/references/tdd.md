# TDD: Red, Green, Refactor

The RED requirement applies to new behavior and bug fixes. When adding tests for correct existing behavior, an initial pass may be valid. Use `code-testing` to assess the cases and their sensitivity; do not remove existing implementation just to manufacture a failure.

Rule for maintained work:

```text
No production code before a test has failed first.
```

Start new behavior with a failing test. For code already written, preserve the worktree baseline and verify the test gap; do not delete or revert user code just to record a TDD cycle retroactively.

## When to Use It

Always for new features, bug fixes, refactors, and behavior changes.

Exceptions (ask the user first): one-off prototypes, generated code, or pure configuration. “Can I skip it just this once?” is not an exception; stop and ask.

## Red-Green-Refactor Cycle

### 1. RED: Write a Failing Test

Write the smallest test that states the expected behavior:

- Test one behavior at a time; “and” in the test name often means the test should be split.
- Use a behavioral name, not `test1`.
- Use real code when possible. A mock only tests the mock, not the implementation.

```python
# Good: clear name, real behavior, one concern
def test_retry_succeeds_after_third_attempt():
    attempts = 0
    def operation():
        nonlocal attempts
        attempts += 1
        if attempts < 3:
            raise RuntimeError("fail")
        return "success"

    assert retry_operation(operation) == "success"
    assert attempts == 3

# Bad: vague name, only checks mock call count
def test_retry_works():
    fn = mock.Mock(side_effect=[RuntimeError, RuntimeError, "success"])
    retry_operation(fn)
    assert fn.call_count == 3
```

### 2. VERIFY RED: Confirm the Failure (Required)

Run the single test and confirm:

- It fails rather than errors, with the expected message.
- It fails because the behavior is missing, not because of a typo.

**New-behavior test passes immediately?** Check whether the behavior already exists or the assertion misses it; do not change a correct expectation to manufacture RED. **Test errors?** Distinguish a missing feature from spelling, import, or environment errors; the latter are not RED evidence.

### 3. GREEN: Minimum Implementation

Write the least code that makes the test pass.

```python
# Good: only enough to pass
def retry_operation(fn):
    for i in range(3):
        try:
            return fn()
        except RuntimeError:
            if i == 2:
                raise

# Bad: unrequested design
def retry_operation(fn, *, max_retries=3, backoff="linear", on_retry=None):
    ...
```

Do not add features, opportunistically refactor unrelated code, or improve beyond the test's requirement.

### 4. VERIFY GREEN: Confirm the Pass (Required)

Run the test and confirm it passes, other tests still pass, and output is clean of errors/warnings. If the test fails, fix the code, not the test. Fix regressions now.

### 5. REFACTOR: Clean Up

Only after the suite is green, remove duplication, improve names, or extract helpers without adding behavior. Keep tests green, then write the next failing test.

## Good Tests and Bad Tests

| Dimension | Good | Bad |
| --- | --- | --- |
| Minimal | One behavior per test; split names containing “and.” | `test_validates_email_and_domain_and_whitespace` |
| Clear | Name describes behavior. | `test1` |
| Intent-revealing | Shows the intended API. | Hides what the code should do. |

## Common Excuses

| Excuse | Reality |
| --- | --- |
| “It is too simple to test.” | Simple code still breaks. A test takes seconds; a fix may take hours. |
| “I'll implement first and test later.” | That is not TDD; the test may only prove the existing implementation works the way it is written. |
| “A mock is enough.” | It proves only that the mock was called as expected, not that behavior is correct. |
| “Tests slow us down.” | Untested code makes every later edit risky. The delay comes from missing tests, not their presence. |
| “Skip it this time.” | Each “this time” becomes the next time. Exceptions come from the user, not the implementer. |

## When Stuck

| Problem | Response |
| --- | --- |
| Unsure what to test | Write the desired API and assertion first; if still unclear, clarify the requirement. |
| Test is too complex | The design may be too complex; simplify the interface. |
| Everything needs mocking | The code may be too coupled; consider dependency injection. |
| Test setup is huge | Extract test helpers; if it remains complex, simplify the design. |

## Bug Fixes and Regression

When a bug is found, first write a failing test that reproduces it, then follow red-green-refactor. The test proves the fix and prevents regression. **Do not fix bugs without tests.**

## Legacy Code: Characterize Before Changing

Do not make a blind change to untested code; you cannot know what you broke.

1. **Write a characterization test first:** do not modify production code. Capture what the code actually outputs today, even if odd. Record the fact before judging it. Then use the safety net to change behavior through red-green.
2. **Use the Mikado Method for large refactors:** record the worktree baseline, then attempt the target change. If compilation or tests expose prerequisites, undo only changes introduced by this experiment, preserve user and other-task changes, and record the prerequisite as a task. Repeat recursively until you find a leaf task that can be completed independently, then integrate bottom-up. Keep main green instead of relying on a long-lived refactor branch.

> References: Michael Feathers, *Working Effectively with Legacy Code* (seams and characterization tests); Ola Ellnestam and Daniel Brolund, *The Mikado Method*.

## Test Behavior, Not Implementation

- Assert observable input/output and side effects, not private methods, internal call counts, or implementation steps. If an ordinary refactor requires rewriting many tests, those tests may be protecting implementation instead of behavior.
- Answer “Is the behavior correct?” rather than “Was the code written the way I expected?”
- **Flaky tests are defects.** If the same test intermittently fails without code changes, repeat it to confirm (for example `pytest --count` or `go test -count`), then isolate ordering, time, randomness, shared state, and concurrency. A rerun that happens to pass leaves uncertainty for the next person.

## Exit Checklist

- [ ] Every new function/method has a test.
- [ ] Every applicable test was observed failing first.
- [ ] Failures were due to missing behavior, not typos.
- [ ] Minimum code made each test pass.
- [ ] All tests pass with clean output.
- [ ] Tests use real code except at necessary boundaries.
- [ ] Relevant boundaries and error branches are covered.
- [ ] Legacy code was characterized before changes.
- [ ] Intermittent failures were investigated rather than hidden by reruns.

If any applicable item is unchecked, the TDD cycle is incomplete; revisit it.

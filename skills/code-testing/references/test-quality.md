# Assertions, Test Doubles, and Test Effectiveness

## Assertions Must Distinguish Errors

- Keep Arrange/Act/Assert clear. Name the scenario and expected result. Multiple related assertions for one behavior are fine; do not mechanically require one `assert`.
- Derive expected values from explicit contract examples, independent fixtures, or a reference model. Do not call the subject, a production helper, or a copied algorithm to compute expected results.
- On success, assert the exact result and required side effects. On rejection, assert error semantics and forbidden writes/sends. A status code or `toBeDefined()` alone does not prove business correctness.
- Scope exception assertions to the expected type and relevant error code; do not pass after catching any exception. Await/return async calls to the runner and assert rejections too.
- Before asserting that an async side effect did not happen, confirm its task ended, cancellation took effect, or controlled scheduler completed. A count of zero immediately after issuing work may only mean it has not run yet.
- Internal call counts are often brittle, but an explicit contract such as “send at most one notification” or “do not charge” may justify a spy. Do not ban interaction assertions categorically.
- Snapshot only stable, meaningful output. Explain each diff before updating; do not turn a regression into a new baseline without review.

## Test Doubles and Fixtures

- Use the real implementation for the subject. Replace boundaries such as clocks, randomness, or external networks; state which layer is simulated and what remains unproven.
- Use stubs for controlled input, spies for necessary side effects, and fakes for simplified dependencies. Choose the least configuration; avoid building a mock world as complex as the implementation.
- If an external dependency is mocked for success, simulate rejection, timeout, and malformed responses when the risk requires it. Real database/protocol behavior needs matching integration or contract tests; mocks cannot prove it.
- Isolate fixtures per test. Always tear down resources the test created, even on assertion failure. Do not let one test create an object that a later test relies on.
- Use a fixed clock/controlled scheduler, temporary directories, and unique resource names. Restore environment variables, global state, and monkeypatches. Do not connect to production data or real billing services for tests without authorization.
- Helpers should encapsulate repeated setup only. Keep key inputs and expected results near the test so readers do not need to trace layers of fixtures to understand it.

## Prove That Tests Catch Defects

Choose one method according to the goal, preferring a real defect; effort should scale with risk:

1. **Real regression:** In a safe isolated copy, keep the new test and verify it fails on the defect version and passes on the fix. The failure must point to the target behavior; import failures, missing dependencies, or schema mismatches do not count.
2. **Focused mutation:** Establish a green baseline. In an isolated copy, break a key comparison, remove deduplication/permission logic, or drop a write, then run affected tests and observe the intended assertion fail. Make one meaningful mutation at a time, restore it, and rerun the original.
3. **Automated mutation tool:** Use an existing tool or one appropriate for an important module. Analyze survived/no-coverage mutants; explain equivalent mutants separately. Compilation failures are not evidence of effective assertions. Do not exclude genuine gaps merely to improve a score.

Do not use reset/checkout to revert an entire user worktree. Record the actual mutation, affected cases, failure reason, and result after restoration. If not performed, say so rather than claiming it would fail in theory.

## Runnable Example: Can the Boundary Error Be Detected?

This is a standalone demonstration, not a requirement that the target repository use these APIs.

Contract: order values are in cents; orders of 10,000 cents or more get free shipping, otherwise the fee is 600 cents; negative subtotals raise `ValueError`. Save the correct implementation as `shipping.py`:

```python
def shipping_fee(subtotal_cents):
    if subtotal_cents < 0:
        raise ValueError("subtotal must be non-negative")
    return 600 if subtotal_cents < 10000 else 0
```

Save the test as `test_shipping.py`; it runs with the Python standard library:

```python
import unittest

from shipping import shipping_fee


class ShippingFeeTests(unittest.TestCase):
    def test_fee_at_free_shipping_boundary(self):
        for subtotal, expected in ((9999, 600), (10000, 0), (10001, 0)):
            with self.subTest(subtotal=subtotal):
                self.assertEqual(shipping_fee(subtotal), expected)

    def test_negative_subtotal_is_rejected(self):
        with self.assertRaises(ValueError):
            shipping_fee(-1)
```

Run `python3 -m unittest -v`. In an isolated copy, change `< 10000` to `<= 10000`: the 10,000-cent subtest should fail with `600 != 0`, proving it detects the boundary defect. If only 9,999 and 10,001 are tested, both implementations pass; branch execution alone did not cover the boundary.

Counterexample: `self.assertEqual(shipping_fee(10000), shipping_fee(10000))` has no independent expected value. Asserting only that the result is an integer also does not verify the free-shipping contract.

# The Seven-Step Ladder: Ask Before Writing Code

Understand the problem first, then climb the ladder before writing any code. Stop at the first rung that **actually covers the requirement**, not merely one that seems plausible.

## Seven Rungs

| Rung | Question | Action |
| --- | --- | --- |
| 1 | **Does this need to exist?** | Reject an arbitrary requirement; briefly explain why no code is needed. |
| 2 | **Does the repository already provide it?** | Search helpers, utilities, types, and existing patterns before writing; the solution may be nearby. |
| 3 | **Is it in the standard library?** | Prefer `pathlib` to `os.path`, `datetime` to handwritten time parsing, and similar standard primitives. |
| 4 | **Is it native to the platform?** | Prefer `<input type="date">` over a date-picker package, CSS over JavaScript animation, and database constraints over application-only validation. |
| 5 | **Can an installed dependency solve it?** | Check `package.json`, `requirements.txt`, and equivalents; do not add a dependency for one helper function. |
| 6 | **Can it be solved in one line?** | Prefer one line over a new file when that remains clear and correct. |
| 7 | **None of the above: write it** | Implement the smallest working behavior. Omit anything not requested. |

If two rungs both apply, choose the higher one and stop.

## How to Use the Ladder

- The ladder is a **habit**, not a research project. First understand the requirement, inspect affected code, and run through the real workflow; only then climb.
- Be **lazy about the solution, not about understanding**. Chasing a tiny diff without understanding the code is the dangerous kind of laziness.
- If two rungs work, use the higher one rather than debating it.

## Never Remove These

These never belong on the ladder:

- Validation at trust boundaries.
- Error handling that prevents data loss.
- Security controls (authentication, authorization, encryption).
- Accessibility foundations.
- Anything the user explicitly requested.

The ladder removes unnecessary complexity, not necessary safety.

## Intentional Simplification

When a simplification sacrifices generality or performance but is sufficient now, add a comment that names the ceiling and upgrade path:

```python
# ponytail: Uses a global lock; switch to per-account locks if throughput becomes a bottleneck.
```

An intentional simplification without a comment is a hidden trap.

## Output

After implementation, use at most three lines to say what was omitted and when it should be added. If the explanation is longer than the code, shorten it; a long defense of a simplification smuggles complexity back into the documentation.

## Counterexamples

| Behavior | Missed rung |
| --- | --- |
| Install `flatpickr` and build a component when a date picker is requested | Rung 4: `<input type="date">` |
| Write a `leftPad` helper | Rung 3: `String.prototype.padStart` |
| Add `lodash` just for `_.cloneDeep` | Rung 5: `structuredClone` may already be available in the standard library/runtime. |
| Define an interface and implementation because there might be a second one later | Rung 1: the future use may never arrive. |

---

> Inspired by [ponytail](https://github.com/dietrichgebert/ponytail) by Dietrich Gebert: “The best code is the code never written.”

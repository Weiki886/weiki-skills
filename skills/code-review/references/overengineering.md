# Over-Engineering Review

Review only unnecessary complexity in the change, not correctness, security, or performance; those belong to the main review. The goal is to remove what is not needed and keep the diff as small as possible.

## Five Labels

| Label | Meaning | Replacement |
| --- | --- | --- |
| `delete:` | Dead code, unused flexibility, or an invented feature | None; remove it. |
| `stdlib:` | Handwritten behavior already in the standard library | Name the standard-library function. |
| `native:` | Dependency/code reimplements platform-native behavior | Name the native capability. |
| `yagni:` | An abstraction with one implementation, unused configuration, or one-caller layer | Inline it until a second real use appears. |
| `shrink:` | Equivalent logic could use fewer lines | Show the shorter version. |

## Output Format

One line per finding: location + label + what to remove + replacement.

`L<line>: <tag> <what to remove>. <replacement>.` For multiple files, use `<file>:L<line>: ...`.

End with one metric: `net: -<N> lines possible.` If nothing should be removed, say `Lean already. Ship.`

## Examples

- `L12-38: stdlib: 27-line email validator. A one-line "@" check is enough for this use; real validation means confirming the address.`
- `L4: native: Added moment for one formatting call. Use Intl.DateTimeFormat; zero dependencies.`
- `repo.py:L88: yagni: AbstractRepository has one implementation. Inline it until a second exists.`
- `L52-71: delete: Retries an idempotent local call. No replacement.`
- `L30-44: shrink: Handwritten loop builds a map. Use dict(zip(keys, values)).`

## Boundary

Remove only unnecessary complexity. Do not review correctness defects, security vulnerabilities, or performance issues here. A smoke test/assertion is the minimum and is not over-engineering; never recommend deleting it. Report only; do not make changes.

---

> Inspired by [ponytail](https://github.com/dietrichgebert/ponytail): “The best code is the code never written.”

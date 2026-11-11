# Baseline for Design Smells

Two constraints:

- **Repository first:** Explicit repository conventions override this baseline. If the repository deliberately uses a pattern, suppress the corresponding smell instead of using this file to overturn local policy.
- **Questions, not verdicts:** Phrase each item as “possible X,” not “violation of X.” Do not repeat checks enforced by tools, such as formatting, lint, and types.

For each smell, ask: **What is it, and how could it be changed?** Match against the diff; skip what does not apply.

## Classic Smells (Fowler, *Refactoring*, Chapter 3)

| Smell | What it is | Possible remedy |
| --- | --- | --- |
| Mysterious name | A function, variable, or type name does not communicate its behavior or contents. | Rename it; if no honest name exists, the design may be unclear. |
| Duplicated code | The same logic shape appears in multiple hunks/files. | Extract the shared behavior and call it from both. |
| Feature envy | A method uses another object's data more than its own. | Move it closer to the data it uses. |
| Data clump | The same fields/parameters repeatedly appear together, suggesting a missing type. | Group them into a type and pass that type. |
| Primitive obsession | A primitive/string stands in for a domain concept that deserves a type. | Give the concept a small dedicated type. |
| Repeated switch | A `switch` or `if` chain over the same type repeats in the change. | Consider polymorphism or a shared mapping. |
| Shotgun surgery | One logic change forces edits across many unrelated files. | Co-locate the things that change together. |
| Divergent change | One file/module changes for several unrelated reasons. | Split responsibilities so each module changes for one reason. |
| Speculative generality | Abstractions, parameters, or hooks support scenarios not in the requirement. | Remove them; inline until a real need appears. |
| Message chain | Callers depend on navigation such as `a.b().c().d()`. | Hide the traversal behind a method on the first object. |
| Middle man | A class/function mostly forwards calls. | Remove it and call the real target directly. |
| Refused bequest | A subclass/implementation ignores or overrides most inherited behavior. | Prefer composition over inheritance. |

## Smells Often Seen in Generated Code

Scrutinize these patterns when reviewing generated code:

| Smell | Example | Possible remedy |
| --- | --- | --- |
| Hallucinated API | A plausible method, parameter, or setting does not exist. | Check official docs and actual type definitions. Do not trust appearances. |
| Excessive defensiveness | `try/catch`, optional chaining, and defaults everywhere hide errors that should surface. | Catch only at real boundaries; internal invariant failures should be visible. Silent fallback can hide bugs. |
| Commentary that repeats code | `// Iterate through users` immediately precedes a loop. | Remove repetition; keep comments that explain why. |
| Textbook abstraction | An interface, factory, and strategy wrapper exist for a single call site. | Inline until a real second use exists. |
| Repository-style break | Names, error handling, logs, or directories differ from established patterns. | Follow the repository instead of adding a second style. |
| Fabricated test | A tautological assertion (`expect(result).toBeDefined()`) or a mock replacing the subject logic. | Make the test fail when the behavior is broken. |
| Apparently complete implementation | Happy path is polished, but boundaries, errors, and concurrency are empty or TODO. | Walk correctness risks and ask about each edge/error path. |
| Invented numbers/facts | A comment or doc states unsourced performance, version, or limits. | Require a source or remove the claim. |

## Potential Blockers

Start as a `question` with a simpler alternative. Escalate to `blocking` only when the change demonstrably worsens the structure:

- A refactor moves complexity without reducing it.
- The change pushes a file beyond a healthy size without splitting it.
- Feature logic is added to a shared module.
- It duplicates a near-identical canonical implementation.
- Silent fallback hides an invariant no one can explain.

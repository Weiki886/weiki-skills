# Clarify Requirements

Ambiguous requirements are a leading cause of rework. Before coding, turn the request into an **acceptance-ready** checklist. If that cannot be done, keep asking.

## Seven Questions Before Implementation

1. **What problem are we solving?** What is the user's or business's underlying goal? This is not merely “which page needs a button?”
2. **What is the acceptance criteria?** How will we prove it is done? Prefer 3–10 independently checkable items over “it works.”
3. **What are the boundaries?** State what is excluded. Keep “while we're here” changes out of scope.
4. **What goes in and comes out?** Where does data come from, what shape is it, and who receives it? What happens for invalid input?
5. **What are the constraints?** State the stack, minimum versions, performance/compatibility requirements, and conventions. Preserve exact values from the request.
6. **What are the failure modes?** What happens if this logic fails, and is that acceptable?
7. **How can it be verified?** How will we produce local evidence that it is correct or incorrect?

## Challenge with a Decision Tree

When a requirement involves several decisions, do not ask a long list of questions all at once. Represent the decisions as a tree, with dependent decisions under their prerequisites.

- **Frontier:** all decisions whose prerequisites are already settled and which can be asked now.
- Ask only about the current frontier in each round. Number each question and provide a recommendation; wait for answers before moving to the next round.
- Defer questions whose prerequisites remain undecided.

| Work type | Owner |
| --- | --- |
| Fact-finding (repository state, tool behavior, environment) | You: inspect it instead of asking the user; report the gap only when it cannot be determined. |
| Decisions (direction, tradeoffs, priority) | User/requester: present the options and let them decide. |

When the frontier is empty, every decision branch has been resolved and no assumption has been silently defaulted. Only then write the implementation plan.

## Red Flags for Ambiguity

- “Optimize it” or “make it better” without observable measures.
- “Make it like X” without a reference or explanation of what should differ.
- Acceptance criteria that cannot be objectively judged.
- Scope that keeps growing as the discussion proceeds.

## Output

Produce a confirmed requirement block containing at least: one-sentence goal, checkable acceptance criteria, explicit non-goals, and key constraints. Use this block as the shared basis for the plan and final acceptance.

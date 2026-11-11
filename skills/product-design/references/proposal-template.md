# Proposal Templates

Choose MiniSpec or FullSpec based on impact. Do not burden a small change with a FullSpec, and do not leave a major change at MiniSpec depth. Both sizes must make trade-offs explicit; only the level of detail differs.

## MiniSpec (Small Change)

Use a concise template when the decision is clear and the impact is limited:

```markdown
### Proposal

<State the change directly: new behavior, API, command, rule, or user-visible outcome.>

### Context

<Why is this needed? Link the Issue, user story, current behavior, or pain point.>

### Current Approach

<How do users solve this today, and why is it insufficient? Write "None" if there is no current approach.>

### Trade-offs and Boundaries

<Alternatives, the selected option and rationale, and what is explicitly out of scope this time. One sentence per item is enough.>

### Acceptance Criteria

<At least one observable, verifiable outcome, including relevant boundaries.>
```

Even a non-trivial MiniSpec must explicitly state what is out of scope.

## FullSpec (Complete Proposal)

Use the full structure for changes with lasting or broad impact:

````markdown
# Proposal: <Specific Change>

## 1. Summary

<In one paragraph: the problem, the user-visible outcome, and the feature's product role.>

## 2. User Stories

- <Concrete scenario 1>
- <Concrete scenario 2>
- <Concrete scenario 3>

**Shared need:** <The common problem behind these scenarios, in one sentence.>

## 3. Target Users

<Who is this for?>

## 4. Current Approach and Its Limitations

- <Current approach 1> -> <Main limitation>
- <Current approach 2> -> <Main limitation>

<Write "None" if there is no current approach.>

## 5. Goals and Scope

- <Goal 1>
- <Goal 2>

## 6. Explicitly Out of Scope

- <Explicit exclusion 1>
- <Explicit exclusion 2>

## 7. Solution

### 7.1 Core Rule

<State the most important product or engineering rule in one sentence.>

### 7.2 Key Decisions and Rationale

| Design Point | Alternatives | Choice | Rationale (user value / cost / risk / scope) |
| --- | --- | --- | --- |
| <Design point 1> | <Option A / Option B> | <Choice> | <Evidence, including competitors or alternatives> |

### 7.3 Core Concepts and Information Structure

<Define the core domain concepts and how information or data is organized. This informs architecture and should be clear enough for an architect to take over.>

## 8. Examples as Specification

**Example 1: <Example name>**

```text
User:
<User input or action>

System:
<System behavior or response>
```

<For every key behavior, cover the current state, proposed state, and boundary or invalid cases. Given/When/Then works well for interactions.>

## 9. Edge Cases

| Scenario | Behavior |
| --- | --- |
| <Edge case 1> | <How the system handles it> |
| <Edge case 2> | <How the system handles it> |

## 10. Prototype / Demo

<For products with an interface, attach a prototype or link created after the text is finalized. Remove this section for interface-free products.>

## 11. Acceptance Criteria

- <A concrete example that can be independently verified and is technology-agnostic.>
- <Describe what users can observe, not which classes or functions were implemented.>
````

## Field-Level Guardrails

- Do not leave "Explicitly Out of Scope" or "Current Approach and Its Limitations" blank; write "None" when applicable.
- Acceptance criteria are not an implementation checklist or exhaustive test suite. Keep only conditions whose failure should block acceptance.
- Every key decision must include at least one rejected alternative and why it was rejected. A "decision" with only one option is not a trade-off.
- Mark uncertain numbers, competitor behavior, and user pain points as `TBD`, and state how they will be verified.

# Handling Review Feedback

## Workflow

1. Read all feedback and restate anything unclear. Ask about ambiguity before making a partial change based on a guess.
2. Verify claims with code, tests, or documentation, then decide whether the feedback is correct and suitable for this repository.
3. If valid, change the code. If not, reply with evidence, possible impact, and an alternative.
4. State the outcome of each item: fixed (include location/Commit), deferred (include reason and Issue), or declined (include technical rationale).
5. Verify each change. After all `blocking` items are closed, request another review and explain the changes and evidence.

## Response Principles

- Discuss code, not people.
- Use evidence instead of “I think,” and explain the concrete consequence.
- Ask when uncertain; do not substitute agreeable language for technical judgment.
- Explicitly state that `nit` and `fyi` items are non-blocking.
- When the request is clear, act directly; avoid performative thanks or promises.

## Disagreements

Distinguish factual disagreements from tradeoff disagreements. Resolve facts with reproduction, tests, or documentation. For tradeoffs, follow repository conventions; when none exist, the author chooses. If still unresolved, ask a maintainer or third party instead of prolonging an argument.

## Rebuttal Format

```text
Evidence: <current code, configuration, or test evidence>
Impact: <specific problem caused by making the suggested change>
Alternative: <how to handle it if the concern is valid>
```

## Avoid

- Agreeing completely or promising a fix before verifying the claim.
- Treating preference as a blocker, or downgrading a real defect to `nit` to be agreeable.
- Reporting a problem without a next step.
- Raising many style issues while missing correctness, security, or requirement issues.

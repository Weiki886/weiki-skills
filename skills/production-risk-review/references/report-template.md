# Production Merge-Risk Review

Lead with an actionable decision. Do not repeat checklist items that passed without findings.

```markdown
## Production Merge-Risk Review

### Scope

- PR: <URL or number>
- Base: `<full-sha>`
- Head: `<full-sha>`
- Change: N commits, M files
- Deployment impact: <automatic / manual / merge only / unknown>
- Data impact: <none / schema / backfill / deletion / unknown>

### Verdict

**Blocked | Conditional | Ready** — <Can this merge now? State the most important risk.>

### Production Risks

| ID | Severity | Location | Trigger | Impact and Recovery | Evidence / Action |
| --- | --- | --- | --- | --- | --- |
| R1 | P1 | `path/file:42` | ... | ... | ... |

List only risks introduced or amplified by this diff. Order `P0 -> P1 -> P2 -> P3`. Do not present unsupported speculation as a finding.

### Required Before Merge

1. [ ] <Condition>; pass criterion: <observable evidence>
2. [ ] <Condition>; pass criterion: <observable evidence>

For `Ready`, write "None". For `Conditional`, do not merge until each condition is met.

### Release and Recovery

- Release order: <actual sequence for migration / service / worker / flag>
- Canary or flag: <scope, expansion criteria, and stop mechanism>
- Rollback trigger: <metric, threshold, and observation window>
- Rollback steps: <actionable steps and expected recovery>
- Post-release verification: <key business, system, database, and queue metrics>

If no release operation is required, say "Code merge only; no additional release ordering identified." Do not invent steps.

### Verification Evidence

- <Actual PR head, CI, key tests, migration checks, static analysis, etc.>

### Unable to Verify

- <Unavailable production config, real data volume, observability, etc.; explain whether it changes the verdict.>
```

## Writing Requirements

- Explain what could go wrong for each finding; do not merely suggest adding validation.
- Every `Conditional` requirement must be objectively verifiable before merge or release.
- A `Ready` verdict still records real production boundaries and residual risks; never claim absolute safety.
- Never expose secret values. Refer only to paths, variable names, credential types, and redaction status.

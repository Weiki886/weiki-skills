---
name: production-risk-review
description: Use to decide whether a PR targeting production is safe to merge or release. Focus on database deletion and migrations, data corruption, system crashes, blast radius, configuration and rollout order, and failed rollback; return Blocked, Conditional, or Ready. This is the second pre-release gate; use code-review for general standards, maintainability, and requirement compliance.
metadata:
  short-description: Review production risks before merge
---

# Production Risk Review

Answer one question: **Can this PR safely merge and enter production now?** Review the consequences of failures after merge and the ability to recover; do not repeat a general review of the entire diff.

## Hard Constraints

- Read-only by default. Do not deploy, run production migrations, change data or configuration, toggle releases, or call side-effecting external services. Run local tests only when repository policy permits and no production credentials are involved.
- Pin the review to the base, head, and full head SHA. Any PR update invalidates the prior conclusion. Never silently mix dirty worktree changes into a review of a committed PR.
- Required CI must have actually run for the current head SHA. Pending, failed, or unexplained skipped checks do not pass. A green check on an earlier Commit cannot clear a new head.
- For every risk, state its trigger, blast radius, evidence, and recovery path. Not finding a counterexample in code does not prove production safety; mark critical production facts not confirmed from the repository as unknown.
- Do not nitpick style, naming, or general maintainability, and do not repeat full requirement-compliance review. Include logic defects only when they can cause data errors, outages, security issues, or uncontrolled release behavior.
- Do not declare Ready merely because tests are green, CI passes, or dangerous keywords were not found. These are evidence, not proof that migrations, configuration, capacity, and rollback are ready.

## Boundary with `code-review`

| Scenario | Skill |
| --- | --- |
| Check standards and requirements, ordinary bugs, test gaps, or maintainability | `code-review` |
| Check for data deletion, outages, blast-radius expansion, or failed rollback after production merge | `production-risk-review` |

Complete the general code review first. If the user requests only production risk review, constrain logic analysis to production consequences and avoid producing a duplicate review list.

## Review Workflow

### 1. Pin the Release Candidate

Record known facts; do not guess unknowns:

- PR/branch, base, full head SHA, commit count, and changed files.
- Deployment method, target environment, and whether merge triggers automatic deployment.
- Whether the change includes migrations, backfills, bulk SQL, DDL, deletion, or data rewrites.
- Changes to configuration, secrets, dependencies, infrastructure, queues, caches, scheduled jobs, or external contracts.
- Feature flags, canary/gradual rollout, service ordering, and rollback path.
- Linked Issue, acceptance criteria, runbooks, SLOs, and required checks on the current head.

If PR metadata or repository evidence is unavailable, mark it unknown. Do not treat a local branch as the remote PR; read PR state in a read-only manner when necessary.

### 2. Map the Blast Radius

Trace real code paths from entry points, not only changed lines:

1. Which users, tenants, regions, and data are affected?
2. Does the change touch authentication, payments, orders, permissions, or another critical business path?
3. Does new behavior take effect at merge, deployment, first request, or migration completion?
4. Can old and new versions, clients, messages, and data coexist?
5. Could the worst case affect one request, tenant, partition, service, or the whole system?

Read [risk-checklist.md](references/risk-checklist.md) for relevant areas. For databases, backfills, rollout order, or rollback, also read [data-and-release.md](references/data-and-release.md).

### 3. Construct and Verify Failure Scenarios

For each candidate risk, answer:

- **Trigger:** What input, data state, concurrency order, configuration, or deployment step could cause it?
- **Likelihood:** Is it demonstrably reachable in code, or does it depend on unknown production conditions?
- **Impact:** Is the data recoverable? How much service is affected? Are money, permissions, or compliance involved?
- **Detectability:** How quickly would tests, error rates, logs, metrics, or user reports reveal it?
- **Recoverability:** Can the team disable a flag, roll back, roll forward, restore a backup, or repair manually? How long would it take?

Prioritize irreversible outcomes and expensive recovery. Do not use “unlikely” to hide a high-impact path without a recovery plan.

### 4. Assign Severity and Gate Result

| Risk | Decision | Example |
| --- | --- | --- |
| `P0` | `Blocked` | Broad data loss/corruption, total or core service outage, authentication or financial-safety failure, with no executable recovery plan. |
| `P1` | `Blocked` | Local service crash, incorrect data, blast-radius expansion, incompatible migration, or rollback that damages data again. |
| `P2` | `Conditional` | Controllable risk that still requires a specified confirmation, monitor, canary, feature flag, or rehearsal before release. |
| `P3` | `Advisory` | Non-blocking advice that does not change the merge decision. |

Use only these conclusions:

- `Blocked`: an unresolved P0/P1 exists, or a critical production fact is missing such that a core risk cannot be assessed. Do not merge.
- `Conditional`: no P0/P1 is demonstrated, but every stated condition must be met before merge/deploy; do not release until then.
- `Ready`: no P0/P1 is found in the diff, P2 conditions have evidence, required CI passed for the current head, and remaining production boundaries that could not be verified are disclosed.

Do not block because code is merely poorly written; do not escalate a small, recoverable error to P0.

### 5. Recheck the Gate

Before reporting, confirm:

- The reviewed head SHA is still the PR head.
- Migrations, configuration, lockfiles, dependencies, and deployment files have not changed since review; checks still correspond to the same head.
- Each P0/P1 is actually reachable, and no upstream validation or database constraint was missed.
- Rollback can still read data written by the new version.
- Monitors can trigger before a large number of users are affected.
- Every unverified item is explicit rather than hidden by Ready.

Any change in revision, scope, or critical assumptions requires a new review.

## High-Risk Signals

- Unbounded or incorrectly scoped `DELETE`/`UPDATE`, `DROP`/`TRUNCATE`, cascading deletion, or hard deletion.
- Incompatible code and migrations, unrecoverable batch work, table locks/full scans, incorrect backfill scope, or rollback that loses new data.
- Unhandled exceptions that terminate a process; unbounded memory, queues, or retries; exhausted connection pools; synchronous blocking on critical paths.
- Race conditions, lost updates, duplicate execution, non-idempotent retries, or partially successful transactions.
- Missing critical configuration, unsafe defaults, or failures when old and new instances run together.
- No timeout, circuit breaker, or degradation on a critical path, allowing a single dependency to spread failure.
- Permission bypass, leaked secrets, sensitive data in logs, or untrusted input triggering dangerous operations.
- No canary or kill switch, an infeasible rollback, or no metrics/alerts to detect an incident after release.

## Output

Use [report-template.md](references/report-template.md). At minimum include:

1. Review scope and pinned SHA.
2. `Blocked` / `Conditional` / `Ready` with a one-sentence reason.
3. Production risks ordered by impact, with `file:line`, trigger, impact, evidence, and recovery.
4. Conditions required before merge.
5. Release order, monitoring, and rollback triggers.
6. Verified evidence and residual risks that could not be verified.

Keep the risk list concise and actionable. Do not list a risk area as “passed” when none was found. Read [sources.md](references/sources.md) when research evidence is needed.

## References

Read only what is needed:

- [risk-checklist.md](references/risk-checklist.md): probes for data, runtime, concurrency, configuration, security, and release risks.
- [data-and-release.md](references/data-and-release.md): database migration, backfill, compatibility rollout, canary, monitoring, and rollback.
- [report-template.md](references/report-template.md): risk table and merge-gate format.
- [sources.md](references/sources.md): public skills, SRE and database-evolution practices, and integration choices.

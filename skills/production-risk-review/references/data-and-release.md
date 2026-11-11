# Data and Release Safety

Read this reference when a change touches persistent state or an actual production release.

## Database Migration Review

Establish three facts: how the schema changes, how existing data moves, and how each application version reads and writes it. All three must remain safe while old and new instances coexist and during the rollback window.

### Deletion, Rename, and Type Changes

Treat these as high risk:

- Stop using a field and run `DROP COLUMN` or `DROP TABLE` in the same release.
- Remove an old table, field, or index before every code path has migrated.
- Rollback code still depends on an object that the migration removes.
- No database backup, logical export, or tested recovery path exists.

Prefer expand/contract and split irreversible changes across releases:

1. Expand: add a nullable field, table, or index while keeping old and new applications compatible.
2. Dual-write or backfill: write new data to the new structure and migrate old data in batches.
3. Switch reads: verify coverage, error rate, and data consistency.
4. Stop old writes: confirm no old client or instance still depends on them.
5. Contract: remove the old structure after the rollback window closes.

Define completion criteria and verification metrics for every phase. Do not put all five phases into one migration.

### Large Tables and Bulk Data

Assess actual DDL lock level and duration; table copies, index rebuilds, WAL/binlog volume; backfill batch size, throttling, checkpoints, retries, and recovery; correctness under concurrent writes; replica lag, connection pools, CPU, I/O, and disk capacity; and partial data left by cancellation.

Do not invent duration estimates when production row counts, indexes, or database versions are unavailable. Record these as facts that must be confirmed before release.

## Release Ordering

Write an explicit timeline for cross-boundary changes:

```text
compatible migration -> deploy compatible code -> enable dual-write/backfill -> verify -> switch reads
-> enable feature gradually -> observe -> stop old path -> clean up old structure
```

Work backward from the actual system: which component must deploy first; when old and new versions may coexist; how old messages, clients, and unfinished jobs are handled; when a flag is enabled and its default; and where to stop or return to a stable point after failure.

## Progressive Rollout

Prefer canaries, staged rollout, or feature flags for high-risk changes. Specify the initial user or traffic scope, expansion criteria and minimum observation window, automatic stop conditions, who can pause or terminate, and business and technical checks required before full rollout.

A canary is not permission to expose 1% of users to an unsafe data deletion. Prove the operation itself safe before discussing traffic percentages.

## Rollback and Roll-forward

"We can revert the PR" is not a rollback plan. Verify that the prior artifact/version remains available; rollback code can read the new schema, messages, and cache; executed migrations are reversible or have a forward repair; backfill or queued side effects can be stopped or compensated; configuration and flags can disable the risk before redeployment; and someone will verify that core flows recovered.

If database writes have already occurred, determine where recovery-window data is stored, how quickly it can be restored, and whether restoration could overwrite valid newer data.

## Post-Release Verification

Specify actionable checks and observation windows, including deployment success, instance health and version consistency; core business success rate, 5xx, latency, timeouts, and retries; database lock waits, slow queries, replica lag, and pool use; queue backlog, dead letters, consumer failures, and duplicate execution; data consistency and key row counts; relevant business and system logs; and when to expand rollout, stop, or roll back.

"Watch the dashboards" is not sufficient. Name the metric, threshold, observation window, decision owner, and action.

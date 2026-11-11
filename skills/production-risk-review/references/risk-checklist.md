# Production Risk Checklist

Read only the sections touched by the diff. Use this checklist to form failure hypotheses; it does not replace tracing call paths, data state, and recovery capability.

## Data and Databases

Check whether `DELETE`/`UPDATE` has stable, bounded predicates and whether predicate fields can be null, duplicated, or user-controlled; whether hard deletion became cascade deletion, cross-table bulk update, `TRUNCATE`, or `DROP`; whether backup, soft deletion, retention, audit history, or a workable recovery plan exists; whether migrations are transactional and can leave partial state; whether large tables create long locks, replica lag, pool exhaustion, or disk amplification; whether backfills are batched, pausable, retryable, observable, and resumable; whether old and new code can coexist across schema versions; whether rollback loses data written by the new version; and whether tests reflect production-scale volume, indexes, and constraints.

Evidence matters: do not assess SQL text alone. Inspect callers, permissions, parameter sources, transaction boundaries, and database constraints. Production row counts, table size, indexes, lock levels, and replication topology are runtime facts. If the repository cannot establish them, list them as pre-release checks.

## Service Crashes and Failure Propagation

Check whether unhandled exceptions, nulls, bounds/type errors, panics, or process exits are reachable; loops, recursion, retries, and batches are bounded; memory, file descriptors, threads, tasks, pools, and queues are bounded; external calls have timeouts, cancellation, circuit breaking, rate limits, and fallback; exceptions are swallowed while success is returned; new configuration can prevent instances from becoming ready; health checks detect dependency failures rather than merely process liveness; and one dependency failure can cascade into core paths through synchronous calls or retry storms.

Trace from the entry point to the hazardous call and check upstream validation. Assign severity only to reachable runtime paths.

## Concurrency, Transactions, and Idempotency

Check for lost updates in read-modify-write flows; duplicate execution from webhooks, messages, jobs, retries, or double-clicks; idempotency keys, unique constraints, optimistic concurrency, and distributed locks that cover real concurrency; transactions spanning remote calls and uncompensated partial success; incompatibility between old and new message or producer/consumer ordering; overlapping scheduled jobs or backfills racing with online writes; and defined handling for lock failure, timeout, and deadlock.

Using a transaction does not prove concurrency correctness. Verify isolation level, lock scope, and observable behavior after rollback.

## Configuration, Dependencies, and Security

Check that new environment variables, secrets, certificates, queues, buckets, DNS, and permissions exist in the target environment; missing configuration fails safely rather than silently selecting a dangerous default; dependency upgrades alter runtime, native module, serialization, or network behavior; authorization still covers new entry points and background jobs; secrets, tokens, and personal data cannot leak into logs, errors, caches, or analytics; external input cannot trigger command execution, traversal, injection, or unauthorized deletion; and dependencies/configuration needed for rollback remain available.

If target-environment access is unavailable, state what is unknown. Unknowns on a critical path cannot be counted as `Ready` without qualification.

## Compatibility and Release Ordering

Check backward compatibility for APIs, events, databases, caches, queues, and file formats; whether old code can handle new data and new code can handle old data; required ordering across services; sequencing of migrations, app deployment, backfills, and feature flags; safe flag defaults and ability to disable a failing feature without redeployment; incompatible windows while old and new instances coexist; and whether an independently unrollbackable high-risk behavior is bundled with unrelated changes.

## Observability and Recovery

Check separate visibility for error rate, latency, throughput, saturation, and key business success; deploy markers, version identifiers, and diagnosable logs; alerts that fire before broad user impact; a named rollback decision maker and rehearsed procedure; pre-defined rollback criteria; restorable backups that do not overwrite valid post-incident data; and whether emergency repair requires downtime and its expected recovery time.

## False-Positive Control

These facts alone are not blockers:

- Many files, a large diff, or broad refactoring when impact paths and rollback units are clear.
- A reversible nullable column that does not affect old code.
- New configuration with a safe default or an explicit startup failure when missing.
- A short-TTL cache change where misses are acceptable.
- A small, verifiable behavior change that is inexpensive to revert.
- Lack of production access when the critical risk does not depend on the unavailable information.

The goal is to prevent real, unacceptable production outcomes, not to promote every unknown to a blocker.

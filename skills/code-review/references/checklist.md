# Quality and Focused Checks

Follow explicit repository standards. The questions below help when no repository rule exists; they are not violations by default.

## General Dimensions

| Dimension | Questions |
| --- | --- |
| Correctness | Does the change meet requirements and acceptance criteria? Are `null`, empty collections, zero, negatives, long inputs, concurrency, and error paths handled without swallowing failures? Are there off-by-one errors, races, inconsistent state, timezone/encoding assumptions, or unsafe retries/repeated execution? |
| Readability and simplicity | Are names meaningful and consistent? Is control flow direct? Is there dead code, low-information commentary, or an equally clear shorter expression? Were unrelated paths coupled to a new condition? |
| Architecture | Does the change follow established patterns and dependency direction? Is abstraction justified? Does it duplicate canonical behavior? Does refactoring actually reduce the concepts readers must understand? Are type boundaries clear? |
| Security | Is input validated at trust boundaries? Could secrets, credentials, or tokens reach source, logs, or errors? Is authentication/authorization complete? Are SQL, commands, templates, or deserialization exposed to injection? Is external data treated as untrusted? |
| Performance | Did the change add N+1 queries, unbounded work, missing pagination, synchronous blocking, or repeated hot-path computation? Are new queries supported by indexes? Without impact or measurement evidence, ask a `question` instead of asserting slowness. |

## By Change Type

| Change | Additional checks |
| --- | --- |
| Bug fix | Is there a regression test that reproduces the bug? Does the fix address the root cause rather than hide symptoms? |
| New feature | Are failure/degradation behavior and impacts on existing behavior clear? Is a flag or gradual rollout needed? |
| Refactor | Is behavior preserved and protected by tests? Is complexity actually lower? |
| Database migration | Can it roll back? Could large-table work lock tables? Can old and new application versions run together? |
| Dependency upgrade | Was the changelog reviewed? Was the lockfile diff inspected? Are known vulnerability/license risks introduced? |
| API change | Is compatibility broken? Is there a versioning or deprecation plan? Is documentation synchronized? |
| Configuration / CI | Could credentials leak? Could failure silently pass? Are permissions minimized? |
| Code deletion | Are callers and external dependencies ruled out? Are related configuration, tests, and docs removed or updated? |

## Test Quality

- Do assertions verify behavior rather than implementation detail or a condition that is always true?
- Would a broken implementation fail the test? Did mocks replace the subject's logic itself?
- Are key boundaries and error paths covered, not only the happy path?
- For a bug fix, does the regression test fail before and pass after the fix, when feasible?
- Coverage indicates execution scope; it does not replace assertion-quality review.

## High-Risk Focus Areas

Do a lightweight scan after reading the diff, then expand only when a risk area is hit.

| Risk area | Focus |
| --- | --- |
| Authentication / authorization / sessions | Default deny; inspect privilege escalation, IDOR, and bypass paths. |
| Secrets / credentials | Keep out of code, logs, and commits; secure transit/storage and minimize permissions. |
| SQL / commands / templates / deserialization | Parameterize SQL, avoid command concatenation, escape templates, and allowlist deserialization. |
| File uploads / external data | Validate type, size, and content; use non-executable storage; treat external input as untrusted. |
| Payments / data migrations | Preserve money/data consistency; ensure rollback and old/new version compatibility; inspect transaction boundaries. |
| Concurrency / retries | Check idempotency, races, duplicate submissions, and partial failure. |
| CI/CD / dependencies / supply chain | Isolate credentials, minimize permissions, expose failures, and review lockfile/dependency changes. |

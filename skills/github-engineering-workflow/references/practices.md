# Engineering Practices

Calibration date: 2026-08-14. Map general principles to repository-level policies. For platform features, licenses, compliance, or team-specific differences, follow the current repository and official documentation.

Classify controls as `MUST` (minimum traceability, verifiability, and ownership), `SHOULD` (default; exceptions need a recorded reason), or `CONDITIONAL` (enable when scale, license, or platform warrants it). Record each exception with the waived rule, rationale, scope, approver, compensating control, owner, and review date.

## Risk Gate Matrix

Raise the risk level when any dimension rises: identity/permissions/funds/privacy/compliance/security; public APIs, persistent data, infrastructure, IAM, secrets, or build/release chain; difficulty or cost of rollback; blast radius across services, regions, tenants, or users; and gaps in automated tests, staging, health signals, or experienced maintainers. Do not use line count to lower risk.

| Gate | Low | Standard | High | Emergency |
| --- | --- | --- | --- | --- |
| Issue, risk rationale, verifiable acceptance | MUST | MUST | MUST | MUST; may be completed afterward |
| Human review and required CI | MUST | MUST | MUST | SHOULD follow normal flow; authorized bypass must be recorded |
| Domain CODEOWNER | By path | SHOULD | MUST | May be reviewed afterward |
| Threat model / privacy review | Not needed or lightweight | Based on impact | MUST | Complete after recovery |
| Deploy and rollback/roll-forward plan | May be N/A | MUST | MUST; rehearse key steps | MUST; prioritize minimum recovery path |
| Progressive delivery and automated health gate | Optional | Based on blast radius | MUST where supported | Verify after recovery |
| SBOM, provenance, signature verification | Based on deliverable | SHOULD | MUST for executable release artifacts | Complete afterward |
| Retrospective and improvement Issues | Optional | On failure | On failure or major near miss | MUST |

If the platform cannot enforce a gate, replace it with an equivalent platform or manual audit and mark it `UNKNOWN` or accepted risk. Do not claim the control is enabled when it is not.

## Repository Governance

- Use rulesets/branch protection to prevent default-branch deletion or force-push and require PRs, passing checks, and appropriate approvals.
- Require domain-owner review for sensitive paths (`.github/workflows/`, `CODEOWNERS`, release scripts, IaC, IAM, payments, migrations) and protect those files themselves.
- Give required checks unique, stable names. Conditional workflows must report on every applicable PR or they may remain pending indefinitely.
- Require re-approval after material changes to an approved diff.
- Use a Merge Queue for high-throughput trunk development so candidates re-run required checks against the latest trunk combination (`merge_group` for GitHub Actions).
- Use GitHub Environments to constrain deployment branches, approvals, concurrency, and environment credentials. Do not release production credentials before protection rules pass.
- Choose and record one merge strategy; do not change established history conventions without agreement.
- Default to trunk-based development / GitHub Flow: merge to trunk or use very short-lived branches (hours to a day). Use Git Flow only when maintaining multiple supported versions long-term.
- Public/open-source repositories should provide `SECURITY.md` with a vulnerability reporting channel and response expectations, plus security advisories and dependency/secret scanning. Private internal repositories may omit public reporting details.
- Sign commits and tags (GPG/SSH). Require verified signatures for public repositories when compliance or supply-chain policy warrants it; optional for personal projects.

## Branch Naming and Lifecycle

A branch name is a human-readable hint, not the authoritative link. Associate work through the PR's `Closes #<issue>`; the branch name should make the owner and intent easy to recognize.

Default format:

```text
<type>/<issue>-<slug>        Example: feat/42-oauth-login
```

| Segment | Rule | Example |
| --- | --- | --- |
| `type` | Use the same vocabulary as Conventional Commits; do not invent another | `feat` |
| `issue` | Numeric Issue number without `#` | `42` |
| `slug` | 2-4 lowercase hyphen-separated words describing intent | `oauth-login` |

Map types to labels consistently across branch, commit, and Issue:

| Branch / commit type | Label | Purpose |
| --- | --- | --- |
| `feat` | `type:feature` | New feature or enhancement |
| `fix` | `type:bug` | Bug fix |
| `docs` | `type:docs` | Documentation-only change |
| `refactor`, `perf`, `test`, `build`, `ci`, `style`, `chore` | `type:chore` | Engineering change without user-facing behavior change; use `area:*` for scope |
| `fix` or `hotfix` plus `type:security` | `type:security` | Security fix; represent security in the label, not a new commit type |

`hotfix/` is the only prefix outside the commit vocabulary and is reserved for `risk:emergency` to signal that gates were compressed. Keep commits under it as `fix:`.

Use lowercase ASCII letters, digits, hyphens, and one `/` separator. Avoid spaces, non-English characters, underscores, camel case, and `#`. Keep one goal per branch; create a new branch if scope changes, and do not reuse a merged branch for unrelated work. Delete short-lived branches after merge.

## Issue, Milestone, and Labels

Write Issues as executable work: context and value, goal, non-goals, acceptance criteria, dependencies, owner, risk, and verification plan. Link Proposal/ADR and Milestone where used. Keep labels structured and stable (`type:*`, `area:*`, `risk:*`, optional `priority:*`); use GitHub state and milestone fields instead of labels that duplicate them. Keep label definitions documented and periodically remove stale labels.

Use Milestones for coordinated multi-Issue outcomes with a goal, target date, scope, owner, and measurable completion criteria. Solo or one-off work may go directly to a Release/Tag.

## Pull Requests and Commits

- Keep changes focused and reviewable; split unrelated behavior and avoid formatting noise.
- PR titles and commit titles must be in English. Use Conventional Commits for commits (for example, `feat(auth): add OAuth login`); keep the subject imperative, concise, and specific.
- PR descriptions explain the problem, solution, scope, risks, tests, compatibility, deployment, and rollback. Link an Issue with `Closes #<n>`.
- Do not claim a test, security check, or manual validation passed unless it was run and observed.
- Use draft PRs for work not ready for merge. Request review after the scope and verification evidence are clear.
- Resolve blocking review feedback and re-request approval after material changes.
- Keep temporary commits (`fixup!`, `squash!`) out of the final trunk history.

## CI and Quality Gates

Use a fast required layer for formatting, lint, type checks, unit tests, and secret scanning; add integration, compatibility, security, and end-to-end checks according to risk. Run the same required workflow on merge queue candidates. Make flaky tests visible, assign an owner and removal/fix date, and do not normalize ignored failures.

Protect workflows with least-privilege permissions, pin third-party Actions to full commit SHAs, avoid executing untrusted code with privileged tokens, and protect deployment environments. Cache only non-sensitive derived data. Store artifacts with provenance and immutable identifiers.

## Architecture and Data Changes

Record consequential choices in an ADR: context, decision, alternatives, consequences, and migration/recovery. Evolve APIs and schemas using expand/migrate/contract where practical. Version migrations; validate them in production-like environments; make large jobs resumable, observable, and rate-limited. Avoid requiring application deployment and an irreversible schema operation to be atomic. If separation is impossible, make downtime, backup, validation, and roll-forward explicit gates.

Give each feature flag an owner, purpose, default, enabled environments, tested combinations, observability, and cleanup date. Do not leave permanent flags or hidden behavior branches.

## Build, Release, and Deployment

Build once and promote the same artifact across test, staging, and production; vary configuration or secret references by environment, not by rebuilding. Record artifact, actor, time, environment, smoke-test result, and health outcome. Promote and roll back by digest, not mutable tag; do not rebuild what is claimed to be the same release.

Bind each Release to its tag, source commit, milestone, Issues/PRs, artifact version/digest, test/security/deployment evidence, user-visible changes, known issues, and upgrade/rollback instructions. Add SBOM, provenance, and signatures according to risk.

Use SemVer where appropriate: `feat` -> minor, `fix` -> patch, breaking (`!` or `BREAKING CHANGE:`) -> major. Maintain `CHANGELOG.md` using Keep a Changelog categories (Added/Changed/Fixed/Removed/Security).

Before release define success/failure signals: core-path success and latency, error rate, resource saturation, and relevant business measures, with thresholds, observation period, owner, and stop/rollback action. For high-risk changes: deploy and smoke-test, expose a small scope, compare with control or baseline, expand in stages, stop or roll back on defined conditions, and record artifact/traffic/metrics/decisions at each stage. Choose canary, blue-green, rolling, or flags based on the architecture and rollback properties, not by ceremony.

## Incidents and Emergency Changes

- Create or update an incident/hotfix Issue with user impact, timeline, owner, and recovery objective.
- Prefer deploying a known-good artifact or minimum fix through the normal pipeline; avoid ad hoc manual steps.
- Bypassing review, checks, or environment approvals requires authorization. Record the exact bypass, reason, evidence, and expiry; afterward restore tests, reviews, docs, release records, and bypassed controls.
- Run blameless retrospectives that distinguish trigger, amplification, detection gaps, and recovery friction. Create a small set of verifiable improvement Issues with owners, priority, and due dates, and track them to closure.

## DORA Metrics

| Category | Metric | Definition |
| --- | --- | --- |
| Throughput | Change lead time | Commit to production availability |
| Throughput | Deployment frequency | Production deployments per time interval |
| Instability | Failed deployment recovery time | Time to recover after a production failure |
| Instability | Change failure rate | Share of deployments requiring rollback/hotfix |
| Instability | Deployment rework rate | Share of unplanned deployments caused by incidents |

Measure one service at a time with a stable definition. Establish a baseline, address the largest bottleneck, change one thing, and observe trends. Do not rank systems, tie metrics to individual performance, or manipulate results. Use sampling and team retrospectives if collection costs exceed improvement value.

## Primary Sources

- [Managing labels](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/managing-labels)
- [Classifying your repository with topics](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics)
- [About tags](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases/about-releases#about-tags)
- [Protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [Secure use of GitHub Actions](https://docs.github.com/en/actions/reference/security/secure-use)
- [Supply-chain security](https://docs.github.com/en/code-security/concepts/supply-chain-security/supply-chain-security)
- [Artifact attestations](https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/use-artifact-attestations)
- [Google: Small CLs](https://google.github.io/eng-practices/review/developer/small-cls.html)
- [DORA delivery metrics](https://dora.dev/guides/dora-metrics/)
- [NIST SP 800-218](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-218.pdf)
- [SLSA v1.2](https://slsa.dev/spec/v1.2/)
- [OpenSSF Scorecard](https://github.com/ossf/scorecard)
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [Semantic Versioning](https://semver.org/)
- [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
- [Trunk-Based Development](https://trunkbaseddevelopment.com/)
- [Architecture Decision Records](https://adr.github.io/)

# Checklists and Templates

## Definition of Ready

- [ ] Proposal/ADR is accepted; goals, non-goals, and user value are clear.
- [ ] Issue belongs to a Milestone where appropriate; owner, dependencies, and blockers are visible.
- [ ] Issue labels follow repository conventions; type and risk are clear, and labels do not replace acceptance criteria or GitHub state.
- [ ] Risk level and rationale are recorded; high-risk work has domain-owner and threat/risk review planned.
- [ ] Acceptance criteria are observable and repeatable.
- [ ] API, data, compatibility, and migration impact are understood.
- [ ] Tests, docs, observability, release, and rollback/roll-forward are planned according to risk.
- [ ] Open questions are not likely to fundamentally change the implementation direction.

## Definition of Done

- [ ] PR is linked to an Issue; human review, blocking feedback, and required CI are complete.
- [ ] Short-lived branch is deleted after merge; no merged branches remain unnecessarily.
- [ ] Branch names, commit subjects, PR titles, and tags use English; no temporary `fixup:`/`squash!` commits remain in trunk history.
- [ ] Issue/PR labels match actual type, scope, risk, and priority; no duplicate or stale labels remain.
- [ ] Trunk build, tests, and applicable security checks pass; no untracked flaky gate is ignored.
- [ ] Docs, ADRs, runbooks, migrations, and feature-flag lifecycle are updated or marked not applicable.
- [ ] Authoritative build artifact is traceable to the source commit; digest and risk-required SBOM/provenance are retained.
- [ ] Target-environment verification is complete, user-health signals are normal, and rollback/roll-forward is actionable.
- [ ] Release notes, Issue, Milestone, residual risks, and follow-up work are updated.

## Proposal Template

```markdown
# <Decision Title>

## Users and Context
<Who is the user? What is the real scenario?>

## Problem and Value
<What is the problem? Why solve it now? What evidence supports it?>

## Trade-offs
- Selected option:
- Alternatives:
- Why this option:
- Why alternatives were rejected:

## Scope of This Change
- In scope:
- Out of scope:
- Boundaries and constraints:

## Design
<Write the text first; attach a prototype or demo after the text is agreed.>

## Acceptance Criteria
- [ ] <Observable, repeatable outcome>

## Risks and Verification Plan
<Key assumptions, risks, verification methods, and exit conditions>

## Decision and Security Impact
- ADR: <required / not required; link>
- Threat model: <required / not required; assets, trust boundaries, and mitigations>
```

## ADR Template

```markdown
# ADR-<NNN>: <Title>

## Status
Proposed / Accepted / Superseded (by ADR-XXX) / Deprecated

## Date
<YYYY-MM-DD>

## Context and Problem
<Decision context and constraints>

## Decision
<Selected option and rationale>

## Alternatives
- <Alternative 1>: <pros and cons>
- <Alternative 2>: <pros and cons>

## Consequences
- Benefits:
- Costs / risks:
- Migration and recovery:
```

## Milestone Template

> Solo or one-off projects may omit Milestones and deliver directly through a Release/Tag. This template is for coordinated team work or multi-iteration delivery.

```markdown
## Goal
<The one outcome this iteration must deliver>

## Scope and Non-goals
- Scope:
- Non-goals:

## Timeline
- Start:
- Due date:

## Acceptance Gates
- [ ] Product decision is complete
- [ ] Architecture supports the iteration's delivery
- [ ] Every Issue is linked and has a clear state
- [ ] Core flows and quality verification pass
- [ ] Release and tag are planned

## Expected Deliverable
<Runnable version, demo, docs, deployment, or other artifact>

## Operations and Release Readiness
- Target environment:
- Health signals and acceptance window:
- Release/tag/artifact strategy:
- Rollback or roll-forward owner:
```

## Issue Template

```markdown
## Context
<User, problem, value, and trigger>

## Goal
<Observable change after this Issue is complete>

## Non-goals
<Explicitly excluded work>

## Links
- Proposal:
- ADR / threat model:
- Milestone:
- Dependencies / blockers:

## Labels
- Type: `type:<feature|bug|docs|chore|security>`
- Area: `area:<module>` (optional)
- Priority: `priority:<p0|p1|p2|p3>` (optional, per repository convention)
- Risk: `risk:<low|standard|high|emergency>`
- Special state: `status:<blocked|needs-info|ready>` (only when needed)

## Risk Level
- Level: low / standard / high / emergency
- Rationale: <data, security, compatibility, migration, infrastructure, blast radius, and recovery>

## Design or Specification
<Full Spec / Mini Spec; key API, data, interaction, or solution points>

## Acceptance Criteria
- [ ] <Given/When/Then or another verifiable outcome>

## Subtasks
- [ ] Implementation
- [ ] Tests
- [ ] Documentation
- [ ] Observability and operations readiness
- [ ] Release, migration, and cleanup

## Verification and Delivery Plan
- Tests:
- Security checks:
- Health signals:
- Deployment / feature flag:
- Rollback or roll-forward:
- Artifact / SBOM / provenance: <required or not, with rationale>
```

## Pull Request Template

> PR titles must be in English. Issue titles and PR descriptions may use the team's collaboration language.

```markdown
## Problem
Closes #<issue>
<One sentence describing the value and reason>

## Risk Level
- Level: low / standard / high / emergency
- Rationale:
- Domain reviewer / CODEOWNER:

## Labels
- Type / area: <inherited from Issue or updated to match the actual change>
- Risk: `risk:<low|standard|high|emergency>`
- Priority: <optional, per repository convention>

## Key Changes
- <Changes focused on this Issue>

## Review Guide
<Where reviewers should focus and any non-obvious behavior>

## Verification
- [ ] Unit / integration / end-to-end: `<command and result>`
- [ ] Build / lint / type checks: `<command and result>`
- [ ] Security / dependency / secret checks: `<command or check>`
- [ ] Screenshot, demo, or manual verification: <link/result>

## Compatibility and Data
- API / client compatibility:
- Schema / migration:
- Feature flag (owner, default, cleanup date):

## Deployment, Observation, and Recovery
- Target environment and rollout strategy:
- Success/failure health signals:
- Stop conditions:
- Rollback or roll-forward:
- Build artifact version/digest:

## Docs and Architecture
- [ ] User docs updated or not needed
- [ ] Architecture/decision record appended or not needed

## Use of AI
- Scope: <none / design discussion / code / tests / docs>
- Key prompt summary: <redact sensitive information>
- Human review: <understanding, tests, and decisions>
```

## Review Checklist

- [ ] PR title is in English and accurately summarizes the change.
- [ ] PR aligns with the Issue, Proposal, and Milestone.
- [ ] Change is focused, without unrelated refactoring or formatting noise.
- [ ] Risk level is justified and gates match the blast radius.
- [ ] Solution direction, module boundaries, interfaces, and data model are sound.
- [ ] Author can explain key logic and generated contributions.
- [ ] Core paths, edge cases, and regression risks are tested.
- [ ] Required CI, build, and security checks pass.
- [ ] Build artifact is traceable and is not rebuilt in later environments.
- [ ] Docs, changelog, compatibility, deployment, health, and recovery details are complete.
- [ ] Feature flags, temporary exceptions, and isolated tests have owners and expiry dates.
- [ ] Blocking feedback is resolved and important decisions are recorded.

## Release Template

```markdown
# <Version> - <Milestone>

## Delivered
- <User-visible feature or engineering capability> (#issue / #pr)

## Fixed
- <Bug> (#issue / #pr)

## Known Issues
- <Impact, workaround, and follow-up plan>

## Verification and Trial
- Deployment / demo URL:
- Verification steps:
- Environment and dependencies:
- Deployment health result:

## Artifact and Supply-Chain Evidence
- Source commit:
- Tag:
- Build run:
- Artifact version and SHA-256:
- SBOM: <link / reason not applicable>
- Provenance / signature and verification result: <link / reason not applicable>

## Upgrade and Rollback
- Upgrade:
- Rollback:

## Documentation
- Project overview:
- User docs:
- Architecture and decision records:
```

## Emergency Change Record

```markdown
## Incident and User Impact
- Incident / security event:
- Start time:
- Current impact:
- Incident lead / owner:

## Recovery Change
- Issue / PR / commit:
- Known-good or fix artifact digest:
- Rollback or roll-forward, and why:

## Gates and Authorization
- Gates completed normally:
- Exact gates bypassed:
- Reason and risk:
- Approver, time, and expiry:
- Compensating controls:

## Verification
- User-health signals:
- Data / security verification:
- Deployment result:

## Follow-up After Recovery
- [ ] Complete review, tests, docs, and release records
- [ ] Revoke temporary permissions, feature flags, or exceptions
- [ ] Create a blameless retrospective and improvement Issues
```

## Audit Evidence Matrix

Mark each item `PASS`, `FAIL`, or `UNKNOWN` and attach a concrete URL, identifier, commit, tag, file path, or command output.

| Check | Minimum evidence |
| --- | --- |
| Design first | Accepted Proposal with trade-offs, non-goals, and acceptance criteria |
| Sound architecture | Boundaries, interfaces, data model, risks, and a coherent vertical path |
| Milestone management | Goal, due date, all linked Issues, and visible progress |
| Actionable Issues | Context, goal, non-goals, acceptance, dependencies, and specification |
| Traceable PRs | Linked Issue, matching scope, and complete verification evidence |
| Effective review | Human approval, resolved blockers, and automated check results |
| Usable trunk | Post-merge build and core tests pass |
| Risk-scaled controls | Clear Issue/PR risk rationale; auditable high-risk gates and exceptions |
| CI feedback | Reliable fast checks on every change; owner for slow checks and expiring flaky-test exceptions |
| Trusted build | Authoritative immutable artifact tied to source commit, build run, and digest |
| Actions security | Least privilege, Actions pinned to full SHAs, workflow owners, no privileged execution of untrusted code |
| Dependencies and secrets | Dependency graph/lockfile/review, secret scanning, and rotation evidence |
| Supply chain | Risk-appropriate SBOM, provenance/signature, and pre-deployment verification |
| Safe deployment | Same artifact promoted, protected environments, smoke test, staged release, and recovery path |
| Compatibility | Compatible API/schema migrations; consumers and recovery plans for breaking changes |
| Observability | User-health signals, stop criteria, and owners defined before release |
| Emergency path | Bypass authorization, scope, expiry, compensating controls, and post-recovery closure |
| Evolving docs | Changes appended; user and engineering docs kept in sync |
| Complete Release | Tag, notes, deliverables, known issues, and trial instructions |
| AI accountability | Prompt summary where applicable, human review, author can explain code |
| Information security | Repository visibility matches content; no credential or restricted-data leak |
| Repository governance | Default-branch protection, approvals, required CI, templates, and ownership boundaries |
| Delivery improvement | Five DORA metrics trended per service with stable definitions to improve bottlenecks |
| Metric integrity | No evidence of manipulating stars, reviews, votes, deployments, failures, or other metrics |

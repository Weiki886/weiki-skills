---
name: github-engineering-workflow
description: Use to plan, execute, or audit a software delivery workflow using GitHub. Covers proposals/ADRs, milestones, issues, branches, commits, pull requests, CODEOWNERS, CI/CD, review, merge, immutable builds, progressive deployment, releases, observability, and retrospectives. First classify solo versus team delivery, then scale process rigor to collaboration size and risk.
---

# GitHub Engineering Workflow

Use GitHub as the source of engineering truth and keep the complete chain traceable: `Proposal → Milestone → Issue → PR → Review/CI → Merge → Build → Deploy → Observe/Learn`.

## Core Principles

1. Complete product decisions and architecture before development; each delivery cycle follows product → architecture → development → acceptance → release.
2. Link changes to Issues and merge through PRs with human review. For team collaboration or multiple iterations, place Issues in a Milestone.
3. Keep the main branch buildable, testable, and runnable. Prefer small changes, small PRs, and timely merges.
4. Append decision and design-change records; do not overwrite prior decisions.
5. Tools may assist engineering work, but people remain responsible for design, review, understanding, and final delivery. Generated code must be explainable and verifiable.
6. Confirm repository visibility and information boundaries. Never put secrets, credentials, or restricted data in a public repository, Issue, PR, log, or prompt.
7. Build once and promote the same immutable artifact between environments; do not rebuild unverified content during deployment.
8. Scale process rigor to risk and collaboration size. Keep individual projects lightweight and apply team gates where warranted; do not substitute uniform thresholds for judgment.

## Choose a Delivery Context

Classify the delivery before choosing process rigor:

- **Solo / one-off:** Use a lightweight path such as Commit, optional Issue, and Tag/Release. Proposal/ADR, Milestone, CODEOWNERS, mandatory human review, CI gates, and DORA metrics may be omitted. The baseline is a usable main branch, traceable commits, and reproducible releases.
- **Team / multiple iterations:** Use the full Proposal → Milestone → Issue → PR → Review/CI → Release chain, plus repository governance and DORA metrics.

Default to team for multiple contributors, repeated iterations, shared releases, or compliance needs. Default to solo for personal, one-off work. For mixed cases, follow the higher risk. The standard workflow below assumes team delivery; solo projects may trim it as described here.

## Risk Levels

Assign each Issue/PR a risk level and record the reason. Reuse repository-defined levels; otherwise use:

- `low`: documentation, tests, internal refactoring, or mechanical changes that do not alter runtime behavior.
- `standard`: bounded feature work or fixes that can be rolled back independently.
- `high`: identity, permissions, money, privacy, public APIs, data migrations, infrastructure, supply chain, production access, or a large blast radius.
- `emergency`: an accelerated change required to restore service or contain an active security incident.

Every level retains the `Issue → Change → Verification → Human Review` audit trail. Only an authorized emergency may temporarily compress gates; complete the missing record after recovery. See the gate matrix in `references/practices.md`.

## Labels, Tags, and Topics

Keep these three objects distinct:

- **GitHub Labels** are working metadata for Issues/PRs: what it is, how large the impact is, and who is handling it. They do not record versions or replace Milestones, Project state, or assignees.
- **Git Tags** point to a Commit as a version reference. A release Tag is immutable; do not move it to a different Commit.
- **GitHub Topics** are stable repository-level subject terms. Use lowercase hyphenated terms, not versions or temporary Issue status.

See `references/practices.md` for naming, use, solo/team adjustments, and ownership, including branch lifecycle and Git-object language. Existing repository conventions take precedence. Without conventions, give each Issue at least one `type:*` Label and a risk level; use `vMAJOR.MINOR.PATCH` Tags for formal releases.

## Entry and Exit Gates

Meet the Definition of Ready before implementation and the Definition of Done before closing an Issue/Milestone. Checklists are in `references/templates.md`.

## Establish Context Before Execution

1. Read repository `AGENTS.md`, `CONTRIBUTING*`, `README*`, `SECURITY*`, Issue/PR templates, branch protections, CI, Labels, and release conventions.
2. Confirm the default branch, active Milestone, linked Issue, repository visibility, test commands, and release method.
3. Confirm system criticality, data sensitivity, deployment environments, risk level, and incident-response practices.
4. Reuse existing naming, branch prefixes, Labels, Tags, Topics, Commit conventions, and merge strategy. Use this skill's defaults only when the repository has none.
5. Keep audits read-only and report evidence. For implementation, stay within the authorized scope.

## Standard Delivery Chain

This workflow defaults to team delivery. For solo work, trim it to Commit, optional Issue, and Tag/Release. See `references/templates.md` for templates and `references/practices.md` for details.

1. **Proposal:** Before coding, state users, problem, value, tradeoffs, non-goals, boundaries, and acceptance criteria. Do not implement an unaccepted proposal.
2. **Architecture:** People define module boundaries, dependencies, interface contracts, data models, and technical risks. Interfaces and call relationships must exist or be clearly marked as open, mocked, or stubbed. A directory tree or technology list is not architecture.
3. **Milestone:** Group Issues into an accepted delivery with goals, scope, non-goals, acceptance criteria, and expected Release. Solo or one-off work may use a Release/Tag instead; establish Milestones for team or multi-iteration delivery.
4. **Issue:** Make an Issue the basic unit of work. Avoid vague titles. Include context, goals, non-goals, links, risk, acceptance criteria, subtasks, verification plan, and applicable Labels.
5. **Commit:** Start from an accepted, actionable Issue. Keep branches short-lived and synchronized; delete them after merge. Default branch format is `<type>/<issue>-<slug>` (for example, `feat/42-oauth-login`). Reuse the Conventional Commits type vocabulary; solo work without an Issue may omit the number. Each Commit expresses one complete intent and remains buildable. By default use Conventional Commits `<type>(<scope>): <summary>` and connect it to SemVer/changelog automation. **Branch names, Commit titles, and Tags must be in English.** Put any additional explanation in the Commit body; Issue titles and bodies may use the team's language.
6. **PR:** **PR titles must be in English.** The body may use the team's collaboration language. Link and close the corresponding Issue, keep scope aligned, and describe the problem, key changes, risks, verification evidence, compatibility, deployment, and rollback. Apply relevant Labels and keep PRs small and frequent.
7. **Review:** Self-review first, then request at least one suitable human reviewer. Automated checks supplement but do not replace human review. Inspect direction, logic, boundaries, tests, CI, and deployability. Classify feedback as `blocking`, `question`, `nit`, or `fyi`; only `blocking` prevents merge. See `code-review` for baseline pinning, standards and requirement checks, review dimensions, and feedback wording.
8. **CI and Build:** Run reproducible builds and fast checks for each change; verify again after merging to main. Put formatting, static checks, unit tests, and critical security checks in the fast tier; run E2E, performance, and dynamic security checks in slower tiers. Treat flaky tests as defects.
9. **Merge:** Merge according to repository policy only after review passes and required checks succeed. Keep main usable, delete short-lived branches after merge, and do not bypass PRs by pushing directly to main.
10. **Deploy and Release:** Use the same automation to promote the same immutable artifact across environments. Separate deployment from feature exposure; use canaries, progressive rollout, or feature flags for high-risk work. At Milestone completion, create a Release and immutable Tag bound to source Commit and artifact evidence.

## Incidents and Emergency Fixes

- Restore service first, then preserve the timeline, alerts, deployments, Commits, and decision evidence.
- Prefer rollback to a known-good artifact or the smallest forward fix through the normal pipeline.
- Reuse the normal automation for emergency work; compress only authorized wait states. After recovery, run a blameless retrospective and create improvement Issues with owners and due dates.

## Output

Concisely report: (1) current Milestone and delivery goal; (2) created, changed, or inspected Proposals, Issues, PRs, Commits, and Releases with links; (3) test, CI, and human-verification results; (4) risk level, artifact, deployment state, and health evidence; (5) unmet gates, exceptions, and blockers; and (6) the smallest next action. Do not claim the workflow is complete without evidence or infer remote CI/review/merge/release from local tests.

## References

Read only what is needed:

- `references/practices.md`: risk gates, Labels/Tags/Topics, repository governance, CI tiers, Actions and supply-chain security, deployment and progressive release, incidents, DORA metrics, and primary sources.
- `references/templates.md`: Definition of Ready/Done and Proposal, Milestone, Issue, PR, Release, emergency-change, review, and audit-evidence templates.

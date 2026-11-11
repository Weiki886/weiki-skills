# Research Sources and Adaptation

This skill incorporates durable practices from the public skills and engineering sources below. Revisit primary sources when updating this reference; do not copy another repository's checklist wholesale.

## Public Skills

- [agentic-awesome-skills: pre-release-review](https://github.com/sickn33/agentic-awesome-skills/blob/main/skills/pre-release-review/SKILL.md)
  - Adopted: read-only release review; pin immutable SHAs; separate findings from things that could not be verified; connect migrations, configuration, secrets, rollout order, and rollback risks to the actual diff.
  - Not adopted: fixed P0/P2 naming and a complete release checklist. This skill uses three verdicts suited to PR merge gates.
- [production-readiness-skills: release-readiness](https://github.com/Nordic-AI/production-readiness-skills/blob/main/skills/release-readiness/SKILL.md)
  - Adopted: green CI does not prove release readiness; immutable artifacts, progressive delivery, executable rollback, smoke tests, and deployment observability matter.
  - Not adopted: mandatory use of canary, blue/green, or feature flags in every project. Choose based on impact and recovery cost.
- [openai-agents-python: final-release-review](https://github.com/openai/openai-agents-python/blob/main/.agents/skills/final-release-review/SKILL.md)
  - Adopted: block only on concrete, reachable release evidence; a changed candidate invalidates an old gate; define explicit unblock criteria.
  - Not adopted: SDK version and release-note responsibilities. This skill reviews production risks for one PR.
- [lobehub: release-risk](https://github.com/lobehub/lobehub/blob/canary/.agents/skills/deep-review/references/dimensions/release-risk.md)
  - Adopted: assess real rollback, repair, and blast-radius costs when apparently correct code fails after deployment; separate unknown production state from proven defects.
  - Not adopted: prompt, UI, or design quality dimensions. Focus is databases, crashes, propagation, and release recovery.

## Primary Engineering Sources

- [GitHub Docs: About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches): required checks must actually be enforced by branch protection; merge queues validate the latest target branch and queued combination.
- [Google SRE Workbook: Canarying Releases](https://sre.google/workbook/canarying-releases/): expose high-risk releases to a small amount of real traffic first to discover problems and validate rollback at lower cost.
- [Google SRE: Reliable releases and rollbacks](https://cloud.google.com/blog/products/gcp/reliable-releases-and-rollbacks-cre-life-lessons): releases cannot be controlled without reliable monitoring and rollback; rushed roll-forward during an incident can amplify impact.
- [Martin Fowler: Evolutionary Database Design](https://martinfowler.com/articles/evodb.html): evolve schema, migrations, and access code together as versioned, tested changes.
- [Martin Fowler: Parallel Change](https://martinfowler.com/bliki/ParallelChange.html): use expand/migrate/contract and a compatibility window for destructive structure changes.
- [GitHub Blog: GitHub availability report, August 2025](https://github.blog/news-insights/company-news/github-availability-report-august-2025/): a real incident illustrates that schema changes such as dropping a column can still be incompatible with applications, while missing observability magnifies impact.

## Design Principles

Keep only material that can change a merge decision:

1. Establish how the change reaches production before judging the code.
2. Base severity on reachable failure paths and recovery cost, not diff size.
3. Separate proven risks from production facts the repository cannot establish.
4. Treat rollback, migration order, configuration, and monitoring as merge evidence.
5. Leave general code-quality review to `code-review` to avoid overlap.

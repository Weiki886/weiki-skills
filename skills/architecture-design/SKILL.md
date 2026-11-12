---
name: architecture-design
description: Use when designing or evolving a software system's architecture before feature implementation. Translate finalized requirements into module boundaries, data ownership, interface contracts, risk decisions, and an implementation-ready system skeleton. Product decisions belong to product-design; feature coding belongs to code-implementation.
metadata:
  short-description: Design evolvable system boundaries and contracts
---

# Architecture Design

Design how a software system will realize finalized product decisions and support future change. Architecture is about responsibilities, dependencies, collaboration, and contracts across system boundaries, not a directory list or a technology inventory.

## Core Principles

1. **Start from agreed requirements:** Use the approved product specification as the source of product behavior. Record missing constraints as open questions or explicit assumptions; do not silently decide product scope.
2. **Design boundaries before internals:** Prioritize module responsibilities, ownership, dependencies, data models, and interfaces. Specify internal algorithms only where a material risk or requirement makes them consequential.
3. **Optimize for delivery and change:** Prefer the simplest architecture that meets functional and quality requirements and supports the expected evolution. Every added service, layer, datastore, or framework needs a concrete benefit, cost, and simpler alternative.
4. **Make contracts real:** Define interfaces and important data shapes in the project's actual contract format or language. Avoid vague placeholders such as “handle errors appropriately.”
5. **Use evidence for uncertainty:** Identify high-risk technical assumptions early and resolve them with a focused experiment, prototype, or detailed design before depending on them.
6. **Treat the architecture as evolvable:** Stabilize the main boundaries early. Evaluate later changes against the existing architecture, and record material decisions and reasons without erasing their history.
7. **Separate design from implementation:** Architecture readiness means boundaries and contracts are clear enough to implement; it does not mean the system's runtime behavior or production quality has been proven.

## Choose the Scope

- **New system or major foundation:** Design the system-level boundaries and the main end-to-end collaboration path. In a repository at the architecture stage, prefer a compiling skeleton with real contracts and wired module calls; keep feature behavior as stubs or controlled fakes.
- **Feature within an established system:** First check whether the existing architecture can support the feature. Extend its contracts where needed; do not redesign the whole system by default.
- **Architecture evolution:** Compare the proposed change with current boundaries, data ownership, operational constraints, and existing decisions. Explain why an additive change is or is not sufficient.
- **Non-code or early discovery:** If there is no implementation repository or the requirements are not settled, deliver an architecture proposal with explicit assumptions and open questions rather than inventing code contracts.

Do not use this skill to decide what product should be built, implement feature behavior, or perform a general code-quality or production-risk review. Route those distinct tasks to `product-design`, `code-implementation`, `code-review`, or `production-risk-review` as appropriate.

## Workflow

1. **Establish the baseline.** Read the finalized product requirements, existing architecture documentation and decisions, repository conventions, runtime/deployment constraints, and relevant code. Separate confirmed facts, constraints, assumptions, and unknowns.
2. **Translate requirements into architecture drivers.** Map capabilities and important user flows to system responsibilities. Define quality-attribute scenarios where they affect design (for example availability, latency, security, privacy, scale, operability, or cost); keep measurable targets sourced or mark them as proposed.
3. **Find and retire major risks.** List uncertain or difficult technical areas, their impact, and the evidence needed to resolve them. Prioritize the risks that could change boundaries, data ownership, or external contracts. Run only focused, reversible validation work; record unresolved risks and their owners.
4. **Define system boundaries.** Name modules by cohesive responsibility. For each, state its owned behavior/data, public contract, and permitted dependencies. Define communication paths and the main request or event flow. Avoid circular dependencies, ambiguous shared ownership, and boundaries that add deployment complexity without a requirement-backed benefit.
5. **Define key models and contracts.** Specify important domain concepts and ownership; APIs or message contracts; validation, errors, authorization, ordering, and consistency expectations where relevant; and compatibility/versioning implications for public interfaces. For frontend/backend systems, consider a typed semantic client boundary so UI code depends on business concepts rather than transport details.
6. **Resolve consequential choices.** Compare viable options for decisions that affect system boundaries, operations, or future change. Record the selected option, rationale, rejected alternatives, consequences, and remaining uncertainty. Verify version-sensitive technology claims against current official documentation when they influence the choice. Read [references/architecture-methods.md](references/architecture-methods.md) for guidance on C4 views, layered system overviews, arc42 documentation, and ADRs. For data/AI platforms or tool-using agent systems, consult [references/platform-architecture-patterns.md](references/platform-architecture-patterns.md); for CI/CD platforms, consult [references/ci-cd-platforms.md](references/ci-cd-platforms.md); for cloud security detection and response platforms, consult [references/security-detection-platforms.md](references/security-detection-platforms.md). Select only the responsibilities justified by requirements.
7. **Build the architecture artifact.** In an implementation repository, express the contracts in code and wire the main collaboration path with stubs or fakes; it should compile without claiming complete behavior. Add only useful diagrams and concise explanation for relationships that code cannot make clear. For a bounded change, provide the smallest contract or decision record that resolves the design question.
8. **Verify the design.** Trace each important product flow through the proposed modules and contracts. Check dependency direction, ownership, error paths, and quality requirements. For a code skeleton, run the relevant formatter, compiler/build, and contract or architecture checks available in the repository. Report exactly what was run and what remains unverified.
9. **Record evolution and hand off.** Summarize decisions and their rationale, the artifact locations, unresolved risks with owners/checks, and how later work can add behavior without reopening settled boundaries. At a milestone or material architecture change, compare with the prior baseline and record what changed and why. Treat finalized records as append-only; supersede rather than silently overwrite them.

## Expected Output

Scale the output to the system and decision at hand. It should make the following clear, as applicable:

- Architecture drivers, constraints, assumptions, and explicit non-goals.
- Module responsibilities, data ownership, dependency rules, and key collaboration flows.
- Important data models and interface/message contracts.
- High-risk assumptions, evidence gathered, unresolved risks, and next checks.
- Consequential decisions, alternatives, rationale, and consequences.
- The compiling system skeleton or the reason a code artifact is not applicable.
- Architecture views or explanation only where they add information beyond the contracts.
- Verification performed and what readiness does not prove.

## Readiness Check

- A reviewer can tell what each module owns and how modules collaborate.
- Key data and interfaces are concrete enough for independent implementation without inventing incompatible contracts.
- Decisions that could reshape the system have either evidence or an explicitly tracked risk.
- The proposed design is no more complex than its requirements justify and supports the next expected increments.
- If a skeleton is in scope, the main path is wired and the project compiles while feature behavior remains unclaimed.
- The record explains material evolution and preserves prior decisions.

## Red Flags

- Calling a folder tree, technology list, or diagram alone a complete architecture.
- Designing modules before understanding the approved product behaviors and constraints.
- Choosing a fashionable technology or distributed deployment boundary without a requirement-backed reason.
- Deferring a high-impact unknown until feature coding depends on it.
- Writing extensive internal detail for low-risk modules while leaving system contracts vague.
- Building a working feature implementation and presenting it as an architecture skeleton.
- Claiming runtime reliability, scalability, or production readiness from static design or compilation alone.

## References

- Read [references/architecture-methods.md](references/architecture-methods.md) when choosing architecture views, documentation depth, or decision-record format.
- Read [references/platform-architecture-patterns.md](references/platform-architecture-patterns.md) when designing a data platform, AI platform, or tool-using agent system.
- Read [references/ci-cd-platforms.md](references/ci-cd-platforms.md) when designing workflow orchestration, build runners, artifact handling, or deployment services.
- Read [references/security-detection-platforms.md](references/security-detection-platforms.md) when designing security telemetry ingestion, detection, incident management, or automated response systems.
- Use the repository's own architecture rules and product decisions when they exist; this skill does not replace them.

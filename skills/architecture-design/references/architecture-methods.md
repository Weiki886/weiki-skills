# Architecture Methods

Use these methods selectively. They are complementary aids, not mandatory document checklists.

## C4 Views

Start with the smallest set of views that clarifies the system. A system context view explains the system and its external actors/dependencies; a container view shows the major applications and data stores; component or code-level views are useful only for parts whose internal structure needs discussion. Most projects do not need every level. Keep diagrams consistent with the contracts and implementation, and do not draw elements that do not affect a decision.

Source: [C4 model diagrams](https://c4model.com/diagrams)

## Layered System Overview

When a reader needs a single orientation view, a layered overview can summarize the system without replacing the detailed architecture:

1. **Drivers:** show the few quality requirements or constraints that materially shape the design.
2. **Context:** place external actors, data sources, and consuming systems around the system boundary.
3. **Capabilities:** group the system's major responsibilities or containers and show their important dependencies.
4. **Flows:** show a small number of representative end-to-end paths, such as data ingestion, user queries, or model delivery.
5. **Deployment and trust boundaries:** add tenants, control/data planes, networks, or storage locations only when these affect ownership, isolation, security, or operations.
6. **Legend and traceability:** define arrow meanings and connect major components or flows to requirement identifiers when the diagram is used for review.

Keep the overview at one abstraction level where possible. If it combines context, components, runtime flows, and deployment topology, label those regions clearly and move detail to separate views when arrows or labels become difficult to follow. Distinguish synchronous calls, asynchronous events, and data movement instead of using unlabeled arrows for all three. Mark proposed elements and unresolved boundaries so readers do not mistake them for implemented facts. Benefits or technology names should explain requirement-backed choices, not serve as promotional decoration.

This pattern is synthesized from the provided layered data-platform diagram. Its Databricks-specific products and services are examples from that diagram, not defaults or recommendations for other systems.

## arc42 Documentation

Use arc42 as a menu of topics when a durable architecture description is needed: context, constraints, solution strategy, building blocks, runtime views, deployment, cross-cutting concepts, quality requirements, risks, and decisions. Do not fill every section mechanically. Select the sections that explain design rationale or guide implementation, and keep text in version-controlled formats when practical.

Sources: [arc42 documentation](https://arc42.org/documentation/), [arc42 method](https://arc42.org/method/)

## Architecture Decision Records

Write an ADR for a consequential choice that future contributors would otherwise have to rediscover. Keep one decision per record and include:

- **Title:** concise decision topic.
- **Status:** proposed, accepted, rejected, deprecated, or superseded.
- **Context:** forces, requirements, and constraints that make a decision necessary.
- **Decision:** the selected option, stated clearly.
- **Consequences:** benefits, costs, constraints, and follow-up work.

Include alternatives when they clarify why the selected option fits. Keep records append-only; create a superseding ADR when a decision changes rather than rewriting history. Small or reversible implementation details do not need an ADR.

Sources: [AWS Prescriptive Guidance on ADRs](https://docs.aws.amazon.com/prescriptive-guidance/latest/architectural-decision-records/), [arc42 ADR tip](https://docs.arc42.org/tips/9-5/)

## Related Skill Example

The public [`api-and-interface-design` skill](https://github.com/addyosmani/agent-skills/tree/main/skills/api-and-interface-design) offers useful contract-first guidance for boundaries and interfaces. This architecture skill applies that idea at the system level and keeps implementation of individual features out of scope.

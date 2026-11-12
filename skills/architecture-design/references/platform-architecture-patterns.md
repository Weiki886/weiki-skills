# Platform Architecture Patterns

Use this reference when designing a platform that coordinates data workloads, AI capabilities, or agents that can invoke tools. The diagrams that informed these patterns are illustrative examples, not target architectures. Do not reproduce their vendor products, component inventory, deployment shape, or arrows without validating them against requirements and evidence.

## Design From Responsibilities

Start with user and system flows, then assign responsibilities. A platform overview may show actors and source systems, major platform capabilities, consumers, important flows, and trust or deployment boundaries. Keep the diagram at a consistent abstraction level, label distinct views, and define arrow semantics. Separate synchronous requests, asynchronous events, and data movement. If a relationship or direction is unclear, verify it rather than inferring behavior from a drawing.

Every box should have a clear owner, contract, and reason to exist. Combine responsibilities when separate services do not provide a requirement-backed benefit. Split them when ownership, scaling, isolation, reliability, or independent change requires it.

## Data and AI Platforms

Consider these responsibilities only when they appear in the requirements:

- **Ingestion:** connect to databases, files, streams, or external applications; define schema handling, incremental progress, replay, deduplication, and source failure behavior.
- **Processing:** transform and validate data; define batch versus streaming semantics, ordering, late or duplicate events, and retry safety.
- **Storage and serving:** identify authoritative data ownership, lifecycle, retention, consistency, and the interfaces used by analytics, applications, or models.
- **Governance:** define identity, authorization, metadata, lineage, audit, and policy enforcement across data access. Do not treat a catalog or metadata store as a substitute for authorization.
- **Consumption:** show how analytics, applications, and ML workloads read or publish data, including freshness and contract expectations.

For a data-lifecycle view, a raw-to-refined-to-consumer progression can clarify quality gates and ownership. Treat named layers such as Bronze/Silver/Gold as one possible vocabulary, not a required architecture. Show batch and real-time paths separately when their latency, delivery, or recovery guarantees differ. Multi-tenant control and data planes are useful only when tenancy, management ownership, isolation, or residency requirements make that split real.

## Tool-Using Agent Platforms

An agent platform may need some of the following responsibilities. Decide from risk, scale, and product requirements rather than copying a reference inventory:

- **Ingress and identity:** authenticate the caller, establish tenant/user context, and constrain which agents or capabilities are available.
- **Agent definitions and orchestration:** version agent configuration and coordinate model calls, tool calls, state transitions, limits, and cancellation. Make the plan/act/observe loop and any delegation boundaries explicit.
- **Model routing:** select among model providers based on capability, latency, cost, availability, and data-handling constraints; define timeout, fallback, and degraded behavior.
- **Tool discovery and execution gateway:** publish typed tool contracts, validate inputs and outputs, apply quotas, authenticate to external systems, and contain or sanitize untrusted responses.
- **Authorization and approval:** enforce policy outside model-generated reasoning, at each consequential action boundary. Apply least privilege and distinguish reads from writes or other side effects. For actions requiring human review, define the pause point, approval identity, expiration, rejection, escalation, and resume behavior.
- **Memory and state:** separate short-lived conversation/task state from durable user or domain memory. Define ownership, retention, deletion, access controls, provenance, and how stale or untrusted content is handled.
- **Sandbox and workload execution:** isolate code or other untrusted workloads when required; set time, resource, network, and filesystem boundaries and define cleanup after success, failure, or cancellation.
- **Durable work coordination:** use a queue or workflow engine only when tasks outlive a request or need durable retries, scheduling, or recovery. Specify idempotency, deduplication, ordering, retry limits, and dead-letter or operator recovery paths.
- **Evaluation, observability, and audit:** treat these as related but distinct. Traces explain execution and latency; evaluations measure behavior against test cases or policies; audit records provide durable evidence of identity, decisions, approvals, and side effects. Define sensitive-data redaction and retention for each.

## Trace the Critical Flows

Document only flows that expose important contracts or failure boundaries. For an agent platform, trace as applicable:

1. **Ordinary request:** caller identity → agent selection → orchestration/model decision → authorized tool call or response → result validation → user response and trace.
2. **Sensitive action:** proposed side effect → policy decision → approval pause and notification → approve/reject/timeout → revalidation and execution or safe cancellation → audit record.
3. **Long-running or failed work:** durable task acceptance → progress/state persistence → bounded retry or compensation → terminal result, cancellation, or operator recovery.

For each path, state which component is authoritative for identity, permission, task status, and side-effect outcome. Show what happens when a model, tool, approval service, queue, or external provider is unavailable. Retries must not silently duplicate irreversible actions.

For data platforms, trace representative ingestion and query/model-consumption paths, including validation, ownership changes, freshness, failure recovery, and governance checks. Avoid drawing every possible source-to-consumer combination; choose paths that test the design's important boundaries.

## Review Questions

- Are trust boundaries explicit between callers, orchestration, models, tools, external systems, and tenant data?
- Can untrusted prompts, retrieved memory, tool outputs, or generated plans cause an unauthorized side effect?
- Are policy checks enforced by a trusted component immediately before the protected action?
- Can operators distinguish a failed action from an action whose outcome is unknown after a timeout?
- Are retries and replay safe for the specific operation, and are irreversible actions auditable?
- Are memory and traces minimized, access-controlled, and retained according to their purpose?
- Are scale, latency, availability, cost, and isolation requirements measurable or clearly marked as unresolved?
- Does each extra gateway, registry, queue, store, router, or plane solve a stated problem that a simpler design cannot adequately solve?

Treat these as architecture questions, not a checklist requiring one service per item. Record omitted responsibilities when the omission is consequential, and validate high-risk assumptions with focused evidence before implementation depends on them.

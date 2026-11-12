# Security Detection Platform Architecture Patterns

Use this reference when designing a security monitoring, detection, investigation, or response platform. The supplied architecture diagram is an educational reference, not a target topology. Cloud providers, event formats, rule frameworks, data stores, retention values, compliance labels, and automation choices shown in it are examples; choose them only when requirements and current evidence support them.

## Model the Detection Lifecycle

Start with the telemetry and incident lifecycle, then assign responsibility and ownership. Depending on scope, a platform may need:

- **Source connectors:** collect cloud audit events, identity events, network telemetry, endpoint signals, Kubernetes events, and SaaS audit data. Define source authentication, scope, polling or subscription behavior, API limits, checkpointing, and source-specific failure handling.
- **Normalization and validation:** map incoming records to a versioned common schema while retaining source provenance and the ability to inspect the original event when policy permits. Define schema evolution, malformed-event handling, timestamp semantics, duplicate detection, and whether enrichment changes the original evidence or creates a derived record.
- **Streaming and durable event handling:** partition and buffer by an explicit key, preserve required ordering, handle late or duplicate delivery, and provide bounded backpressure and replay. A streaming broker is one option, not a required box.
- **Evidence storage:** separate operational event processing from long-term search or retention when their access, latency, cost, or retention requirements differ. Specify tenant isolation, encryption, access control, legal hold/deletion behavior, retention provenance, and integrity expectations. Retention periods must come from product, customer, and legal requirements rather than an example diagram.
- **Context enrichment:** join telemetry with asset ownership, identity, geography, threat intelligence, or resource tags. Define freshness, source precedence, confidence, failure behavior, and how analysts distinguish observed facts from inferred context.
- **Detection and correlation:** manage rules and analytics, correlate across sources, assign severity/confidence, and produce explainable evidence. Version detection content, test it against representative historical and synthetic data, record the rule version that produced each alert, and track false-positive/false-negative tradeoffs.
- **Alert and incident workflow:** deduplicate or group alerts without losing their individual evidence; define priority, SLA clock semantics, assignment, status transitions, escalation, and closure/reopen behavior. Make the incident/case record the authoritative place for investigation state if that is the chosen model.
- **Investigation and hunting:** support queries, saved hunts, timelines, and evidence access under explicit analyst permissions. Ensure that derived context and query results preserve links to source records and collection time.
- **Response orchestration:** run bounded, authorized response actions such as disabling an identity, revoking credentials, isolating a resource, or blocking traffic. Define action preconditions, target verification, approval thresholds, idempotency, timeout/unknown-outcome handling, compensating actions, and operator override.
- **Audit, reporting, and notification:** preserve evidence of rule changes, analyst decisions, approvals, and response outcomes as required. Treat notifications as delivery mechanisms, not the authoritative incident record. Map reporting to only the frameworks, controls, and metrics actually in scope.

Do not create one service for each responsibility automatically. A smaller deployment may implement several as modules in one system. Split boundaries where trust, data ownership, scale, lifecycle, or independent availability makes it valuable.

## Trace Critical Flows

Document representative paths that expose data quality, authorization, and failure behavior:

1. **Telemetry to alert:** source event → authenticated collection → normalization/validation → durable stream or buffer → optional enrichment → versioned detection → alert/case creation → analyst notification. Show how malformed, late, duplicate, or unavailable-source data is represented and recovered.
2. **Investigation:** alert → case assignment → event and context retrieval → analyst query and decision → evidence/timeline updates → resolution or escalation. Define provenance, access decisions, and how evidence remains available for later audit.
3. **Automated or approved response:** detection/analyst action → policy and target checks → approval when required → action execution → verified outcome or explicit unknown state → case/audit update. Never treat a detection score alone as authorization to take a destructive action.
4. **Rule change and back-test:** reviewed rule change → validation and test corpus → controlled deployment/version activation → replay or shadow evaluation where appropriate → monitored alert-quality results → rollback path.

Make feedback loops explicit. For example, a response workflow may change the environment that generates future telemetry; specify how that action is verified, recorded, and prevented from recursively triggering unsafe duplicate remediation.

## Security, Data Quality, and Operational Boundaries

- Treat incoming telemetry and third-party intelligence as untrusted data. Validate schemas and bounds; do not let event fields control authorization or trigger execution without policy checks.
- Preserve distinctions among observed facts, normalized fields, enrichment, detection rationale, and analyst conclusions. Record provenance and timestamps so an investigation can reconstruct what was known at decision time.
- Define clock skew, late arrival, duplicate delivery, partial collection, source outages, and replay semantics. Detection gaps should be visible rather than silently interpreted as “no threat found.”
- Apply least privilege to connectors, analysts, rule authors, approvers, and response executors. Separate permission to author a rule from permission to deploy it or authorize a disruptive response.
- Constrain automated response by action class, target scope, confidence and context requirements, rate limits, approval policy, and a tested recovery path. Prefer reversible actions or staged containment when impact is uncertain.
- Protect evidence and case data with tenant isolation, access logging, encryption, retention controls, and sensitive-field minimization appropriate to the requirements.
- Treat rule content as code: review, version, test, stage, deploy, and roll back it. Track which rule version and input evidence caused each alert.
- Distinguish platform health from security coverage. Monitor ingestion delay and completeness, parser/schema failures, rule execution lag, queue age, enrichment freshness, alert volume, case backlog, and response outcomes where those measures are meaningful.
- Do not infer compliance from a diagram or a list of standards. Identify in-scope controls and evidence requirements, then map architecture capabilities to them with the responsible owner.

## Architecture Review Questions

- Which sources are in scope, what coverage is expected, and how is a source outage or collection gap detected?
- Is the normalized schema versioned, and can investigators trace a detection back to source evidence and enrichment provenance?
- What delivery, ordering, replay, retention, and deletion guarantees apply to telemetry and derived records?
- How do rule changes get reviewed, tested, deployed, evaluated, and rolled back? Can each alert identify the active rule version?
- How are alerts grouped without hiding distinct events or losing their evidence and severity?
- Which system owns case state, evidence links, analyst decisions, and the incident timeline?
- What authorizes each response action, and which actions require a human approval? How are timeout, duplicate execution, rejection, and partial success handled?
- Can operators distinguish “no detection” from “no data,” delayed data, or failed rules?
- Are audit evidence, retention, privacy, and compliance requirements sourced and assigned to owners?
- Which proposed components materially improve coverage, investigation time, response safety, or scale, and which can remain simpler?

Use these questions to surface decisions and evidence gaps, not as a mandatory component checklist. Keep vendor products and standards illustrative, label assumptions, and validate high-impact detection and response risks before implementation depends on them.

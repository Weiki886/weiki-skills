# CI/CD Platform Architecture Patterns

Use this reference when designing a CI/CD service or a substantial workflow-execution subsystem. The reference diagram is educational, not a target topology. Its named products, storage choices, runner types, and component boundaries are illustrative; derive the design from required triggers, workload, trust model, scale, and operating responsibilities.

## Model Responsibilities and State

Start with the lifecycle of a workflow run, then assign ownership. A platform may need responsibilities for:

- **Source event intake:** receive repository events, scheduled triggers, or manual dispatch; authenticate the event source, validate signatures where applicable, deduplicate deliveries, and define replay behavior.
- **Workflow definition processing:** validate and normalize workflow configuration into executable jobs and dependencies. Define expression evaluation, matrix expansion, concurrency, and how invalid or excessive workflows are rejected.
- **Run coordination:** own run/job/step state, transitions, attempts, cancellation, timeouts, and recovery after coordinator failure. Persist the canonical state separately from derived indexes or live log streams.
- **Queueing and scheduling:** apply fairness, priority, quotas, concurrency limits, and cancellation to waiting work; match jobs to eligible capacity without treating a runner label as an authorization control.
- **Runner lifecycle:** provision, lease, heartbeat, monitor, terminate, and clean up execution environments. Define isolation and reset guarantees between jobs, including for self-hosted capacity.
- **Credentials and secrets:** authorize access by workflow, repository, environment, and job. Prefer short-lived, audience-bound identity federation where supported; define secret masking, exposure limits, rotation, and what untrusted pull requests may access.
- **Logs and artifacts:** stream logs separately from durable run state; store build outputs, test reports, and other artifacts with explicit access, integrity, provenance, and retention rules.
- **Caches and registries:** define cache keys, write permissions, trust scope, eviction, and invalidation. Treat caches as performance hints, not trusted build outputs. Define image/package publication, scanning or signing requirements when the product needs them.
- **Deployment coordination:** model environments, approval gates, promotion, rollback, and deployment status. When possible, promote the same immutable, identified artifact across environments instead of rebuilding different bytes for each stage.
- **Status and notification:** publish stable check/run results to source control and notify users without allowing notification delivery to become the source of truth for workflow state.

These responsibilities do not imply one service or datastore per bullet. Keep boundaries aligned with consistency, security, scaling, and independent operation needs.

## Trace Trust-Sensitive Flows

Trace the flows that determine safety and operability:

1. **Ordinary build:** authenticated event → workflow validation and expansion → run/job state creation → queued job → eligible runner lease → step execution → logs/artifacts → terminal state and source-control status.
2. **Untrusted contribution:** fork or otherwise untrusted change → trigger classification → restricted permissions and credential policy → isolated execution → artifact/cache access rules → result publication. Do not assume workflow code is safe because it is stored in the repository.
3. **Deployment:** approved source/release → immutable artifact identity → environment authorization and any required human approval → deployment execution → health/result recording → rollback or recovery path.
4. **Cancellation or infrastructure failure:** cancellation/timeout/provider loss → stop signal and lease expiry → worker cleanup → attempt/run reconciliation → safe retry or explicit terminal/unknown state.

For each flow, identify the authoritative owner of run status, job lease, credential issuance, artifact identity, and deployment outcome. Define behavior for duplicate events, scheduler restarts, runner loss, partial logs, expired credentials, and an external deployment whose result is unknown after a timeout. Retries must not silently repeat a non-idempotent deployment.

## Security and Reliability Boundaries

- Treat workflow files, build scripts, dependency hooks, logs, artifacts, and cache contents as potentially untrusted inputs.
- Separate control-plane privileges from code-execution privileges. A runner should receive only the job-scoped credentials and network access it needs.
- Make hosted and self-hosted runner trust assumptions explicit. Self-hosted workers may persist state or share a network; document isolation, cleanup, patching, and access boundaries rather than assuming equivalence with ephemeral workers.
- Keep secrets out of logs and artifacts, and do not rely on log masking as the only protection against exfiltration.
- Bind artifact identity to the source revision and build run. Define who can overwrite, read, promote, or retain artifacts.
- Make queues bounded by explicit concurrency and resource policies. Define starvation/fairness behavior and prevent a lost lease from causing two active workers to execute the same job unintentionally.
- Treat caches as an integrity boundary: define which workflows can populate and consume shared entries, and prevent untrusted jobs from poisoning trusted release builds.
- Record durable audit events for permission changes, secret access decisions, approvals, and deployments when auditability is a requirement; operational logs alone may not provide that guarantee.

## Architecture Review Questions

- Which triggers are trusted, and how are signatures, duplicate deliveries, replay, and unauthorized dispatch handled?
- How are workflow definitions validated, versioned, and limited to protect the control plane from excessive or malicious workloads?
- What is the run-state machine, and which component is authoritative after retries, cancellation, or worker loss?
- What isolation exists between jobs, repositories, tenants, and hosted versus self-hosted runners?
- How are runner leases, heartbeats, timeouts, cleanup, and capacity exhaustion handled?
- What exact permissions and credentials are available to pull-request, branch, tag, and deployment workflows?
- Can an untrusted build influence a trusted artifact or shared cache later consumed by a release workflow?
- Are artifacts immutable and traceable from source revision through environment promotion?
- What does rollback mean for stateful deployments, and how is an ambiguous external outcome reconciled?
- Which components are required by measured workload or security needs, and which can remain a simpler internal module?

Use these questions to expose decisions, not to prescribe a fixed product design. Mark unknown trust assumptions and validate high-impact ones before the architecture depends on them.

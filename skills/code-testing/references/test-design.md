# Derive Test Cases from Contracts

## Determine the Correct Result Before Reading the Implementation

Use expectations in this order: explicit user requirements/accepted Proposal, acceptance criteria, interface and protocol contracts, then agreed compatibility behavior. Use implementation and existing tests to discover candidate paths and contradictions; they do not automatically override these sources.

If a requirement only says “handle reasonably” or “support pagination,” find an existing contract. Mark unresolved expectations for confirmation and continue covering the explicit parts. Do not infer business policy from code and lock that guess into a test.

When characterization tests are needed for legacy code, label them as documenting current behavior. If current behavior conflicts with requirements, preserve evidence of the defect rather than elevating incorrect output into a business contract.

## Minimal Case Map

The following is a format example, not a required standalone document:

| Rule source | Scenario / input | Expected result and side effects | Layer | Test location |
| --- | --- | --- | --- | --- |
| Free shipping at 10,000 cents | 9,999 / 10,000 / 10,001 cents | 600 / 0 / 0 cents shipping fee | Unit | Corresponding boundary case |
| Only the owner may edit | B edits A's resource | Contract-defined rejection; record remains unchanged | API integration | Unauthorized-edit rejection case |

Every key rule needs at least one test that distinguishes a correct implementation from a plausible incorrect one. Repeating data for a covered path is less valuable than a missing rejection path.

## Select Scenarios by Risk

Use only rows that apply to the target behavior; do not impose the full matrix on routine changes.

| Risk | Candidate cases | Key assertions |
| --- | --- | --- |
| Value range / boundary | Representative equivalence classes; below/at/above threshold; empty/missing/oversized/Unicode | Correct value, error type and ownership; distinguish missing, empty string, and zero. |
| Combined rules | Roles × ownership; conflicting conditions | Which rule wins; use a decision table when useful, not a pointless Cartesian product. |
| State change | Valid/invalid transition, repeated action, read after restart/reload | Before/after state and visible result; invalid transition has no partial write. |
| Failure path | Dependency timeout/rejection, malformed response, write failure, cancellation | Error propagation, rollback, resource cleanup; do not merely assert that an exception exists. |
| Idempotency / retry / concurrency | Replay same idempotency key, retry after partial success, controlled interleavings | Final state, deduplication, side-effect count; control races with barriers/events, not sleeps. |
| Persistence / contract | Serialization round-trip, reconnect and read, real schema/constraint | Data is actually readable and format compatible; an in-memory return value does not prove it was stored. |
| User flow | Primary action succeeds, error message, state after refresh | User-visible behavior and required backend result; screenshots do not replace functional assertions. |

Derive boundary values from the rule, not by copying implementation constants mechanically. Give parameterized cases readable names.

## Choose the Test Layer

- **Unit:** Pure computation, parsing, validation, and state decisions; quickly cover branches and boundaries without retesting language/framework behavior.
- **Integration:** Routing to business logic, real database constraints/transactions, filesystem, serialization, and adapters; use isolated resources. A SQLite substitute cannot prove production-database-specific behavior.
- **Contract:** Requests, responses, error semantics, and compatibility across a service boundary. Matching hand-written stubs and clients do not prove the real service contract; use independent schema/provider evidence.
- **E2E:** Critical user journey through real application layers. Replace only explicitly identified external dependencies and report which edges remain simulated. UI tests against mocked APIs cannot prove backend correctness.

Put each risk at the smallest effective layer. Higher layers add wiring/configuration/integration evidence; do not duplicate identical detail at every layer.

## When to Use Property-Based or Fuzz Testing

For parsers, encoders, sorting, money calculations, state machines, or other large input spaces, use the project's existing property-based/fuzz tools when useful:

- Define the valid input domain and property, such as serialization round-trip fidelity, preserving a collection's elements under sorting, or unchanged state after repeating an idempotent action.
- Derive properties from the contract. Encoder and decoder may share the same bug, so combine round trips with independent known examples. A constant function can satisfy idempotency; do not rely on one property only.
- Save failing seeds and minimize counterexamples into fixed regression cases. Record generation range/budget; one random run does not prove exhaustiveness.
- Without an existing tool, start with concrete and parameterized examples. Follow dependency policy before adding tools; do not introduce a full framework for a small change.

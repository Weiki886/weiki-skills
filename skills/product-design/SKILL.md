---
name: product-design
description: Use before investing in development for a new product, feature, or substantial requirement. Covers research, alignment, a text-first two-step design process, and a finalized Proposal. Express tradeoffs as alternatives, a choice with reasons, and explicit non-goals; define behavior with examples and scale detail using MiniSpec or FullSpec. Use for product proposals, PRDs, feature designs, and requirement plans. A feature list without tradeoffs is not a complete design.
metadata:
  short-description: Make product decisions and write a Proposal before development
---

# Product Design

Product value depends on building the right thing, not merely building something. Before development, determine what to build, for whom, which problem it solves, and what is out of scope; then align the team. A well-built solution in the wrong direction has no value, and late discovery makes rework more expensive.

The output is an engineering decision document. Once finalized, treat it as read-only; document changed requirements in a new revision instead of overwriting the prior decision.

## Core Principles

1. **Three parts of a decision:** For each design choice, state the alternatives, the selected option, and why; explicitly state what is not included this time. A feature list without tradeoffs is not a complete product design.
2. **One increment at a time:** Each Proposal decides one clear point. Split adjacent decisions into later proposals. A narrow proposal that can be accepted, implemented, and verified is better than one that attempts to solve every future problem.
3. **Keep it direct:** Start with a real example, then generalize. Do not introduce multiple mechanisms when one rule explains the behavior.
4. **Design by subtraction:** Remove redundant and unnecessary burdens, including boilerplate, manual steps, and exceptions users must remember. Do not add features merely to appear complete.
5. **Make boundaries explicit:** State what is and is not solved, which cases are valid, which are errors, and whether existing behavior changes.
6. **Generalize from real scenarios:** Derive a need from the common ground among real user stories; do not mechanically add a feature for one isolated case.
7. **Do not reverse the two-step order:** First describe the decision in text; then prototype from the finalized text. Products without a UI (compilers, SDKs, CLIs, etc.) need only the first step.
8. **Evolve documents append-only:** Freeze finalized documents. Record changes in a new revision so prior decisions remain traceable.
9. **Mark unverifiable claims as TBD:** Competitor claims, user pain, and numeric thresholds without sources must be marked for confirmation, not invented.
10. **People own tradeoffs:** Tools can improve drafting efficiency, but people decide each tradeoff and may reject it. Detail exists to support judgment, not to substitute length for judgment.

## Choose the Context

State two decisions before starting; the user may correct them:

- **UI or no UI:** UI products (apps, websites, IDE extensions) need both text and prototype. Products without a UI (compilers, SDKs, CLIs, protocols) need the text only; prototype/Demo is not applicable.
- **Spec size:** Use `MiniSpec` for a small, clear decision. Use `FullSpec` for durable or broad-impact changes, with explicit boundaries, alternatives, and acceptance criteria. Do not force small changes into a full template or let large changes remain vague.

Choose the document location based on collaboration: use an Issue and status Labels for team work (see “Status Flow”); a solo project may use `docs/` or an Issue and may omit Labels. Do not omit the decision tradeoffs, two-step sequence, or append-only evolution.

## Reflect the Request Before Research

Before researching or drafting, summarize in a few lines:

- The problem, the intended user, and what success looks like.
- What the user stated versus what you are assuming; mark unresolved details as TBD.
- Ask the user to correct the direction before proceeding.

This is alignment on direction, not wording. Correcting the understanding now costs far less than reworking a finalized design.

## Workflow

1. **Research the domain and competitors:** Investigate common industry approaches, relevant competitors, and their tradeoffs. Summarize approaches, strengths, weaknesses, what to learn, and what to avoid. Support conclusions with links or first-hand use evidence; do not invent competitor capabilities. Use a collaboration document or Wiki if useful; team consensus does not require the research to be in the Issue.
2. **Reach alignment:** Discuss the findings and converge on one direction before detailed design, so later contributors are not acting on different assumptions.
3. **Refine using the two-step method:**
   - **Step 1, text:** Describe the problem, desired behavior, non-goals, and boundaries as decisions, not UI.
   - **Step 2, prototype:** After the text is finalized, make interactions and shape concrete. Before real code exists, a standalone prototyping tool is acceptable. Once the repository has working application code, build the prototype on that code as a display-only PR that will not be merged, so it stays connected to the real product. Attach the prototype/image/link below the text instead of creating a separate UI-design Issue.
4. **Finalize as a Proposal:** Put it in an Issue for team work or a design document for solo work. A non-merging draft PR may support line-by-line review; copy the finalized proposal back to the Issue. Then treat it as read-only; capture later changes in a new revision.

## Proposal Contents

| Field | Description |
| --- | --- |
| Motivation / user story | Which real scenario or pain triggered this; summarize the shared need in one sentence. |
| Target users | Who this is for. |
| Current approach and gaps | How users solve it today and what redundant or manual work remains; write “none” if applicable. |
| Scope and explicit non-goals | What is included and excluded; required for any non-trivial proposal. |
| Key decisions and rationale | Core rules or options, alternatives considered, selected choice, and reasons (including competitor or alternative approaches). |
| Core concepts and information structure | Define domain concepts and how information/data is organized as input to architecture. |
| Prototype / Demo | For products with a UI, attach the prototype built after the text is finalized; omit for products without a UI. |
| Acceptance criteria | Concrete examples of success that can be verified independently and do not depend on implementation technology. |

See `references/proposal-template.md` for MiniSpec and FullSpec formats.

## Define Behavior with Examples

Examples are part of the specification, not decoration. For each important behavior, show:

- **Current state:** What happens now?
- **Proposed state:** What will happen after the design takes effect?
- **Boundaries and invalid cases:** Which inputs are valid, what errors occur, and how conflicts or omissions are handled?

Interaction behavior may use Given/When/Then: initial state, user action, and observable result. The three-part current/proposed/edge-case pattern gives reviewers and implementers stronger evidence than abstract prose alone.

## Status Flow

Use Labels before and after finalization. Reuse the Label and Issue governance in `github-engineering-workflow`; do not redefine it here:

- `proposal`: draft under design; open to discussion and changes.
- `Proposal-Accepted`: accepted for development; read-only thereafter, with changes captured in a new revision.
- `Proposal-Denied`: rejected; record why and archive it. Rejection is a normal outcome, not a failure.
- `Proposal-NoPlan`: valid idea, not planned for this cycle; record it for possible later work.

Use `FullSpec` or `MiniSpec` to indicate detail level. See `references/process.md` for examples, including accepted and rejected proposals.

## Responsibility Boundaries

- This skill owns pre-development product decisions: research, tradeoffs, the two-step method, and Proposal quality.
- `github-engineering-workflow` owns the process artifacts: Issues, Labels, Milestones, PRs, and solo/team adjustments.
- `code-implementation` owns work after a Proposal is accepted; `code-review` owns implementation review.

## Red Flags

| Assumption | Correction |
| --- | --- |
| “Listing every feature is a design document.” | A feature list without alternatives and tradeoffs is not product design; add options, selection, and rationale for each decision. |
| “Prototype first; write the text later.” | This reverses the required order. A prototype follows finalized text so the discussion is about decisions, not just UI. |
| “One proposal should settle all related needs.” | Decide one increment at a time; move adjacent decisions to later Proposals. |
| “A small change should use the full template to look rigorous.” | Scale detail to impact; a MiniSpec is enough when it clearly states the fields. |
| “A large change can start with two lines and be figured out later.” | A FullSpec must explain boundaries, alternatives, and acceptance; do not leave it vague. |
| “A competitor probably works this way; include that.” | Verify the claim or mark it TBD. Do not invent competitor behavior. |
| “Requirements changed, so overwrite the original document.” | Keep finalized versions read-only and document the change in a new revision. |
| “A rejected proposal is a failure.” | Record the reason and archive it. Rejecting a wrong direction early is product-design value. |

## Acceptance Checklist

A product-design document is ready when:

- [ ] The request summary has been confirmed; assumptions and TBDs are explicit.
- [ ] The direction reflects domain and competitor research with traceable evidence.
- [ ] In-scope and out-of-scope behavior are clear.
- [ ] Key decisions explain alternatives and rationale rather than listing features.
- [ ] Core concepts and information structure are clear enough for architecture work.
- [ ] A UI product has a corresponding prototype after the text is finalized.
- [ ] Examples define current behavior, proposed behavior, and boundaries; acceptance criteria are verifiable.
- [ ] Status and spec-size Labels are complete and traceable.

## Output

Concisely report: (1) context (UI/no UI and MiniSpec/FullSpec) with reasons; (2) key research findings and sources; (3) tradeoffs (alternatives, choice, rationale); (4) explicit non-goals; (5) Proposal location (Issue or document path) and status Labels; and (6) unresolved TBDs and the smallest next action.

## References

Read only what is needed:

- `references/proposal-template.md`: MiniSpec and FullSpec templates and field guidance.
- `references/process.md`: research checklist, request reflection format, prototype staging, draft PR review, and example index.

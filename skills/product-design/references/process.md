# Process Details and Examples

## Research Checklist

Industry and competitor research should answer five questions:

1. What is the common industry approach to this problem?
2. Which competitors or open-source projects address it, and what trade-offs do they make?
3. What are the strengths and weaknesses of each approach?
4. What should we borrow, and what should we avoid?
5. How does our context differ, and why can we not copy an existing solution as-is?

Requirements:

- Attach sources to conclusions (official documentation, source code, or evidence from real use); mark unsupported claims `TBD`.
- Do not invent competitor capabilities, version behavior, or market conclusions.
- Choose a suitable research medium (collaborative document, wiki, etc.); team consensus is enough, and an Issue is not mandatory.

## Confirming the Brief

Before researching or drafting, summarize the request for confirmation:

```text
My understanding:
- Problem to solve: ...
- Target users: ...
- What success looks like: ...

Explicitly stated by the user: ...
My assumptions: ...
Open questions (TBD): ...
```

Revise the brief after corrections. When the request already includes sufficient goals and constraints, restate it once instead of asking redundant questions.

## Prototypes by Project Stage

- **No existing code (early project):** Build the prototype independently in a prototyping tool; the key requirement is that it can be run and reviewed locally.
- **Existing production code:** Build the prototype on top of the real codebase and present it as a display-only PR that will not be merged (essentially, web coding). Its purpose is to clarify the intended product and behavior. Do not keep layering features onto an unmerged early prototype, or it may drift from production logic.
- Put prototype images and design links directly below the product-design text. Do not create a separate Issue solely for UI design.

## Reviewing a Draft PR

A non-mergeable draft PR can support line-by-line review of the design document and prototype:

1. Commit the design draft to a draft PR so reviewers can comment inline.
2. Once discussion converges, move the final version back to the Issue and close the draft PR without merging it.
3. Apply the appropriate status and specification-size labels to the Issue. Treat the finalized document as read-only; make later changes as a new revision.

This is a team workflow convention, not a requirement for solo projects.

## Examples

Interface-free product (textual decision only, no prototype):

- [goplus/xgo#2797 Function Decorators](https://github.com/goplus/xgo/issues/2797): a language-syntax proposal.

Product with an interface (approved text followed by a prototype):

- [goplus/builder#494 v1.3 UI Design](https://github.com/goplus/builder/issues/494)
- [goplus/builder#3214 animation UI](https://github.com/goplus/builder/issues/3214)

Decision states:

- [goplus/xgo#2802 Flat Mode](https://github.com/goplus/xgo/issues/2802): accepted and labeled `FullSpec`.
- [goplus/xgo#2667 defer command syntax](https://github.com/goplus/xgo/issues/2667): rejected with rationale. Rejection is a valid outcome, not a failure.

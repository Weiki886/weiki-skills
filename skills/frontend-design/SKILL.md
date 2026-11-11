---
name: frontend-design
description: Use when designing, rebuilding, reviewing, or refining a frontend interface, including websites, landing pages, dashboards, SaaS products, components, forms, empty states, and interaction flows. Choose build, refine, redesign, or review; inherit the existing stack and design system; then establish a subject-grounded visual direction, implement production-ready UI, and verify it through real rendering. Not for backend-only or API-only work.
metadata:
  short-description: Design intentional interfaces and verify real rendering
---

# Frontend Design

Deliver an interface with a clear visual point of view that supports its task and holds up in real use, rather than applying a generic template.

## Non-Negotiable Constraints

- User requirements, existing brand, design system, platform conventions, and repository rules take precedence. When the user asks for a high-fidelity reproduction, prioritize fidelity instead of replacing it with personal taste.
- Preserve the existing stack, component system, and working behavior. Do not replace the framework, add dependencies, redesign the global theme, or expand scope without a real need.
- Separate creation from review: build/refine may change code within the authorized scope; review/audit is read-only by default. Fix the review target first and do not use review as a pretext for refactoring.
- Do not invent users, customers, metrics, testimonials, logos, asset rights, or design standards. When content is missing, use only the factual copy needed for understanding and operation, and state assumptions.
- Accessibility is part of implementation, not cleanup. Establish semantic structure, keyboard operation, visible focus, labels, contrast, and target size as you build.
- Source inspection does not replace visual verification. When rendering is available, inspect the actual page, interactions, and key states. If it is not, state what remains unverified.
- Leave general logic, test gaps, and maintainability to `code-review`; leave production data, outage, and rollback risk to `production-risk-review`. This skill owns frontend experience and interface implementation issues that directly affect it.

## Choose a Mode

- **build:** Create a page, flow, or component. Identify the interface's job, establish a visual direction, and implement it.
- **refine:** Improve an existing interface's clarity, usability, or polish. For an AI-generated first draft with generic effects, over-carded layout, or weak hierarchy, use the staged visual-reduction workflow in [ai-generated-ui-refinement.md](references/ai-generated-ui-refinement.md). Preserve its identity, information architecture, business content, and out-of-scope behavior; change only what serves the target.
- **redesign:** Replace the visual language. Preserve product facts, content, features, and constraints; treat the existing UI as evidence rather than mandatory styling.
- **review / audit:** Evaluate screenshots, a URL, a running page, or code. Fix the target first; for a PR, record the base/head or corresponding commits.

When scope is unclear, make and state the smallest reasonable assumption. Ask a focused question only when missing brand, stack, audience, or primary task information would materially change the outcome.

## Read the Real Context Before Acting

Inspect the affected route/component and neighboring UI. Establish:

- What the product is, who uses it, the surface's primary job, and its success action.
- Existing tokens, components, typography, colors, spacing, icons, illustrations, images, and copy style.
- Layout and interaction conventions, themes, breakpoints, target devices, supported languages, and accessibility requirements.
- Loading, empty, error, success, disabled, long-content, and insufficient-permission states.
- Repository-prescribed lint, typecheck, test, build, and browser-verification methods.

Reuse real components, content, and assets where possible. When departing from the design system, explain why and bound the impact; do not create a competing visual language.

For border-heavy surfaces, theme typography, response-text reveals, unstable numbers, or drag feel, read [surface-and-type-refinement.md](references/surface-and-type-refinement.md). Improve actual grouping, readability, and stability; theme weight changes, blur, and springs are contextual choices.

For navigation, routing, Back/Up behavior, drawers, dialogs, transitions, or deep links, read [navigation-and-routing.md](references/navigation-and-routing.md). Establish destination, container, and history behavior before changing interactions; a screenshot alone cannot verify these contracts.

When a request says only “popup,” or feedback, confirmation, and content capacity may call for different surfaces, read [overlay-selection.md](references/overlay-selection.md). Choose by interruption and task capacity; define modality, focus, timeout, and recovery rather than treating every overlay alike.

For sidebar grouping, nested navigation, utility placement, compact icon modes, or long menu labels, also read [sidebar-navigation.md](references/sidebar-navigation.md). Retain all permitted destinations and make every mode accessible beyond hover.

For image loading, pending requests, refresh, prefetching, empty results, or failure feedback, read [loading-empty-and-error-states.md](references/loading-empty-and-error-states.md). Distinguish initial load from refresh, establish truthful state transitions, and choose recovery actions before styling indicators.

For form controls, uploads, code entry, autocomplete, multi-step forms, or validation, read [forms-and-validation.md](references/forms-and-validation.md). Establish field semantics, submitted values, validation timing, and recovery before choosing controls; apply only the relevant modules.

For Select/combobox versus action-menu confusion, small/large option sets, searchable lists, or popup placement, read [select-and-dropdown.md](references/select-and-dropdown.md). Distinguish data selection from commands and preserve the chosen pattern's keyboard and commit behavior.

For crowded layouts, inconsistent spacing, alignment, container widths, or responsive grids, read [layout-and-spacing.md](references/layout-and-spacing.md). Reuse existing tokens and choose density by task; example widths, column counts, and spacing values are not universal requirements.

For cards, collection lists, metric tiles, tables, carousels, masonry, timelines, or contextual containers, read [cards-and-lists.md](references/cards-and-lists.md). Choose representation by reading and interaction task; preserve record identity, order, and action semantics instead of adding variety for its own sake.

For easing, entry/exit timing, spring feedback, or interrupted overlay transitions, read [motion-and-easing.md](references/motion-and-easing.md). Define the motion and lifecycle together, retain platform/design-system conventions, and verify reduced-motion behavior.

For touch feedback, press/ripple effects, animated control states, or mobile gesture polish, read [mobile-microinteractions.md](references/mobile-microinteractions.md). Preserve operation semantics and cancellation; flips, stacks, and sheet gestures are scope-sensitive interactions rather than mandatory visual effects.

For multi-stage success feedback, linked selection summaries, staggered stacks, snap-then-draw connections, wheel settling, or trigger-origin reveals, read [compound-microinteractions.md](references/compound-microinteractions.md). Separate operation outcomes from presentation stages and define interruption/reversal before sequencing effects.

For parallax, sticky section navigation, collapsing covers, viewport reveals, or scroll-linked presentation, read [scroll-interactions.md](references/scroll-interactions.md). Choose the correct scroll/visibility driver and preserve native scrolling, actual data, content access, and fallback behavior.

## Establish a Design Direction

For a substantial new page or redesign, define a concise direction before writing UI code:

1. **Thesis:** One sentence connecting the visual idea, product subject, and user task.
2. **System:** Define color, type, spacing, density, shape, hierarchy, and motion by role.
3. **Signature:** Choose one memorable element grounded in the domain object, workflow, data, or brand.
4. **Risk:** Name one deliberate departure from the obvious solution and explain how it serves the task.
5. **Restraint:** Identify areas that must stay quiet so the interface does not compete with itself.

For complex work, compare 2–3 directions internally and choose one; show alternatives only when the user needs to decide. See [design-direction.md](references/design-direction.md).

Run a **subject-swap test**: if the product were replaced with an unrelated subject, would the main visual choices still work unchanged? If so, they may be template defaults. Any familiar pattern retained should have a subject, hierarchy, functional, or brand reason.

## Implement the Interface

- The first viewport should establish both identity and the primary task. A tool should expose the working experience directly; do not wrap it in an oversized marketing page unless requested.
- Align information, visual, and action hierarchies. Usually emphasize one primary action per view; subordinate secondary actions through placement, style, or progressive disclosure.
- Operational interfaces prioritize scanning, comparison, and repeated actions. Editorial, brand, or showcase surfaces may prioritize composition and rhythm. Do not force every product into the same card grid.
- Containers should express real grouping. Use cards for related or repeated objects; nest them only when a real hierarchy exists. Boundaries, whitespace, and surface changes should aid understanding.
- Give typography display, body, and (when needed) utility roles. Prefer existing fonts; do not add dependencies for novelty. Control line length, leading, and numeric alignment.
- Motion should explain state, hierarchy, or feedback. Prefer `transform` and `opacity`, avoid `transition: all`, and honor reduced motion.
- Write from the user's perspective. Keep action names consistent across buttons, progress, and completion. Explain what happened and what to do next in errors; give empty states one clear action.
- Use semantic elements and the project's interaction primitives. Icon-only controls need accessible names; form controls need persistent visible labels; destructive actions need confirmation or an undo path.
- New pages and structural redesigns should cover desktop and narrow screens by default. Follow explicit device requirements; desktop-first tables still must avoid accidental page overflow and inaccessible key actions.
- Use CSS Grid/Flex for layout instead of JavaScript measurement. Keep selector responsibilities predictable and avoid fragile cascades from competing spacing rules.
- Set image dimensions to control layout shift. Give meaningful images alternative text, keep decorative images out of the accessibility tree, and optimize large lists and media for actual page load.

See [implementation.md](references/implementation.md) for implementation details and interface anti-patterns.

For AI-generated UI drafts, see [ai-generated-ui-refinement.md](references/ai-generated-ui-refinement.md) for the diagnostic checklist, seven-stage cleanup sequence, reusable prompts, and acceptance criteria.

## Review the Interface

Assess these dimensions in order of impact on the user's task:

1. **Task and flow:** Is the main action clear? Does the user know where they are, what to do next, and how to recover from failure?
2. **Visual craft:** Do hierarchy, typography, spacing, alignment, color, density, and composition serve the content together?
3. **System consistency:** Does the UI follow the brand, components, tokens, and platform conventions?
4. **Resilience:** Do empty/error/long-content states, responsive layouts, themes, keyboard operation, assistive technology, and performance meet the use-case baseline?

Report only evidence-based issues that change the decision. Use `Blocking`, `Should fix`, `Polish`, or `Follow-up`; give no more than six primary findings. Each finding states observable evidence, user impact, and a practical fix. Do not present personal preferences as defects. See [review.md](references/review.md).

## Verify and Close

1. Run lint, typecheck, tests, and build appropriate to the change; follow repository conventions.
2. When possible, render the real page. Perform one batch inspection, fix confirmed issues together, then do at most one confirmation pass; do not polish indefinitely.
3. For a small component change, verify affected states and one representative viewport. For a new page, flow, or responsive redesign, verify desktop and narrow screens. Add theme, language, permission, or key-state variants when in scope.
4. Check hierarchy, overflow, overlap, truncation, focus, hover/active, empty/error states, long copy, image loading, console errors, and key interactions.
5. Use screenshots or a running URL as evidence. If rendering was impossible, say “visual verification not performed”; CSS inspection alone does not prove the page works.

See [verification.md](references/verification.md) for test matrices and evidence guidance.

## Completion Report

Briefly report:

- The mode and one-sentence visual direction.
- The interfaces changed and important design tradeoffs.
- Project checks executed.
- Pages, viewports, and states actually rendered.
- Browsers, devices, content, or backend dependencies still unverified.

Claim delivery only when implementation, relevant checks, and available rendering evidence are complete.

## References

Read resources as needed, not all at once:

- [design-direction.md](references/design-direction.md): subject research, surface types, visual direction, typography, and anti-template checks.
- [surface-and-type-refinement.md](references/surface-and-type-refinement.md): five detail refinements, surface boundaries, theme type, response reveals, numeric stability, and direct dragging.
- [implementation.md](references/implementation.md): layout, components, accessibility, forms, responsiveness, motion, performance, and CSS engineering.
- [review.md](references/review.md): design-review dimensions, severity, finding format, and comparing alternatives.
- [verification.md](references/verification.md): rendering matrix, two-pass repair limit, and completion evidence.
- [navigation-and-routing.md](references/navigation-and-routing.md): eight navigation modules, scoped prompts, Back/Up and overlay contracts, deep links, and regression scenarios.
- [overlay-selection.md](references/overlay-selection.md): seven presentation patterns, interruption/capacity decisions, three guardrails, lifecycle rules, and regression checks.
- [sidebar-navigation.md](references/sidebar-navigation.md): eight sidebar modules, grouping, route/ancestor cues, compact-mode access, scoped prompts, and reachability checks.
- [loading-empty-and-error-states.md](references/loading-empty-and-error-states.md): eight feedback modules, scoped prompts, content retention, safe retry, and state-transition verification.
- [forms-and-validation.md](references/forms-and-validation.md): thirteen form modules, control selection, scoped prompts, validation timing, and submission regression checks.
- [select-and-dropdown.md](references/select-and-dropdown.md): five selection rules, select/combobox/menu semantics, contextual option thresholds, popup placement, and keyboard verification.
- [layout-and-spacing.md](references/layout-and-spacing.md): ten layout modules, container/grid/spacing decisions, scoped prompts, density, and reflow verification.
- [cards-and-lists.md](references/cards-and-lists.md): fourteen container modules, task-based selection, scoped prompts, collection semantics, and interaction regression checks.
- [motion-and-easing.md](references/motion-and-easing.md): five motion choices, scoped prompts, timing and spring boundaries, reduced motion, and lifecycle verification.
- [mobile-microinteractions.md](references/mobile-microinteractions.md): twelve optional feedback modules, scoped prompts, input cancellation, gesture boundaries, and state/performance checks.
- [compound-microinteractions.md](references/compound-microinteractions.md): six coordinated patterns, stage dependencies, truthful outcomes, interruption, picker/reveal boundaries, and regression checks.
- [scroll-interactions.md](references/scroll-interactions.md): six optional scroll modules, scoped prompts, driver selection, sticky/refresh/data boundaries, and restoration/performance checks.
- [sources.md](references/sources.md): public frontend/design skills and standards, with integration decisions.

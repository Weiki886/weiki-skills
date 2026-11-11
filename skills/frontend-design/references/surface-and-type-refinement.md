# Surface and Typography Refinement

Use for a focused polish pass on border-heavy surfaces, uneven light/dark typography, abrupt response text, unstable numbers, or sluggish drag feedback. A restrained interface can still use borders, immediate text, unchanged theme weights, and no spring when those choices best support the task.

Contents: [Scope](#establish-the-refinement-scope) · [Diagnosis](#diagnose-before-editing) · [Five refinements](#five-refinement-modules) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Refinement Scope

Preserve business content, layout relationships, information architecture, operations, and existing theme preferences. Reuse surface, text, typography, and motion tokens; do not replace the font family, add a theme, or introduce dragging to complete this checklist. Compare real content in the supported themes and fonts before changing values. Keep changes local unless a shared token is the confirmed cause.

Use [ai-generated-ui-refinement.md](ai-generated-ui-refinement.md) for broad visual reduction, [layout-and-spacing.md](layout-and-spacing.md) for grouping, [motion-and-easing.md](motion-and-easing.md) for animation lifecycle, and [mobile-microinteractions.md](mobile-microinteractions.md) for input cancellation and feedback. This reference adds the surface/type decisions; it does not replace their contracts.

## Diagnose Before Editing

- Repeated heavy borders fragment related content, or removing them would erase a useful boundary.
- Surface levels are indistinguishable in a supported theme or forced-colors mode.
- Body text appears too heavy or too thin in the actual font, language, and target environment.
- Response reveals blur readable content, replay on updates, or delay text that is already available.
- Numeric updates shift neighboring controls despite unchanged digit count, or overflow when precision/length changes.
- Drag visuals lag behind input, overshoot their bounds, or leave an incorrect state after cancellation.

Judge observable readability, grouping, stability, and response rather than treating “plastic” as a measurable defect. Apply only relevant modules.

## Five Refinement Modules

### 1. Surface Levels Before Repeated Frames

**Goal and rationale:** Express grouping with a coherent surface hierarchy and spacing, reducing redundant outlines.

**Rules:** Use existing canvas, inset, surface, and raised roles where meaningful; do not create a new shade for every block. Remove only redundant framing and retain borders/dividers that clarify fields, tables, selection, or boundaries. Keep focus, error, and interaction cues recognizable with adequate contrast. “Subtle” must not mean imperceptible. Test light/dark and supported high-contrast/forced-colors modes; add or retain system-color outlines where background/shadow separation disappears. Do not suppress user color preferences to preserve the visual effect.

**Prompt:**

```text
Refine grouping using the existing surface roles and spacing. Remove redundant heavy frames while retaining boundaries needed for fields, tables, focus, selection, and errors. Keep layers distinguishable in supported themes and forced colors; do not replace useful boundaries with nearly identical backgrounds or create a shade for every block.
```

### 2. Evaluate Theme-Specific Type Weight

**Goal and rationale:** Balance perceived text weight across supported themes without sacrificing legibility.

**Rules:** Inspect the rendered body and supporting text with the actual font files, fallback scripts, sizes, colors, and available weights. If dark-theme text demonstrably looks too heavy, evaluate a modest role-specific weight adjustment using supported font weights. Keep the existing weight when it reads better; a theme difference is not an acceptance requirement. Preserve heading/emphasis roles unless separately justified. Avoid global thinning, reduced contrast, font-smoothing hacks, or animated weight changes as substitutes for readable type. Check wrapping and control alignment after any adjustment, including missing-font fallback and language coverage.

**Prompt:**

```text
Compare body and supporting typography in the actual light/dark themes and supported scripts. If dark text is visibly too heavy, evaluate a modest supported weight adjustment for those roles only. Preserve headings, emphasis, contrast, readability, wrapping, and user preferences. Keep identical weights if they work better; do not globally thin text or rely on font-smoothing overrides.
```

### 3. Optional Ink-Like Response Reveal

**Goal and rationale:** Soften newly arriving response blocks when a brief transition helps reading continuity.

**Rules:** Prefer clear immediate text or a brief opacity transition. An ink-like reveal is an optional treatment, not literal ink particles or a mandatory blur. If scoped, keep any blur small, brief, and limited to the newly arrived block; remove the filter completely at rest. Avoid large-area or looping filters and do not replay on existing content, theme changes, or each streamed token. Preserve the existing stream cadence, cancellation, error state, scroll anchoring, text selection, copying, links, and code blocks. Do not wait for a whole response or slow delivery for decoration. Keep semantic text in one source and status announcements meaningful without announcing every frame/token. Reduced motion and unsupported/slow paths show readable text directly; canceled animation must not leave text blurred or hidden.

**Prompt:**

```text
Optionally soften newly arriving response blocks with a short fade or restrained ink-like reveal. Keep text readable and ready content available immediately; do not buffer the response or animate every token. Preserve streaming, selection/copy, links, code, scroll position, cancellation, and status behavior. Remove blur at rest and provide a direct reduced-motion/fallback path without hidden content or replaying old paragraphs.
```

### 4. Stable Numeric Typography

**Goal and rationale:** Keep changing numeric values easy to compare without unnecessary horizontal movement.

**Rules:** Prefer `font-variant-numeric: tabular-nums` in the existing typeface when it provides the required figures. Verify the loaded font and fallback actually supply equal-width digits. Use an existing monospace face for code, identifiers, or numeric roles when appropriate; do not make all prose monospace or add a font merely for counters. Tabular digits do not guarantee a constant total width when digit count, sign, currency, separators, precision, or locale changes. Plan alignment and sufficient responsive space for real values without clipping or silently abbreviating them. Preserve actual values, formatting, and copy/accessibility semantics. For animated digits, reuse the rolling-number rules in [mobile-microinteractions.md](mobile-microinteractions.md).

**Prompt:**

```text
Stabilize changing numbers using tabular numerals in the existing font where supported, with a suitable existing fallback if needed. Keep prose typography unchanged. Preserve actual values, units, locale, precision, and copying; allow enough space for changing digit count, signs, and separators rather than assuming tabular figures alone prevent all movement. Check aligned columns and nearby controls with realistic extremes.
```

### 5. Direct Dragging and Restrained Settling

**Goal and rationale:** Keep existing drag feedback aligned with input and make release settle predictably.

**Rules:** Reuse the current drag primitive, coordinate model, bounds, and commit/snap policy. Track the active pointer directly; do not debounce movement or put an ease-out/spring transition between the pointer and dragged object. Reserve optional resistance for defined boundaries and a promptly settling spring for release where useful; no visible bounce is also valid. Preserve scroll arbitration, stable hit targets, focus, non-drag/keyboard alternatives, and cancellation. New input must interrupt settling without a jump or stale commit. Do not infer a successful drop/save from animation completion. Clean up pointer capture, listeners, and transforms on cancellation/unmount; reduced motion retains direct tracking and reaches the intended final state without decorative oscillation.

**Prompt:**

```text
Refine the existing drag interaction with direct responsive tracking and a predictable release. Keep bounds, snap/commit rules, cancellation, scrolling, focus, and non-drag/keyboard access intact. Add optional restrained boundary resistance or a promptly settling release spring only where useful; do not smooth pointer tracking into lag. New input interrupts settling, and canceled work cannot commit or leave transforms/listeners behind.
```

## Combined Prompt

```text
Polish the scoped interface using existing design tokens, fonts, components, and behavior. Reduce redundant frames with surface levels and spacing while keeping essential boundaries, contrast, focus, and forced-colors access. Evaluate body weight in actual light/dark rendering; adjust only where readability improves. Use optional brief response-block fades or restrained ink-like reveals without buffering streams, blurring settled text, or replaying existing content. Stabilize numeric roles with supported tabular figures and space for realistic formatted values while preserving prose and data. Keep existing dragging direct and release feedback restrained, cancelable, and accessible. Preserve content, architecture, operations, themes, and reduced-motion behavior; apply only relevant refinements and verify them in real rendering.
```

## Acceptance and Regression Checks

Use the bounded process in [verification.md](verification.md). Mark relevant cases pass/fail/unverified or explain N/A. This skill update supplies guidance; a future implementation must gather its own rendering evidence.

| Scenario | Expected evidence |
| --- | --- |
| Surface hierarchy | Related content groups clearly; necessary control/table boundaries, selected/error states, and focus remain identifiable. |
| Theme and fonts | Supported light/dark themes, scripts, font loading/fallback, and zoom retain readable weight, contrast, wrapping, and emphasis. |
| Forced colors | Controls and important groups remain distinguishable when surface or shadow treatments are overridden. |
| Response delivery | Short/long and streamed content, cancellation/error, selection/copy, code/links, and existing scroll behavior work without replay or delayed delivery. |
| Reveal fallback | Reduced/zero motion, interruption, and disabled animation leave all available text readable with no residual filter. |
| Numeric stability | Same-length updates and changes in length, sign, units, precision, locale, and font loading preserve accurate data and usable alignment without clipping. |
| Drag lifecycle | Pointer tracking, boundary release, rapid re-grab, canceled/invalid drops, and failed persistence retain correct state and cleanup. |
| Access and cost | Keyboard/non-drag paths remain usable; representative content does not accumulate filters, timers, listeners, or observed drag/rendering stalls. |

## Primary References

- [MDN: font-variant-numeric](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/font-variant-numeric): tabular figures and numeric glyph features.
- [W3C: Non-text Contrast](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html): distinguishable control and state cues; decorative borders are not universally required.
- [MDN: forced-colors](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/forced-colors): user palette overrides and affected surface/shadow properties.
- [W3C: Animation from Interactions](https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html): guidance on disabling nonessential interaction motion.
- [W3C: Dragging Movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html): non-drag pointer alternatives are distinct from keyboard access.

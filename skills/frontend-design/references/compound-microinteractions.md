# Compound Microinteractions

Use when one operation needs coordinated feedback across several stages or components: submission feedback, linked selection summaries, staggered stacks, snapping timelines, wheel pickers, or trigger-origin transitions. Apply only patterns that clarify the existing task; missing particles or chained animation is not itself a defect.

Contents: [Contract](#establish-the-sequence-contract) · [Diagnosis](#diagnose-before-editing) · [Six patterns](#six-compound-patterns) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Sequence Contract

Reuse [mobile-microinteractions.md](mobile-microinteractions.md) for individual effects, [motion-and-easing.md](motion-and-easing.md) for timing and lifecycle, and [forms-and-validation.md](forms-and-validation.md) for submission and value semantics. Use [cards-and-lists.md](cards-and-lists.md) for collection identity and [navigation-and-routing.md](navigation-and-routing.md) for route/overlay behavior. Do not duplicate those contracts or install a new animation engine merely to orchestrate effects.

Before implementation, identify the trigger, authoritative state, stage dependencies, cancellation/reversal policy, and final usable state. A compact sequence table is useful when several stages can be interrupted:

| Stage | Starts when | Visible feedback | Exit and interruption |
| --- | --- | --- | --- |
| Input acknowledged | A valid activation occurs | Existing pressed/selection feedback | Release/cancel clears the press; activation follows the existing control semantics. |
| Operation pending, if applicable | Existing work starts | Truthful pending feedback | Resolve to actual success/failure; preserve supported cancellation and retry. |
| Outcome presented | The relevant state/result is known | Correct value, mark, or error | Keep usable immediately; superseded results cannot overwrite newer state. |
| Optional flourish | Its preceding presentation stage settles and remains relevant | Brief decoration or coordinated emphasis | Cancel, skip, or coalesce without changing the operation result. |

Sequence only genuine dependencies. A checkmark and its derived summary may animate together from one state update; a flourish may follow a mark. Do not serialize all elements or all network work. Presentation stages may use supported completion callbacks/promises, but business success must come from the operation. Handle cancellation/rejection of animation work and invalidate stale callbacks. No stage may depend solely on a guessed timeout or an event that disappears when motion is disabled.

Keep duplicate-operation protection separate from decorative playback. Protect an existing in-flight submission as required; do not lock every control for the whole animation, swallow valid toggles, or block Cancel/Back. End in the latest intended state after rapid reversal, unmount, route change, or zero-duration motion. Bound the total sequence and stagger budget; several individually short animations can still create a slow interaction.

## Diagnose Before Editing

- A click shows success before validation or the actual operation succeeds.
- Independent animations use unrelated state, so counts, progress, selection, and color disagree.
- Fixed delays leave the sequence stuck or allow an older completion to overwrite a new interaction.
- A visual lock blocks cancel/retry or drops valid repeated input.
- Staggered or clipped content remains invisibly focusable, or newly shown content cannot be reached.
- A wheel commits transient values while still moving or requires dragging to select precisely.
- Reversal, reduced motion, resize, or unmount leaves particles, masks, listeners, or pending state behind.

## Six Compound Patterns

### 1. Submission Button: Press → Confirmed Mark → Optional Particles

**Goal and rationale:** Acknowledge activation and communicate the actual outcome with an optional short finishing cue.

**Rules:** Keep immediate press feedback distinct from pending submission. Validate and submit according to the existing form contract; show a success mark only after confirmed success, or clearly represent the existing optimistic policy. A rejection must show recovery instead of celebration. If useful, complete a small shape transition before presenting the mark, then emit a bounded particle burst. Never delay a ready result or required navigation to finish the flourish. Preserve a stable hit target, meaningful accessible name/status, and focus when replacing text. Restrict duplicate submission while needed by the operation, release protection on all outcomes, and keep supported cancellation available. Particles are optional and should disappear for reduced motion or a quieter product style.

**Prompt:**

```text
Refine this submission button with immediate press feedback, truthful pending state, and a confirmed success mark. Optionally follow the mark with a brief bounded particle burst. Sequence presentation dependencies using the existing motion primitive; never infer success from a timer. Preserve validation, failure/retry, focus, accessible status, cancellation, and duplicate-submission safeguards without locking the interface for decoration.
```

### 2. Linked Selection: Checkmark, Progress, Count, and Surface

**Goal and rationale:** Make the real consequences of a selection visible as one coherent update.

**Rules:** Derive the checkbox, count, progress, and optional surface emphasis from the same selection model. Preserve selection scope, mixed state, filters, denominator, formatting, and existing save/reconciliation behavior. Do not add synthetic metrics or alter business calculations to support the effect. Animate presentation together while exposing correct logical values immediately; intermediate digits are not submitted data. Reverse toward the latest state on uncheck, changing totals, or failed persistence, rather than replaying stale frames. Keep labels and contrast readable throughout; color is supplementary and a gradient is unnecessary. Announce meaningful results once where useful, not on every frame.

**Prompt:**

```text
Coordinate the existing checkbox mark, derived count/progress, and optional background emphasis from one authoritative selection state. Keep logical values immediate and presentation synchronized. Preserve scope, mixed state, calculations, validation, and save/failure behavior. Rapid checking/unchecking must settle to the latest values; reduced motion shows those values directly without frame-by-frame announcements.
```

### 3. Staggered Stack Expansion

**Goal and rationale:** Clarify progression in an existing expandable card stack without delaying access to the collection.

**Rules:** Reuse the stack's current record identity, order, expansion controls, and focus policy. Choose stagger direction from the intended reading order and layering; bottom-to-top is an example, not a required reorder. Keep individual offsets small and cap total delay as the collection grows. Collapsing may reverse the visual sequence when useful; rapid reversal cancels or retargets unfinished work. Keep hidden layers noninteractive, visible records reachable, and nested actions independent. A static expanded list is an equivalent reduced-motion path. Do not introduce a stack or hide existing content during a visual-only task.

**Prompt:**

```text
Refine the existing expandable card stack with a small bounded stagger that follows its reading order and layer model. Preserve records, order, reachable controls, and focus. Support collapse and interrupted re-expansion without invisible targets or accumulated delays; use immediate expansion for reduced motion. Add no new hidden content or stack behavior outside scope.
```

### 4. Snap, Settle, Then Draw Timeline Connections

**Goal and rationale:** Distinguish tentative positioning from a settled connection in an existing draggable timeline or diagram.

**Rules:** Define valid targets, snapping tolerance, settled state, and commit behavior using the existing interaction model. Schedule connection growth only for the current valid settled placement; do not infer success from the end of a spring. Preserve existing committed connections during dragging unless the task explicitly replaces them. A tentative line can be suppressed or simplified if appropriate, but do not remove essential relationship feedback. Cancel the pending draw when dragging resumes, the target becomes invalid, or persistence fails. Provide keyboard and non-drag placement alternatives. Motion-disabled mode shows the correct final connection immediately. Adding movable timeline nodes is a behavior change requiring scope support.

**Prompt:**

```text
Refine the existing node-placement flow so connection growth begins only after a valid snap settles. Preserve committed relationships and essential drag feedback, cancel stale drawing when movement resumes, and handle invalid placement, cancellation, and failed saves. Keep non-drag/keyboard placement and an immediate reduced-motion final connection.
```

### 5. Wheel Selection: Inertia → Snap → Bounded Settle

**Goal and rationale:** Support precise selection with a predictable settling point when a wheel matches the platform and task.

**Rules:** Prefer the existing native/platform picker. If a custom wheel is justified, define bounds, step, item identity, disabled choices, and when a preview becomes a committed value. Inertia and edge resistance are optional; keep them bounded and stop/retarget on new input. Snap to a valid item without repeated oscillation or accidentally committing intermediate values. Preserve explicit Apply/Cancel if present and reconcile dependent values such as dates. Provide labeled keyboard and non-drag controls or an appropriate direct-entry/select alternative; do not assign spinbutton semantics to an arbitrary list. Keep one meaningful selected value for assistive technology. Reduced motion skips decorative momentum while preserving selection and page scrolling.

**Prompt:**

```text
Use the existing platform picker, or refine this scoped wheel with bounded inertia, stable valid-item snapping, and optional restrained edge resistance. Preserve preview-versus-commit timing, limits, disabled items, Apply/Cancel, and dependent values. New input must interrupt settling. Retain keyboard/non-drag selection, meaningful accessible values, and a reduced-motion path without trapping page scroll.
```

### 6. Trigger-Origin Circular Reveal

**Goal and rationale:** Provide optional spatial continuity between a trigger and an already scoped page or overlay transition.

**Rules:** Reuse the current transition system and route/modal lifecycle. For pointer activation, map the actual activation point into the animated surface's coordinate space. Keyboard activation can use the trigger center or a neutral transition; direct entry, missing triggers, and reduced motion need a usable fallback. Cover the intended surface at its current size, account for resize/scroll, and avoid clipping focus outlines or accessible content. Reverse toward the stored origin only when that origin remains meaningful; otherwise close normally. Keep masks decorative and noninteractive. Never delay navigation, trap focus, hold scroll locks, or make essential content depend on reveal completion. Use this large-area effect sparingly and measure its cost rather than assuming clipping is cheap.

**Prompt:**

```text
Optionally refine this existing transition with a restrained circular reveal from the activation point mapped to the target surface. Provide trigger-center or neutral fallback for keyboard/direct entry, resize, missing origin, and reduced motion. Preserve navigation, focus, dismissal, history, and cleanup; decorative masks must not intercept input, gate content, or delay the operation.
```

## Combined Prompt

Choose only relevant patterns; a sequence does not require every effect.

```text
Refine the scoped interaction using the existing components and motion system. Identify authoritative state, triggers, dependent stages, coordinated parallel updates, interruption/reversal, and the final usable state. Consider confirmed button feedback with optional particles, linked selection summaries, bounded stack staggering, settle-then-draw connections, a platform-appropriate wheel, or an optional trigger-origin reveal. Preserve real results, calculations, commit timing, permissions, content, order, navigation, and focus. Protect duplicate operations only where needed; coalesce decoration without dropping valid input or blocking Cancel/Back. Cancel stale callbacks and clean up on unmount or route changes. Bound total motion, preserve keyboard/non-drag access, and provide reduced/zero-motion final states. Do not fabricate success, install an animation engine, or expand behavior merely to use the catalog.
```

## Acceptance and Regression Checks

Use [verification.md](verification.md) and existing project checks. Mark applicable scenarios pass/fail/unverified or N/A with a reason. A screenshot cannot prove stage order, input handling, or interruption safety.

| Scenario | Expected evidence |
| --- | --- |
| Outcome truth | Slow, failed, canceled, and successful operations show accurate states; marks/particles never fabricate success. |
| Sequence and interruption | Relevant stages start in order; coordinated effects share state; rapid input/reversal leaves the latest intended result. |
| Lock lifetime | Duplicate writes are prevented as required; failure/abort releases protection; Cancel/Back and valid edits remain usable. |
| Linked values | Counts, denominators, mixed state, progress, and rollback remain correct with filters, changed totals, and failed saves. |
| Stacks and timelines | Order/focus stay meaningful; hidden layers are inactive; interrupted/invalid snaps cannot draw stale connections. |
| Wheel | Inertia stops on new input; endpoints, disabled items, snap, dependencies, precise selection, and Apply/Cancel follow the contract. |
| Reveal | Pointer, keyboard, direct entry, resize, missing trigger, Back, and interrupted close retain usable content and correct focus/history. |
| Reduced/zero motion | Logical outcomes and cleanup complete without animation events, momentum, particles, or a full-screen moving mask. |
| Performance and cleanup | Representative collections and target devices show bounded total delays and no retained particles, masks, timers, listeners, or stale callbacks. |

## Primary References

- [W3C: Animation from Interactions](https://www.w3.org/WAI/WCAG22/Understanding/animation-from-interactions.html): guidance on disabling nonessential interaction-triggered motion.
- [MDN: Animation.finished](https://developer.mozilla.org/en-US/docs/Web/API/Animation/finished): completion promises and handling canceled animation work when this API is used.
- [W3C APG: Spinbutton](https://www.w3.org/WAI/ARIA/apg/patterns/spinbutton/): keyboard and value semantics when the chosen picker is actually a spinbutton.

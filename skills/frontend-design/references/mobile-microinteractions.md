# Mobile Microinteractions

Use when refining touch feedback in mobile apps, embedded mobile surfaces, or mobile web. Make input and state changes perceptible while preserving content, layout, semantics, and operation outcomes. Select useful effects; a clear color, icon, or text change can already be sufficient feedback.

Contents: [Contract](#establish-the-feedback-contract) · [Diagnosis](#diagnose-before-editing) · [Twelve modules](#twelve-microinteraction-modules) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Feedback Contract

Identify the existing trigger, logical state, operation, feedback, cancellation, and recovery for each affected component. Separate pressed feedback, accepted input, pending work, and confirmed success. Preserve permissions, payloads, request frequency, and the existing optimistic-update/reconciliation policy. Do not equate an animation ending with a successful save.

Do not install an animation engine, add haptics, reorganize content, or introduce gestures merely to fill the catalog. Card flips, card stacks, and sheet snap points can change access and interaction, so introduce them only when requested or already within scope. A visual-refinement request alone does not authorize hiding content behind a new interaction. Prefer existing platform/component behavior and inspect the installed runtime before configuring effects.

Reuse [motion-and-easing.md](motion-and-easing.md) for curves, durations, reduced motion, interruption, and cleanup. Values such as 0.94–0.96 scale or 200–300ms are examples to evaluate, not mandatory settings; smaller controls may need less or no scaling. Reuse [forms-and-validation.md](forms-and-validation.md) for controls, [cards-and-lists.md](cards-and-lists.md) for containers, [loading-empty-and-error-states.md](loading-empty-and-error-states.md) for pending feedback, and [navigation-and-routing.md](navigation-and-routing.md) for drawers and overlays. Read the relevant sections rather than duplicating their contracts.

For feedback spanning dependent stages or several coordinated elements, use [compound-microinteractions.md](compound-microinteractions.md). Keep this document's input and state contracts while defining sequence, cancellation, and settling behavior.

Keep feedback immediate, optional motion restrained, and hit targets stable. Honor keyboard, touch, pen, assistive technology, and reduced-motion paths supported by the product. On web, use existing device-independent activation semantics; a press animation may begin on pointer down, but it must not move the business action from click/release to pointer down. Handle cancellation, scrolling, interrupted gestures, and lost pointer capture without stuck pressed state or unintended activation.

Limit or coalesce decorative playback separately from operations. Do not globally debounce clicks: it can swallow intended toggles, keyboard activation, or successive edits. Preserve existing duplicate-write safeguards without adding latency to every action. Animated layers must not intercept input, enter the accessibility tree as extra controls, or accumulate after completion.

## Diagnose Before Editing

- Users cannot distinguish pressed input, pending work, confirmed state, and failure.
- Feedback moves the target, blocks its label, or delays a normal operation.
- Pointer cancellation, scrolling, or fast toggling leaves a stuck animation/state.
- Decorative bounce, particles, shimmer, or 3D motion distracts from the task.
- Dragging is the only way to complete an otherwise simple action.
- Hidden card faces or stacked items remain focusable or contain unreachable controls.
- Rolling numbers or shimmer misrepresent data, request progress, or readiness.
- Reduced-motion or low-performance paths lose feedback or fail lifecycle cleanup.

Missing a ripple, spring, flip, or particle burst is not itself a defect. Judge recognition, task completion, and recovery before adding ornament.

## Twelve Microinteraction Modules

### 1. Button Press Feedback

**Goal and rationale:** Acknowledge contact without implying that the operation has completed.

**Rules:** Reuse the native/design-system pressed state. Add a small visual scale only when helpful, preserving the hit target and readable text; spring release is optional. Clear the effect on release, cancellation, lost capture, or removal. Do not implement the business action in both pointer and click handlers. Keep keyboard activation and disabled state correct; a scroll that started on a button must not accidentally activate it.

**Prompt:**

```text
Refine this button's pressed feedback using the existing component. Optionally add a very small scale/release transition without moving its hit target or delaying activation. Clear feedback on cancellation and preserve native click, keyboard, disabled, and duplicate-submission behavior.
```

### 2. Switch Thumb Feedback

**Goal and rationale:** Make the actual on/off change easy to perceive.

**Rules:** Keep checked semantics and label stable, with non-color state cues. Reuse the existing track/thumb transition; add restrained elasticity only where it fits the product. Keep the thumb inside its intended track and preserve rapid reversal. Distinguish a local checked change from a pending or rejected save according to the existing form contract; do not create optimistic persistence as decoration.

**Prompt:**

```text
Refine the switch thumb/track transition with the existing motion tokens and optional restrained elasticity. Keep state legible without color alone, support rapid reversal, and preserve keyboard access. Show pending or failed persistence honestly rather than treating motion as successful saving.
```

### 3. Ripple Feedback

**Goal and rationale:** Indicate a contact point when a ripple fits the platform and design system.

**Rules:** Prefer an existing ripple primitive. Pointer-triggered ripples may originate at the contact point; keyboard activation can use a suitable centered cue. Decorative layers must not intercept input or create duplicate activations. Clip only the ripple to the intended shape, not the focus outline, labels, or popup. Bound repeated effects and clean them up on cancellation/unmount. Do not impose Android-style ripples on every platform or combine them with redundant effects.

**Prompt:**

```text
Use a restrained ripple only if it matches the current platform and component system. Place pointer ripples at the contact point and provide equivalent keyboard feedback. Clip the decorative layer without clipping focus, preserve activation semantics, and prevent overlapping effects or resource accumulation.
```

### 4. Like and Favorite Feedback

**Goal and rationale:** Acknowledge a positive state change without overstating the result.

**Rules:** A short icon transition may suffice; a small particle burst is optional. Tie playback to the intended state transition, not every render, data refresh, or canceled press. Preserve the existing request, optimistic update, and failure policy. Repeated like/unlike input must reconcile to the intended final state; throttle visual bursts separately rather than dropping operations. Keep particles brief, bounded, noninteractive, and removable for reduced motion. Do not celebrate a failed operation as success.

**Prompt:**

```text
Refine existing like/favorite feedback with a brief icon response and optional small particle burst. Preserve the operation and reconciliation policy, coalesce only decorative playback, and support rapid toggling and failure. Do not replay on rendering or imply confirmed success from animation alone.
```

### 5. Checkbox Mark Drawing

**Goal and rationale:** Make a selection change perceptible while keeping its meaning immediate.

**Rules:** Retain native or existing checkbox semantics, label activation, focus, and mixed state. Animate only a decorative check path if desired; set logical state immediately. Do not hide the actual control from keyboard or assistive technology. Match stroke style to existing icons; a prescribed stroke direction is not a functional requirement. Fast checking/unchecking and reduced motion must reach the correct mark without stale drawing callbacks.

**Prompt:**

```text
Optionally animate the checkbox's decorative check stroke while preserving immediate checked/mixed semantics, label activation, and keyboard focus. Reuse the current icon style, support rapid reversal, and show the correct mark immediately when reduced motion is enabled.
```

### 6. Slider Follow Feedback

**Goal and rationale:** Make the selected position track input accurately.

**Rules:** Keep the thumb and displayed value responsive to the actual pointer or keyboard input; smoothing must not introduce a lagging or false value. Liquid fill deformation is optional and must not distort the track, labels, or hit target. Preserve bounds, step, units, precise-entry/non-drag alternatives, and existing input-versus-commit behavior. Do not create a new network request per animation frame. Handle cancellation and settle cleanly with reduced motion or no liquid effect.

**Prompt:**

```text
Keep slider position and values directly aligned with input. Add subtle liquid/fill styling only if useful, without lag, distorted targets, or changed bounds and step. Preserve keyboard and non-drag entry plus existing request/commit timing, and settle correctly after cancellation.
```

### 7. Rolling Numbers

**Goal and rationale:** Help users notice a value change where animated digits improve understanding.

**Rules:** Keep the authoritative value, unit, sign, precision, and locale formatting intact. Animated intermediate digits are presentation, never transaction values or fabricated metrics. Maintain stable width/alignment and one meaningful accessible value, without announcing every frame. Coalesce frequent updates to the latest value; allow immediate display for sensitive financial, rapidly updating, or reduced-motion contexts. Do not delay copying or accessing the actual value until rolling ends.

**Prompt:**

```text
Use a brief rolling-number transition only where it helps recognize change. Preserve the actual formatted value and stable alignment, expose one meaningful accessible value, and avoid frame-by-frame announcements. Coalesce frequent visual updates and provide immediate values for reduced motion or accuracy-sensitive use.
```

### 8. Skeleton Feedback

**Goal and rationale:** Explain an initial wait without a blank or misleading interface.

**Rules:** Reuse the state and layout rules in [loading-empty-and-error-states.md](loading-empty-and-error-states.md). Match the expected region's geometry and reserve space. Static placeholders are valid; shimmer/gradient is optional functional feedback, not a reason to add decorative effects. Keep it subdued and remove unnecessary movement for reduced motion. Hide placeholder detail from assistive technology while exposing meaningful pending status. Render ready content promptly; never add a minimum wait or clear useful content during refresh just to show skeletons. Failure and successful empty states must replace pending appropriately.

**Prompt:**

```text
Reuse the existing loading-state contract to show a scoped skeleton only while needed. Match layout geometry, reserve space, and make shimmer optional and reduced-motion aware. Keep pending status accessible without exposing placeholder noise, show ready content immediately, and preserve usable content during refresh.
```

### 9. Two-Sided Card Transitions

**Goal and rationale:** Support an existing front/back content relationship where it serves the task.

**Rules:** Introducing a second face or hiding details behind a flip is a behavior change and needs scope support. Provide an explicit labeled toggle and a predictable return path; do not make nested links trigger the flip. Only the visible face may be interactive or exposed as the current content, and focus must not be stranded in the hidden face. Keep all details available without 3D motion in reduced-motion mode. Preserve layout and content dimensions without clipping, mirrored text, or surprise automatic flipping. Do not impose one-open-card exclusivity unless the product requires it.

**Prompt:**

```text
Refine an existing two-sided card with an optional restrained flip and a clear labeled toggle. Keep only the active face focusable/exposed, protect nested actions, and preserve all details and a return path. Use a non-3D reduced-motion transition; do not introduce hidden content or exclusivity outside scope.
```

### 10. Menu Icon Morphing

**Goal and rationale:** Visually connect the trigger with the drawer's open/closed state.

**Rules:** Use a single accessible control with a current label and expanded state; decorative strokes should not become separate controls. Morphing lines is optional. Derive the icon from actual drawer state, including Escape, backdrop, route selection, and other dismissal paths. Preserve focus indication, hit area, stroke consistency, and rapid reversal. Respect the existing placement and close control; a persistent sidebar does not automatically need a hamburger-to-close transformation.

**Prompt:**

```text
Optionally morph the existing menu trigger icon between open/close visuals while keeping one accessible control and stable hit target. Synchronize it with actual drawer state across all dismissal paths, preserve focus and rapid reversal, and remove unnecessary motion when requested.
```

### 11. Stacked Card Feedback

**Goal and rationale:** Make depth and progression understandable in an already appropriate card-stack flow.

**Rules:** Introducing a stack, swipe-to-dismiss, reordering, or hiding list items is a behavior/layout change, not automatic polish. Keep record identity, order, and available content intact. Use clear next/previous or equivalent non-drag controls, and preserve keyboard access. Define cancellation, snap-back, and any existing commit threshold without adding destructive swipe effects. Prevent a drag from also firing a card click. Keep z-order and interactive faces synchronized; lower items must not create invisible focus targets or overlap unrelated controls. Limit rendered layers based on measured need while keeping the full collection reachable.

**Prompt:**

```text
Refine the existing card stack only if that interaction is in scope. Preserve order and record identity, provide non-drag and keyboard controls, and handle cancellation/snap-back without accidental clicks or destructive actions. Keep visible layers and focus targets synchronized and the entire collection reachable.
```

### 12. Bottom-Sheet Gesture Feedback

**Goal and rationale:** Make an existing draggable sheet track touch predictably and settle in an intended state.

**Rules:** Reuse a supported sheet primitive and its modal/nonmodal contract. Snap points, velocity/position thresholds, dismissal, and resistance must be defined by the actual task; half/full anchors are not mandatory. Keep an explicit close action and non-drag expand/collapse alternatives where relevant. Handle nested scrolling, the virtual keyboard, safe areas, interrupted gestures, and pointer cancellation without disabling page gestures globally. Coordinate focus, background interaction, scroll locks, and cleanup with the overlay lifecycle. Preserve unsaved-work protections; a downward gesture must not silently discard a draft. Avoid introducing new gestures or snap states during a visual-only pass.

**Prompt:**

```text
Refine an existing bottom sheet using its supported gesture and overlay primitive. Keep drag tracking direct, snap points task-appropriate, and settling restrained. Preserve non-drag controls, focus, nested scrolling, keyboard/safe-area behavior, cancellation, and unsaved work. Add no new snap states or dismissal gestures outside scope.
```

## Combined Prompt

Apply only modules that solve an actual feedback problem; do not layer all twelve effects onto one page.

```text
Improve mobile input feedback using the existing component system, platform conventions, and motion tokens. Preserve content, layout, business operations, permissions, and state semantics. Consider restrained press, switch, ripple, like, checkmark, slider, number, skeleton, and menu-icon feedback only where useful. Card flips, stacks, and bottom-sheet snap gestures require an existing or explicitly scoped interaction. Keep pressed, pending, confirmed, and failed outcomes distinct. Coalesce decorative playback without swallowing valid input, handle scrolling and pointer cancellation, and keep keyboard and non-drag paths usable. Honor reduced motion and target-device performance without losing final state, focus, or cleanup. Do not add an animation engine, fake progress, or extra network calls; verify actual input, rapid reversal, and failure paths.
```

## Acceptance and Regression Checks

Use the existing project tests and bounded rendering process in [verification.md](verification.md). Mark relevant checks pass/fail/unverified or explain N/A. A static screenshot cannot prove touch responsiveness, cancellation, or gesture arbitration.

| Scenario | Expected evidence |
| --- | --- |
| Press and cancellation | Press/release, drag away, scroll start, pointer cancellation, and lost capture leave correct feedback and no unintended action. |
| Repeated activation | Rapid taps, keyboard activation, toggle reversals, and re-rendering do not duplicate requests, drop valid input, or accumulate effects. |
| Operation outcomes | Pending, success, rejection, and rollback follow the existing contract; particles, rolling digits, or completed motion do not fabricate success. |
| Selection and values | Checkbox/mixed, switch, slider, and numeric values remain accurate with correct accessible semantics and stable targets. |
| Loading | Skeletons preserve geometry, settle to content/empty/error correctly, and never delay ready content or erase refresh context. |
| Faces and layers | Card faces/stacks retain meaningful reading/focus order, reachable records, independent actions, and no hidden interactive controls. |
| Sheets and menus | All dismissal paths synchronize icon/state; snap/scroll/keyboard/safe-area behavior preserves focus, background rules, and drafts. |
| Access and motion | Keyboard, touch, non-drag alternatives, and reduced/zero-motion paths preserve equivalent outcomes and meaningful feedback. |
| Performance and cleanup | Representative target devices show no observed blocking, runaway loops, retained particles/listeners, or new console errors; untested devices are declared. |

## Primary References

- [W3C: Pointer Cancellation](https://www.w3.org/WAI/WCAG22/Understanding/pointer-cancellation.html): avoid accidental activation and support cancelable pointer interaction.
- [W3C: Dragging Movements](https://www.w3.org/WAI/WCAG22/Understanding/dragging-movements.html): provide a non-drag single-pointer alternative where dragging is not essential; keyboard access remains a separate concern.
- [MDN: Pointer Events](https://developer.mozilla.org/en-US/docs/Web/API/Pointer_events): pointer lifecycle, cancellation, capture, and touch-interaction handling; verify runtime/platform support when implementing.

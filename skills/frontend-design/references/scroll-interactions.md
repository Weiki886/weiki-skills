# Scroll Interactions

Use when refining scroll-linked orientation or presentation in mobile apps, mobile web, profiles, content pages, feeds, and campaign pages. Choose effects that improve the task; a static interface with usable native scrolling is valid.

Contents: [Contract](#establish-the-scroll-contract) · [Diagnosis](#diagnose-before-editing) · [Six modules](#six-scroll-modules) · [Implementation](#implementation-and-performance) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Scroll Contract

Preserve business content, document order, layout flow, destinations, data, and request semantics. Do not replace native scrolling, add scroll snapping, introduce refresh gestures, or move content behind a new interaction just to demonstrate an effect. Reuse the existing scroll container, platform primitives, components, and animation tools.

For each affected region, identify the actual scroller, axis, start/end offsets, affected properties, reduced-motion/static fallback, resize/content-change behavior, and cleanup owner. State which model applies:

| Model | Behavior | Typical use |
| --- | --- | --- |
| Continuous scroll-linked | Presentation maps to current scroll position; reversing scroll reverses the mapping. | Parallax, collapsing cover, optional narrative chart treatment. |
| Threshold/state-based | A condition changes at a boundary; semantics do not require a continuous animation. | Sticky styling, current-section indication. |
| Viewport-triggered | Visibility starts a short time-based effect, often once per content identity. | Optional reveal or count-up. |
| Gesture plus async state | Pull distance drives feedback until release; a real request then owns pending/success/failure. | Existing pull-to-refresh. |

Do not force all six modules to be reversible, continuously scrubbed, or eased identically. Reuse [motion-and-easing.md](motion-and-easing.md) for restrained motion and reduced-motion behavior; its elapsed-time examples are not fixed durations for scroll progress. Use [loading-empty-and-error-states.md](loading-empty-and-error-states.md) for refresh/requests, [navigation-and-routing.md](navigation-and-routing.md) for navigation, and [mobile-microinteractions.md](mobile-microinteractions.md) for gesture cancellation and non-drag controls.

Keep the complete content and actual values available without decorative motion. Unsupported APIs, script failure, reduced motion, direct entry, and keyboard navigation must not leave essential content transparent, offscreen, or covered. Do not steal focus, rewrite browser history, or block touch/wheel/keyboard scrolling for decoration.

## Diagnose Before Editing

- A header or sticky control obscures content, anchor targets, or keyboard focus.
- Animated geometry creates gaps, layout shifts, threshold loops, or clipped text.
- Scroll-linked values lag behind input because a new time animation starts on every event.
- Enter/exit thresholds flicker or target the wrong nested scroll container.
- Reveal effects hide content after direct entry, restored scroll, script failure, or reduced motion.
- A refresh animation conflicts with native gestures or misrepresents request completion.
- A chart or number appears to change its underlying data when the user scrolls.
- Listeners, observers, or animation work accumulate after navigation or content changes.

Do not report missing parallax, reveals, or narrative refresh as defects by themselves. Explain the actual orientation, access, or performance problem first.

## Six Scroll Modules

### 1. Restrained Parallax Header

**Goal and rationale:** Add limited depth to suitable visual content without interfering with reading.

**Rules:** Apply only where the content and product support it. Map the actual scroll offset into a bounded decorative transform and clamp at the intended endpoints. Preserve the header's layout space and asset dimensions; prevent exposed empty edges or harmful image cropping. Keep text, hit targets, and important image subjects stable and readable. Avoid broad parallax across dense lists. Provide an equivalent static header for reduced motion or unsupported runtimes; do not add a delayed spring that makes the background chase the user's scroll.

**Prompt:**

```text
Add restrained parallax only to the scoped decorative header. Map the correct scroller's position to a bounded transform, clamp at the endpoints, and preserve layout space and meaningful image content. Keep reading and controls stable, avoid lagging time-based restarts, and provide a static fallback with reduced motion.
```

### 2. Sticky Category or Section Navigation

**Goal and rationale:** Keep useful navigation available while users browse a long page.

**Rules:** Prefer existing sticky layout behavior with an offset accounting for the actual header and safe area. Inspect ancestor overflow/containment before adding JavaScript pinning. Preserve flow dimensions so entering/exiting sticky state does not jump. Distinguish filters, true tabs, and in-page section links; styling does not make them the same interaction. Preserve their existing selection/query behavior. If section tracking is needed, define a deterministic active-section rule for tall, short, overlapping, and end-of-page sections. Keep horizontal navigation keyboard/touch accessible. Offset anchor targets and focus to avoid hiding them beneath the sticky region. Scrolling alone must not push history entries or unexpectedly move focus.

**Prompt:**

```text
Keep existing category/section navigation available with a stable sticky layout below the real header. Preserve filter, tab, or section-link semantics. Where applicable, track the visible section deterministically, keep labels reachable, and account for anchor/focus offsets without layout jumps or scroll-generated history entries.
```

### 3. Collapsing Cover and Navigation Title

**Goal and rationale:** Retain page identity as a large introductory header leaves view.

**Rules:** Define continuous, clamped endpoints for the cover and compact navigation treatment; support reverse scrolling and restored nonzero positions. Keep controls and focus stable instead of physically moving the active control between containers. If using duplicate visual titles, preserve one meaningful heading structure and avoid duplicate interactive/accessibility content. Prefer restrained transform/opacity treatment with sufficient contrast throughout. Recalculate geometry when header size, fonts, images, orientation, or content change, without a measurement/write feedback loop. Changing background opacity does not require a gradient decoration. In reduced motion, keep a readable static or discrete compact-header alternative.

**Prompt:**

```text
Refine the scoped cover-to-navigation transition with bounded scroll mapping and reversible endpoints. Preserve page identity, readable contrast, one meaningful heading structure, and stable controls/focus. Handle restored scroll and changing geometry; use a usable static/discrete reduced-motion fallback rather than hiding the title.
```

### 4. Optional Viewport Reveal

**Goal and rationale:** Add a subtle introduction to selected content without controlling access to it.

**Rules:** Treat this as viewport-triggered time-based motion unless continuous scrubbing is explicitly intended. Use a suitable visibility observer with the correct root, margins, and threshold; a universal 15% threshold can fail for tall regions. Revealing once should track stable content identity rather than DOM reuse in a virtual list. Default content to visible; enable initial hiding only when enhancement is ready and immediate focus/direct-entry paths remain safe. Do not delay above-the-fold critical content or confuse reveal with fetching/lazy loading. Keep displacement small and remove it for reduced motion. Re-entry does not need to replay a one-shot reveal.

**Prompt:**

```text
Use a subtle one-shot reveal only for appropriate noncritical content, triggered by visibility in the actual scroller. Choose thresholds for the content dimensions, preserve stable item identity, and keep content visible if enhancement is unavailable. Do not gate focus, direct entry, or ready content behind the animation, and do not treat revealing as data loading.
```

### 5. Pull-Linked Refresh Feedback

**Goal and rationale:** Explain pull, release, and real refresh states when that gesture already belongs to the product.

**Rules:** Reuse existing pull-to-refresh and request primitives. Before release, map eligible pull distance to bounded feedback; only an intentional threshold/release starts the existing refresh operation. Once refreshing, use real async state rather than requiring continued pulling or pretending progress is still scroll-driven. Reuse cancellation, completion, failure, content retention, and retry behavior from the loading reference. Do not intercept ordinary scrolling, browser refresh, system Back, nested lists, or sheet gestures globally. Keep explicit refresh/non-drag access where needed. Narrative artwork is optional, must be available/authorized, and must not add a fake wait before content returns.

**Prompt:**

```text
Refine an existing pull-to-refresh interaction with bounded pull-distance feedback and clear release readiness. Start one refresh through the existing contract, then let actual pending/success/failure drive feedback and settling. Preserve content, cancellation, gesture coexistence, and non-drag refresh access; do not add narrative assets or artificial waits without need.
```

### 6. Scroll-Linked Data Presentation

**Goal and rationale:** Support an explicitly useful narrative treatment of real data without changing its meaning.

**Rules:** Decide between a one-time viewport-triggered count-up and continuous scroll-linked presentation; they are different models. For scrubbing, define the bounded range and whether reversal resets only the decoration. Keep the true value, units, comparison basis, and accessible representation available independently. Intermediate visual values are not transaction data, exports, or proof of progress. Do not make users finish scrolling to discover a balance or operational metric. Avoid tying real task-progress indicators to scroll. Reuse number semantics from [mobile-microinteractions.md](mobile-microinteractions.md), suppress frame-by-frame announcements, and show accurate static data for reduced motion. Use a deliberate mapping curve only when appropriate; do not force ease-out on direct tracking or let smoothing obscure endpoints.

**Prompt:**

```text
Use animated data presentation only where the narrative benefits. Choose either viewport-triggered playback or a defined continuous scroll mapping, preserving true values, units, and one meaningful accessible representation at all times. Keep intermediate values decorative, never tie task completion to scroll, and provide immediate static data with reduced motion.
```

## Implementation and Performance

- Prefer native/CSS layout for sticky behavior. Where supported by target runtimes, consider CSS scroll/view timelines or existing platform animation primitives for continuous mapping, with feature detection and a static fallback. Do not add a polyfill or animation library automatically.
- Use Intersection Observer for visibility/threshold decisions, not as a guarantee of pixel-exact continuous scroll updates. Choose the correct root and disconnect observers when no longer needed. Avoid huge threshold arrays to imitate a continuous timeline.
- For a JavaScript fallback, keep scroll handlers bounded, cache geometry when valid, separate layout reads/writes, and avoid per-frame full-page state updates. Re-measure when content changes rather than assuming initial dimensions never change.
- Animation-frame scheduling can organize rendering work; it does not by itself reduce scroll event frequency. Throttle nonvisual work when justified. Debounce can suit settled work, but trailing debounce is inappropriate for direct visual tracking that must follow the finger.
- For discrete boundary flicker, use stable layout, deterministic section selection, or separate enter/exit thresholds where needed. Do not add arbitrary debounce to continuous motion or hide a layout feedback loop with timing delays.
- Preserve native input behavior. Use passive listeners where supported when not canceling input; do not globally call `preventDefault` or disable touch actions to obtain decorative control.
- Suspend unnecessary offscreen work, share measurement/update paths where useful, and remove listeners, observers, timers, and pending frames on unmount/navigation. Profile representative long lists and target devices before claiming smoother scrolling.
- Keep focus, anchor targets, text selection, browser Find, zoom, and supported writing directions usable. A decorative transform must not become the sole way to reach or understand content.

## Combined Prompt

Apply only relevant effects rather than treating a static page as incomplete.

```text
Improve scoped scroll interaction using the existing scroller, platform primitives, and design system. Choose explicitly between continuous position mapping, threshold changes, one-shot viewport reveals, and pull-plus-request feedback. Where useful, add restrained parallax, stable sticky navigation, reversible cover collapse, safe reveals, existing refresh feedback, or truthful narrative data treatment. Preserve native scrolling, content order, actual values, focus, anchors, history, and request semantics. Clamp endpoints, handle restored scroll and changing geometry, prevent boundary flicker without lagging the finger, and keep content available with reduced motion or unsupported APIs. Bound rendering work and clean up observers/listeners. Do not add scroll hijacking, fake progress, new gestures, dependencies, or delayed content; verify actual bidirectional scrolling and representative target-device performance.
```

## Acceptance and Regression Checks

Use project tests and the bounded rendering process in [verification.md](verification.md). Mark applicable cases pass/fail/unverified and explain N/A; screenshots alone do not prove tracking or performance.

| Scenario | Expected evidence |
| --- | --- |
| Driver and endpoints | Correct scroller/root, axis, mapping model, bounds, initial position, and intended reverse/one-shot behavior. |
| Entry and restoration | Direct anchors, browser restored scroll, reload, route return, and nonzero initial position show readable content and correct navigation state. |
| Geometry changes | Images/fonts loading, long or translated content, appended items, resize, orientation, zoom, and safe areas do not cause jumps or loops. |
| Boundaries and speed | Slow threshold crossing, rapid reversal, momentum, large jumps, and end-of-page sections produce stable, accurate states. |
| Access and fallbacks | Keyboard/touch/assistive paths, browser Find, anchor focus, reduced motion, and unsupported enhancement keep all content and true values available. |
| Refresh | Below-threshold release cancels; eligible release starts one operation; pending/success/failure settle correctly and preserve context without gesture conflicts. |
| Data and history | Scrolling does not change underlying data, duplicate requests, fabricate progress, push navigation history, or steal focus. |
| Scale and cleanup | Representative lists and devices show no observed new jank, request floods, accumulating callbacks, or console errors after repeated navigation. |

Record components, actual scrollers, viewports, input sequences, fallback modes, and performance observations. Declare untested browsers/devices and avoid promising universal smoothness from API choice alone.

## Primary References

- [MDN: Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API): visibility/intersection thresholds and their limits.
- [MDN: animation-timeline](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation-timeline): scroll/view timelines; verify target support before use.
- [MDN: Document scroll event](https://developer.mozilla.org/en-US/docs/Web/API/Document/scroll_event): handler cost and the distinction between frame scheduling and actual throttling.

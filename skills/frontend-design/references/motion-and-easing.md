# Motion and Easing

Use when refining dialog, popover, menu, drawer, disclosure, switch, toast, or other UI transitions. Specify the interaction purpose and transition contract instead of asking only for smoother motion.

Contents: [Contract](#establish-the-motion-contract) · [Diagnosis](#diagnose-before-editing) · [Five choices](#five-motion-choices) · [Implementation](#implementation-and-lifecycle) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Motion Contract

Preserve component semantics, state, content, actions, and navigation. Improve feedback without adding decorative motion everywhere, delaying available content, or replacing the animation library. Reuse the design system's motion tokens and the installed framework's component lifecycle. An existing usable no-animation interaction is valid.

For each affected transition, record its trigger, entry/exit or state pair, animated properties, endpoints, duration or spring configuration, curve, interruption behavior, and reduced-motion alternative. Curve, duration, distance, delay, and lifecycle are separate decisions. Equivalent components should share role-based tokens; entry and exit may differ when useful, but do not force different curves if an established symmetric transition works well.

For small overlays without existing guidance, 200–350ms can be an initial evaluation range, with examples such as 280ms entry or 240ms toggle. It is not a required duration for every component, spinner, or physics-based spring. Choose by distance, size, input method, task, and observed response. Demonstration timings such as 900ms are not production defaults; do not claim an exact duration universally feels slow. Do not add a delay merely to make the easing visible.

Follow [navigation-and-routing.md](navigation-and-routing.md) for modal/nonmodal semantics, focus, history, and dismissal. Use [loading-empty-and-error-states.md](loading-empty-and-error-states.md) for truthful progress and status. Animation must support those contracts, not redefine them.

## Diagnose Before Editing

- The transition has no defined purpose, curve, endpoint, or consistency with peer components.
- Large translation, scaling, repeated bounce, or long delays compete with content.
- Entry/exit feels abrupt or sluggish, even though duration is consistent.
- Rapid toggling restarts from the wrong value, accumulates handlers, or leaves an invisible overlay.
- Focus, background interaction, scroll locks, or pointer handling disagree with visual state.
- Progress animation invents completion or continues after a request has settled.
- Reduced-motion mode still scales/slides large surfaces or never completes cleanup.

Compare actual open, close, interruption, and reduced-motion behavior. A still image cannot establish timing, easing, frame consistency, or cleanup correctness.

## Five Motion Choices

Treat these as task-based defaults rather than an exhaustive list or global bans. Project/platform motion guidance takes precedence. CSS keyword spelling and a library's easing identifiers may differ; verify the installed API instead of passing CSS names blindly.

| Choice | Typical use | Starting decision |
| --- | --- | --- |
| `ease-out` | A small surface arriving and settling | Consider for dialog/drawer entry without overshoot. |
| `ease-in` | A surface departing | Consider for a brief exit without a long slow start. |
| `ease-in-out` | A visible element changing between peer states | Consider for toggles and bounded state changes. |
| `linear` | Uniform decorative rotation or intentional constant-rate interpolation | Keep independent from measured task progress. |
| Spring | Optional physical feedback or continuity | Use only when the existing engine and product purpose justify it. |

### 1. Ease-Out: Entry and Settling

**Goal and rationale:** Bring a surface into view promptly and settle gently near its endpoint.

**Rules:** Consider a short decelerating entry for dialogs, drawers, and menus when compatible with the existing system. Use restrained opacity and small translation or scale only when helpful; a large drawer need not scale. Align the movement origin with the trigger or surface where meaningful. Avoid overshoot for serious confirmation flows. `ease-out` is a CSS keyword, not a universal entry requirement or a prohibition on using it for a suitable exit.

**Prompt:**

```text
Refine this overlay entry using the existing deceleration token, or evaluate a short ease-out transition if none exists. Use only useful opacity and small positional/scale changes, with no bounce or delayed content. Treat 280ms as a starting example and verify the actual component, focus, and reduced-motion behavior.
```

### 2. Ease-In: Brief Departure

**Goal and rationale:** Give dismissal a clear, prompt ending.

**Rules:** Consider acceleration for departure where it matches the platform. Avoid a long slow beginning that makes Close feel unresponsive. Exit can be shorter than entry or immediate when the task benefits; do not force equal timing. Keep dismissal semantics, focus return, and background activation synchronized with the component lifecycle. Do not keep the page blocked until an arbitrary visual timer expires.

**Prompt:**

```text
Refine closing with the existing exit token, considering a brief ease-in where appropriate. Keep the response to Close immediate and the movement small. Preserve focus return, scroll/background cleanup, and interruption handling; do not force exit to match entry duration or delay dismissal for a decorative effect.
```

### 3. Ease-In-Out: Peer State Changes

**Goal and rationale:** Give a contained two-way state change a balanced rhythm.

**Rules:** Consider symmetric easing for switches, selection indicators, and appropriate disclosures. Preserve immediate logical state and accessible feedback; moving a switch must not imply that an asynchronous save already succeeded. A tab/content change need not animate all content. Keep disclosure text complete and layout stable; use the existing supported height/layout transition rather than guessing an arbitrary maximum height. Symmetric easing can also be valid for a dialog if it fits the established motion language.

**Prompt:**

```text
Use the existing symmetric token or evaluate ease-in-out for this bounded state change. Treat 240ms as a starting example, preserve complete content and immediate state feedback, and keep any asynchronous save distinct from visual completion. Avoid animating unrelated content or introducing height clipping.
```

### 4. Linear: Uniform Interpolation

**Goal and rationale:** Maintain an intentional constant rate where changing speed would miscommunicate the motion.

**Rules:** Distinguish decorative indeterminate rotation from determinate progress. A progress indicator must reflect observed work; `linear` controls interpolation, not the real task's rate, and must not advance a fake percentage. Stop or settle indicators when their operation ends. Linear opacity or another deliberate transition is not inherently incorrect, but it is usually not the first choice for spatial overlay arrival. Honor reduced motion while preserving a meaningful pending state.

**Prompt:**

```text
Use linear easing only where constant-rate interpolation serves this feedback, such as an existing indeterminate indicator. Keep determinate values tied to observed work, never a fabricated timer. Stop on completion/failure and preserve a non-moving reduced-motion indication without leaving pending state stuck.
```

### 5. Spring: Optional Physical Response

**Goal and rationale:** Provide controlled physical continuity or a small expressive response when it helps the interaction.

**Rules:** A spring is a model, not one fixed CSS easing keyword, and may be configured without visible bounce. Use the installed engine only if already justified; do not add a dependency for a minor transition. Check its supported duration/bounce or stiffness/damping/mass controls and their precedence. Do not assume one library's parameters transfer to another. Keep settling bounded and overshoot restrained; avoid repeated oscillation or attention-seeking bounce for confirmation, errors, or reading surfaces. A toast does not require a spring merely because it is lightweight. Remove unnecessary displacement/bounce for reduced motion.

**Prompt:**

```text
Use an optional spring only if it fits this product and the existing animation engine. Configure a restrained, promptly settling response using the installed API, with little or no overshoot. Avoid repeated bounce, do not add a library, and provide a reduced-motion alternative. Do not force a spring on every toast or important dialog.
```

## Implementation and Lifecycle

For multi-stage or coordinated effects, use [compound-microinteractions.md](compound-microinteractions.md) to define dependencies and interruption; retain the lifecycle rules here.

Keep changes scoped to the affected components and shared motion tokens where needed:

- Prefer existing CSS transitions for simple fixed-endpoint effects. Animate only intended properties; avoid `transition: all`. Reuse a motion engine for behavior it already owns rather than layering competing animations over it.
- Check milliseconds versus seconds and per-property settings. A backdrop and panel may have distinct transitions; they must still represent one coherent overlay state.
- Prefer small transform/opacity changes where suitable, but do not assume they guarantee smooth performance. Keep transform ownership clear so animation does not overwrite positioning, drag state, or existing transforms.
- Preserve opening, open, closing, and closed lifecycle behavior from the component primitive. While closing, avoid invisible tabbable controls, orphaned focus traps, lingering backdrops, or early background interaction. Do not globally disable pointer events to cover a lifecycle bug.
- Support rapid open/close/open and repeated dismissal from the current visual state where feasible. Cancel superseded work; stale completion callbacks must not remove a reopened overlay or restore focus twice.
- Cleanup must work with no animated property change, cancellation, unmounting, route changes, and reduced/zero-duration motion. Do not depend solely on one `transitionend` event or an assumed timeout.
- Detect `prefers-reduced-motion` on web or the platform equivalent. Remove unnecessary movement/scaling and loops; use immediate transitions or restrained fades where suitable. Keep final content, status, focus, and cleanup correct without the animation.
- Never tie data submission, validation, navigation, or success claims to decorative animation completion. Existing semantics and verified operation outcomes remain authoritative.

Inspect the installed runtime's documentation when implementing. For example, Motion distinguishes time-based transitions from spring configurations, and some spring parameters override others; do not combine settings without checking their meaning.

## Combined Prompt

Apply only the transitions that improve the requested interaction.

```text
Refine motion using the current design system, component lifecycle, and installed animation tools. Define entry/exit or state pairs with explicit properties, endpoints, curves, and timing. Consider ease-out for settling entry, brief ease-in for departure, ease-in-out for peer state changes, linear for intentional uniform interpolation, and restrained springs only when justified. Treat 200–350ms as an evaluation range for small overlays, not a universal rule. Keep movement small, content available, and operation status truthful. Preserve focus, dismissal, background interaction, scroll locking, and interrupted open/close behavior. Support reduced or zero motion without stuck overlays or stale callbacks. Add no animation library or decorative effects merely for smoothness; verify real transitions rather than inferring them from a screenshot.
```

## Acceptance and Regression Checks

Use the bounded rendering process in [verification.md](verification.md), existing project tests, and controlled interaction sequences. Mark relevant scenarios pass/fail/unverified and explain N/A.

| Scenario | Expected evidence |
| --- | --- |
| Entry and exit | Correct final endpoints, coherent tokens, restrained travel, and no flash, clipping, or delayed available content. |
| Rapid interruption | Open/close/open, repeated Escape, and repeated trigger activation leave one correct state without stale callbacks or duplicate focus return. |
| Focus and layers | Tab order, modal background behavior, pointer interaction, scroll locks, and dismissal remain correct during and after transition. |
| Reduced and zero motion | No unnecessary scale/translation/oscillation; content and cleanup work without waiting for animation events. |
| Async feedback | Loading and progress remain truthful; visual completion does not imply a successful submission or save. |
| Dynamic content | Long text, validation errors, resized viewports, nested overlays, and route changes do not clip content or strand a backdrop. |
| Runtime performance | No new console errors, runaway loops, accumulated handlers, or observed jank on representative target hardware. |

Record the actual components, timing/configuration changes, input sequences, reduced-motion setting, and devices/browsers exercised. A screenshot, static CSS inspection, or build alone cannot prove transition timing or lifecycle correctness.

## Primary References

- [MDN: Easing Functions](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/easing-function): CSS curve semantics and supported timing-function forms.
- [MDN: prefers-reduced-motion](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/prefers-reduced-motion): detecting a user's reduced-motion preference.
- [Motion: React Transitions](https://motion.dev/docs/react-transitions): time-based and spring configuration; consult the installed version before applying library-specific parameters.

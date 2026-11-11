# Rendering Verification

## 1. Evidence Levels

From weaker to stronger:

1. Source and CSS inspection.
2. Automated lint/typecheck/test/build.
3. Automated accessibility and performance checks.
4. Real-browser rendering and interaction.
5. Target-device, assistive-technology, and real-content validation.

Do not infer something visible only at a later level from an earlier one. Without real rendering, do not claim that visual layout was verified.

## 2. Scale the Matrix to the Change

| Change | Minimum verification |
| --- | --- |
| Small component visual adjustment | One representative viewport + affected interaction/state + relevant project checks. |
| New component | Applicable normal/empty/error/disabled/long-content states + keyboard path. |
| New page or flow | Desktop and narrow screen + first viewport, main task, and key states. |
| Structural redesign | Desktop and narrow screen + key old-state regression + existing theme/permission scope. |
| Navigation, overlay, or routing changes | Applicable history, dismissal/focus, direct-entry, refresh, and permission scenarios in [navigation-and-routing.md](navigation-and-routing.md). |
| Overlay component selection | Applicable modality, timeout/recovery, top-layer, focus, keyboard/touch, capacity, and status-announcement scenarios in [overlay-selection.md](overlay-selection.md). |
| Sidebar organization or modes | Applicable destination inventory, route/ancestor matching, disclosure, compact/drawer access, long-label, permission, and short-viewport scenarios in [sidebar-navigation.md](sidebar-navigation.md). |
| Loading, empty, or error feedback | Applicable first-load, refresh, retry, image, stale-response, and empty/error transition scenarios in [loading-empty-and-error-states.md](loading-empty-and-error-states.md). |
| Forms and validation | Applicable label/autofill, paste/IME, control semantics, keyboard, async validation, upload, and submission scenarios in [forms-and-validation.md](forms-and-validation.md). |
| Select, combobox, and action menu | Applicable value/command semantics, small/large choice sets, search, popup collision, keyboard/focus, commit, clearing, permissions, and mobile scenarios in [select-and-dropdown.md](select-and-dropdown.md). |
| Layout and spacing | Applicable wide/narrow layouts, real-content density, reflow, text-spacing, safe-area, and alignment scenarios in [layout-and-spacing.md](layout-and-spacing.md). |
| Surface and typography refinement | Applicable boundary/contrast, theme/font fallback, forced-colors, response delivery/reveal, numeric alignment, and drag interruption scenarios in [surface-and-type-refinement.md](surface-and-type-refinement.md). |
| Cards and collection containers | Applicable record/order semantics, actions, image/metric accuracy, carousel/table/disclosure/popover behavior, and scale scenarios in [cards-and-lists.md](cards-and-lists.md). |
| Motion and easing | Applicable entry/exit, rapid interruption, focus/layer cleanup, reduced/zero motion, async feedback, and dynamic-content scenarios in [motion-and-easing.md](motion-and-easing.md). |
| Mobile microinteractions | Applicable press/cancellation, repeated input, state accuracy, non-drag access, card layers, sheet gestures, and device-performance scenarios in [mobile-microinteractions.md](mobile-microinteractions.md). |
| Compound microinteractions | Applicable stage order, outcome truth, interruption/reversal, lock release, linked values, stack/snap/wheel behavior, reveal fallback, and zero-motion cleanup scenarios in [compound-microinteractions.md](compound-microinteractions.md). |
| Scroll interactions | Applicable driver/endpoints, restored position, changing geometry, sticky/anchor access, threshold stability, refresh, data truth, fallback, and scale scenarios in [scroll-interactions.md](scroll-interactions.md). |
| Explicit responsive work | Project breakpoints; absent project rules, at least narrow/medium/wide representative widths. |
| Multiple themes/languages | Each supported theme and languages with high-risk text length. |

Choose viewport widths based on the project and content, not fixed device labels. If repository policy allows a smaller matrix, follow it and state the evidence boundary.

## 3. Limit Rendering to Two Passes

Complex design can invite endless polishing. Use a bounded loop:

1. Finish implementation and project checks.
2. First batch render: inspect viewport, states, interactions, accessibility, and console together.
3. Fix confirmed issues in one pass.
4. Second pass confirms fixes and key regressions.
5. Stop unrelated polishing and record remaining suggestions.

Add another pass for a newly discovered structural issue only when you explain why. Do not repackage personal preference as a defect on every pass.

## 4. Browser Checks

### Visual

- First-screen identity and primary task.
- Content hierarchy, alignment, spacing, grouping, signature element.
- Overflow, truncation, overlap, horizontal scrolling, and unusual whitespace.
- Image crop, aspect ratio, focal point, and load failure.
- Layout shifts between states.
- Colors, borders, shadows, and scrollbars in the target theme.

### Interaction

- Primary path completes.
- Hover, active, focus-visible, selected, and disabled states respond.
- Modal/drawer/menu open/close, focus return, and scroll behavior.
- Repeated clicks, slow network, loading, and failure states.
- Form validation, error location, successful submit, and back path.
- Confirmation, undo, or recovery for destructive actions.

### Keyboard and Assistive Technology

- Tab order is predictable; there is no focus trap.
- All controls have visible focus.
- Icon-only controls have names and form fields have labels.
- Assistive technology can perceive dynamic feedback when needed.
- Core content remains usable with zoom and reduced motion.
- Checkbox, radio, menu, and dialog behavior matches native expectations.

### Runtime

- No new console errors.
- No failed critical XHR/fetch or sharp rise in repeated requests.
- No 404s for important assets.
- No obvious layout shift, long task, or unusual memory growth.
- Large lists, images, and fonts do not block first interaction.

## 5. Project Checks

Prefer commands from repository docs or CI. Common examples:

```sh
npm run lint
npm run typecheck
npm test
npm run build
```

Do not assume the package manager or script name. Inspect `package.json`, `AGENTS.md`, CI, and CONTRIBUTING first. Static checks alone do not replace UI rendering.

## 6. Completion Evidence

State in the final report:

- Actual URL/route or component opened.
- Viewports and themes used.
- States exercised.
- Commands run and results.
- Screenshot path or accessible link.
- Devices/browsers/states not rendered or covered.

Example:

```text
Verification:
- Rendered the home page and empty state at 1280px and 390px
- Completed search, filter, clear, and focus restoration with the keyboard
- `npm run typecheck`, `npm test`, and `npm run build` passed
- Not verified: Safari or a real iOS software keyboard
```

Do not infer that visuals, interactions, or accessibility are correct because lint/build passed.

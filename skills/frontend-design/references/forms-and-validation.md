# Forms and Validation

Use for registration, data entry, profile editing, filters, questionnaires, uploads, and multi-step submission. Improve identification, control selection, feedback, and recovery using the existing design system.

Contents: [Contract](#establish-the-form-contract) · [Diagnosis](#diagnose-before-editing) · [Thirteen modules](#thirteen-form-modules) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Form Contract

For deciding whether a form belongs inline, in a modal, drawer, or route, read [overlay-selection.md](overlay-selection.md). Preserve the field and draft contracts below.

Preserve fields, business rules, submitted values, permissions, and existing side effects. Do not add all thirteen controls to a short form, introduce endpoints, change consent defaults, or replace the component library just to satisfy a checklist. Prefer native controls or accessible components already in the project; inspect their actual capabilities before customizing.

For each affected field, establish its label, data type, allowed values, required/optional status, units, default, dependencies, serialization, validation timing, and server errors. Identify whether an action edits a draft, applies a filter, saves immediately, or submits the form. Keep those behaviors distinguishable.

Use semantic labels and grouping, suitable `type`, `inputmode`, and `autocomplete`, visible focus, keyboard operation, and non-color status cues. A placeholder is an example or hint, never the only field identification. Preserve password-manager, paste, autofill, and input-method behavior. Client checks help users; server validation remains authoritative.

Use [loading-empty-and-error-states.md](loading-empty-and-error-states.md) for pending, retry, and uncertain submission outcomes, and [navigation-and-routing.md](navigation-and-routing.md) for popup dismissal, focus return, and multi-step navigation. Reuse those contracts instead of inventing a second feedback system.

## Diagnose Before Editing

- Field meaning disappears after typing or autofill; labels overlap values or errors.
- A generic text field conceals constrained choices, units, formats, or dependencies.
- Selection controls look alike despite different choice semantics.
- Dragging, hovering, or color is the only way to operate or understand a control.
- Validation interrupts unfinished input, or remote responses overwrite newer values.
- Submission, upload, and disabled states are ambiguous; failures erase user work.
- Mobile keyboards, long labels, zoom, or translated text make fields unusable.

Choose the smallest applicable module set. Diagnose behavior with real input; screenshots alone cannot establish autofill, focus, validation, or submission correctness.

## Thirteen Form Modules

### 1. Persistent Labels and Optional Floating Labels

**Goal and rationale:** Keep a field's meaning visible before, during, and after editing.

**Rules:** An ordinary visible label is a valid default. Use floating labels only when consistent with the design system. Bind the label to the control; preserve readable size and contrast in focused, filled, blurred, autofilled, and prefilled states. Do not overlap values, hints, errors, or password-manager controls. Optional motion must honor reduced motion. Keep required status and format instructions available outside a disappearing placeholder.

**Prompt:**

```text
Give each field a persistent, associated label and clear required/optional guidance. Retain standard labels unless floating labels fit the existing system. If floating labels are used, verify focus, blur, prefilled values, autofill, long labels, and reduced motion without overlap or lost instructions.
```

### 2. One-Time Code Entry

**Goal and rationale:** Make fixed-length verification codes easy to enter, paste, correct, and autofill.

**Rules:** Match the existing code length and character set. Prefer one semantic input with optional visual cells; use `autocomplete="one-time-code"` and an appropriate input mode where supported. Treat codes as strings so leading zeros survive. If existing separate cells are retained, provide understandable labeling/order and support full-code paste, replacement, deletion/backspace, and correction without focus traps. Advance only within the code entry where predictable; do not unexpectedly submit or jump elsewhere when the last character arrives. Preserve the current verification/expiry/resend contract; never log codes or claim autofill works on untested devices.

**Prompt:**

```text
Improve the existing verification-code field with optional visual cells while preserving one coherent entry experience. Support leading zeros, full-code paste, correction, backspace, and platform autofill where available. Keep focus predictable and verification explicit according to the existing flow; do not add authentication behavior or automatically submit on the last character.
```

### 3. File Selection and Optional Drag-and-Drop

**Goal and rationale:** Make allowed files, upload status, and recovery clear with more than one input method.

**Rules:** Keep a labeled, keyboard-operable file picker; dragging is an enhancement. State actual format, size, and count limits before selection, and validate both picker and drop paths. Client `accept` hints do not replace server checks. Show only supported previews; release temporary preview resources and do not execute uploaded content. Distinguish selected, uploading, processing, complete, and failed files. Show measured progress only when available; otherwise use indeterminate feedback. Reuse existing cancellation/retry/removal behavior and avoid duplicate uploads.

**Prompt:**

```text
Refine the existing upload control with a clickable accessible picker and optional drop zone. Explain real limits, provide safe supported previews, and distinguish selection, transfer, processing, success, and failure. Keep progress truthful and preserve the existing upload API and retry contract.
```

### 4. Date Ranges

**Goal and rationale:** Make related start/end values understandable and valid.

**Rules:** Two clearly labeled date inputs are valid; a linked calendar is optional. Preserve locale display, serialized values, date-only versus timestamp semantics, timezone, inclusive/exclusive boundaries, and existing limits. Reject an end before the start without silently changing the user's choice. Add shortcuts only when useful and supported; define today, week start, and month boundaries in the relevant locale/timezone. Support typing, keyboard navigation, clearing, and narrow-screen selection; do not mandate dual calendars on mobile.

**Prompt:**

```text
Improve the existing start/end date selection with clear labels, linked constraints, and actionable range errors. Use an accessible calendar only where helpful; retain direct entry and mobile usability. Preserve timezone, locale, boundary, and serialization rules. Add only relevant shortcuts with defined date semantics.
```

### 5. Numeric Range Sliders

**Goal and rationale:** Support approximate range exploration without sacrificing precise entry.

**Rules:** Use sliders when spatial selection helps; keep numeric inputs for exact values or a non-drag alternative. Define units, min/max, step, rounding, and whether thumbs may cross. Label each thumb, expose its current value/bounds, preserve stable tab order, and support keyboard adjustments. Synchronize inputs and thumbs without request floods. Verify touch assistive technology on target devices or record it as unverified.

**Prompt:**

```text
Use a range slider only where it improves this numeric filter. Keep precise, non-drag entry available, label minimum and maximum controls, expose units and bounds, and support keyboard operation. Synchronize values and enforce the existing step, rounding, and crossing rules without excessive requests.
```

### 6. Autocomplete Suggestions

**Goal and rationale:** Reduce repeated typing while keeping free text and selected records distinct.

**Rules:** Establish whether arbitrary text is allowed or an existing option must be selected. Reuse an accessible combobox with expanded state, associated popup, active option, and expected Arrow/Enter/Escape behavior; preserve normal text editing. Choose trigger length and debounce from language, data size, and endpoint cost, not a universal two-character rule. Respect composition events, cancel or ignore stale responses, and distinguish pending, no match, and request failure. Keep display labels separate from submitted identifiers; editing a selection must not silently retain an obsolete ID.

**Prompt:**

```text
Refine autocomplete using the existing accessible combobox. Define whether free text is permitted, support pointer and keyboard selection, and distinguish loading, no results, and failure. Preserve composition input, bound requests, ignore stale responses, and keep the submitted identifier synchronized with the visible selection.
```

### 7. Radio Groups

**Goal and rationale:** Communicate a mutually exclusive choice.

**Rules:** Prefer native radio inputs sharing the correct group name, with a meaningful group label such as a `fieldset`/`legend`. Make option text clickable without intercepting nested links. Preserve selection defaults, required status, arrow-key behavior, and adequate target spacing. Selection must not unexpectedly submit or navigate.

**Prompt:**

```text
Represent this mutually exclusive choice with a labeled radio group. Keep option labels clickable, selected state visible beyond color, and keyboard behavior native. Preserve existing defaults and validation; selecting an option must not unexpectedly submit the form.
```

### 8. Checkboxes

**Goal and rationale:** Express independent selections and explicit acknowledgments.

**Rules:** Keep independent checkboxes distinct from radio groups and switches. Add select-all only for a real collection; define its scope across filters or pagination and represent partial selection honestly. Preserve consent wording and defaults; do not preselect agreement or add it to a bulk selector. Provide associated labels and keyboard operation; keep links within agreement text independently usable.

**Prompt:**

```text
Use labeled checkboxes for independent selections. Add a scoped select-all control only when the existing collection needs it, with accurate mixed state. Preserve consent defaults and text, keep agreement links operable, and do not bundle consent into bulk selection.
```

### 9. Selects and Choice Popups

**Goal and rationale:** Keep bounded option lists compact and understandable.

**Rules:** Prefer native select or the project's accessible equivalent. Use search for genuinely long lists; an ordinary select does not need a custom modal. Keep selected label, submitted value, clear/reset behavior, and unavailable options consistent. A custom popup needs correct focus/keyboard/dismissal semantics; a mobile sheet is optional and follows the existing overlay contract. Opening, closing, or searching must not silently commit a different value or discard an explicit draft choice.

**Prompt:**

```text
Refine this bounded choice using the existing select component. Add search only for a long list, keep selected labels and submitted values consistent, and provide usable keyboard and dismissal behavior. Use a mobile sheet only if it fits the platform and existing overlay contract.
```

### 10. Switches

**Goal and rationale:** Express an on/off setting with clear persistence behavior.

**Rules:** Use switch semantics for on/off settings, and checkboxes for acknowledgments or independent selections where those semantics fit. Keep the switch label stable while its state changes. Expose checked state, support Space and visible focus, and do not rely on color alone. Distinguish immediate saving from changes applied by a later Save action. If saving fails, reconcile/revert and explain the actual state rather than leaving a false success.

**Prompt:**

```text
Use a labeled switch for this on/off setting with stable wording, keyboard operation, and a visible non-color state cue. Preserve whether changes save immediately or require Save. Show pending/failure honestly and reconcile the displayed state after a failed update.
```

### 11. Multiline Text

**Goal and rationale:** Support descriptions, notes, and other text that needs more than one line.

**Rules:** Use a labeled textarea; choose bounded auto-growth or usable resizing/scrolling without moving the cursor or hiding actions. Display a counter only when a real limit or user need exists. Match backend counting rules, including Unicode behavior; never silently truncate pasted or composed text. Preserve newlines and explain limits near the field. Plain Enter inserts a newline rather than accidentally submitting.

**Prompt:**

```text
Use a multiline textarea with a persistent label and bounded growth or usable resizing. Preserve newlines, paste, and composition input. Show real length limits and consistent counting without silent truncation or cursor jumps; Enter should retain normal multiline editing behavior.
```

### 12. Multi-Step Forms

**Goal and rationale:** Divide a genuinely complex task into logical, navigable groups.

**Rules:** Keep short forms short. A wizard is a scope-sensitive flow change, not a mandatory visual upgrade. When appropriate, show named steps and truthful progress; retain values on Back/Next and focus the new step heading or relevant error. Validate relevant fields before advancing and all applicable fields at final submission, including server-side checks. Reconcile downstream values when earlier answers change. Preserve existing draft/storage policy; do not persist passwords, codes, or sensitive fields to browser storage by default. Keep intermediate buttons from accidentally submitting the final operation.

**Prompt:**

```text
Use a stepper only if this long form benefits from logical grouping and the flow change is in scope. Show accurate progress, retain values on Back/Next, and manage focus. Validate the current step and final applicable payload, reconcile changed dependencies, and preserve the existing draft policy without introducing sensitive browser storage.
```

### 13. Validation and Submission Feedback

**Goal and rationale:** Help users correct mistakes at a useful moment and understand whether submission succeeded.

**Rules:** State requirements before input. Usually validate on blur or submit, then revalidate a previously invalid field as it is corrected; immediate checks are useful only when they help unfinished input. Respect composition and debounce remote/expensive checks, not every local operation. Discard obsolete async validation results. Associate nearby error text with its field and expose invalid state; do not rely on red or success icons alone. After failed submit, help users reach the first error or an accessible summary without erasing input. Keep form-wide server errors distinct from field errors. Pending is not success; prevent duplicate submission while providing understandable recovery. Preserve authoritative server validation and the existing error-disclosure policy.

**Prompt:**

```text
Add actionable validation at the right moment: clear instructions first, blur/submit checks where appropriate, and timely correction feedback after an error. Respect composition, bound remote checks, and ignore stale responses. Associate errors accessibly, retain user input, and make submit pending, server rejection, and confirmed success distinct without duplicate submissions.
```

## Combined Prompt

Apply relevant modules incrementally, not all thirteen by default.

```text
Improve the existing form while preserving fields, business rules, payloads, permissions, and side effects. Reuse native or existing accessible controls and choose types by task semantics. Keep labels and instructions available after entry; floating labels are optional. Where relevant, support code paste/autofill, click and drag upload, date and numeric ranges, autocomplete, distinct radio/checkbox/select/switch behavior, multiline text, and logical steps. Preserve keyboard operation, composition, mobile input, precise non-drag alternatives, and predictable focus. Validate at useful moments, ignore stale async results, explain errors near fields, retain user work, and distinguish pending from confirmed success. Avoid duplicate submissions, fake progress, new endpoints, and sensitive draft persistence. Keep the product's visual system and verify the actual input and recovery paths.
```

## Acceptance and Regression Checks

Use the existing test framework and [verification.md](verification.md). Exercise only applicable controls; mark pass/fail/unverified or explain N/A. Static screenshots cannot prove these behaviors.

| Scenario | Expected evidence |
| --- | --- |
| Labels and layout | Empty, focused, filled, blurred, autofilled, invalid, disabled, and long-label states remain readable at target widths and zoom. |
| Code entry | Leading zeros, full-code paste, replacement, deletion, and supported autofill work without surprise submission or focus escape. |
| Upload | Picker and drop paths apply the same limits; progress is truthful; failed/retried uploads do not duplicate or lose unrelated input. |
| Dates and ranges | Ordering, bounds, units, rounding, locale/timezone, direct entry, and keyboard alternatives preserve submitted semantics. |
| Suggestions and selects | IME input and keyboard selection work; out-of-order responses cannot restore obsolete choices or identifiers. |
| Selection semantics | Radios remain exclusive; checkboxes remain independent; select-all scope/mixed state and switch persistence are correct. |
| Text and steps | Newlines, Unicode limits, paste, backward navigation, changed dependencies, and focus preserve valid user work. |
| Validation | Untouched/incomplete fields are not prematurely marked wrong; field and form errors remain discoverable and actionable. |
| Submission | Repeated clicks, Enter, slow response, server rejection, and retry preserve data and avoid duplicate writes; success requires confirmation. |
| Input access | Keyboard, touch, reduced motion, and relevant assistive technology are exercised; unsupported/unavailable device paths are explicitly unverified. |

## Primary References

- [W3C WAI: Labeling Controls](https://www.w3.org/WAI/tutorials/forms/labels/): labels, associations, and usable click targets.
- [W3C WAI: Validating Input](https://www.w3.org/WAI/tutorials/forms/validation/): constraints and useful validation feedback.
- [W3C APG: Combobox](https://www.w3.org/WAI/ARIA/apg/patterns/combobox/): suggestion semantics and keyboard interaction.
- [W3C APG: Switch](https://www.w3.org/WAI/ARIA/apg/patterns/switch/): stable labels and on/off semantics.
- [W3C APG: Multi-Thumb Slider](https://www.w3.org/WAI/ARIA/apg/patterns/slider-multithumb/): thumb labeling, stable tab order, and touch accessibility caveats.
- [W3C WAI: Multi-page Forms](https://www.w3.org/WAI/tutorials/forms/multi-page/): logical steps and progress.
- [MDN: autocomplete](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes/autocomplete): field-purpose and one-time-code tokens.

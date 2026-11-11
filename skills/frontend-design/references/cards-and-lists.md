# Cards and Lists

Use when choosing or refining containers for independent content, metrics, recommendations, structured records, filters, events, workflows, or notifications. Match the representation to the user's reading and interaction task; variation is not a goal by itself.

Contents: [Contract](#establish-the-content-contract) · [Diagnosis](#diagnose-before-editing) · [Fourteen modules](#fourteen-container-modules) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Content Contract

Preserve business content, record identity, ordering, permissions, navigation destinations, and action side effects. Reuse existing tokens and components; do not add a carousel, masonry library, bulk operation, persistence mechanism, or endpoint just to complete this catalog. Check the installed library/version before assuming a component exists.

Identify what users need to do: compare rows, scan summaries, explore images, choose filters, follow chronology, or complete steps. A plain list or table can be better than cards. Several modules can compose, such as a table with optional striping; they are not fourteen mutually exclusive styles or fourteen mandatory additions. Uniform cards are acceptable when the underlying content and task are uniform.

Reuse [layout-and-spacing.md](layout-and-spacing.md) for container width, insets, responsive grid, density, and spacing tokens; do not impose an 8px scale. Use [loading-empty-and-error-states.md](loading-empty-and-error-states.md) for image and collection feedback, [forms-and-validation.md](forms-and-validation.md) for selection and step forms, and [navigation-and-routing.md](navigation-and-routing.md) for links, overlays, and focus behavior. Read only relevant modules.

Apply only states supported by the component's meaning. Static content does not need hover motion or selection; unread, expanded, or pending styling must reflect real state. Keep repeated variants coherent across themes without forcing white backgrounds, shadows, identical heights, or decorative wrappers. Preserve keyboard operation, visible focus, meaningful semantics, and non-color state cues.

## Diagnose Before Editing

- Every module has a decorative card wrapper even when a list/table would scan better.
- Comparison data is spread across tiles without visible column relationships.
- Primary links, row selection, and secondary actions compete or trigger each other.
- Images distort, crop meaningful subjects, shift layout, or fail without fallback.
- Carousels hide content without discoverable controls, or masonry scrambles reading order.
- Repeated items have inconsistent alignment, clipped text, or inaccessible hover-only actions.
- Expanded, selected, unread, empty, pending, and failed states are absent or misleading.
- A large collection overwhelms rendering, or optimization breaks focus and record identity.

Do not infer interaction correctness from screenshots. Establish semantic structure and state transitions before decorative treatment.

## Fourteen Container Modules

### 1. Basic Information Cards

**Goal and rationale:** Group one independent object with its summary and relevant actions.

**Rules:** Use the current surface, radius, inset, border/shadow, and typography tokens. Provide a clear title and align equivalent content/actions; let real content determine height. Use a border, tonal surface, or spacing where sufficient rather than adding shadows everywhere. Make a whole card interactive only when it has a clear primary action; do not nest buttons or links inside another interactive element. Secondary actions must remain distinct and keyboard accessible.

**Prompt:**

```text
Refine independent information cards using the existing theme and spacing tokens. Establish a clear title, summary, and consistent action placement without extra decoration or clipped content. Use semantic primary links/actions and keep secondary controls independently operable.
```

### 2. Metric Cards

**Goal and rationale:** Make a real metric understandable at a glance.

**Rules:** Emphasize the value with its label, unit, time window, and comparison basis where relevant. Preserve precision, formatting, and source semantics. Distinguish zero, unavailable, stale, and pending values. An increase is not inherently good; choose trend meaning and colors from the metric, locale, and product conventions. Include text/sign/shape cues beyond color. Never invent values, percentages, trends, or sparklines; keep definitions accessible when needed.

**Prompt:**

```text
Present real metrics with a clear value, label, unit, period, and comparison basis. Distinguish zero from unavailable or stale data. Map trend meaning to this metric and product convention, using non-color cues; do not assume green always means an increase or fabricate trends.
```

### 3. Horizontal Collections and Carousels

**Goal and rationale:** Let users explore a related collection horizontally when that serves discovery.

**Rules:** Prefer a simple scrollable row when sufficient. Do not convert every horizontal group to a rotating carousel. Provide discoverable, labeled controls and a non-drag path; touch gestures must coexist with page scrolling. Choose item/page movement from viewport and content rather than forcing one card per swipe. Indicators are optional and must be usable if interactive. Keep focus predictable and offscreen-slide controls from becoming unreachable or misleading. Do not introduce autoplay by default; existing autoplay needs pause control and must stop on focus/hover according to the carousel contract, without unexpected restart. Honor reduced motion.

**Prompt:**

```text
Use an accessible horizontal row or carousel only where it helps exploration. Provide discoverable previous/next or equivalent non-drag access, predictable focus, and responsive item visibility. Preserve touch scrolling and reduced motion; avoid autoplay and mandatory one-card paging unless required by the existing design.
```

### 4. Masonry Collections

**Goal and rationale:** Accommodate varied image/content heights in visual discovery.

**Rules:** Use masonry for suitable visual content, not records requiring row comparison or strict sequence. Preserve meaningful DOM, reading, and focus order; do not visually reorder a chronological or ranked feed. Use a tested existing layout or a compatible fallback instead of assuming experimental CSS support. Reserve image space, preserve aspect ratios, and keep long content available. Do not crop/truncate merely to enforce height. Responsive columns and appended items must not move the focused item unpredictably.

**Prompt:**

```text
Use masonry only if varied visual content benefits from it. Preserve image proportions, meaningful reading/focus order, and access to long content. Reuse a supported layout with responsive columns and a fallback; retain a grid/list for ordered or comparison-heavy records.
```

### 5. Tables with Sticky Headers

**Goal and rationale:** Keep column meaning visible while users scan structured records.

**Rules:** Retain semantic table headers/cell associations and a useful accessible name/caption. Add sticky headers when scrolling requires them, inside an intentional scroll container. Prevent collisions with fixed page navigation and obscured focus; maintain column alignment. Align numeric and text columns by role. Preserve sort/filter/pagination and row-action semantics. Avoid duplicate accessible header copies. Horizontal overflow should be contained without hiding required data. A surrounding card is optional. Use virtualization only when warranted by collection size and supported accessibility behavior; pagination can be sufficient.

**Prompt:**

```text
Present comparison data as a semantic table with clear column relationships. Add a sticky header only where scrolling benefits, preserving alignment and visible focus beneath fixed navigation. Contain overflow and preserve existing sort, filter, pagination, and actions; do not add virtualization or a decorative card without need.
```

### 6. Alternating Row Surfaces

**Goal and rationale:** Help users track a row across dense data when striping improves scanning.

**Rules:** Striping is optional, not a replacement for semantic structure, spacing, or headers. Use restrained theme tokens with readable text and controls. Keep selected, focused, hovered, disabled, and error states distinguishable from stripes. Check filtered, sorted, paginated, and virtualized rows for consistent visible striping without changing row identity. Avoid wrapping every row in a separate card merely to add separation.

**Prompt:**

```text
Add subtle alternating row surfaces only if they improve this list/table. Keep text readable and selected, focused, and error states distinct across themes. Preserve stable row identity and the existing sorting/filtering behavior; avoid extra card wrappers.
```

### 7. Action and Navigation Cards

**Goal and rationale:** Make a destination or action recognizable in a workspace.

**Rules:** Use an anchor for navigation and a button for an action. Combine an existing icon style, specific title, and brief description where useful. Do not depend on hover, lifting motion, or icons alone to convey interactivity. Keep focus visible, disabled/unavailable meaning honest, and secondary controls separate. Equal visual alignment must not truncate essential labels or descriptions.

**Prompt:**

```text
Design workspace entry cards with a clear destination/action, concise title, and optional consistent icon. Use links for navigation and buttons for actions, with keyboard focus and touch feedback. Keep secondary controls separate and avoid hover-only cues or forced-height clipping.
```

### 8. Image and Cover Cards

**Goal and rationale:** Connect an image to its title, summary, and relevant metadata.

**Rules:** Choose aspect ratio by content; 16:9 or 4:3 are examples, not universal requirements. Fit product artwork, documents, or covers when cropping would lose meaning; crop photography deliberately with a suitable focal point. Reserve image dimensions, preserve useful alternative text, and handle failure through the existing image component. Keep overlays readable without obscuring the subject; do not bake essential text into the image. Lazy-load suitable offscreen images, not critical first-screen imagery indiscriminately.

**Prompt:**

```text
Create image/content cards with a ratio and fit strategy appropriate to the actual assets. Preserve meaningful subjects and readable titles, reserve image space, and reuse loading/failure behavior. Do not impose one crop or lazy-load policy on every image.
```

### 9. Expandable Panels

**Goal and rationale:** Offer optional details without obscuring the summary or primary task.

**Rules:** Reuse accessible disclosure/accordion components or suitable native disclosure. Make the trigger a meaningful control with expanded state and a clear relationship to its panel. Hidden controls must not remain in the tab path; avoid losing focus when collapsing. Preserve entered values and validation visibility if a panel contains fields. Do not collapse essential warnings/actions merely to simplify the first screen. Remember expansion only within the existing state/storage policy; new persistent storage is not a default requirement. Respect reduced motion.

**Prompt:**

```text
Use an accessible expandable panel for optional details, with a clear summary, keyboard trigger, and accurate expanded state. Keep essential actions visible, preserve form values and error discoverability, and manage hidden content/focus correctly. Reuse existing expansion persistence rather than adding storage.
```

### 10. Tag and Filter Groups

**Goal and rationale:** Expose related filters with honest selection semantics.

**Rules:** Decide single versus multiple selection and immediate versus applied filtering first. Use radio, checkbox, or pressed-button semantics appropriate to the existing model. Keep labels and selected states perceivable beyond color; let tags wrap without losing keyboard order. Distinguish applied filters from unsaved choices. Preserve query parameters, submitted IDs, and existing clear/reset behavior; a new filter card must not invent new options or endpoints. Follow the selection rules in [forms-and-validation.md](forms-and-validation.md).

**Prompt:**

```text
Group existing filter choices with clear single/multi-select semantics, wrapping labels, and visible selected state. Preserve immediate-versus-Apply behavior, submitted values, and reset/query contracts. Reuse form-control semantics and avoid adding a card or filter capability unnecessarily.
```

### 11. Timelines

**Goal and rationale:** Make event order and timing understandable.

**Rules:** State ascending/descending order and display the relevant timestamp/timezone consistently. Keep underlying timestamps available where relative times need clarification. Use an ordered semantic structure when appropriate; decorative lines and dots must not be the only state cue. Preserve simultaneous events, missing times, and known gaps instead of inventing chronology. Align timestamps and wrap event details without clipping. A presentation improvement must not alter audit records or imply unsupported completion.

**Prompt:**

```text
Present real events as a readable timeline with explicit ordering and consistent time/timezone display. Keep event text and status understandable without decorative nodes. Handle simultaneous or missing timestamps honestly and preserve complete details and record integrity.
```

### 12. Workflow Steppers

**Goal and rationale:** Explain the actual current stage and next available action.

**Rules:** Distinguish an informational process tracker from an interactive form wizard. Reflect actual completed, current, pending, failed, or skipped states; do not equate a selected step with completion. Only actionable steps get interactive semantics. Wrap or use a vertical layout when horizontal labels no longer fit. For form navigation, reuse [forms-and-validation.md](forms-and-validation.md) rather than duplicating its validation/persistence rules. Do not introduce a new workflow to decorate an existing status display.

**Prompt:**

```text
Show the existing process as a truthful stepper, distinguishing current, completed, pending, and relevant failure/skip states. Keep non-actionable steps informational and adapt orientation for available width. For a form wizard, preserve its navigation and validation contract.
```

### 13. Notification Lists

**Goal and rationale:** Make message relevance, time, unread status, and available actions easy to scan.

**Rules:** Use clear titles, readable summaries, timestamps, and a text/semantic unread cue in addition to any dot. Keep time visually secondary but readable. Reuse the actual mark-read contract; opening a menu or focusing an item must not unexpectedly mark it read. Batch actions are optional existing capabilities: define selection scope and handle partial failure honestly. Preserve message identity, ordering, permissions, and pagination. Let variable-length text wrap rather than forcing identical row heights. New arrivals should not steal focus or unexpectedly jump scroll position.

**Prompt:**

```text
Refine notification rows with readable titles, summaries, time, and accessible unread state. Preserve existing read/unread transitions, ordering, and actions; add no new bulk operation. Keep long messages usable and prevent incoming items from stealing focus or disrupting scroll position.
```

### 14. Popovers and Contextual Details

**Goal and rationale:** Reveal brief contextual information/actions without losing the underlying task.

**Rules:** Choose semantics from content: a tooltip is noninteractive help; actions or editable content need an appropriate menu/dialog/popover pattern. Do not make useful content hover-only; provide keyboard/touch access. Follow the existing open/close, Escape, outside-interaction, and focus-return contract, protecting unsaved work. Keep the overlay anchored and within the viewport without obscuring its trigger or task. An arrow and stronger shadow are optional visual aids, not correctness requirements. Do not trap focus in a nonmodal popup or place interactive controls inside a tooltip. Use [navigation-and-routing.md](navigation-and-routing.md) for overlay details.

**Prompt:**

```text
Use a contextual popover only for suitable secondary information or actions. Choose correct tooltip/menu/dialog semantics, provide keyboard and touch access, and preserve predictable dismissal and focus return. Keep positioning within the viewport, protect unsaved work, and treat arrows/shadows as optional styling.
```

## Combined Prompt

Use relevant modules incrementally; do not add all fourteen containers to one page.

```text
Choose and refine content containers by task while preserving data, ordering, permissions, navigation, and side effects. Reuse the existing theme, spacing, layout, and components. Prefer plain lists or semantic tables for scanning/comparison, cards for independent objects, and truthful metric displays for real indicators. Use horizontal exploration, masonry, disclosure, filters, timelines, steppers, notifications, or contextual popovers only where appropriate. Preserve reading/focus order, meaningful image crops, independent actions, and accessible keyboard/touch behavior. Keep states honest and relevant, avoid hover-only access or autoplay, and do not add dependencies, persistence, bulk operations, or virtualization merely for variety. Verify real content, responsive overflow, state changes, and large-collection behavior using the existing test and rendering tools.
```

## Acceptance and Regression Checks

Use [verification.md](verification.md) and existing project tests. Mark applicable cases pass/fail/unverified and explain N/A. Preserve the bounded rendering process; a successful screenshot or build does not prove collection interactions.

| Scenario | Expected evidence |
| --- | --- |
| Representation and scope | Comparison, chronology, and selection remain understandable; no content, capability, or unauthorized state mutation is introduced. |
| Repeated and long content | Stable record identity, aligned actions, readable long/missing content, and consistent tokens across target themes/widths. |
| Metrics and timestamps | Units, comparison periods, zero/missing/stale values, trend meaning, ordering, and timezone remain accurate. |
| Images and masonry | No distortion or harmful crop; reserved space/fallback works; visual, DOM, and focus order remain meaningful after resize/append. |
| Carousel and gestures | Non-drag controls work; slide boundaries, focus, offscreen content, touch scrolling, and any pause controls behave correctly. |
| Table and row states | Header associations and alignment survive scrolling; sticky elements do not hide focus; stripes remain distinct from selection/error. |
| Disclosure and steps | Expanded state, hidden focusability, values/errors, workflow status, and back/forward behavior remain correct. |
| Actions and notifications | Primary/secondary controls do not trigger each other; mark-read/filter/batch scope and partial failures preserve the existing contract. |
| Popovers | Keyboard/touch opening, Escape/outside dismissal, viewport placement, and focus return match content semantics. |
| Scale and feedback | Applicable loading/empty/error and pagination work; any virtualization preserves identity, focus, and accessible context without request floods. |

Measure representative collection sizes before adding performance machinery. Reuse pagination, windowing, and image components when justified; record unverified device, assistive-technology, and scale limits rather than promising universal support.

## Primary References

- [W3C APG: Carousel](https://www.w3.org/WAI/ARIA/apg/patterns/carousel/): discoverable navigation, focus, and rotation controls.
- [W3C APG: Accordion](https://www.w3.org/WAI/ARIA/apg/patterns/accordion/): disclosure triggers, expanded state, and keyboard behavior.
- [W3C WAI: Tables Tutorial](https://www.w3.org/WAI/tutorials/tables/): semantic headers and data-cell relationships.

# Overlay and Dialog Selection

Use when a request says only “add a popup,” when transient surfaces are confused, or when feedback, confirmation, and content capacity do not match their container. Choose by interruption, task length, persistence, and the need to retain context; component names vary across design systems.

Contents: [Decision contract](#establish-the-overlay-contract) · [Diagnosis](#diagnose-before-editing) · [Seven patterns](#seven-presentation-patterns) · [Three guardrails](#three-guardrails) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Overlay Contract

Before implementation, answer:

1. Does this information/action need to interrupt the current task, or can the user continue?
2. Is the task a short explanation, one-way status, brief decision, or substantial/independent workflow?
3. Does the user need to retain the current page's context, draft, or comparison data?
4. How does the surface open, close, time out, affect focus/history, and recover from failure?

Prefer the least interruptive pattern that still makes the message perceivable and the task operable. Content length is one signal, not a pixel/word-count threshold. Fit the task to viewport, zoom, text scaling, keyboard, touch, and screen-reader behavior. Preserve project primitives and lifecycle from [navigation-and-routing.md](navigation-and-routing.md); use [loading-empty-and-error-states.md](loading-empty-and-error-states.md) for asynchronous feedback and recovery, and [forms-and-validation.md](forms-and-validation.md) for drafts and submission.

| Pattern | Use when | Persistence and interaction | Interruption |
| --- | --- | --- | --- |
| Tooltip | Brief supplementary explanation for a control or term | Available on hover and keyboard focus; dismissible; no focusable actions inside | Non-modal |
| Toast / transient status | Brief, low-risk confirmation or progress result that needs no action | May time out only when safe and readable; users can continue | Non-modal |
| Inline alert / status | Page- or region-specific information, validation, warning, or recoverable error | Persists while relevant; actions may be included if needed | Non-modal |
| Modal dialog / alert dialog | A short decision or message that truly requires a response before continuing | Focus and background behavior match actual modality; explicit close/decision | Modal only when it truly blocks background interaction |
| Popconfirm | A brief, contextual confirmation attached to one action | Confirm/cancel; keeps trigger context | Local transient interruption |
| Drawer / sheet | A contextual detail, comparison, or substantial but bounded workflow | Independent scroll and explicit close/save; may be modal or non-modal | Depends on implementation, not its visual shape |
| Full page / route | An independent, shareable, multi-step, or large task that deserves its own space | Real route, meaningful Back/Up, direct-entry behavior as applicable | Normal navigation |

## Diagnose Before Editing

- A short tooltip is carrying an action, form, or lengthy explanation.
- A transient message disappears before it can be read, or is the only place a recoverable error appears.
- A modal is used for routine success feedback or a decision that does not block the current task.
- A drawer is assumed to be non-modal even though it disables the underlying page, or vice versa.
- A bounded popup clips choices or hides its trigger/focus; a mobile anchored list is too cramped for touch.
- Multiple blocking layers compete for focus, backdrop, Escape/Back, scroll lock, or screen-reader context.
- A popup command is mistaken for a selected field value, or an inline alert is exposed as an unrelated global notification.
- A large task has no route, back/close, draft-preservation, or direct-entry policy.

An absent component is not a defect by itself. Judge whether users can understand the message, finish or safely cancel the task, and recover without losing context.

## Seven Presentation Patterns

### 1. Tooltip: Brief Supplementary Explanation

**Goal and rationale:** Explain a control without moving the user into another task or hiding essential information.

**Rules:** Keep content brief and non-interactive. It should appear on pointer hover and keyboard focus; `Escape` dismisses when applicable. Hover-only/long-press-only access is insufficient. Keep focus on the trigger, associate the description semantically, and allow enough time to move the pointer to the tooltip when hover is used. On touch, ensure the explanation is available through a suitable explicit/help control or another discoverable path. Do not put validation, warnings that require action, forms, or buttons inside a tooltip; use a non-modal dialog/popover when content must be interactive.

**Prompt:**

```text
Use a tooltip only for brief supplementary, non-interactive text. Make it available on pointer hover and keyboard focus, keep focus on the trigger, support Escape dismissal, and ensure touch users have a discoverable alternative. If the content needs an action, form, or durable warning, select a popover/dialog/inline message instead.
```

### 2. Toast: Brief Transient Status

**Goal and rationale:** Confirm a low-risk outcome without taking focus or blocking work.

**Rules:** Use for concise information such as a completed, non-critical action when no user response is needed. Do not make auto-dismiss the only way an important error, required next step, or destructive consequence is communicated. Keep duration readable and consistent with platform/accessibility requirements; provide pause/close or persistent alternatives where needed. Avoid announcing duplicate messages repeatedly. Coalesce or queue only in a way that does not hide meaningful outcomes; a global single-toast limit is not universal. Provide an accessible status announcement without moving focus.

**Prompt:**

```text
Use a concise non-modal toast only for a low-risk status that requires no action. Keep focus in place and announce the result accessibly. Do not let a timeout erase an important error or required action; provide a persistent inline message or actionable surface. Avoid duplicate announcement floods while preserving distinct important outcomes.
```

### 3. Inline Alert: Persistent Contextual Message

**Goal and rationale:** Keep relevant warning, validation, or recovery information visible in the page region it concerns.

**Rules:** Place it near the affected content and preserve it while the condition remains relevant or until the user resolves/dismisses it according to the product contract. An alert can include a useful action; do not make every alert assertive or steal focus. Use an appropriate status/alert semantic based on urgency, and avoid repeatedly announcing the same message. Distinguish error, warning, informational, and success states with text/icon as well as color. A recoverable field error may belong next to the field rather than in a global banner.

**Prompt:**

```text
Show this persistent condition inline near the affected content, with a clear message and a relevant recovery action if one exists. Keep it visible while the condition matters, distinguish severity without color alone, and avoid focus theft or repeated assertive announcements. Use field-level feedback when only one field is affected.
```

### 4. Modal Dialog: Brief Blocking Decision

**Goal and rationale:** Request a decision or response only when proceeding without it would be unsafe or incoherent.

**Rules:** Make true modality real: background content is visually and programmatically unavailable, focus enters and stays within the dialog, and returns appropriately on close. Provide a named dialog, a clear cancel/close path unless policy genuinely requires a decision, and a primary action whose result is explicit. Never auto-dismiss an active modal. Keep content as brief as the task allows, but do not use an arbitrary word-count limit; if it needs substantial reading, comparison, or editing, use an appropriate scrollable layout or a route. Long content with semantic structure should not be flattened into one screen-reader description. A drawer/sheet can itself be modal; shape does not determine modality.

**Prompt:**

```text
Use a modal dialog only when the user must resolve this brief decision before continuing. Implement real background inertness, accessible naming, sensible initial focus, contained tab order, explicit actions/close, and focus restoration. Do not auto-dismiss or flatten complex content into one announcement. If the task is lengthy, compare a contextual drawer/sheet or full route instead.
```

### 5. Popconfirm: Contextual Lightweight Confirmation

**Goal and rationale:** Confirm a simple, reversible/contextual action near its trigger without opening a full dialog.

**Rules:** Use only for a short question with a clear pair of actions and a stable trigger context. Keep labels specific (for example, “Delete draft” rather than “OK”) and make cancel easy. Ensure keyboard operation, focus behavior, and dismissal are correct for the component's actual pattern. For irreversible, high-impact, destructive, or consequential actions that need explanation, use a deliberate modal/alert dialog instead. Do not treat a visual popconfirm as permission or authorization.

**Prompt:**

```text
Use an anchored confirmation only for this short, contextual decision. Name the action in both the question and confirm button; keep cancel clear and keyboard access predictable. For irreversible or consequential changes requiring explanation, use the established modal confirmation flow. Preserve authorization and operation safeguards.
```

### 6. Drawer / Sheet: Contextual, Larger Surface

**Goal and rationale:** Provide more room for detail or bounded editing while retaining useful connection to the current page.

**Rules:** Define whether the drawer is modal or non-modal. If modal, follow dialog focus, background, Escape/Back, scroll-lock, and return-focus contracts; if non-modal, do not trap focus or disable unrelated page content. Give the panel an accessible name, explicit close/save/cancel controls, internal scrolling, responsive sizing, and safe-area/virtual-keyboard handling where relevant. Preserve drafts and clarify unsaved-change behavior. A drawer is not automatically better for a long, multi-step or independent task; promote that task to a route when it needs direct linking, a durable URL, broad workspace, or a distinct navigation flow.

**Prompt:**

```text
Use an appropriately sized drawer/sheet for this contextual, bounded task. Declare and implement its modal/non-modal behavior, internal scroll, accessible title, explicit close/save/cancel, focus/history handling, safe areas, and draft preservation. Move it to a route if the task needs direct entry, a multi-step workflow, or substantially more workspace.
```

### 7. Full Page / Route: Independent Workflow

**Goal and rationale:** Give a large, multi-step, shareable, or independently navigable task the space and URL contract it needs.

**Rules:** Reuse the existing router and hierarchy. Provide a meaningful Back/Up or cancel path; do not invent history for direct entry. Preserve or intentionally discard drafts according to the existing contract. Verify route loading, direct entry/refresh, permissions, and recovery where in scope. A new route is a behavior/navigation change, not a styling workaround; do not add it unless the task's autonomy or capacity justifies it.

**Prompt:**

```text
Represent this large or independent task as a route using the existing router. Define a real parent/Back/Up path, direct entry and refresh behavior, permissions, and draft preservation. Do not manufacture browser history or expand the route structure if a contextual surface already fits the task.
```

## Three Guardrails

### 1. Avoid Competing Blocking Layers, Not Every Nested Surface

Do not stack unrelated modal dialogs, confirm dialogs, or sheets with independent backdrops and focus traps. A necessary subordinate non-modal popup (for example, a select list inside a dialog) may be valid when it is coordinated as the top layer, closes predictably, and preserves the parent. Prefer finishing/dismissing the current surface before launching a new blocking task. Define z-order, Escape/Back ownership, focus restoration, scroll lock, and portal boundaries for any supported nested layer.

### 2. Do Not Hide Important Errors in a Short-Lived Toast

Choose feedback by consequence and recovery: brief low-risk outcomes may use a transient status; a correctable field error belongs near the field; a page/region issue can use a persistent inline alert; a task-blocking condition may require a modal alert dialog. Keep the message readable, available long enough, programmatically perceivable, and actionable where appropriate. A failure is not automatically modal.

### 3. Match Modal Capacity to the Task

Avoid putting a long, multi-step, or structurally complex editing task in a cramped modal. A brief form or decision can be a modal when the available space and focus contract work; use a drawer/sheet for contextual bounded work, or a route for a broad independent task. Confirm at narrow viewport, zoom, and with long content before deciding it fits.

## Combined Prompt

```text
Choose the presentation by interruption, task/copy capacity, persistence, and context: tooltip for brief supplementary noninteractive help; toast for low-risk transient status; inline alert for persistent contextual conditions; modal for a necessary short blocking decision; anchored confirmation for a simple contextual action; drawer/sheet for larger bounded contextual work; route for a broad independent or shareable workflow. These are heuristics, not rigid word-count rules. Preserve project components, action/data semantics, validation, authorization, drafts, routing, and operation outcomes. Do not auto-dismiss important errors, focus-steal for non-modal status, or stack competing blocking layers. Define the exact modality, keyboard/touch behavior, focus and scroll lifecycle, timeout/dismissal, and recovery. Keep necessary nested non-modal controls coordinated. Verify realistic content, narrow/zoomed layouts, keyboard, screen-reader status, Escape/Back, and failure paths.
```

## Acceptance and Regression Checks

Use [verification.md](verification.md) and applicable navigation, form, loading, and select checks. Mark each applicable case pass/fail/unverified or N/A with rationale. A screenshot cannot prove focus, announcement, timeout, or history behavior.

| Scenario | Expected evidence |
| --- | --- |
| Pattern semantics | Help, status, persistent condition, value selection, command, confirmation, and navigation use the correct interaction/semantic pattern. |
| Timeout and recovery | Transient feedback remains perceivable; important errors persist; retry/fix actions work without data loss or duplicate operations. |
| Modal behavior | Only truly modal surfaces inert the background; focus entry, containment, close, Escape policy, restoration, and scroll cleanup are correct. |
| Nested layers | Necessary child popups work; only the intended top layer closes; no competing backdrops, trapped focus, or hidden active controls. |
| Capacity and responsive layout | Realistic short/long content, forms, zoom, narrow viewport, virtual keyboard, and internal scroll remain usable. |
| Touch and keyboard | Tooltip alternatives, trigger, actions, dismissal, and focus are available without hover or pointer-only behavior. |
| Routes and drafts | Full-page tasks support direct entry/refresh and correct Back/Up; save/cancel preserves the established draft contract. |
| Assistive technology | Status is announced without unnecessary focus movement; alerts are not repeatedly assertive; dialogs have names and meaningful focus order. |

## Primary References

- [W3C APG: Tooltip](https://www.w3.org/WAI/ARIA/apg/patterns/tooltip/): focus/hover behavior, Escape, and non-interactive tooltip content. This pattern is marked work in progress in APG.
- [W3C APG: Alert](https://www.w3.org/WAI/ARIA/apg/patterns/alert/): non-focus-stealing important status and risks of alerts that disappear too quickly or occur too often.
- [W3C APG: Modal Dialog](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/): focus and background behavior for genuinely modal dialogs.
- [W3C: Status Messages](https://www.w3.org/WAI/WCAG22/Understanding/status-messages.html): programmatically exposing results, waiting, progress, and errors without unnecessary context changes.

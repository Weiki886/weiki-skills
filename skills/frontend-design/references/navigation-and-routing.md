# Navigation and Routing Refinement

Use when users lose their place, cannot return reliably, or confuse routes with drawers and overlays. Applies to apps, web, mobile web, and administration tools; choose modules that fit the platform rather than adding every navigation control.

Contents: [Scope](#scope-and-navigation-contract) · [Diagnosis](#diagnose-before-editing) · [Eight modules](#eight-navigation-modules) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Scope and Navigation Contract

For choosing tooltip/status/alert/dialog/confirmation/drawer/page patterns by interruption and capacity, read [overlay-selection.md](overlay-selection.md). This document remains the shared route, history, focus, and dismissal contract.

Preserve business content, existing destinations, information architecture, and task flow. Add orientation and correct broken navigation within the requested scope; do not invent sections to fill a tab bar, replace the router, or introduce a second navigation framework. A full deep-link feature may require hosting or native configuration beyond a UI adjustment; identify that boundary instead of claiming completion from frontend changes alone.

Inspect the current router, route definitions, platform navigation, and design system. Record a short contract for affected destinations:

| Destination | Container | Selected section / parent | Entry and exit behavior | State to preserve |
| --- | --- | --- | --- | --- |
| Existing route or view | Full page / modal / non-modal overlay | Existing hierarchy | In-app entry, direct entry, Back, Up, close | Relevant filters, scroll, selection, draft |

Distinguish these actions:

- **Back:** return through actual navigation history. It can differ from the logical parent.
- **Up / parent:** navigate to a defined ancestor in the product hierarchy, even after direct entry.
- **Close / cancel:** dismiss a transient container and restore the underlying context according to its contract.
- **Primary-section switch:** select a top-level destination; preserve or reset its nested state according to the existing product policy.

For direct entry with no known in-app predecessor, provide an explicit parent/root action where applicable. Do not fabricate visited pages, repeatedly push the same route, or assume `history.length` proves the previous entry belongs to the app. Keep browser/system Back under platform control. Do not intercept it globally to force users to stay in the product.

A screenshot can reveal missing cues but cannot prove history, focus, gestures, authorization, or deep-link behavior. With screenshots only, deliver proposed contracts and label runtime checks unverified.

## Diagnose Before Editing

- Primary-section identity disappears unexpectedly on child pages.
- Selected state is missing, stale, color-only, or disagrees with the route.
- Back is unavailable, loops, exits unexpectedly, or silently discards a draft.
- Drawers, dialogs, and pages use conflicting dismissal/history behavior.
- Nested views lack a useful parent or hierarchy cue.
- Focus, scroll, filters, or selection are lost after returning.
- Transitions conceal content, block actions, or contradict navigation direction.
- Direct links, refresh, login return, or invalid targets lead to the wrong view.

Do not infer a defect from an absent tab bar, breadcrumb, animation, or custom gesture alone. Judge whether the actual user can locate themselves, finish the task, and recover.

## Eight Navigation Modules

Modules are independent. Resolve route/history and accessibility failures before decorative transitions; visual cleanup is not a prerequisite for usable navigation.

### 1. Primary Navigation / Bottom Bar

**Goal and rationale:** Keep top-level destinations recognizable and reachable without overwhelming content.

**Rules:** Reuse the existing primary navigation. A bottom bar is suitable for a small set of mobile peer destinations (often 3-5); web or dense tools may use a header or sidebar instead. Never invent destinations to meet a count. Use short labels and consistent icons; define whether deeper pages keep or hide the bar according to the existing shell and task. Preserve selected-section context either way. Account for safe areas and content padding so fixed controls do not cover content or focus.

**Prompt:**

```text
Improve primary navigation using only the existing top-level destinations and platform conventions. Use a bottom bar only where appropriate for the mobile shell; otherwise retain the established header or sidebar. Keep labels and icons consistent, define visibility on child pages, and prevent content overlap. Do not add destinations or change information architecture.
```

### 2. Current Destination and Selected State

**Goal and rationale:** Make the current location perceptible and consistent with actual route state.

**Rules:** Derive selection from resolved routes, including nested routes and direct entry, rather than a click-only variable or ambiguous string-prefix match. Distinguish the current page from its containing section. Use more than color alone and keep keyboard focus distinct from selection. For web route navigation, use links and applicable `aria-current`; do not apply ARIA tabs merely because navigation looks like tabs. Reserve tab semantics and keyboard behavior for an actual tabbed interface.

**Prompt:**

```text
Make the current route and containing section clear using the existing visual system. Derive active state from routing, including child routes, refresh, and direct links. Provide a non-color cue and appropriate accessible current/selected semantics. Keep focus styling distinct and preserve existing navigation behavior.
```

### 3. Sidebar and Drawer Navigation

For sidebar grouping, nested disclosure, compact icon mode, utility placement, and long labels, use [sidebar-navigation.md](sidebar-navigation.md). Keep the route and overlay contracts in this document as the shared foundation.

**Goal and rationale:** Make secondary destinations discoverable without unnecessarily occupying content space.

**Rules:** Keep a persistent desktop sidebar non-modal. If a mobile drawer overlays the page as a modal, provide a named trigger with expanded state, a clear close action, focus containment, background inactivity, and focus return. Support Escape and backdrop dismissal where appropriate. Platform Back may dismiss the top drawer before navigating when that is the established platform contract; avoid duplicate handlers. Do not require a swipe to open or close it. Selecting a destination should update the route and close a transient drawer; persistent sidebars remain available.

**Prompt:**

```text
Normalize the existing sidebar or responsive drawer. Provide an accessible trigger, active destination, and explicit dismissal. Treat an overlay drawer as modal with managed focus and background interaction; keep a persistent sidebar non-modal. Close transient navigation after selection and preserve route state. Do not add a drawer if existing navigation already serves the task.
```

### 4. Back Actions and Mobile Gestures

**Goal and rationale:** Provide reliable recovery through the platform's expected controls, without making gestures the only path.

**Rules:** Follow the Back/Up distinction above. Use native or router-supported back behavior first. Do not implement a global right-swipe handler on the web or override browser edge gestures, horizontal scrolling, carousels, text selection, or OS navigation. Add custom gestures only when the target runtime and request warrant them; account for locale direction. A visible back/parent/close action must remain available where needed. Within the app root, do not synthesize an extra parent, while preserving browser/system Back and normal app exit. Protect unsaved work according to the existing policy.

**Prompt:**

```text
Make return navigation reliable through existing platform and router APIs. Define Back, parent, and close separately, including direct entry and unsaved work. Keep an accessible visible return action. Preserve native gestures and avoid custom swipe handlers that conflict with scrolling or OS/browser Back; add a gesture only if supported and in scope.
```

### 5. Breadcrumbs and Hierarchy Cues

**Goal and rationale:** Help users understand and jump to ancestors in genuinely nested content.

**Rules:** Breadcrumbs show logical ancestry, not click history. Add them for useful web/admin depth, not every shallow mobile screen. Use an accessible navigation label, consistent decorative separators, links for available ancestors, and non-link text for the current page with appropriate current-page semantics. Derive labels and parents from the existing hierarchy. If collapsing intermediate levels, keep them keyboard accessible and identifiable. Do not expose restricted ancestor names or create fictitious parent routes.

**Prompt:**

```text
Add or repair hierarchy cues only where depth makes them useful. Derive breadcrumb ancestors from the existing route/content model, link accessible parents, and show the current page as non-link text. Keep separators consistent and collapsed ancestors accessible. Preserve permissions and do not invent parent pages.
```

### 6. Dialogs, Popovers, Sheets, and History

For distinguishing value selectors, command menus, and navigation disclosures, read [select-and-dropdown.md](select-and-dropdown.md). This section remains the shared overlay lifecycle contract.

**Goal and rationale:** Make transient containers distinguishable from independent pages and restore context reliably.

**Rules:** Classify each overlay instead of applying one dismissal rule to all. Modal dialogs need an accessible name, sensible initial focus, contained focus while open, inactive background, an accessible close/cancel path, and appropriate focus restoration. Non-modal popovers must not trap focus or mark the entire background inert. Use Escape/backdrop dismissal only when safe for that component; do not silently discard edits or trigger destructive actions. Coordinate nested overlays so one close action dismisses only the intended top layer.

Ordinary transient overlays do not need a route entry. A route-backed detail modal or shareable sheet may intentionally use history. Preserve that contract: in-app open/close must not create loops, direct entry needs a stable fallback, and browser Back/Forward must match the URL. Never impose either "all overlays enter history" or "no overlay enters history."

**Prompt:**

```text
Classify existing dialogs, sheets, and popovers as modal, non-modal, transient, or route-backed. Normalize naming, focus, background interaction, safe dismissal, and context restoration for each type. Preserve unsaved-work protection. Keep ordinary transient overlays local, and maintain deliberate URL/history behavior for route-backed ones. Verify top-layer close, direct entry, and browser Back/Forward.
```

### 7. Route Transitions and Feedback

**Goal and rationale:** Clarify a change of context without delaying interaction or creating disorientation.

**Rules:** Motion is optional. Reuse platform transitions; prefer immediate, clear feedback over adding animation everywhere. Peer switching can be instant or use a restrained fade; hierarchical motion, when used, must reverse consistently on Back. For a left-to-right push where the new page enters from the right, a matching pop returns it toward the right; do not hard-code a contradictory exit direction. Adapt to locale/platform conventions. Honor reduced motion, provide loading feedback when needed, and keep route titles, focus, and announcements meaningful without animation. Avoid a new animation dependency solely for navigation polish.

**Prompt:**

```text
Make route changes perceptible using the existing platform transitions or concise loading/state feedback. If motion is useful, distinguish peer switches from hierarchical pushes and reverse the same spatial relationship on Back. Respect locale direction and reduced motion. Do not delay controls, hide focus, or require animation to understand navigation.
```

### 8. Deep Links and Direct Entry

**Goal and rationale:** Let supported destinations open independently through a URL, notification, bookmark, or share action with correct context.

**Rules:** Reuse existing canonical routes and parameter validation. On web, check direct load/refresh and hosting fallback; on native platforms, check link registration and handling when in scope. Derive title, active section, and parent action from the target. Preserve valid query/filter state and define direct-entry Back/Up behavior without manufacturing visited history. Apply authentication and authorization to direct entry; after login, resume only a validated internal destination, never an arbitrary redirect. Handle missing, expired, malformed, and forbidden targets clearly without leaking protected data. Do not place secrets in shareable URLs. A screenshot or a successful in-app click does not prove deep linking works.

**Prompt:**

```text
Verify supported deep links using the existing routing system. Direct entry and refresh must resolve the correct destination, parameters, title, active section, and parent action. Preserve authentication, authorization, and safe login return. Handle invalid or unavailable targets clearly, and verify hosting/native link prerequisites where in scope. Do not invent routes or browser history, expose secrets in URLs, or claim unsupported platform behavior is complete.
```

## Combined Prompt

Apply only modules relevant to the requested navigation change:

```text
Normalize navigation for this existing interface while preserving business content, destinations, information architecture, and task flow. First map routes, containers, active sections, logical parents, history behavior, and state restoration. Use existing platform/router APIs. Make current location and primary navigation clear without adding unnecessary tabs, drawers, or breadcrumbs. Distinguish Back through history, Up to a logical parent, and Close for overlays. Preserve native gestures and accessible visible alternatives. Manage modal focus and background interaction; keep non-modal overlays non-modal and protect unsaved work. Preserve intentional route-backed overlays and avoid duplicate history entries or loops. Use optional restrained, reversible transitions with reduced-motion support. Verify supported deep links, refresh, safe login return, authorization, and invalid targets. Test actual interactions and report unsupported or unverified platform behavior. Do not add unrelated modules or change the router or business logic.
```

## Acceptance and Regression Checks

Mark each applicable scenario pass/fail/unverified; explain N/A instead of forcing all eight modules into every product.

| Scenario | Expected evidence |
| --- | --- |
| Normal hierarchy | Enter a child, use Back and parent actions separately, and confirm correct route, selected section, and preserved list/filter/scroll state. |
| Browser/system navigation | Exercise Back/Forward or the supported system equivalent, repeated navigation, root behavior, and rapid actions without loops or duplicate entries. |
| Direct entry and refresh | Open a supported deep URL in a fresh session, refresh, and verify correct title, navigation context, and parent fallback. |
| Authentication and bad targets | Verify intended login return, forbidden/missing/malformed routes, and safe redirect handling without protected-content exposure. |
| Drawers and overlays | Open, select, close, Escape, and applicable backdrop/system Back; check top-layer handling, draft protection, and focus restoration. Include route-backed direct entry where used. |
| Keyboard and accessibility | Verify current-location semantics, labels, predictable focus, modal containment, non-modal escape, and no lost or obscured focus after navigation. |
| Layout and motion | Check relevant narrow/wide layouts, safe areas, long labels, and reduced motion; check RTL when supported. Native gestures require the target device/runtime. |

Use the bounded verification process in [verification.md](verification.md). Prefer focused interaction tests in the existing test framework when navigation state changes; CSS inspection alone is insufficient. Report actual routes, viewports, input methods, commands, outcomes, and unverified environments. External operations such as production hosting changes remain outside a local UI refinement unless authorized.

## Primary References

- [Android navigation principles](https://developer.android.com/guide/navigation/principles): Back, Up, back stacks, and direct-entry behavior; adapt to the target platform rather than copying Android rules to every web app.
- [WAI-ARIA modal dialog pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/): modal semantics, keyboard behavior, and focus management.
- [WAI-ARIA breadcrumb pattern](https://www.w3.org/WAI/ARIA/apg/patterns/breadcrumb/): ancestor navigation and current-location semantics.
- [MDN: Working with the History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API/Working_with_the_History_API): history entries and navigation events; use the application's router abstraction where present.
- [MDN: prefers-reduced-motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion): respect user motion preferences for optional transitions.

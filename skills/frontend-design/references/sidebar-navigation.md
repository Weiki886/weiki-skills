# Sidebar Navigation

Use for crowded or inconsistent side navigation in administration tools, SaaS workspaces, dashboards, and consoles. Improve grouping, orientation, density, and responsive access while retaining every destination available to the current user.

Contents: [Contract](#establish-the-sidebar-contract) · [Diagnosis](#diagnose-before-editing) · [Eight modules](#eight-sidebar-modules) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Sidebar Contract

Preserve existing destinations, route URLs, permissions, action side effects, and task flows. Presentation grouping and placement can change within this refinement; do not silently change route hierarchy, rename business concepts, remove entries, or expose unauthorized destinations. Preserve the existing permission-filtering policy: retaining entries is not permission to reveal hidden features. A disabled item is not an authorization control.

Inventory affected entries with stable identity, label, destination/action, parent/group, visibility rule, current-route matching, and proposed placement. Distinguish a parent destination from a disclosure button. Use existing navigation configuration and components rather than duplicating a route/permission registry. Base frequency claims on known workflows or evidence; do not invent usage metrics.

For navigation semantics, history, transient drawers, dismissal, and focus, follow [navigation-and-routing.md](navigation-and-routing.md). This reference adds sidebar organization and compact-mode decisions, not a second router or overlay system. Use [layout-and-spacing.md](layout-and-spacing.md) for width, spacing, density, safe areas, and reflow. Reuse the current icon set, theme, and tokens instead of imposing line icons or an 8px scale.

Use a named navigation landmark, structured lists, links for destinations, and buttons for disclosure/actions. Ordinary navigation does not require ARIA `menu` or `tree` roles; use a composite widget only when its full semantics and keyboard behavior fit the existing interaction. Include an existing skip-to-content path or equivalent efficient access when repeated navigation precedes the main task.

## Diagnose Before Editing

- Unrelated entries are flat or grouped under unclear headings.
- Active state does not match direct entry, child routes, refresh, or browser history.
- Every branch opens at once, or a collapsed parent hides all location cues.
- Utility controls compete with primary tasks or cover the scrollable navigation.
- Icons, labels, spacing, and default/hover/focus/current states conflict.
- Compact mode makes nested links, labels, or the expansion control unreachable.
- Hover tooltips are the only way to identify truncated or icon-only items.
- Long labels, zoom, short windows, or narrow screens clip destinations or focus.

No grouping, icon rail, footer pinning, or search is mandatory for a small sidebar. Evaluate whether the current user can find and reach all permitted entries.

## Eight Sidebar Modules

### 1. Business-Oriented Groups

**Goal and rationale:** Make related destinations easier to scan as coherent categories.

**Rules:** Group by the existing business workflow or domain, not arbitrary visual symmetry. Use short, descriptive group headings with reduced visual emphasis but readable contrast. Separate groups with consistent whitespace; avoid unnecessary cards and heavy dividers. Preserve entry identity and destinations when moving items. If a sidebar is already short and clear, grouping may add needless noise. Do not change accessible names to abbreviations merely to fit.

**Prompt:**

```text
Group existing permitted sidebar entries by the product's business workflow, retaining their labels, destinations, and permissions. Use readable, visually secondary group headings and consistent whitespace. Avoid arbitrary groups, extra wrappers, and unnecessary categorization of a short navigation list.
```

### 2. Current Route and Ancestors

**Goal and rationale:** Keep location accurate after every supported entry path.

**Rules:** Derive current state from the existing router/route metadata, not the last clicked item or ambiguous string prefixes. Match nested, parameterized, and overlapping paths deliberately; preserve query/hash semantics where meaningful. Mark the actual current link appropriately, for example `aria-current="page"`, and distinguish ancestor context from a second current page. On route entry/change, reveal the active branch and keep the active item discoverable without stealing focus. Reconcile manual collapse and remembered state without reopening a deliberately closed branch on every render. If the active child is hidden in compact/collapsed mode, retain a visible ancestor cue and an accessible way to reveal it. Focus indication must remain distinct from current state.

**Prompt:**

```text
Derive sidebar current state from the existing router, including direct links, nested routes, refresh, and Back/Forward. Reveal the active branch on navigation, distinguish current links from ancestors, and keep focus styling separate. Preserve deliberate disclosure choices without losing location cues or repeatedly forcing a branch open.
```

### 3. Primary and Utility Regions

**Goal and rationale:** Keep routine business work prominent and utility actions predictable.

**Rules:** Place primary destinations and account/help/settings utilities according to the actual workflow. Treat bottom utility placement as a useful pattern, not a claim that settings are always low frequency. Use a flexible column with a scrollable navigation region and a separate utility region when appropriate; on short viewports, all entries and actions must remain reachable. Avoid absolute/fixed positioning that overlaps the list or hides focus. Preserve logout and other command behavior as actions rather than converting them to navigation links. Do not add missing utilities or reorder routes outside scope.

**Prompt:**

```text
Separate primary navigation from existing account, help, or settings utilities according to the actual workflow. Keep utilities predictably placed without overlapping the list; ensure every entry remains reachable in short windows and at zoom. Preserve action versus link semantics and add no new destinations.
```

### 4. Nested Disclosure

**Goal and rationale:** Limit initial density while keeping child destinations easy to reveal.

**Rules:** Reuse disclosure controls with accurate expanded state and associated child lists. Close inactive branches initially only where this improves the task; the active-route branch and explicit user choices need deliberate handling. A parent that navigates and expands needs distinct, labeled controls or an established unambiguous interaction; one click must not trigger both unexpectedly. Hidden descendants must leave the tab sequence. If collapse would hide focus, move it to the owning trigger. Do not invent deeper nesting. Remember expansion only under the existing state/storage policy, using stable IDs and appropriate account/workspace scope; do not persist private navigation data by default. Respect reduced motion and supported writing direction for chevrons.

**Prompt:**

```text
Normalize nested sidebar disclosure with labeled controls, accurate expanded state, and predictable keyboard behavior. Keep active-route context discoverable, preserve intentional user choices, and remove hidden descendants from the tab path. Separate parent navigation from expansion when both exist; reuse the existing persistence policy and honor reduced motion.
```

### 5. Consistent Icons and Spacing

**Goal and rationale:** Support recognition without creating a competing visual system.

**Rules:** Reuse the project's icon style, sizing, alignment, and spacing tokens. Line icons are an option, not a reason to replace a coherent filled-icon system. Keep text primary when icons are unfamiliar; decorative icons should not duplicate accessible names. Ensure badge/count/status meaning remains available when the sidebar changes size. Do not infer live status or invent badges from styling alone.

**Prompt:**

```text
Normalize sidebar icons, alignment, and icon-to-label spacing using the existing design system. Retain readable labels, avoid redundant spoken icon names, and keep real badge/status meaning available in all modes. Do not add icons or replace a coherent icon family just for variety.
```

### 6. Compact Rail and Responsive Drawer

**Goal and rationale:** Reclaim workspace width without losing destination access.

**Rules:** Add a compact icon mode only when requested or useful within scope. Give the expand/collapse toggle a stable, discoverable place, accessible name, and state; retain keyboard access after switching. Every icon needs a meaningful accessible name plus a way for sighted keyboard/touch users to discover the full label. Hover-only tooltips are insufficient; provide focus hints and an explicit expansion or labeled navigation path for touch. Nested destinations need accessible disclosure/flyout behavior, not labels with no route access. Keep focus stable when controls disappear. Persist mode only through the existing policy. At narrow widths use the established responsive drawer/alternative; never silently replace the destination set with a few bottom tabs. Modal drawer behavior belongs to [navigation-and-routing.md](navigation-and-routing.md).

**Prompt:**

```text
Provide compact sidebar mode only where it serves this workspace. Keep the toggle, full destination labels, nested links, and status cues accessible by keyboard and touch as well as pointer. Preserve focus and the existing preference policy. On narrow screens reuse the navigation drawer contract without dropping entries or introducing a new navigation system.
```

### 7. Default, Hover, Focus, Current, and Unavailable States

**Goal and rationale:** Explain interaction and availability without conflicting signals.

**Rules:** Use coherent theme tokens and non-color cues where needed; a left accent bar and changed text color are examples, not mandatory styling. Keep current, hover, expanded, and keyboard focus distinguishable. For unavailable entries, follow existing visibility and discoverability policy; explain why and how to proceed when appropriate. `aria-disabled` or a forbidden cursor alone does not prevent activation: enforce disabled behavior for pointer and keyboard while preserving backend authorization. Do not reveal restricted destinations or apply a broad opacity reduction that makes necessary text unreadable. Do not use a tooltip as the only reason/action explanation.

**Prompt:**

```text
Differentiate normal, hover, keyboard-focus, current, expanded, and unavailable navigation states with the existing theme. Enforce unavailable behavior for every input method and explain it where appropriate. Preserve permission filtering; disabled appearance is not authorization and must not reveal hidden destinations.
```

### 8. Long Labels and Full-Name Access

**Goal and rationale:** Keep navigation legible at constrained widths without making labels ambiguous.

**Rules:** Prefer enough width or controlled wrapping when appropriate; uniform one-line rows are not mandatory. If truncation is needed, retain the full accessible name and offer full visible text on focus as well as hover, with a touch-accessible expanded view. Do not rely on the HTML `title` attribute alone. Keep different destinations distinguishable when their shortened labels share a prefix. Test translations, zoom, badges, and compact mode without clipping hit targets or focus. Tooltips contain descriptions only; interactive child links need a suitable popup pattern.

**Prompt:**

```text
Handle long sidebar labels with appropriate width, wrapping, or deliberate truncation. Preserve full accessible names and provide full visible labels through keyboard, pointer, and touch paths. Keep similar labels distinguishable and verify translations, zoom, badges, and focus without relying only on hover or title text.
```

## Combined Prompt

Apply the relevant modules incrementally; do not add every control to an already clear sidebar.

```text
Refine the existing sidebar while retaining every destination permitted to the current user, its URL, and its action/permission contract. Group entries by the existing business workflow; separate primary and utility regions without overlap. Derive current state from routing, reveal the active branch on navigation, and preserve manual disclosure choices. Reuse accessible links, disclosure buttons, icons, theme, and spacing tokens. Add compact mode only where useful, keeping full names and nested destinations accessible by keyboard and touch rather than hover alone. Distinguish focus, current, expanded, and unavailable states, and handle long labels without lost meaning. Reuse existing state persistence and responsive drawer behavior; add no search, routes, permission exposure, or storage just to fill the checklist. Verify all entries remain reachable after direct entry, refresh, Back/Forward, resize, zoom, account changes, and short-window scrolling.
```

## Acceptance and Regression Checks

Use the existing project tests and [verification.md](verification.md). Mark relevant cases pass/fail/unverified and explain N/A. A screenshot does not prove entry reachability, route matching, permissions, or keyboard behavior.

| Scenario | Expected evidence |
| --- | --- |
| Entry inventory | All previously permitted links/actions remain reachable with unchanged destinations and effects; permission-hidden entries remain hidden. |
| Route matching | Direct entry, refresh, Back/Forward, nested/overlapping routes, and relevant query/hash changes produce correct current/ancestor cues. |
| Disclosure | Route entry reveals context; manual toggles work; hidden descendants cannot receive focus; parent navigation and expansion do not conflict. |
| Compact and drawer modes | Toggle, names, badges, and nested links remain accessible; keyboard/touch paths work; modal focus/dismissal follows the navigation contract. |
| Layout and labels | Long/translated labels, short windows, narrow widths, zoom, and supported RTL keep every entry and focus indicator reachable without overlap. |
| Interaction states | Hover, focus, current, expanded, and unavailable states are distinct; disabled activation is prevented across input methods. |
| Context changes | Existing preference handling survives reload as specified; account/workspace/permission changes reconcile entries and stale stored state safely. |
| Main-task access | Repeated navigation does not obstruct reaching content; selection and drawer close leave focus in the expected place. |

Menu search is an optional product capability, not a required cleanup step. Add it only when requested or already in scope, and preserve the same permission and route model.

## Primary Reference

- [W3C APG: Disclosure Navigation Example](https://www.w3.org/WAI/ARIA/apg/patterns/disclosure/examples/disclosure-navigation/): navigation landmarks, links, nested lists, disclosure buttons, and current-page semantics. This is illustrative guidance, not a production component to copy without target-browser and assistive-technology verification.

# Select and Dropdown Patterns

Use when a design confuses value selection with commands, uses a popup for every small choice, or makes long option lists hard to search and operate. This reference covers form selects, searchable choices, action menus, and their popup placement.

Contents: [Contract](#establish-the-component-contract) · [Diagnosis](#diagnose-before-editing) · [Five rules](#five-selection-rules) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Component Contract

First identify what the user is doing:

| Pattern | User intent | Selection/activation result |
| --- | --- | --- |
| Select / select-only combobox | Choose a value for a field or filter | Value is selected, shown in the control, and committed according to the form/filter contract. |
| Editable combobox / autocomplete | Enter text, optionally choosing a suggestion | Text may be free-form or constrained; keep displayed text and submitted identifier/value synchronized. |
| Radio/checkbox group | Choose from a small visible set | Value(s) change in place; use for mutually exclusive or independent options. |
| Action menu | Choose a command such as Edit or Export | The command runs; the menu does not become a value-bearing field. |
| Navigation disclosure | Reveal or visit destinations | Links navigate; disclosure controls expand/collapse. This is not a command menu or select. |

Preserve form submission timing, filter application, query parameters, defaults, clear behavior, destructive-action confirmation, permissions, and submitted values. Do not style alone as semantics: two components may share a popup surface while retaining distinct controls and outcomes. Reuse existing select, combobox, menu, listbox, dialog, and sheet components. Read [forms-and-validation.md](forms-and-validation.md) for field contracts, [navigation-and-routing.md](navigation-and-routing.md) for overlays/navigation, and [sidebar-navigation.md](sidebar-navigation.md) for navigation groups.

## Diagnose Before Editing

- A row action looks like a form field or retains a misleading selected value.
- A field commits immediately when the surrounding form expects an explicit Apply/Save.
- A simple two-way or small choice is hidden behind an extra popup interaction.
- Many options require excessive scrolling, or a search field is added to an already short list.
- Users can type arbitrary text when only allowed values should be submitted, or vice versa.
- The list is clipped by viewport edges or covers its trigger/keyboard focus.
- Mobile long lists are difficult to operate in a tiny anchored popup.
- A custom list uses visual arrows but omits focus, typeahead, selection, Escape, or focus-return behavior.

Option counts are heuristics, not universal breakpoints. Choose by label length, available width, decision frequency, density, locale, input method, and existing product patterns. Keep multi-select distinct from a radio group; a small multi-select may still need a compact list or popup.

## Five Selection Rules

### 1. Separate Value Selection from Commands

**Goal and rationale:** Make it clear whether the user is setting data or executing an action.

**Rules:** A Select/combobox has a persistent field label/value and follows field/filter commit rules. An action menu is opened by a button and presents commands; activating a command runs it and does not leave an option selected in the trigger unless the product has a separate persistent mode/state. Use clear labels such as “Status” for a value field and “More actions” for a command trigger. Visual cues may support the distinction but cannot replace semantics. Destructive commands follow the existing confirmation/undo contract. For navigation links, use the navigation disclosure pattern, not action-menu semantics.

**Prompt:**

```text
Separate value selection from commands. Keep Select/combobox as a labeled field whose value is displayed and committed by the existing form/filter contract. Use a button-triggered action menu for commands that execute immediately; do not show a misleading selected command in the trigger. Preserve destructive-action confirmation and navigation semantics.
```

### 2. Inline Small Choice Sets When They Help

**Goal and rationale:** Avoid an unnecessary open-then-select interaction for a small, easily scanned set.

**Rules:** For two simple mutually exclusive values, consider radios or a segmented control if space, semantics, and product density allow. For roughly three or four short options, consider a visible group. These are starting heuristics only: long labels, narrow layouts, frequent forms, or dense toolbars may justify a Select. Do not use a segmented control when labels wrap badly or the choice is not a small set of peer modes. For multiple independent values use checkboxes, not radios. Preserve defaults and avoid adding an accidental preselection.

**Prompt:**

```text
For a small visible option set, consider inline radio/checkbox or segmented controls according to single-versus-multiple selection semantics. Two mutually exclusive short values may use radios; three or four can stay visible when they fit. Keep Select when space, label length, density, or the product pattern makes it clearer. Preserve defaults and submitted values.
```

### 3. Search and Bound Long Option Collections

**Goal and rationale:** Keep large collections navigable without making short lists slower.

**Rules:** Give popup lists a sensible viewport-aware maximum height and internal scrolling. Add search when the collection, labels, or user task makes scanning inefficient; around eight or more choices may prompt evaluation, not an automatic rule. Native select/listbox typeahead may already suffice. If filtering is provided, preserve keyboard focus and explain no matches separately from loading/error. Debounce or bound remote queries as appropriate, honor composition input, ignore stale responses, and do not issue duplicate requests. Define whether the query clears on close/reopen. For multi-select, state whether chosen values remain visible while filtering and provide a clear way to review/clear them. Keep long option names concise and meaningful to assistive technology.

**Prompt:**

```text
Keep the option popup within available viewport height and scroll internally. Add search only when the collection and task benefit; treat eight options as a prompt to assess, not a hard threshold. Preserve typeahead/keyboard focus, distinguish no matches from failure, support IME input, and keep remote filtering bounded with stale results ignored. Define selected-value visibility and clearing for multi-select.
```

### 4. Place Popups in Available Space

**Goal and rationale:** Keep options and commands visible and reachable near their trigger.

**Rules:** Prefer a mature positioning primitive that handles collision, flipping, resizing, scroll containers, zoom, and visual viewport changes. Open above or below according to actual available space and menu dimensions; do not assume fixed downward placement. Keep a useful relationship to the trigger and avoid covering it when alternatives exist. Do not change logical option order solely because the panel opens upward. For small mobile choice sets, a native select or accessible anchored popup may work; for a long list, a dialog/bottom sheet can provide room for search and touch targets when it matches platform conventions. A sheet is not mandatory: preserve focus, explicit dismissal, Back/Escape, safe areas, and scroll locking according to the existing overlay contract. Reposition on virtual keyboard and viewport resize.

**Prompt:**

```text
Position the existing popup using its collision-aware primitive, choosing above or below from real available space and keeping the trigger/context visible. Preserve option order. On mobile, choose native/anchored or a modal sheet by list length and platform convention; keep keyboard, touch, safe-area, resize, Back/Escape, focus, and scroll behavior correct.
```

### 5. Keep Selection and Menu Interaction Accessible

**Goal and rationale:** Make each pattern predictable across keyboard, pointer, touch, and assistive technology.

**Rules:** Use native select where it meets the task; use a select-only combobox/listbox or editable combobox only when their distinct behaviors are implemented. Support the relevant opening, typeahead/search, Arrow, Home/End as appropriate, selection, Escape, and focus-return contracts. A listbox option should not contain nested interactive buttons or links. Action menus use a correctly named button with expanded state and the expected menu keyboard/close behavior; ordinary form option lists must not receive `menu` semantics. Keep hover, current selection, and keyboard focus visually distinct and not color-only. Clicking outside closes only where appropriate and must not discard an uncommitted value unexpectedly. Disabled commands must not activate by keyboard or pointer; do not reveal actions the user is not authorized to see.

**Prompt:**

```text
Use the semantic pattern that matches the task: native/select-only or editable combobox for values, listbox options for choices, menu button for commands, and navigation links/disclosures for destinations. Implement the corresponding keyboard, focus, dismissal, and selected/expanded states. Do not give a value list menu semantics or nest interactive controls in listbox options.
```

## Combined Prompt

Apply the relevant rules and keep numeric thresholds contextual.

```text
Refine this popup component by identifying whether it selects a value, accepts editable text/suggestions, executes a command, or reveals navigation. Preserve values, commit timing, query behavior, permissions, and destructive-action safeguards. Prefer visible controls for small choices when they fit; use search and a bounded internal scroller when a long collection needs them. Position above or below based on real viewport space, and choose a mobile sheet only when list length and platform conventions justify it. Preserve native/ARIA semantics, keyboard focus, typeahead/search, IME, Escape/Back, clearing, and selection state. Do not use fixed option-count cutoffs or unify Select and command-menu behavior merely because their popups look alike.
```

## Acceptance and Regression Checks

Use [verification.md](verification.md), [forms-and-validation.md](forms-and-validation.md), and [navigation-and-routing.md](navigation-and-routing.md) as applicable. Mark relevant cases pass/fail/unverified or N/A with rationale.

| Scenario | Expected evidence |
| --- | --- |
| Intent | Value fields retain selection/commit semantics; commands execute once without becoming a selected field value; navigation remains navigation. |
| Small and large sets | Two-way, three/four-choice, long-label, multi-select, and large collections use an appropriate representation at target widths. |
| Search | Keyboard/typeahead and IME work; no-match/loading/error differ; remote filtering is bounded and stale responses do not replace newer results. |
| Position and resize | Popup flips or resizes without clipping at top/bottom, scroll containers, zoom, virtual keyboard, and viewport changes. |
| Keyboard and assistive tech | Trigger name/state, arrow/typeahead, Home/End where supported, selection, Escape, focus return, and visible focus match the chosen pattern. |
| Commit and clearing | Selected label/value, query parameters, explicit Apply, reset, reopen, multi-select visibility, and clear behavior match the existing contract. |
| Commands and permissions | Destructive confirmation, disabled behavior, single activation, and permission-hidden actions remain correct. |
| Mobile | Touch targets and sheet/anchored behavior preserve Back, dismissal, safe areas, scrolling, and no lost draft selection. |

## Primary References

- [W3C APG: Combobox](https://www.w3.org/WAI/ARIA/apg/patterns/combobox/): editable versus select-only behavior and popup interaction.
- [W3C APG: Listbox](https://www.w3.org/WAI/ARIA/apg/patterns/listbox/): single/multiple selection, keyboard support, and limitations for interactive option content.
- [W3C APG: Menu Button](https://www.w3.org/WAI/ARIA/apg/patterns/menu-button/): command-menu trigger and menu interaction semantics.

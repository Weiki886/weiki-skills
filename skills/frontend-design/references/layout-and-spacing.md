# Layout and Spacing

Use for crowded pages, inconsistent spacing, excessive line lengths, weak alignment, or layouts that fail across screen sizes. Apply to web pages, dashboards, landing pages, forms, and app surfaces while respecting their different density needs.

Contents: [Contract](#establish-the-layout-contract) · [Diagnosis](#diagnose-before-editing) · [Ten modules](#ten-layout-modules) · [Combined prompt](#combined-prompt) · [Verification](#acceptance-and-regression-checks).

## Establish the Layout Contract

Preserve business content, information architecture, actions, reading order, and existing capabilities. Improve grouping and spacing without removing information or adding empty decoration. Do not hide content below the fold, collapse sections, reorder navigation, or replace the global design system merely to make a screenshot look simpler.

Inspect existing container widths, breakpoints, grid, spacing tokens, density variants, type scale, and supported writing directions. Reuse them first. When no system exists, establish a small, task-appropriate set of container, gutter, inset, and section-gap tokens. Document necessary exceptions instead of forcing every distance onto one numeric multiple.

Values such as a 1200px content cap, twelve columns, a 4/8px spacing family, or 16px mobile gutters are examples, not universal requirements. Distinguish the page shell from a readable prose column, a form column, and a data workspace. A dense operations dashboard needs different spacing from an editorial or marketing page. Inspect the installed CSS framework and theme rather than assuming its spacing scale is based on 8px.

## Diagnose Before Editing

- Prose stretches across a wide screen without a readable measure.
- Related items are farther apart than unrelated sections, or every gap has a different value.
- Columns, labels, cards, and actions have no shared alignment anchors.
- Containers accumulate nested padding or content touches viewport edges.
- The first screen is overloaded, or oversized empty space hides the primary task.
- Fixed heights clip long text, errors, translated labels, or enlarged text.
- Mobile stacking changes reading/tab order or creates page-wide horizontal scrolling.
- Visual cleanup relies on deleting content, arbitrary spacers, or absolute positioning.

Start with container and alignment constraints, then spacing relationships. Verify representative real content, not just identical placeholder cards. Use [ai-generated-ui-refinement.md](ai-generated-ui-refinement.md) for decorative cleanup when relevant; neither workflow must precede a more urgent usability fix.

## Ten Layout Modules

### 1. Content Width and Reading Measure

**Goal and rationale:** Keep wide layouts useful and long text readable.

**Rules:** Use a fluid container capped by an appropriate maximum, with gutters that still fit narrow screens. Choose prose measure from actual font, language, and content; the page-shell width is not automatically a suitable text width. Center a bounded shell when consistent with the product; retain existing workspace alignment when needed. Full-bleed surfaces may extend beyond the shell while inner content aligns with it. Do not squeeze essential data tables into a prose column.

**Prompt:**

```text
Bound the page shell using the existing container system and responsive gutters. Give prose and forms an appropriate inner width rather than stretching text across the shell. Align full-bleed section content with nearby content while retaining necessary workspace width. Treat 1200px as an optional starting example, not a required fixed width.
```

### 2. Responsive Grid

**Goal and rationale:** Give components a shared alignment structure across available widths.

**Rules:** Reuse the existing grid or flex system. Twelve columns can help complex desktop composition; a simple stack or fewer columns can be better for a small surface. Define consistent gutters and content-driven spans. Allow children to shrink/wrap, preserve semantic DOM and keyboard order, and stack when content no longer fits. Do not adopt a new breakpoint system for one component.

**Prompt:**

```text
Align modules to the existing responsive grid or flex structure with consistent gutters. Use column spans only where they serve this content; simplify to an ordered stack at narrow widths. Preserve DOM and keyboard order and avoid rigid widths or a forced twelve-column system on small components.
```

### 3. Spacing Tokens

**Goal and rationale:** Make repeated spacing relationships predictable and maintainable.

**Rules:** Reuse semantic spacing tokens before adding numbers. If absent, choose a small scale, for example 4, 8, 16, 24, 32, and 48px, with density variants only where useful. Distinguish container padding, sibling gaps, and section spacing. Prefer parent-owned `gap` where appropriate to avoid doubled sibling margins. Borders, optical adjustments, typography, safe areas, and accessibility constraints need not all be multiples of 8px. Do not rewrite framework configuration outside the requested scope.

**Prompt:**

```text
Replace arbitrary repeated spacing with the project's tokens and clear inset, sibling-gap, and section-gap roles. If no scale exists, define a small consistent scale appropriate to the interface. Avoid doubled spacing and allow justified optical or accessibility exceptions; do not assume every distance or framework utility must be an 8px multiple.
```

### 4. First-Viewport Priorities

**Goal and rationale:** Make the page's purpose and main task discoverable without overload.

**Rules:** Prioritize by surface: a landing page may emphasize a title and action, while a dashboard may need alerts, filters, and key data immediately. Preserve information order and required status cues. Do not impose a full-height hero, arbitrary content count, or blank spacer to push secondary content down. Keep scroll affordances and useful information density; whitespace must not delay urgent work.

**Prompt:**

```text
Clarify the first viewport around this surface's actual task. Adjust spacing and emphasis without removing, hiding, or reordering business information. Keep important data, alerts, and actions discoverable; do not force a marketing hero or artificial blank area onto a working interface.
```

### 5. Container Insets

**Goal and rationale:** Separate content from its boundary without accumulating excessive padding.

**Rules:** Use consistent inset tokens for comparable cards, panels, and dialogs. Align headings, body content, and actions to shared internal anchors. Allow compact or edge-to-edge media variants where intentional. Avoid nested wrappers adding the same inset repeatedly. Size containers from content; do not force identical heights by clipping text or feedback.

**Prompt:**

```text
Normalize card, panel, and dialog insets using existing tokens. Align headings, content, and actions consistently, remove accidental nested padding, and retain intentional compact or edge-to-edge variants. Let real content determine height without clipping long copy or validation feedback.
```

### 6. Section Separation

**Goal and rationale:** Make relationships visible through proximity and consistent section gaps.

**Rules:** Usually keep related items closer than independent sections. Choose gaps by hierarchy and density rather than making every higher-level gap larger without limit. Remove redundant separators only when grouping remains understandable. Retain meaningful table rules, boundaries, and separators when whitespace alone is insufficient. Establish one owner for each section gap.

**Prompt:**

```text
Use consistent section gaps to distinguish independent modules, keeping related content closer together. Reduce redundant separators where spacing is sufficient, but retain boundaries needed for scanning or comparison. Avoid doubled margins and excessive blank space.
```

### 7. Viewport Gutters and Safe Areas

**Goal and rationale:** Keep content and controls clear of screen edges and system obstructions.

**Rules:** Reuse responsive page gutters; 16px is one possible mobile starting value. Use logical inline spacing where appropriate and account for device safe-area insets when supported. Full-bleed media can extend to edges while captions and controls retain safe placement. Check notches, fixed navigation, the virtual keyboard, and focus visibility without adding duplicate padding.

**Prompt:**

```text
Apply consistent responsive page gutters and supported safe-area insets. Keep text and controls clear of edges, notches, sticky navigation, and the virtual keyboard. Preserve intentional full-bleed media while keeping its controls accessible; avoid duplicate nested gutters.
```

### 8. Vertical Rhythm

**Goal and rationale:** Keep reading and scanning relationships stable as content changes.

**Rules:** Establish repeatable heading/body, label/control, row, and section gaps. Use coherent line height with content-driven block heights. Equalize comparable cards only if content remains complete; lists do not require identical heights when text lengths differ. Test expanded errors, wrapped buttons, missing content, and user text-spacing overrides. Avoid blank spacer elements as layout repairs.

**Prompt:**

```text
Normalize vertical spacing for headings, body copy, fields, repeated items, and sections. Use content-driven heights and readable line height, keeping comparable components consistent without truncating longer content. Verify wrapped text and expanded feedback rather than forcing every row to a fixed height.
```

### 9. Alignment Anchors

**Goal and rationale:** Give related content stable visual reference points.

**Rules:** Align prose and form content to the relevant reading-direction start. Centered display headings and end-aligned numeric columns can be appropriate; consistency is by role, not a universal left-align rule. Align controls, text baselines, and action groups deliberately. Use logical properties where appropriate for supported RTL locales. Do not use visual reordering that conflicts with reading or focus order.

**Prompt:**

```text
Establish consistent alignment anchors for equivalent headings, content, controls, and actions. Use start alignment for reading content and purposeful exceptions such as numeric-column alignment. Respect supported writing directions and preserve semantic reading and focus order.
```

### 10. Hierarchy Through Whitespace

**Goal and rationale:** Reinforce priority and grouping with spacing as well as typography.

**Rules:** Give primary content enough separation to be recognizable, while keeping labels with controls and actions near the content they affect. Pair spacing with type, contrast, and placement; a larger empty area does not automatically make an item more important. Retain the density necessary for comparison and task completion. Do not add cards, arbitrary gaps, or decorative sections just to create hierarchy.

**Prompt:**

```text
Use proximity and whitespace to clarify hierarchy alongside typography and contrast. Keep related labels, controls, and actions together, separate independent groups, and preserve useful density. Avoid oversized empty areas or new decorative containers that make the task harder to reach.
```

## Combined Prompt

Use relevant modules as a scoped layout pass rather than imposing every example value.

```text
Improve layout and whitespace while preserving business content, information architecture, actions, and reading order. Reuse existing containers, breakpoints, grids, and spacing tokens. Bound page and prose widths appropriately, align modules to a responsive structure, distinguish insets from sibling and section gaps, and maintain safe viewport gutters. Clarify first-viewport priorities for this product rather than forcing a hero. Keep vertical rhythm and alignment consistent without fixed-height clipping, oversized empty spaces, or unnecessary cards. Treat 1200px, twelve columns, 8px multiples, and 16px gutters as optional examples. Verify wide and narrow widths, long/translated content, feedback states, zoom, text-spacing overrides, and focus visibility with real rendering.
```

## Acceptance and Regression Checks

Use the bounded process in [verification.md](verification.md). Report actual viewport/content variants and pass/fail/unverified results; a spacing-token audit is not proof of usable rendering.

| Scenario | Expected evidence |
| --- | --- |
| Wide screens | Shell and reading widths stay useful; data workspaces retain required comparison space. |
| Narrow widths and reflow | Content stacks without lost actions or page-wide horizontal scrolling; necessary tables/maps use a contained strategy. |
| Spacing and grouping | Repeated roles use consistent tokens, related items stay together, and nested padding does not accumulate. |
| Real content and states | Long labels, translations, missing images, empty/error/loading states, and wrapped actions remain complete. |
| Zoom and text spacing | Enlarged text and user spacing overrides cause no lost content, clipped labels, or inaccessible controls. |
| Input and navigation | DOM/tab order stays meaningful; focus, virtual keyboard, safe areas, and sticky elements do not hide the task. |
| Density and scope | Primary work remains discoverable without deleting information, forced below-fold hiding, or excessive empty space. |

## Primary References

- [W3C: Reflow](https://www.w3.org/WAI/WCAG22/Understanding/reflow.html): resizing and zoom without lost content or unnecessary two-dimensional scrolling.
- [W3C: Text Spacing](https://www.w3.org/WAI/WCAG22/Understanding/text-spacing.html): tolerate user text-spacing overrides rather than forcing one author-defined rhythm.
- [Tailwind CSS: Theme Variables](https://tailwindcss.com/docs/theme): token-driven utilities; inspect the installed version and theme before choosing spacing values.

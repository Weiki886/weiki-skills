# Refining AI-Generated UI Drafts

Use this workflow when an AI-generated or rapidly assembled first draft looks generic, over-decorated, crowded, or visually flat. It applies to web pages, mobile web, app home screens, product landing pages, and dashboards.

## Scope and Principles

This is a visual refinement workflow, not a feature redesign. Unless the user explicitly expands scope:

- Preserve every existing business capability and meaningful piece of content.
- Preserve information architecture, navigation meaning, and task flow. Do not reorder sections or move controls in a way that changes how a task works.
- Do not add features, sections, data, claims, or decorative assets.
- Prefer removing unnecessary decoration over adding new visual elements.
- Make visual choices serve the content and task; reuse the existing brand, design system, platform conventions, and accessibility requirements.
- Improve generic placeholder copy only when the product context or user provides factual replacement content. Never invent business facts. If the intended wording is unknown, retain it or flag it for confirmation.

The seven stages below are the default order for a broad cleanup. Inspect the draft first, skip stages with no relevant issue, and make changes incrementally so each stage can be reviewed. A brand system or a deliberate campaign concept may justify gradients, expressive type, multiple semantic colors, or other effects; retain them only when they have a clear user, brand, or comprehension purpose.

For a focused detail pass on surface levels, light/dark text weight, optional response reveals, numeric stability, or drag feel, use [surface-and-type-refinement.md](surface-and-type-refinement.md). These are conditional refinements, not additional mandatory stages or reasons to reintroduce heavy effects.

## Diagnose the Draft

Check for these common failure patterns before editing:

| Area | Warning signs |
| --- | --- |
| Effects | Large decorative purple/blue gradients, gradient text/borders, glows, or stacked backdrop blur without a functional reason |
| Icons | Emoji used as controls, or mixed outline, filled, and cartoon icon styles |
| Containers | Every section is a rounded card, cards are nested without hierarchy, or shadows and borders fragment the page |
| Color | Several saturated colors compete, with no stable role or semantic meaning |
| Type and hierarchy | Unrelated font families, arbitrary sizes/weights, or all content given equal emphasis |
| Copy | Generic welcome/launch language, invented sample metrics, or button labels that do not say what happens |

Do not treat a warning sign as an automatic defect. Confirm it against the product, users, brand, and actual task.

## Seven-Stage Refinement

Keep this order for a broad visual-reduction pass. After each stage, render the affected view and compare it with the previous state. Do not continue into unrelated changes if a stage already meets the goal.

### 1. Remove Unnecessary Effects

Remove decorative gradients, glow, heavy blur, and diffuse shadows when they do not communicate brand, state, depth, or interaction. Prefer a quiet, solid surface as the default canvas. Keep effects that carry real meaning or are required by the established brand; do not flatten meaningful focus, selection, elevation, or status cues.

Prompt:

```text
Remove decorative background, text, and border gradients, glow, and backdrop blur that do not communicate brand, state, or interaction. Use a calm surface treatment. Preserve all existing content, functionality, information order, focus indicators, and meaningful state cues. Do not add new decoration.
```

### 2. Consolidate the Color System

Start with role-based colors: canvas/surface, primary text, secondary text, primary action, focus, and semantic states when needed. A neutral foundation with one primary accent is a useful default, not a universal limit. Preserve established brand colors and required success, warning, error, and data-visualization colors. Check contrast; never reduce contrast to make a palette look softer.

Prompt:

```text
Consolidate colors into clear roles. Use a restrained neutral foundation and the existing brand's primary accent for important actions. Keep additional colors only where they communicate a distinct semantic state or data category. Preserve accessible contrast, all existing content, and interaction states; do not introduce decorative colors.
```

### 3. Unify the Icon System

Replace emoji used as functional or navigation icons with the project's existing icon library or established icon set. Keep icons consistent in stroke/fill, size, alignment, and role-based color. Do not add an icon dependency just for this cleanup. Preserve emoji when it is actual user-authored content or an intentional part of the brand rather than an interface glyph.

Prompt:

```text
Replace emoji used as interface icons with the project's existing consistent icon set. Match icon size, stroke or fill, alignment, and color to the design system. Preserve each icon's meaning and accessible name, and keep emoji that is actual user content. Do not add a new icon dependency or change functionality.
```

### 4. Simplify Containers and Grouping

Use spacing first, then subtle dividers, then a container where it clarifies a real relationship. Remove cards that merely wrap isolated content; avoid nested cards and unnecessary shadows. Keep containers that establish meaningful grouping, distinguish interactive tools, represent repeated records, or provide necessary contrast. Do not remove boundaries needed to understand controls or data.

Prompt:

```text
Reduce card usage where containers do not express a real grouping. Use spacing first and subtle dividers where needed; keep cards for meaningful grouped, repeated, or interactive content. Remove unnecessary nesting and heavy shadows. Preserve content order, boundaries needed for clarity, and all behavior.
```

### 5. Clarify Visual Hierarchy

Identify the user's primary task and the content that supports it. Make that content easiest to find through size, position, contrast, whitespace, and grouping; subordinate secondary information and actions without hiding or deleting them. Respect the existing information architecture and established navigation order. Do not promote a guessed action over the actual product goal.

Prompt:

```text
Clarify the visual hierarchy around the page's existing primary user task. Give its supporting content and primary action the strongest appropriate emphasis; make secondary utilities quieter without hiding or removing them. Preserve the current information architecture, task flow, all features, and content.
```

### 6. Normalize Typography and Spacing

Prefer the existing type family and tokens. If none exist, choose a readable family with the required language coverage and define a small set of roles for headings, body, and supporting/utility text. Use a consistent size, weight, and line-height scale; avoid arbitrary bolding. Keep line lengths, wrapping, and responsive behavior comfortable. A single family is a useful default, but brand or editorial roles may justify a second family.

Prompt:

```text
Normalize typography using the existing font and design tokens. Establish a compact, consistent scale for headings, body text, and supporting text, with deliberate weights and readable line heights. Preserve language coverage, all wording, responsive wrapping, and the established brand voice; do not arbitrarily bold text.
```

### 7. Make Copy Specific and Actionable

Replace empty generic labels only with accurate, context-grounded wording. Prefer action labels that describe an outcome (for example, "Continue reading from Chapter 4" when that state is known) over vague labels such as "Get started." Make errors, empty states, and status messages explain the real condition and next action. Do not fabricate user names, progress, numbers, testimonials, or product behavior. If truth cannot be established, ask or mark the copy for confirmation rather than inventing it.

Prompt:

```text
Replace generic placeholder copy only where the product context provides an accurate alternative. Make buttons describe their actual outcome, and make empty, error, and status messages specific and useful. Do not invent business facts, user data, or behavior. If accurate wording is unknown, leave it unchanged and flag it for confirmation. Keep all meaningful content.
```

## Combined Prompt

Use this only when a single iteration is appropriate; for a complex page, apply and verify one stage at a time.

```text
Refine this existing UI to feel deliberate and production-ready, without changing its functionality or information architecture. Preserve every feature, meaningful content item, navigation relationship, and task flow. First remove decorative gradients, glows, heavy blur, and shadows that have no brand, state, or interaction purpose. Consolidate colors into clear roles with a restrained neutral foundation and the existing brand accent; retain semantic colors and accessible contrast. Replace emoji used as interface icons with the existing consistent icon set, but preserve emoji that is actual user content. Reduce unnecessary cards and nested containers, using spacing and subtle dividers where they clarify grouping while keeping useful boundaries. Clarify hierarchy around the actual primary user task and visually subordinate, but do not hide, secondary actions. Normalize typography and spacing using existing tokens and readable responsive rules. Replace generic placeholder copy only when accurate product-specific wording is available; never invent data or claims. Do not add features, content, or decorative elements. After each meaningful stage, render and inspect the page for regressions.
```

## Acceptance Checklist

- [ ] Decorative effects were removed or justified by a clear brand, state, depth, or interaction role.
- [ ] Colors have stable roles, semantic states remain distinguishable, and contrast is adequate.
- [ ] Functional icons use a consistent system; actual user content was not mistaken for interface chrome.
- [ ] Containers express real grouping; unnecessary cards, nesting, and shadows are gone.
- [ ] The primary task is easy to locate without hiding secondary functions or changing task flow.
- [ ] Typography, spacing, and line lengths are coherent across relevant viewports and languages.
- [ ] Copy is specific and truthful; no invented business facts, metrics, or user data were introduced.
- [ ] All original features, meaningful content, and information architecture remain intact.
- [ ] Real rendering confirms the page has no new overflow, clipping, overlap, inaccessible states, or broken interactions.

## Iteration Guidance and Exceptions

- Prefer one stage per iteration with a screenshot or browser check after each meaningful change. If the change is small and low risk, combine adjacent stages while preserving the order.
- Tailwind, CSS modules, and plain CSS can all express these rules; follow the repository's existing implementation style.
- Product tools, reading apps, dashboards, and B2B surfaces often benefit from restrained effects and card use. Campaigns, entertainment, and brand-led experiences may need expressive color and motion; retain them when they serve the concept and remain accessible.
- If the user explicitly requests a deliberately maximal, playful, or high-fidelity style, follow that direction while preserving usability and accessibility.

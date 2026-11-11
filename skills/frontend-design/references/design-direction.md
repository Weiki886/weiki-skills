# Design Direction

## Turn the Brief into a Designable Problem

Before choosing colors or drawing a layout, establish these facts:

| Dimension | Question |
| --- | --- |
| Surface | Marketing page, workspace, reading surface, showcase, or embedded component? |
| Audience | Who uses it, in what context, and how quickly must they complete a task? |
| Job | What is the one primary task on this surface? |
| Outcome | What visible result proves the task is complete? |
| Content | Which assets/content are real, and which are still needed? |
| Constraints | What are the brand, framework, design-system, device, performance, accessibility, and compliance constraints? |
| States | How are first use, normal, empty, loading, partial failure, full failure, and insufficient permission handled? |

Make and state the smallest assumption when information is missing. Do not invent customers, metrics, reviews, or cases to make the interface look complete.

## Choose Expression by Surface

- **Persuade:** Help visitors understand and act; useful for landing, pricing, and event pages. The first screen should communicate value and action with factual content.
- **Operate:** Help users complete repeated tasks; useful for admin, dashboard, editor, and settings screens. Favor scan efficiency, stable layout, real data density, and feedback over decoration.
- **Read:** Help users understand content; useful for docs, articles, and help centers. Favor structure, reading rhythm, navigation, and long-form durability.
- **Experience:** Make the work or interaction itself the subject; useful for portfolios, galleries, and interactive showcases. The interface should recede behind the content.

Different surfaces in one product can use different modes: marketing is Persuade, in-product settings are Operate. Do not force one expression merely because both belong to the same brand.

## Define the Visual Direction

For a substantial new page or redesign, write a compact one-page direction, not a long mood board.

### Thesis

One sentence: “What visual/interaction mechanism helps which user complete which task?”

### Color

Define 4–6 role-based colors, not just a palette:

- canvas;
- primary surface;
- text;
- muted text;
- action;
- danger/success/focus, only as needed.

State the proportion of primary, accent, and neutral colors. Spreading color evenly usually erases hierarchy.

### Typography

Define 2–3 roles:

- display: brand or chapter rhythm, used sparingly;
- body: readability and preferably the project's existing font;
- utility: data, labels, code, or supporting information when needed.

Explain the relationship among roles, sizes, weights, and line height. Ground font choices in the brand, subject, content, or existing system, not a trend.

### Composition

Explain the structure in one sentence and use a minimal ASCII wireframe to compare layouts. Show only block relationships and the visual path; do not hide information-structure problems behind decoration.

State:

- The first viewport's visual entry point.
- The most important information or action.
- How sections are grouped.
- How the structure holds up for long content, narrow screens, and higher density.

### Signature

Choose one memorable element genuinely grounded in the subject: a data-visualization method, domain-specific graphic language, distinctive but usable navigation, material surface, or content-driven typographic move. It must improve comprehension or identity, not merely decorate the background.

### Risk and Restraint

State one deliberate departure from convention and why it is justified; also identify areas that stay quiet. A focused expression is clearer than making every area compete.

## Check for Template Defaults

These choices can be valid, but should not be automatic:

- Cream background, large contrast serif, and terracotta accent.
- Black background with one acid-green or vermilion accent.
- Large number, small label, and gradient glow.
- `01 / 02 / 03` numbering, monospaced eyebrow, or all-pill buttons.
- One card pattern for every section.
- Giant marketing headline while the actual product is hidden below the fold.
- The same scroll fade repeated for every section.

Run the **subject-swap test**: if the product were replaced with something unrelated, would the design choice still work? If yes, look for a subject-specific alternative or explain why the pattern is structurally right.

Do not sacrifice familiar controls, clear boundaries, or task efficiency merely to be “different.” Distinction can come from content organization, typographic detail, data presentation, or brand voice rather than an exaggerated layout.

## Compare Directions

- For a small refine, form one direction and implement it.
- For a new component or single-page build, internally compare 2–3 brief directions and choose the best fit.
- For a brand-led page or large redesign, compare at least three directions that differ in expression, structure, and signature—not only color.

Prioritize whether each direction:

1. Makes the main task easier to understand and complete.
2. Could belong specifically to this product/subject.
3. Can be fully executed within engineering, content, and device constraints.
4. Establishes hierarchy instead of making every element equally loud.

## Content and Copy

Copy is interface material:

- Headings say what the user can do or see; avoid abstract slogans.
- Use concrete action/result labels, such as “Save changes” instead of “Submit.”
- Keep an action's name consistent from initiation to completion.
- Errors explain what happened and how to fix it.
- Empty states explain why they are empty and offer a next step.
- Do not speak about the product in first person or use vague “Oops.”
- Format numbers, units, dates, currency, and language for the locale.
- Treat long, short, and user-generated copy as layout inputs, not edge cases.

Never invent social proof, customer logos, prices, safety promises, or business metrics to complete a composition.

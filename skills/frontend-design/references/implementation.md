# Frontend Implementation

## 1. Inherit Before Replacing

Find the existing implementation patterns:

- Page shell, layout, and navigation.
- Component library and state patterns.
- Design tokens and themes.
- Icons, images, and font loading.
- CSS architecture, responsive strategy, and test tools.

Prefer composing/extending existing components. Create a new primitive only when current components cannot reasonably meet the requirement; explain why reuse is insufficient.

## 2. Information and Layout

For a focused layout and whitespace pass, use [layout-and-spacing.md](layout-and-spacing.md); adapt container, grid, and spacing choices to the existing system and task density.

For task-based content containers, use [cards-and-lists.md](cards-and-lists.md); retain plain lists or tables when they serve scanning and comparison better than cards.

- Put identity and the main task before secondary information in the first viewport.
- Use type, position, contrast, whitespace, and grouping to establish hierarchy; do not add cards and borders to everything.
- Give each independent object a clear boundary. Group or nest only where a real hierarchy exists.
- Align tables, lists, and dashboards for scanning/comparison; give narrative pages reading rhythm and composition.
- Keep controls in a stable relationship to content so button positions do not drift with content length.
- Set container widths to fit content instead of stretching everything across the screen.
- Plan for long text, missing fields, very long usernames, dense numbers, and wide tables.

## 3. Visual System

For detailed surface/type polish, use [surface-and-type-refinement.md](surface-and-type-refinement.md); retain useful boundaries and evaluate theme weights and numeric stability in the actual fonts.

- Use semantic tokens or existing variables for color; avoid repeating magic values in components.
- Keep radius, shadows, borders, and surface levels restrained and consistent.
- Limit type-scale complexity; use readable line lengths and line height appropriate to the font.
- Consider tabular numerals for numeric columns, pricing, and comparisons.
- Use one icon library. Unfamiliar icon-only actions need discoverable help and an accessible name.
- In light/dark/high-contrast/brand themes, ensure scrollbars, form controls, disabled states, and focus states also work.

## 4. Accessibility Baseline

Follow stricter project rules; otherwise use WCAG 2.2 AA as an important baseline:

- Use native semantic elements: `<button>` for actions and `<a>` for navigation.
- Keep heading order continuous and landmarks clear.
- Make all interactions operable by keyboard alone.
- Keep focus visible and prevent sticky headers, modals, or overlays from obscuring it.
- Give form controls persistent labels; associate errors with fields and help users locate them after failed submit.
- Give icon-only buttons accessible names.
- Use live regions for status updates, toast messages, or asynchronous validation when appropriate.
- Provide useful alternative text for meaningful images; keep decorative content out of the accessibility tree.
- Meet text and essential UI contrast requirements.
- Pointer targets should be at least 24×24 CSS px or qualify for a WCAG 2.2 exception; follow stricter project/platform requirements.
- Do not prevent zoom with `user-scalable=no` or similar.
- Honor `prefers-reduced-motion`; provide pause/stop controls for long automatically moving content.
- Provide click/keyboard alternatives to gesture/drag interactions unless the interaction inherently requires them.

Accessibility tools detect only some issues; keyboard and real-render verification are still needed.

## 5. Forms and Interaction States

For control selection and detailed form behavior, use [forms-and-validation.md](forms-and-validation.md); apply its modules selectively rather than replacing every field with a custom component.

For Select/combobox, command menus, and choice popup placement, use [select-and-dropdown.md](select-and-dropdown.md); select semantics by outcome, not by popup appearance.

- Use appropriate input `type`, `inputmode`, and `autocomplete`.
- Do not block paste.
- Make labels clickable and associate checkbox/radio labels with their controls.
- Use placeholders only for examples, never as the only label.
- Keep validation near the field and explain how to fix errors.
- Keep submission state clear and prevent accidental duplicate submits without disabling buttons unnecessarily early.
- Dangerous actions need confirmation or an undo window.
- Give loading, disabled, hover, active, focus, selected, empty, error, and success states visible differences.
- Manage focus when opening a modal/drawer and return it to the trigger on close.
- Menus, popovers, and dialogs must not break scroll locking, keyboard navigation, or escape/back paths.

## 6. Responsiveness and Input Methods

- Design around content constraints, not only device labels.
- New pages and structural changes should be checked on desktop and narrow screens. Follow explicit device requirements; if a project explicitly supports only one class of device, state the boundary.
- For wide tables, provide a scroll container, column priority, or compression strategy; do not let the whole page drift horizontally.
- Let flex/grid children shrink and wrap; handle overflow in long-text containers.
- Consider touch target, virtual keyboard, safe area, and whether hover exists for the actual input method.
- Do not depend on runtime DOM measurement for basic layout; prefer CSS primitives.

## 7. Motion

For easing selection, entry/exit contracts, optional springs, and interruption checks, use [motion-and-easing.md](motion-and-easing.md); reuse existing motion tokens and component lifecycles.

For mobile press, ripple, control feedback, and gesture refinement, use [mobile-microinteractions.md](mobile-microinteractions.md); preserve activation/cancellation semantics and distinguish optional decoration from behavior changes.

For dependent feedback stages or coordinated summaries, use [compound-microinteractions.md](compound-microinteractions.md); keep results authoritative and sequences interruptible without locking valid operations for decoration.

For scroll-linked transforms, sticky navigation, viewport reveals, and pull feedback, use [scroll-interactions.md](scroll-interactions.md); distinguish continuous mapping from triggered playback and keep native scrolling and content access intact.

Motion must answer: “What does this help the user understand?” Examples include entry, hierarchy changes, feedback, or spatial continuity.

- A coherent motion pattern is more useful than scattered hover effects.
- Animations must be interruptible and must not block quick actions.
- Transition only properties that change; avoid `transition: all`.
- Prefer lower-cost properties such as transform/opacity.
- In reduced-motion mode, preserve content and state changes while removing unnecessary movement.
- Stop purely decorative loops for reduced motion. Motion must not keep core content hidden or be the only error/success signal.

## 8. Performance and Stability

- Give images explicit dimensions to avoid layout shift; lazy-load as appropriate and do not defer critical first-screen assets.
- Use a reasonable font fallback and `font-display`; do not add large font packages for tiny visual differences.
- Use pagination, virtualization, `content-visibility`, or a compatible strategy for large lists.
- Avoid repeated layout reads during rendering; keep per-keystroke work bounded for high-frequency input.
- Compress video rather than relying on large GIFs; provide a reduced-motion or static alternative.
- Prevent console errors, unhandled promise rejections, resource 404s, and flashes without layout.

## 9. CSS and Component Engineering

- Continue using the project's CSS/Tailwind/CSS Modules/styled-components system; do not replace it for one page.
- Keep selector responsibilities clear and avoid competing base/override rules.
- Use tokens for spacing and hierarchy; components own their layout instead of relying on accidental parent selectors.
- Component APIs should express product intent rather than expose many visual boolean switches.
- Do not use absolute positioning to conceal overlap that normal document flow should solve.
- Prefer clear variants in one component over duplicating full page regions for responsive/state differences.

## 10. Reference Designs and Design Systems

When the user provides a screenshot, Figma, or existing design:

1. Identify tokens, spacing, typography, component variants, breakpoints, and states.
2. For high-fidelity work, follow the reference instead of creatively “improving” it.
3. For missing motion/error states/interactions, fill them consistently with the design language and label assumptions.
4. If a design-system component is missing, first show why existing components cannot be composed or extended.
5. Explain deviations in terms of user value and maintenance impact.

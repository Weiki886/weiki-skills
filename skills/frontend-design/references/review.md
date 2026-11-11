# Interface Review

## 1. Pin the Review Target

Confirm what is being reviewed:

- Screenshot, running URL, component/page source, or PR UI change.
- Product goal, primary user, and core task.
- Brand and design system.
- Target viewport, theme, language, and key states.
- Base/head or current Commit for a PR.

With source code only, state that visuals and interactions cannot be fully verified. A running page is stronger evidence than a static screenshot; a screenshot is stronger than reading CSS alone.

Interface review is not general code review. For logic defects, test gaps, or maintainability issues, route to `code-review`, unless they directly make the UI unusable or misleading.

## 2. Four Review Dimensions

### Task and Flow

- Can users tell on the first screen what the product/surface does?
- Is the primary action clear, without competing primary buttons?
- Are the steps to finish the task necessary?
- Are current status, result, and next step visible?
- Do empty/error/permission states offer a recovery path?
- Do navigation, back, and cancel match user expectations?

### Visual Craft

- Does visual hierarchy match information hierarchy?
- Do font roles, size, line height, and line length support reading?
- Do spacing, alignment, and density create a stable rhythm?
- Does color establish priority rather than spread evenly?
- Are composition and signature grounded in the subject?
- Does motion aid understanding and stay restrained?

### System Consistency

- Are existing components and tokens reused?
- Were fonts, radius, shadows, colors, or interaction modes added without justification?
- Does the same action keep the same name and meaning across screens?
- Are desktop/mobile/keyboard platform conventions respected?
- Are design-system deviations explained?

### Resilience and Accessibility

- Do narrow screens, long content, empty/error states, themes, and localization work?
- Are keyboard access, focus, labels, contrast, and target size adequate?
- Are overflow, overlap, truncation, and layout shifts present?
- Do images, fonts, icons, and interaction resources load reliably?
- Are there console errors, and do key actions give timely feedback?

## 3. Severity

| Level | Criteria | Example |
| --- | --- | --- |
| `Blocking` | Prevents the core task, access, or trust; materially misleading. | Keyboard cannot submit a form; an error has no recovery; an overlay permanently covers key content. |
| `Should fix` | Does not block completion but materially hurts efficiency, comprehension, or brand quality. | Primary/secondary action unclear; a key mobile column is inaccessible; error gives no remedy. |
| `Polish` | Visual/copy refinement that does not prevent task completion. | Minor spacing mismatch, inconsistent secondary label, overly strong motion. |
| `Follow-up` | Worth doing later but out of current scope. | Broader design-system consolidation or legacy component migration. |

Do not downgrade accessibility or core-task blockers because “the change is small.”

## 4. Verdict

- `Ready`: no `Blocking` issue; `Should fix` items are resolved or explicitly do not affect delivery.
- `Conditional`: no fatal issue, but listed `Should fix` items must be completed before merge/delivery.
- `Rework`: at least one `Blocking` issue requires structural redesign or key implementation rework.

This is an interface-experience verdict, not a production release gate. `production-risk-review` handles database, stability, and rollback risks.

## 5. Output Format

Provide no more than six primary findings, ordered by impact. For each:

```text
[Blocking] Form error cannot be located
Evidence: Submit produces only a global toast; the field has no aria-invalid and focus stays on the button.
Impact: Keyboard and screen-reader users cannot tell which field to fix.
Fix: Show an error beside the field, associate its description, and focus the first invalid field after failed submit.
```

Do not write only “feels crowded” or “not premium enough.” Evidence must be observable and the fix actionable.

End with one highest-priority correction. Add low-priority suggestions only when needed.

## 6. Compare Multiple Options

When asked to choose among alternatives:

- Give each option a `Ready / Conditional / Rework` verdict.
- Compare 3–5 dimensions relevant to the task.
- Name one winner and the fact that could change the decision.
- Do not copy every losing option's full flaw list; report only decision-changing differences and issues the winner must fix.

## 7. Visual Review Red Flags

- The home page could be reused by changing only the product name.
- Every section uses the same card pattern with no real hierarchy.
- Decoration dominates the first screen over the actual product task.
- Typography, color, and composition come from a trend/template instead of subject or brand.
- Only the perfect path has states.
- Dark theme, long copy, or narrow screens immediately break.
- Mouse works but keyboard does not.
- Errors, empty states, and success messages do not change what the user should do next.
- Motion is abundant but explains no state.
- New visual tokens coexist with the existing system as a competing language.

# Public Sources and Integration Choices

Research date: 2026-11-06. These sources inform design decisions. This skill reorganizes the guidance into English and does not bind runtime instructions to externally changing files.

## Main Sources

| Source | What is incorporated | Choice and boundary |
| --- | --- | --- |
| [Anthropic frontend-design](https://github.com/anthropics/skills/blob/main/skills/frontend-design/SKILL.md) | Ground design in the product subject instead of visual tropes; define typography, color, layout, and signature; plan before building; treat copy as design material. | Keep “have a point of view” and “spend boldness once”; do not ban specific fonts, palettes, or styles. |
| [Vercel Web Interface Guidelines](https://github.com/vercel-labs/web-interface-guidelines) | Production UI details for semantics, focus, forms, motion, long content, themes, localization, and performance. | Keep rules that affect usability/stability; do not chase every remote update or elevate copy punctuation preferences into universal requirements. |
| [Vercel web-design-guidelines skill](https://github.com/vercel-labs/agent-skills/tree/main/skills/web-design-guidelines) | Design reviews should produce locatable, actionable findings. | Do not fetch a moving branch at runtime; this skill carries a stable baseline and lets repository rules override it. |
| [Microsoft frontend-design-review](https://github.com/microsoft/skills/blob/main/.github/skills/frontend-design-review/SKILL.md) | Distinguish creation from design review; assess task, craft, credibility, and design-system consistency; prioritize by impact. | Exclude product-specific workflow and hard-coded scoring; evidence and user tasks matter more than a score. |
| [PaulRBerg frontend-design skill](https://github.com/PaulRBerg/agent-skills/blob/main/skills/frontend-design/SKILL.md) | Read the real product, preserve working behavior, use subject-swap critique and real rendering, and state completion evidence. | Do not inherit its desktop/single-theme default; choose a matrix based on project and task. |
| [WCAG 2.2 Target Size (Minimum)](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html) | Pointer target minimum of 24×24 CSS px or an applicable exception; projects may be stricter. | Include stable accessibility baselines without duplicating the whole WCAG standard. |
| [Nielsen Norman Group: 10 Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/) | System status, user control, consistency, error prevention, and recovery as review lenses. | Treat as heuristics, not regulation or absolute truth. |
| [Impeccable](https://github.com/ikunops/agent-dotfiles/blob/main/skills/function-specific/frontend/impeccable/SKILL.md) | Persuade/Operate/Read/Experience surface types, bounded render checks, and actionable visual feedback. | Do not adopt its many external scripts/commands or dependencies that cannot be guaranteed across environments. |
| [Humble Design Review](https://github.com/humbleteam/design-review) | Limit finding count, prioritize by impact, and distinguish design decisions from broad critique. | Keep few high-signal findings; do not make delivery depend on a single 0–4 score. |

## Integration Principles

1. External skills are evidence, not runtime dependencies. Read this skill's local references for applicable rules.
2. Prefer cross-project decision principles. The brief and repository determine fonts, colors, animation libraries, frameworks, and module layout.
3. Do not copy substantial source text or inherit assumptions about licenses, paths, or tools.
4. Keep design review separate from code review so one PR is not blocked by duplicate checklists.

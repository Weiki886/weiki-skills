# Technical Article Guidance

Explain how something works, why, or its underlying principles in a way readers can reproduce. Value comes from accuracy and reproducibility, not literary flourish.

For a long article or unclear direction, first offer a one-sentence thesis, outline, and 2–3 title options for confirmation; then draft.

## Suggested Structure

Select and trim sections as needed; not every article needs them all:

1. **Hook:** Give the conclusion or pain point so readers know within 30 seconds whether to continue.
2. **Context and boundaries:** State who this is for, the version/prerequisites, and what the article does not solve.
3. **Principles:** Explain the design rationale, core concepts, mechanisms, or tradeoffs.
4. **Approach / implementation:** Start with the minimum working version and progress in verifiable steps.
5. **Verification and results:** Show observable output, performance, screenshots, or data.
6. **Common pitfalls:** Include real pitfalls, root causes, and ways to avoid them.
7. **Summary and references:** Reconnect to the thesis and link to official docs, source, or papers.

## Code and Command Rules

- Code must actually run and reproduce the described result; do not include “looks right” pseudocode.
- Run commands in the target environment and state OS, shell, tool, and version.
- Before each code block, explain the problem it solves, prerequisites, and expected output.
- Include only important excerpts for long code; link to the full repository/project or give a reproducible procedure instead of dumping everything.
- For APIs, configuration, and dependencies, state versions and compatibility. Mark uncertain items `[unverified]`.

## Accuracy and Reproducibility

- Support every conclusion with a source (official docs, source code, RFC, experiment, or first-hand debug record); identify version and date.
- Give reproducible experimental conditions (hardware, data, parameters, command) and state limitations of a single run.
- Distinguish **fact**, **inference**, and **personal opinion**. Do not hide uncertainty with vague wording.
- State the solution's **applicability and limits**, including when not to use it and what it costs.
- Mark unverified content `[needs verification]` or state the observed conditions. Do not pretend it was verified or silently omit important information.

## Argument Progression and Evidence Anchors

- **Progress the question:** Carry the main question through four levels: phenomenon → engineering → mechanism → decision. Explain what is wrong, where the engineering is stuck, why it happens and what it costs, and what to do next. This is an argument progression, not repetition of the same sentence.
- **Anchor judgments:** Attach each important conclusion to verifiable evidence such as a number, scale, ratio, or explicit comparison. Without data, give a concrete comparison and context; otherwise “more efficient” or “large impact” becomes vague.

## Visuals and Scanability

- Prefer real output, screenshots, comparison tables, or a minimal flow diagram for conclusions/results. If one visual explains it, do not use a long paragraph instead.
- Use short paragraphs, headings, and lists so readers can scan the structure in ten seconds; one level should cover one idea.
- Keep code, tables, figures, and prose consistent. Never say “the output is shown below” without showing it.

## Voice and Length

- Be professional and restrained, with high information density. Avoid piles of abstractions; explain or link a term at first use.
- Write for the target reader's prior knowledge: do not explain what everyone knows, but do explain important prerequisites.
- Cover one idea per section. A heading may use “how + why” so the point remains clear when skimmed.

---
name: content-writing
description: Use when writing technical articles (tutorials, source-code walkthroughs, lessons learned) or marketing content (product promotion and conversion). Choose technical or marketing mode first, then adapt voice, structure, and length to the audience, goal, and platform. Write in the language requested by the user.
metadata:
  short-description: Write technical articles and marketing content
---

# Content Writing

Turn worthwhile ideas into articles readers want to finish and that withstand factual scrutiny. Technical writing earns trust through accuracy and reproducibility; marketing writing prompts action through relevance and credibility.

## Core Principles

1. Before drafting, state in one sentence what the reader will take away. If that is unclear, do not write yet.
2. Identify the audience, purpose, and platform before deciding voice, structure, or length. Do not use word count as the goal.
3. Every fact, number, example, and command needs a source. Remove claims without a source.
4. Make it clear before making it short: the first draft may be verbose; the revision should remove every unnecessary sentence.
5. Avoid formulaic, generic prose. Use concrete detail and information that reflects real experience instead of piles of adjectives.
6. Follow one thesis and one through-line. Each article should solve one problem or support one decision.
7. Use the language the user requests; preserve technical terms where appropriate.

## General English Writing Guidance

- Prefer active voice: “OpenAI released a new model,” not “A new model was released by OpenAI.”
- Prefer affirmative, direct statements: explain what something is and what value it provides rather than defining it only by what it is not.
- Choose pronouns for the context: “you” for informational/tool content, “I” for opinions or personal experience, and “we” for team or official announcements.
- Follow the platform's conventions for punctuation, capitalization, dates, units, and spelling variants; stay consistent within one article.

## Choose the Writing Mode

Choose one before drafting:

- **Technical:** Explain how something works, why it works, or its underlying principles. Value comes from accuracy and reproducibility. Use for tutorials, source-code walkthroughs, lessons learned, comparisons, and technical explanations.
- **Marketing:** Explain why to choose the product, what the reader will gain, and what action to take. Value comes from relevance and credibility. Use for product promotion, lead generation, brand stories, and landing-page copy.

Default to technical when the goal is transferring or preserving knowledge; default to marketing when the goal is driving a click, inquiry, or purchase. For technical marketing, let the conversion goal drive the structure and use technical facts as evidence; do not drift between the two modes. See `references/technical.md` and `references/marketing.md` for structures and guardrails.

## Seven Questions Before Writing

Answer these for either mode:

1. **Who is the reader?** What do they already know, where will they read this, and what problem do they actually have? Write for a specific person, not “everyone.”
2. **What is the thesis?** State in one sentence what the reader should take away.
3. **What is the topic and distinct angle?** What will the reader search for, what do existing articles say, and what non-obvious perspective can this article add?
4. **What is the goal and CTA?** Technical content may invite saving, following, or further study; marketing content may ask the reader to try, inquire, or buy.
5. **What voice and length fit?** Derive these from the audience and platform, not from an assumption that longer is better.
6. **What are the sources?** Technical writing needs runnable code, verified commands, or data; marketing writing needs real cases, numbers, or customer testimony.
7. **What is the publication format?** Markdown or rich text? Is a lead image, summary, tags, or cover text needed?

Do not draft until these questions can be answered. If questions 2 or 6 cannot be answered, clarify first. For a long article or unclear direction, start with a one-sentence thesis, 2–3 title options, and an outline. Mark research gaps that need first-hand information and get confirmation before drafting. Develop long pieces section by section rather than producing the whole article at once.

## Workflow

1. Choose the mode and answer the seven questions; lock one thesis and target audience.
2. Clarify the division of work: the author supplies first-hand material, the core angle, and genuine reactions; the assistant can help with evidence, analogies, expansion, structure, and background. If first-hand material is essential and missing, ask for it instead of inventing it.
3. Collect primary evidence: for technical writing, verify that the examples actually run; for marketing, verify real cases and data.
4. Create an outline using a technical structure or PAS/AIDA as appropriate; see the references.
5. Draft for completeness, accuracy, and detail before trimming.
6. Verify every number, code sample, command, link, version, name, and quotation. See `references/checklist.md`.
7. Edit for naturalness and clarity: remove boilerplate and redundancy, normalize terminology and formatting, and review the prose from the reader's perspective. See `references/checklist.md`.
8. Deliver the final content in the platform's required format, with any requested summary, tags, or cover text.

## Guardrails by Mode

**Technical:** Code must be runnable and commands must be tested in a real environment. Explain applicability and limitations. Cite sources and versions for conclusions. Mark uncertainty as `[needs verification]`; do not silently remove it.

**Marketing:** Avoid empty claims. Without real numbers, examples, or testimony, do not claim improvement or leadership. Promote one action with one primary CTA. Address only the needs of the audience you actually serve.

## Output

Concisely report: (1) writing mode and target audience; (2) one-sentence thesis; (3) chosen structure (technical, PAS, or AIDA); (4) key facts and their sources; (5) approximate length and platform format; and (6) optional items requiring user confirmation, such as title, voice, or image/code placement. Never invent sources, data, or customer opinions.

## References

Read only what is needed:

- `references/technical.md`: technical article structure, code examples, accuracy, and reproducibility.
- `references/marketing.md`: PAS/AIDA, differentiation, pain points, CTA, and social proof.
- `references/seo.md`: titles, keywords, and summaries shared by both modes.
- `references/checklist.md`: source ownership, fact-checking, natural prose, scanability, and final editing.

# Source Ownership, Fact-Checking, and Delivery QA

Set clear author/tool boundaries, then review in layers. Prefer marking something “needs verification” to pretending it is verified or making it up.

## Before Drafting: Divide Responsibilities

The author must supply the material most likely to be fabricated. If it is missing, stop and ask:

- **First-hand observations and real experience:** things actually done, lessons learned, code/results actually run. Never invent a scenario such as “one time, for example…”
- **Core angle and judgment:** the idea and position that hold the article together. Candidate angles are fine; the author makes the final decision.
- **Real emotions and impressions:** “I sat there stunned for a while,” not “I was deeply moved.”
- **People behind the data:** recognizing a real person behind “1,000 paid orders” requires the author's genuine perspective.

Appropriate support work includes finding evidence and analogies, expanding an established angle, adding background, and proposing clearer structure.

If the source material is thin, gather more material first; do not use writing tricks to hide the gap.

## Fact-Checking

- [ ] Numbers, percentages, dates, and versions have sources and consistent definitions.
- [ ] Code and commands actually run (technical); cases, customers, and data are real (marketing).
- [ ] Links work and point to the right place; names, products, and proper nouns are spelled correctly.
- [ ] No “obviously” or “everyone knows” skips an argument.
- [ ] Unverified/uncertain content is marked `[needs verification]` or removed; nothing is fabricated.
- [ ] Citations include version/date; experiments/data state their definitions and limitations.

## Remove Formulaic Signals

Rewrite common signals when they appear; direct prose is better:

| Category | Typical signal | Edit |
| --- | --- | --- |
| Empty preface | “It is worth noting,” “In conclusion,” “As everyone knows,” “With the continued development of…” | Delete the preface and keep the fact. |
| Stock phrases | “Put simply,” “In essence,” “In other words,” “This means,” “It cannot be denied,” “Firstly…secondly…finally” | Use conversational language or remove it. |
| Inflated verbs | “Empower,” “Enable,” “Build,” “Lead,” “Reshape,” “Milestone,” “Play a key role” | State what was actually done. |
| Connector piles | “Furthermore,” “Meanwhile,” “In addition,” “Not only…but also” | Remove unnecessary transitions; connect facts in natural order. |
| Three-part slogans | Three neat adjectives or three parallel short sections | Break the pattern or use two/four items. |
| Polished antithesis | “Official docs teach commands, not work” and similar neat opposites | Make the wording less slogan-like and more natural. |
| False either/or | “It is not X, but Y” | Say Y directly. |
| Grand symbolism | “Marks,” “bears witness,” “is proof of,” “symbolizes” | Return to the evidence. |
| Summary finale | A final “in conclusion” paragraph that repeats everything | Let the final sentence end naturally. |
| Excessive setup | Long background/value statement before the point | Start with the conclusion, scenario, or problem. |
| Fictional anecdote | “For example, one time…” without real experience | Use a real detail or remove it. |
| Bold overload | A bold label at the start of every paragraph | Keep one or two emphasis points; integrate the rest. |

Say “save users time,” not “increase user efficiency,” when that is what you mean.

## Add a Human Voice

Removing formula is only the baseline. Preserve a real author's voice:

- [ ] **Take a position:** react to facts instead of neutrally repeating them; “I prefer…” or “After hitting this issue…” can be appropriate.
- [ ] **Vary rhythm:** alternate short and long sentences; follow a few short sentences with a developed thought.
- [ ] **Trust the reader:** do not explain every metaphor, add excessive caveats, or assume the reader is a novice.
- [ ] **Acknowledge complexity:** include costs, uncertainty, and when the advice does not apply; that is more credible than claiming perfection.
- [ ] **Be concrete:** prefer “It kept writing code at 3 a.m. while no one was watching” to “The result was concerning.”
- [ ] **Allow slight imperfection:** appropriate conversational language, first person, or a brief aside beats mechanically uniform prose.

## Scanability

- [ ] Each paragraph covers one idea and is short enough to scan on a phone (roughly four lines or fewer).
- [ ] Headings can be read alone to understand the article's argument.
- [ ] Lists/code/tables/figures support the prose; no “as follows” without a following example.
- [ ] Important conclusions have real results or examples, not just adjectives.

## Proofread

- [ ] Keep one through-line; remove or move tangents.
- [ ] Explain a term at first use and keep terminology consistent.
- [ ] Title and body match; no bait-and-switch.
- [ ] Removing a paragraph still leaves a coherent article; trim sentences until another deletion would lose information.
- [ ] Match the platform format (Markdown rendering, rich text, lead-image ratio, summary length).

## Final Read: Does It Feel Human?

Ask one question: **Does this read like a real person carefully telling you about something they care about, or like a machine outputting information?**

Check four things:

- [ ] **Warmth:** emotion comes from lived memory (“I froze for a moment”), not generic description (“This is astonishing”).
- [ ] **Distinctiveness:** is there an angle only this author would write? If any writer/tool could produce it unchanged, it is not distinctive enough.
- [ ] **Posture:** does it sound like an informed person in conversation, or like a lecturer or brand campaign?
- [ ] **Flow:** does attention break anywhere and force a reread? That break is a rhythm problem to fix.

If a passage feels formulaic, rewrite it in a more conversational, personal, and imperfect voice.

## Optional Delivery QA Report

```text
- Fact check: PASS / FAIL (items to fix)
- Formulaic language: PASS / FAIL (signals found)
- Human voice: PASS / FAIL (specific passages)
- Next: fix the 1–3 highest-impact issues
```

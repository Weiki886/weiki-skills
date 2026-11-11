---
name: project-readme
description: Use to create, improve, or audit a repository README, especially for CLI, MCP, and local developer tools. Classify solo/public and create/improve/audit first, then use repository evidence to write a Quick Start, task-oriented usage, security/data boundaries, troubleshooting, known limitations, and documentation links. Ask instead of inventing unverified claims, and do not overwrite an existing README without direction.
metadata:
  short-description: Write and audit evidence-based project READMEs
---

# Project README

A README is neither a parameter manual nor an advertisement. It is the shortest path to helping a new reader trust a project and get a first result.

Do not fill sections for their own sake. A reader should be able to answer three questions: **What is this? Is it right for me? Can I get a first result and understand the risk boundaries?**

## Core Principles

1. Every claim needs evidence. Omit claims that cannot be verified in the repository or confirmed by the user; do not soften a guess into vague prose.
2. A Quick Start is mandatory: give the shortest real path from installation to a useful result. If its commands cannot be verified, ask rather than leaving blanks or inventing them.
3. When a README already exists, ask whether to replace it, preserve selected material, or extend it in place. Do not guess or overwrite user-written content.
4. Establish identity and an entry point above the fold. For public projects, use the project name/logo, one-line description, real badges, and navigation, then put Quick Start immediately after. Use a GIF/screenshot only when it conveys more than text.
5. Show rather than assert: terminal output, screenshots, code, and verified numbers are stronger than adjectives.
6. Organize by task, not by parameter. Explain “what do I want to do → which command does it”; leave complete option lists to `--help`.
7. Delete any line that does not help the reader decide, trust, or act.
8. If the project touches local files, databases, credentials, networks, or model calls, explain what it reads, writes, and sends off-device and its default read-only/confirmation/backup/fail-closed posture. Make no security promise without code evidence.
9. Keep the README as a front door; link deep details to `docs/` or `CONTRIBUTING.md` instead of turning the README into a manual.
10. Match the project; do not force one template onto CLI, MCP, libraries, apps, and research repositories.
11. Avoid unverified boasts such as “industry-leading,” “production-ready,” or “extreme performance.” See the guardrails in `content-writing`.

## Choose the Audience

Classify the README's reader to set the level of detail:

- **Solo** (personal project, internal tool, or personal script): the reader is mainly you, including your future self. Required: a one-line description, Quick Start, and known limitations. Badges, contribution guide, community channels, star history, and roadmap are optional. The baseline is that you can still run it three months later.
- **Public** (open source, distributed software, or team-shared): the reader is a stranger. Include description, value, Quick Start, usage, configuration, documentation index, contribution, and license. Add badges and community information only when they really exist.

Default to public if the repository is public, published to a package manager, or has external users; default to solo for private personal projects and one-off work. For mixed cases, use public detail.

## Choose the Mode

| Mode | When to use | First step |
| --- | --- | --- |
| **create** | No README exists, or the user explicitly requests a rewrite | Inspect the repository and collect facts. |
| **improve** | A README exists and needs work | Read it, ask how to handle the existing content, then proceed. |
| **audit** | The user requests diagnosis only | Verify each README claim against the repository and report findings. |

**Gate:** If a README exists, pause for the user's choice: (1) replace it entirely, (2) rewrite while retaining specified material, or (3) extend it in place. “Help me write a README” does not imply option 1.

## Above the Fold and Developer-Tool Style (Public Projects)

Readers decide whether to continue after one screen. The first screen should show what the project does.

For CLI, MCP, and local developer tools, prefer restrained, verifiable developer documentation:

```markdown
<div align="center">
  Optional logo
  # Project name
  **One-line description**
  Real badges only
  Language switcher (only when the other README exists)
  Anchor navigation (when the document exceeds 100 lines)
</div>
---
Capability overview: actual commands/tools, inputs, outputs, and default safety posture
```

Three rules:

- **One-line description:** Keep it memorable and roughly one line: what it is, who it is for, and its most important factual distinction. Save principles and background for later.
- **Capability overview:** List actual commands/tools, inputs, outputs, read/write objects, and the default safety posture. Avoid generic claims like “fast, safe, and intelligent.”
- **Quick Start next:** Prefer 1–3 shortest steps. The final step must verify installation or service discovery. Put complex configuration later.

Visual material is optional. Choose GIFs/asciinema, screenshots, and real terminal output only when they add information. If there is no demo asset, do not invent one; give real commands, expected output, and verification steps instead.

Solo projects may omit logos, badges, and navigation, but a one-line description and visible output are still useful.

## Evidence Ladder

Before writing each claim, classify its evidence:

| Level | Source | Use |
| --- | --- | --- |
| 1 | **In the repository:** source, tests, examples, configuration, CI, Tags, screenshots | Write it directly. |
| 2 | **Confirmed by the user:** positioning, differentiation, metrics, screenshots, cases | Write it directly. |
| 3 | **Externally verifiable:** package page, Release, public benchmark | Write it with a link. |
| 4 | **No source** | Omit. Ask if important; otherwise delete. |

Never invent installation commands, CLI parameters, API signatures, configuration keys, environment variables, paths, performance numbers, compatibility claims, license terms, or community links.

See `references/evidence.md` for the repository scan and evidence rules.

## Workflow

1. **Classify audience and mode:** solo/public and create/improve/audit. If a README exists, ask how to handle it before changing it.
2. **Collect repository evidence:** inspect package metadata, entry points, structure, configuration, docs, Git/CI, assets, read/write paths, credentials, networks, model calls, and error codes. This stage gathers facts, not conclusions; see `references/evidence.md`.
3. **Classify the project and reader:** CLI, MCP, library, app, research project, Agent Skill, or monorepo. Use this to select sections; see `references/structure.md`.
4. **Verify Quick Start:** find and verify the first command that produces a useful result after installation. If it cannot be verified, ask and do not proceed past this gate.
5. **Plan the sections:** select sections for the project type and identify evidence for each. Omit unsupported sections. Also decide above-the-fold layout, Quick Start verification, and whether security/data flow, error codes, or troubleshooting are needed.
6. **Draft:** identity and capabilities → Quick Start → features → installation/configuration → task-based usage → rationale/background → security/privacy boundaries → errors/troubleshooting → known limitations → development/deep docs. Explain rationale after the reader has a working path, not before it.
7. **Run the quality gate:** check each item in `references/checklist.md`, especially factual accuracy and executable steps.
8. **Deliver:** provide a draft in the conversation by default; write to disk only when explicitly requested. Re-run the checklist before writing.

## Output

Concisely state: (1) audience and mode; (2) project type and target reader; (3) section plan and evidence source for each section; (4) the README draft; (5) claims requiring user confirmation because evidence is missing; and (6) known omissions. Never label a guess as verified. Explicitly list unsupported claims rather than disguising them with vague language.

## Red Flags

- Missing or blank Quick Start, or invented commands with no repository entry point.
- Overwriting an existing README without asking how to handle it.
- Unconfirmed performance, compatibility, user-count, or star-count claims.
- Badges that point to nonexistent CI, packages, or licenses.
- Filling template sections with empty headings or “TODO/coming soon.”
- Embedding a full API manual instead of linking to it.
- Reproducing a long `--help` table instead of showing task-based commands.
- An opening screen with only slogans or principles and no real entry point or verifiable result.
- Omitting security/data-flow information for tools that touch local data, credentials, networks, or model calls.
- Promising read-only operation, no data leakage, fail-closed behavior, or zero configuration without code/test evidence.
- Troubleshooting that says only “restart” without a symptom, error code, cause, and remedy.
- A one-line description that is a paragraph, or rationale that appears before the first runnable path.
- Adjectives such as “powerful,” “elegant,” “extreme,” or “out of the box” replacing evidence.
- Forcing contribution guides, codes of conduct, or community links onto a solo project when none exist.
- Architecture diagrams containing nonexistent components or call relationships.
- Writing files without user authorization.

## References

Read only what is needed:

- `references/structure.md`: reading flow, section selection by project type, and section-writing guidance.
- `references/evidence.md`: repository scan, evidence levels, and claims that must not be invented.
- `references/checklist.md`: pre-write quality gate for facts, usability, presentation, and language.

# weiki-skills

A collection of reusable agent skills for product design, frontend design, GitHub engineering, implementation, testing, review, technical writing, and project documentation.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**Language:** English | [简体中文](README.zh-CN.md)

**Contents:** [Quick Start](#quick-start) · [Skills](#skills) · [Repository Layout](#repository-layout) · [Supported Tools](#supported-tools) · [Installation](#installation) · [Usage](#usage) · [Design Decisions](#design-decisions) · [Security Boundaries](#security-boundaries) · [Limitations](#limitations) · [Development and Validation](#development-and-validation) · [Contributing](#contributing) · [License](#license)

This repository contains ten `SKILL.md` skills. Each skill is a self-contained directory: `SKILL.md` defines when it applies, `references/` contains details loaded as needed, `assets/` may contain reusable output resources, and `agents/openai.yaml` provides Codex-specific metadata. Install only the skill directories you need; the same content can be used across supported agents.

## Quick Start

Install `project-readme` in Codex:

```sh
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo Weiki886/weiki-skills \
  --path skills/project-readme

test -f ~/.codex/skills/project-readme/SKILL.md && echo "project-readme installed"
```

After `project-readme installed` appears, restart Codex or begin a new session. The skill can then be selected for requests to create, improve, or audit a project README. See [Supported Tools](#supported-tools) for other agent installation locations.

## Skills

| Skill | Purpose | Entry point |
| --- | --- | --- |
| [architecture-design](skills/architecture-design) | Translate finalized requirements into evolvable module boundaries, data ownership, interface contracts, risk decisions, and an implementation-ready system skeleton. | `skills/architecture-design/SKILL.md` |
| [product-design](skills/product-design) | Product decisions from research and alignment through text-first specification, optional prototype, and MiniSpec/FullSpec proposals. | `skills/product-design/SKILL.md` |
| [frontend-design](skills/frontend-design) | Build, refine, or review frontend interfaces using an appropriate visual direction, accessible implementation, and rendered verification. | `skills/frontend-design/SKILL.md` |
| [github-engineering-workflow](skills/github-engineering-workflow) | GitHub-based engineering delivery, scaled for solo or team work across Issues, PRs, review/CI, releases, and retrospectives. | `skills/github-engineering-workflow/SKILL.md` |
| [content-writing](skills/content-writing) | Technical or marketing content shaped for its reader, purpose, and publishing platform. | `skills/content-writing/SKILL.md` |
| [code-review](skills/code-review) | Review diffs for engineering conventions and requirement alignment, with a fixed baseline and evidence-backed findings. | `skills/code-review/SKILL.md` |
| [production-risk-review](skills/production-risk-review) | Pre-merge production risk review focused on data loss, database migrations, crashes, failure propagation, release order, and rollback. | `skills/production-risk-review/SKILL.md` |
| [code-implementation](skills/code-implementation) | Implement features, fixes, refactors, and behavior changes through clarification, planning, TDD where appropriate, debugging, and verification. | `skills/code-implementation/SKILL.md` |
| [code-testing](skills/code-testing) | Derive meaningful tests from requirements and contracts, cover boundaries and failures, check that tests catch defects, and run required suites. | `skills/code-testing/SKILL.md` |
| [project-readme](skills/project-readme) | Create or improve project READMEs using repository evidence, an executable Quick Start, task-based usage, security boundaries, troubleshooting, and limitations. | `skills/project-readme/SKILL.md` |

## Repository Layout

```text
skills/
├── <skill-name>/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   └── references/
└── ...
```

The entrypoint stays focused. Supporting guidance is loaded only when relevant to a task.

## Supported Tools

The skill content is plain Markdown and can be adapted by agents that support instruction files. `agents/openai.yaml` is Codex-specific metadata; other tools may ignore it. Installation paths and automatic discovery behavior vary by agent, so verify the target tool's current skill conventions.

## Installation

### Codex Installer

Use the built-in installer for one skill:

```sh
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo Weiki886/weiki-skills \
  --path skills/<skill-name>
```

Replace `<skill-name>` with a directory under `skills/`.

### Clone and Copy

```sh
git clone https://github.com/Weiki886/weiki-skills.git
cp -R weiki-skills/skills/<skill-name> ~/.codex/skills/
```

For another agent, copy the selected skill directory to that agent's documented skills location. To install several skills, copy only the directories you selected rather than the entire `skills/` folder.

### Download an Archive

Download and extract the repository archive, then copy selected skill directories under `skills/` to the target agent's skills directory.

## Usage

Skills are discoverable through their `SKILL.md` descriptions. Typical requests include:

- `product-design`: design a feature before implementation, write or review a proposal/PRD, research alternatives, and create a prototype after text approval.
- `architecture-design`: turn finalized product requirements into module boundaries, data ownership, interface contracts, risk decisions, and a compiling system skeleton where applicable.
- `frontend-design`: build, refactor, review, or polish a website, dashboard, product interface, component, or form, then verify its rendered behavior.
- `github-engineering-workflow`: establish repository conventions, break down Issues/PRs, prepare a release, or review a delivery/incident process.
- `content-writing`: write a technical article, tutorial, source walkthrough, product description, or marketing copy.
- `code-review`: review a PR, branch, uncommitted change, or incoming review feedback.
- `production-risk-review`: assess whether a PR could cause database deletion, data corruption, service crashes, failure propagation, or an unrecoverable release.
- `code-implementation`: implement a feature, fix a defect, refactor, or change behavior.
- `code-testing`: add tests for generated code, strengthen weak test cases, add regression coverage, or run required pre-commit tests.
- `project-readme`: create, improve, or audit a project README.

To request one explicitly, include its name, for example: `Use project-readme to audit this repository's README.`

Testing example: `Use $code-testing to cover boundary and failure cases for this change, verify that the important tests catch regressions, and run the required pre-commit test suite.`

Production review example: `Use $production-risk-review to assess whether PR #123 is safe to merge and release, focusing on database migrations, bulk deletion, service crashes, failure propagation, and rollback.`

## Design Decisions

- `SKILL.md` is the single source of truth; the repository does not maintain duplicate skill bodies for each agent.
- Skills focus on decision points, workflows, and evidence of completion. They do not replace project-specific language rules, frameworks, or `AGENTS.md` instructions.
- Solo projects may scale down process rigor, but must not discard essential verification, evidence, known limitations, or security boundaries.
- The repository favors distinct capability areas over adding skills that merely restate existing ones.

## Security Boundaries

Skills are instruction text; they do not include executable installation scripts or standalone dependencies. Risk comes primarily from installation and from the target agent's permissions.

| Action | Reads | Writes |
| --- | --- | --- |
| Browse or clone | Repository files on GitHub | Local clone directory |
| Codex installer | Selected skill in this repository | Selected target skills directory |
| Manual `cp -R` install | Selected skill directory | Target agent's skills directory |
| Agent loads a skill | Skill instructions and project context | Depends on the task and its authorization |

Do not place secrets, private configuration, or restricted data in skills, Issues, PRs, or logs. Confirm the install destination. When an agent uses a skill, the target project's security and authorization rules still apply.

## Limitations

- This repository currently contains ten skills; it is not a complete marketplace for every engineering task.
- English is the default README language; a Simplified Chinese version is available in [README.zh-CN.md](README.zh-CN.md). Product names, code identifiers, and other proper nouns retain their canonical spelling.
- Validation checks skill structure and metadata, but cannot guarantee identical loading behavior across third-party agents.
- `agents/openai.yaml` is used by Codex only; other tools may ignore it.
- The repository does not currently provide a separate `CONTRIBUTING.md`, community channel, or CI status page; this README does not claim otherwise.

## Development and Validation

At minimum, verify that each skill has the required entrypoint and valid frontmatter:

```sh
find skills -mindepth 2 -maxdepth 2 -name SKILL.md -print

ruby -e 'require "yaml"; ARGV.each { |p| t = File.read(p); m = t.match(/\A---\n(.*?)\n---\n/m) or abort("missing frontmatter: #{p}"); y = YAML.safe_load(m[1], permitted_classes: [Symbol]); abort("bad name: #{p}") unless y["name"] == File.basename(File.dirname(p)); abort("missing description: #{p}") if y["description"].to_s.empty? }; puts "skill frontmatter OK"' skills/*/SKILL.md
```

When a local path such as `~/.codex/skills/<name>` points to a skill in this repository, edits take effect in local Codex behavior. Validate changes before committing.

## Contributing

Issues and pull requests are welcome. For changes beyond a small fix, open an Issue first to align on the capability area, evidence sources, and overlap with existing skills. Commit subjects and PR titles must be in English.

## License

[MIT](LICENSE)

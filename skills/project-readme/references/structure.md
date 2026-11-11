# README Structure and Section Guidance

## Reader Flow

Choose sections in this order and remove anything the repository cannot support with evidence:

```text
Header: logo / project name / one-line summary / badges / language switch / anchor navigation
-> Capability summary: commands or tools, inputs/outputs, default safety posture
-> Quick Start (required; 1-3 steps with a verifiable result)
-> Features: user outcome + evidence of implementation
-> Installation and prerequisites (details, versions, paths)
-> Task-based usage / core tools or API
-> Configuration (environment variables, hosts, data directories, as applicable)
-> Why it exists / rationale and design trade-offs
-> Security boundaries / privacy and data flow
-> Error codes / troubleshooting
-> Known limitations
-> Development
-> Design-document links
-> Contribution (only with a real contribution process)
-> Community (only with an actual channel)
-> License
```

Ordering constraints:

1. Do not ask readers to configure anything before they know what the project is.
2. Show the shortest run path and verification before exhaustive features or rationale.
3. Put background and trade-offs after readers can run the project. Establish trust without blocking the first screen.
4. If the project reads or writes local data, credentials, makes network requests, or calls models, make the relevant safety boundary discoverable before risky operations. Do not hide costs in the Quick Start.

## Adapt Sections to Project Type

Project type determines reader expectations. Remove irrelevant sections instead of leaving them empty.

### CLI Tool

Readers want to know what the installed tool does. Include a one-line summary, Quick Start and verification, task-based commands with real output, installation prerequisites/version/path, configuration, read/write boundaries and confirmation/backup defaults, troubleshooting, and known limitations. Usually omit a full option table, API docs, source tree, decorative badges, and contribution guides for solo projects.

### MCP or Local Developer Service

Readers want to know how to register it, what tools expose, and whether data leaves the machine. Include an accurate package name/version strategy, verified host configuration, service discovery/connection check, tool names and parameter/authorization/failure behavior, credential setup, privacy/data flow, errors, and platform limits. Avoid generic MCP explanations, unverified `npx <name>` commands, imaginary host configuration, copied JSON schemas, real secrets, absolute-security claims, and unrelated model recommendations.

### Library or SDK

Readers want to know which problem it solves. Include a one-line summary, package-manager installation, a minimal runnable example (about 5-10 lines), core concepts/types, and language/runtime compatibility. Link to complete API reference instead of reproducing it. Avoid long tutorials, internal details, unconfirmed roadmaps, or nonexistent community channels.

### Application or Product

Readers want to see the product. Lead with a real screenshot/GIF, then a one-line summary and differentiator, access/install instructions, first-use steps, and key features. Link out to developer docs; avoid source layout, code-level API documentation, implementation internals, and contribution guides for non-open-source projects.

### Research Repository

Readers want to know whether results are credible and reproducible. Include the paper/preprint, key benchmark results, reproduction steps (data, environment, commands), dependency/runtime versions, and known limitations. Omit API docs, generic tutorials, unsupported maintenance promises, unconfirmed roadmaps, and nonexistent community links.

### Agent Skill Repository

Readers want to know what each skill does and how to install it. Include a one-line summary, skill list and purpose, installation (script or clone + copy), compatible agents (only verified), and directory structure. Usually omit architecture diagrams, API references, implementation details, speculative roadmaps, and nonexistent community channels.

### Monorepo

State what packages it contains, give each package a one-line description and link, and document shared development/build entry points. Keep per-package installation and API details in package-level docs.

## Section Writing

### One-Line Summary

State what it is, who it is for or what situation it addresses, and one factual differentiator. Avoid unverifiable superlatives ("fastest," "best," "industry-leading," "production-ready"); use specific facts instead.

| Weak | Better |
| --- | --- |
| `A powerful modern CLI toolkit` | `A CLI for restoring missing Codex history after switching providers; standard-library only, Python 3.9+` |
| `An industry-leading AI skill collection` | `A personally maintained Agent Skills collection for implementation, testing, review, and delivery workflows` |

### Quick Start

Quick Start is required. Keep it to 1-3 steps and include an observable success signal: version output, health check, generated file, service discovery, or a minimal successful request. Use a command verified from the repository. If Node versions or package names matter, keep Quick Start and installation instructions consistent.

### Usage: Organize by Task

The README answers "What do I want to do?"; `--help` answers "Which options exist?" Do not reproduce the full help output.

Good: one short task description followed by the real command:

~~~markdown
List currently visible conversations

```bash
cskit export --list
```

Export a specific conversation to the desktop

```bash
cskit export 2BLOG --output ~/Desktop/CodexExports
```
~~~

Avoid a table that simply repeats every `--help` option. Explain an option separately only when its behavior is non-obvious, destructive, or conflicts with another option.

### MCP Host and Tool Configuration

- Get `command`, `args`, server names, and environment variables from package entry points, documentation, or actual execution.
- Never place secrets in project-level configuration examples. Prefer Keychain, interactive login, or placeholder environment variables.
- Follow each host configuration with a service-discovery check such as `/mcp`, `codex mcp list`, or the verified equivalent.
- Explain tool names, parameters, authorization side effects, return values, and failures. Link to `docs/` for full schemas.
- Use an environment-variable table only when configuration is substantial; include defaults, requirement status, purpose, and valid values.

### Rationale and Trade-offs

Place this after usage. Explain an observed symptom first, then its cause, then why the project chose its design. Do not put it on the first screen before readers can run the project.

### Security, Privacy, and Data Flow

If the project touches local files, databases, clipboard, credentials, network, providers, or model calls, state the boundary. Publicly distributed projects should prefer a table; solo projects may use a concise paragraph but must not omit facts that affect user data or security.

| Capability / command | Reads | Writes | Network / external service |
| --- | --- | --- | --- |

Document default permissions, path validation, symlink handling, temporary-file cleanup, log redaction, dry-run, confirmation, backup, rollback, fail-closed behavior, and what leaves the machine where applicable. Verify claims such as "read-only," "no data collection," or "nothing is uploaded" in source, tests, or configuration. Do not make promises on behalf of a provider about retention, training, or jurisdiction.

### Error Codes and Troubleshooting

For integration-oriented projects, group real error codes by input, permission, resource, provider, or output and state whether retry is appropriate. Use symptom -> cause -> action:

```markdown
- **`PATH_NOT_ALLOWED`:** The file is outside the permitted root. Provide a normalized absolute path or add its parent directory to the allowlist.
```

Only document errors present in code or reproducible by users. Do not treat restart/reinstall as universal advice.

### Known Limitations

Include unsupported use cases, known bugs, platform constraints, and prerequisite dependencies, including for solo projects. Acknowledged boundaries build trust better than claims of universality.

### Architecture Diagram

Optional. Use ASCII or Mermaid only when components, boundaries, and relationships can be verified from the code. Never invent components or call paths; omit the section rather than draw a decorative but false diagram.

### Feature Highlights

Use 3-8 selective points. Pair the user outcome with repository evidence; do not disguise the full feature inventory as highlights.

### Badges

Add only when the underlying resource exists and the URL is stable: license, CI, package version, or downloads. Do not guess badge endpoints. A solo project usually needs no more than a license badge.

### Deeper Documentation

Organize links by reader intent, not by filename. For example: getting started, configuration, or contributing. Group design docs by proposal, ADR, standards, security, tests, and releases. Every linked file must exist; do not mechanically list every file in `docs/` on the front page.

### Development

Give contributors real installation, test, build, and check commands, and explain which important risks the tests cover. Commands should work from a clean environment. State system dependencies, secrets, and platform requirements explicitly.

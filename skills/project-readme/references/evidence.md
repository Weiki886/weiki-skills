# Evidence Ladder and Repository Scan

## Evidence Ladder

| Level | Source | How to use it |
| --- | --- | --- |
| 1 | Repository-native evidence (source, tests, CI, configuration, existing docs) | State directly and cite the source. |
| 2 | User confirmation (positioning, metrics, screenshots, examples) | State directly and mark as user-confirmed. |
| 3 | Externally verifiable evidence (package registry, release, benchmark) | State with a link. |
| 4 | No source | Omit. Ask if important; otherwise remove. |

## Repository Scan

Use this checklist as the first step in create and improve modes.

### Metadata

- [ ] Package manifests (`package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, etc.): name, version, description, license.
- [ ] Package scope, recommended pinning, runtime requirements, and unpublished status.
- [ ] Remote URL (`git remote -v`).
- [ ] Existing tags and releases.
- [ ] Contents of `LICENSE*` files.
- [ ] Logo, bilingual README, and badge resources actually referenced.

### Code and Entry Points

- [ ] Executable entry point (CLI `bin`, `main`, or entry point).
- [ ] Public modules and exported APIs.
- [ ] CLI subcommands, arguments, defaults, exit codes, and help text.
- [ ] MCP server name, transport, tools, argument schemas, and registration entry point.
- [ ] Examples and sample directories.
- [ ] Test signals (`tests/`, `__tests__/`, `*.test.*`).

### Configuration

- [ ] `.env.example` or sample configuration.
- [ ] Configuration files (`*.yml`, `*.toml`, `*.json`, `*.config.*`).
- [ ] Environment variable names, defaults, valid values, and whether each is required.
- [ ] Verified host configuration (e.g. Claude Code or Codex MCP settings).
- [ ] Dockerfile / docker-compose files, if any.

### Security and Data

- [ ] Files, databases, directories, clipboard contents, or system permissions read.
- [ ] Paths, database tables, temporary files, caches, and backups written.
- [ ] Dry-run, confirmation, rollback, permission checks, path normalization, and symlink handling.
- [ ] Network destinations, providers, telemetry, and model calls.
- [ ] Secret storage and whether sensitive data can enter logs or errors.

### Documentation and Delivery

- [ ] Existing README sections and repository-relative links.
- [ ] Design proposals, ADRs, architecture docs, tutorials, and runbooks.
- [ ] CI workflows, release flow, artifact types, and deployment requirements.
- [ ] Supported operating systems, runtimes, and known limitations.

## Repository Evidence Notes

- Read source and configuration before documenting behavior; names and comments alone are not proof.
- Run commands only when safe and available. Report what was actually executed and its result.
- If a claim depends on an inaccessible external service or production environment, label it unknown rather than implying verification.
- Keep local paths relative in documentation unless a real user-facing absolute path is required.

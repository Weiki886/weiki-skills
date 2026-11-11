# README Quality Gate

Run this checklist before showing a draft and again before writing the file.

## Facts and Executability

- [ ] Project name, one-line summary, install commands, runtime requirements, CLI/API usage, configuration, paths, and license terms come from the repository or user confirmation.
- [ ] Uncertain claims are marked for confirmation or removed; no vague claims remain.
- [ ] Quick Start uses a real entry point, executable commands, and an observable verification result.
- [ ] MCP/CLI tool names, subcommands, arguments, defaults, host configuration, and error codes were checked individually.
- [ ] Every code block has a language identifier.
- [ ] Every URL, local document, image path, package reference, and community link exists or has been confirmed by the user.
- [ ] No API key, token, or secret appears in the README.
- [ ] Security claims (read-only, least privilege, no upload, fail-closed, automatic backup, etc.) have source, test, or configuration evidence.
- [ ] For local data, credentials, network, or model calls, the README explains what is read, written, and sent outside the machine.
- [ ] Dry-run, confirmation, backup, rollback, and permission requirements for dangerous writes match the implementation.

## Presentation and Language

- [ ] The first screen has the project name, one-line summary, and shortest run path; badges, logos, and language switches actually exist.
- [ ] The capability summary names commands/tools, inputs and outputs, and default safety posture instead of using slogans.
- [ ] Long READMEs include anchor navigation, with section names organized around reader tasks rather than internal implementation.
- [ ] Avoid emoji by default. If requested, do not rely on emoji to carry meaning.
- [ ] No empty headings, `TODO`, "coming soon," or "under construction" remains.
- [ ] GIFs, screenshots, and terminal output add real information; no demo is fabricated to fill the first screen.
- [ ] Usage is organized around tasks instead of copying the full `--help` option list.
- [ ] Feature bullets pair a user outcome with implementation evidence and remain selective.
- [ ] MCP projects include verified host configuration and a service-discovery check without exposing secrets.
- [ ] Troubleshooting includes a real symptom/error, cause, and action instead of generic restart/reinstall advice.
- [ ] Known limitations cover actual platform, version, data, compatibility, or capability boundaries.
- [ ] Development commands work; deeper documentation is grouped by purpose (proposal, ADR, standards, security, tests, releases, etc.).
- [ ] Rationale and design trade-offs follow usage and Quick Start.
- [ ] No unverifiable claims such as "industry-leading," "extreme performance," or "zero configuration" remain.
- [ ] Architecture diagrams show only verifiable components and relationships.
- [ ] Every line helps readers decide, trust, or act; remove the rest.
- [ ] English is consistent in prose and examples, except intentional product names, commands, and identifiers.
- [ ] No generic filler or formulaic language remains.

## Existing README

- [ ] If a README already exists, the user was asked how to handle it (replace, retain sections, or expand).
- [ ] In improve mode, list what to keep, move, merge, or delete and why.

## Write Gate

- [ ] Write to disk only after the user explicitly asks to write or save the file.
- [ ] Confirm the target path before writing; do not invent one.
- [ ] Re-run the checklist immediately before writing.

# Official Sources and Integration Choices

Research date: 2026-10-23. These are primary sources from their maintainers. “Official” refers to each project; there is no single official testing-skill standard across tools. Links may evolve; verify target versions when updating this skill.

## Official Skills and Testing Agents

| Source | What is incorporated | Scope and exclusions |
| --- | --- | --- |
| [Anthropic webapp-testing](https://github.com/anthropics/skills/blob/main/skills/webapp-testing/SKILL.md) | Observe the real page before interacting; manage test-server lifecycle; collect screenshots/logs. | Local web focus. Do not copy its Python stack or helper scripts. `networkidle` and fixed waits in examples are not general rules. |
| [Microsoft Playwright skill](https://github.com/microsoft/playwright/blob/main/packages/playwright-core/src/tools/skills/playwright-cli/SKILL.md) and [test-generation reference](https://github.com/microsoft/playwright/blob/main/packages/playwright-core/src/tools/skills/playwright-cli/references/test-generation.md) | Separate scenario planning, generation, execution, and failure diagnosis; add explicit assertions after recording actions; distinguish test drift from product regression. | Playwright-specific. Do not copy CLI tables, one-test-per-file rules, or change the contract automatically to match observed incorrect behavior. |
| [Playwright Test Agents](https://playwright.dev/docs/test-agents) | Consider separation of planner, generator, and healer responsibilities. | This is an official testing-agent workflow, not a generic skill. Installing these agents or delegating to subagents is not required. |

## Official Testing Guidance

- [Microsoft .NET unit testing best practices](https://learn.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices): readable naming, clear arrange/act/assert, isolation, and stability; follow the target project's syntax.
- [Testing Library guiding principles](https://testing-library.com/docs/guiding-principles/): verify observable behavior through realistic usage rather than private component state.
- [Playwright best practices](https://playwright.dev/docs/best-practices) and [page readiness API](https://playwright.dev/docs/api/class-page#page-wait-for-load-state): semantic locators, isolation, and retrying assertions; follow current API guidance for readiness.
- [pytest parametrization](https://docs.pytest.org/en/stable/how-to/parametrize.html) and [flaky tests](https://docs.pytest.org/en/stable/explanation/flaky.html): representative parameterized inputs and diagnosis of state/order problems; pytest plugins are not assumed to be built in.
- [Google Testing Blog — Code Coverage Best Practices](https://testing.googleblog.com/2020/08/code-coverage-best-practices.html): coverage identifies gaps; there is no universal cross-project threshold. Combine it with effectiveness evidence.
- [Stryker mutant states and metrics](https://stryker-mutator.io/docs/mutation-testing-elements/mutant-states-and-metrics/): distinguish killed, survived, no-coverage, and invalid mutations; use focused mutation according to risk, not as a mandatory tool or score.
- [Hypothesis quickstart](https://hypothesis.readthedocs.io/en/latest/quickstart.html): generated cases supplement explicit examples for large input domains; properties, domains, and reproducibility still need to be clear.

## This Repository's Synthesis

The requirement-first expected result, risk-to-case mapping, complete pre-submission suite, and evidence report are a synthesis for this repository's development practices; no individual source is claimed to require every step. Keep TDD in `code-implementation`, comprehensive review in `code-review`, and delivery process in `github-engineering-workflow` to avoid duplicating ownership.

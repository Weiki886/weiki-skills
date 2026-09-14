# weiki-skills

面向 AI 编码 agent 的可复用 skills 仓库，沉淀可复现的软件开发方法论。

![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Agent Skills](https://img.shields.io/badge/Agent%20Skills-open%20standard-6C48D7.svg)

本仓库把「怎么把一个项目从想法交付到生产再复盘」这类高频、可复用的工程经验，封装成 AI 编码 agent 可直接加载的 skill。每个 skill 都是自包含的指令包：`SKILL.md` 负责触发与主流程，`references/` 放按需加载的细则与模板，`agents/` 放可选 subagent 配置。

skill 遵循开放的 **Agent Skills** 标准（`SKILL.md`），因此同一份内容可以在 Codex、Claude Code、Cursor、Trae 等工具间复用，只是安装目录不同。

与一次性 Prompt 不同，skill 会长期维护、随场景裁剪强度，目标是在不同项目里反复得到一致、可验证的结果。

## 为什么是「工程交付」而不是「代码片段」

代码片段到处都有，但「交付过程」的决策更值钱：什么时候该上 Proposal、Issue 和 Milestone，什么时候直接 Commit；PR 到底该小到什么程度；一次发布要绑哪些证据才能可回溯；出了事故先回滚还是先修。这类判断一旦写清楚并被 agent 稳定执行，比任何单一函数的实现都更能影响项目质量。

## 已收录 Skills

| Skill | 定位 | 语言 | 路径 |
| --- | --- | --- | --- |
| [github-engineering-workflow](skills/github-engineering-workflow) | 以 GitHub 为载体的软件工程交付规范：Proposal/ADR → Milestone → Issue → PR → Review/CI → Merge → Release → 复盘，先区分个人/团队交付场景再按协作规模裁剪流程强度 | 中文 | `skills/github-engineering-workflow` |
| [content-writing](skills/content-writing) | 内容写作：技术文章 + 营销文案，先区分 technical / marketing 文体，覆盖选题、结构、初稿、事实核查与反 AI 味校对 | 中文 | `skills/content-writing` |
| [code-review](skills/code-review) | 代码审查：先钉住 diff 基线并区分 self / peer 场景，再按规范符合度与需求符合度双轴分别审查，输出带严重度分级的意见与 verdict | 中文 | `skills/code-review` |
| [code-implementation](skills/code-implementation) | 代码实现：把需求实现成正确、可验证、可合入的代码，覆盖澄清需求 → 实现计划 → TDD 红绿重构 → 系统化调试 → 交付验证，完成后交给 code-review 收口 | 中文 | `skills/code-implementation` |
| [project-readme](skills/project-readme) | 项目 README：为 CLI、MCP 和本地开发者工具新建、改进或审计 README，强调证据阶梯、可运行 Quick Start、任务化用法、安全/数据边界、排错与已知限制 | 中文 | `skills/project-readme` |

### github-engineering-workflow 包含

```
skills/github-engineering-workflow/
├── SKILL.md                    # 触发条件 + 主流程（先定场景 → 标准链路 → 事故 → 输出要求）
├── references/
│   ├── practices.md            # 风险门禁矩阵、仓库治理、CI 分层、供应链安全、部署、DORA 度量
│   └── templates.md            # DoR/DoD、Proposal/ADR/Milestone/Issue/PR/Release/紧急变更模板
└── agents/
    └── openai.yaml             # 可选 subagent 配置（仅 Codex 使用）
```

### content-writing 包含

```
skills/content-writing/
├── SKILL.md                    # 触发 + 先定文体 + 写作前 7 问 + 主流程 + 两条路径红线
├── references/
│   ├── technical.md            # 技术文章结构、代码规范、准确性与可复现
│   ├── marketing.md            # PAS/AIDA、痛点拆解、CTA 与社会证明
│   ├── seo.md                  # 标题/关键词/摘要 SEO（两文体共用）
│   └── checklist.md            # 事实核查 + 反 AI 味 + 校对
└── agents/
    └── openai.yaml             # 可选 subagent 配置（仅 Codex 使用）
```


### code-review 包含

```
skills/code-review/
├── SKILL.md                    # 触发 + 先定场景 + 钉基线 + 双轴审查 + 严重度分级 + 主流程
├── references/
│   ├── checklist.md            # 五个质量维度详细清单、按变更类型专项检查、完整勾选清单
│   ├── smells.md               # 设计坏味道基线（Fowler 12 种 + AI 生成代码特有坏味道）
│   └── feedback.md             # 审查意见的写法与话术、接收意见的规则与响应流程、分歧处理
└── agents/
    └── openai.yaml             # 可选 subagent 配置（仅 Codex 使用）
```

### code-implementation 包含

```
skills/code-implementation/
├── SKILL.md                    # 触发 + 先定场景（throwaway/maintained）+ 强度档位 + 主流程七步 + 输出要求
├── references/
│   ├── clarify.md              # 澄清需求 7 问 + design tree 拷问 + 模糊需求红旗
│   ├── ladder.md               # 七层梯子——写代码前先问七层，最好的代码是没写的代码
│   ├── planning.md             # 实现计划结构 + 无占位符约束 + 自检清单
│   ├── tdd.md                  # 红绿重构循环 + 好/坏测试 + 常见借口
│   ├── debugging.md            # 四阶段系统化调试 + 常见反例
│   └── verification.md         # 交付前验证门 + 常见伪证据
└── agents/
    └── openai.yaml             # 可选 subagent 配置（仅 Codex 使用）
```

### project-readme 包含

```
skills/project-readme/
├── SKILL.md                    # 触发 + solo/public 与 create/improve/audit + 证据阶梯 + 主流程
├── references/
│   ├── structure.md            # 阅读流、CLI/MCP 等项目类型取舍、安全边界、排错与设计文档写法
│   ├── evidence.md             # 仓库扫描清单、安全/数据面证据、禁止编造项
│   └── checklist.md            # README 写入前质量门禁
└── agents/
    └── openai.yaml             # 可选 subagent 配置（仅 Codex 使用）
```

## 目录结构

```
weiki-skills/
├── README.md                   # 本文件
├── LICENSE                     # MIT
├── .gitignore                  # 忽略 .DS_Store 等
└── skills/                     # 每个 skill 一个目录（单一事实来源，各工具共用）
    └── <skill-name>/
        ├── SKILL.md            # 入口：name + description + 主流程
        ├── references/         # 按需加载的细则与模板，不默认全文加载
        └── agents/             # 可选 subagent 配置
```

## 支持的工具

skill 使用开放的 Agent Skills 格式，同一份目录可直接用于以下工具，只需放到各自对应的目录：

| 工具 | 全局目录 | 项目目录 | 说明 |
| --- | --- | --- | --- |
| Codex | `~/.codex/skills/` | — | 本仓库主目标；`agents/openai.yaml` 仅 Codex 使用 |
| Claude Code | `~/.claude/skills/` | `.claude/skills/` | 同 `SKILL.md`，忽略 `agents/` |
| Cursor | `~/.cursor/skills/` | `.cursor/skills/` | 同 `SKILL.md`，忽略 `agents/` |
| Trae | `~/.trae/skills/`（国内版 `~/.trae-cn/skills/`） | `.trae/skills/` | 同 `SKILL.md`，忽略 `agents/` |

> 保持「单一事实来源」：请勿为不同工具各复制一份内容再分别维护，否则必然漂移。需要时把 `skills/<name>/` 复制或软链到目标工具的目录即可。

## 安装

### 方式一：Codex 内置 `skill-installer`（推荐，仅 Codex）

在 Codex 中对它说：

```
安装 Weiki886/weiki-skills 仓库里的 github-engineering-workflow、content-writing、code-review、code-implementation 和 project-readme skill
```

或直接调用安装脚本（等价命令）：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo Weiki886/weiki-skills \
  --path skills/github-engineering-workflow skills/content-writing skills/code-review skills/code-implementation skills/project-readme
```

安装后 skill 落到 `~/.codex/skills/`，重启 Codex（或下一个会话）后可用。

### 方式二：git clone + 复制到目标目录

通用做法，适用于任意工具：

```bash
git clone https://github.com/Weiki886/weiki-skills.git

# Codex
cp -R weiki-skills/skills/github-engineering-workflow ~/.codex/skills/
cp -R weiki-skills/skills/content-writing ~/.codex/skills/
cp -R weiki-skills/skills/code-review ~/.codex/skills/
cp -R weiki-skills/skills/code-implementation ~/.codex/skills/
cp -R weiki-skills/skills/project-readme ~/.codex/skills/

# Claude Code
cp -R weiki-skills/skills/github-engineering-workflow ~/.claude/skills/
cp -R weiki-skills/skills/content-writing ~/.claude/skills/
cp -R weiki-skills/skills/code-review ~/.claude/skills/
cp -R weiki-skills/skills/code-implementation ~/.claude/skills/
cp -R weiki-skills/skills/project-readme ~/.claude/skills/

# Cursor（全局）
cp -R weiki-skills/skills/github-engineering-workflow ~/.cursor/skills/
cp -R weiki-skills/skills/content-writing ~/.cursor/skills/
cp -R weiki-skills/skills/code-review ~/.cursor/skills/
cp -R weiki-skills/skills/code-implementation ~/.cursor/skills/
cp -R weiki-skills/skills/project-readme ~/.cursor/skills/

# Trae（国内版全局）
cp -R weiki-skills/skills/github-engineering-workflow ~/.trae-cn/skills/
cp -R weiki-skills/skills/content-writing ~/.trae-cn/skills/
cp -R weiki-skills/skills/code-review ~/.trae-cn/skills/
cp -R weiki-skills/skills/code-implementation ~/.trae-cn/skills/
cp -R weiki-skills/skills/project-readme ~/.trae-cn/skills/
```

### 方式三：下载 zip 后解压

下载仓库 zip、解压后，把 `skills/` 下需要的 skill 目录（如 `github-engineering-workflow`、`content-writing`、`code-review`、`code-implementation`、`project-readme`）放到上表对应工具的目录即可。

## 使用

skill 由 `SKILL.md` 的 `description` 负责被触发，不需要手动「调用」。例如在做以下事情时，agent 会自动加载 `github-engineering-workflow`：

- 搭建或完善一个仓库的 GitHub 开发规范
- 把一轮迭代拆成 Issue、PR 并设计门禁
- 写/审一个 Proposal、ADR、Milestone、Issue 或 PR
- 准备一次可复现的 Release，或复盘一次交付/事故

加载后，它会先让你确认「个人 / 团队」交付场景，再按协作规模裁剪流程强度，避免用统一重量级流程压到个人小项目上。

`content-writing` 会在你要求写技术文章或营销文案时触发：写博客 / 教程 / 公众号，或写产品介绍 / 获客转化 / 品牌内容。加载后先区分 technical / marketing 文体，再走「选题 → 结构 → 初稿 → 事实核查 → 反 AI 味校对」。

`code-implementation` 会在你要求实现新功能、修复缺陷或重构代码时触发：写实现 / 修 bug / 做重构。加载后先区分一次性（throwaway）与需维护（maintained）场景，再走「澄清需求 → 实现计划 → TDD 红绿重构 → 系统化调试 → 交付验证」，完成后交给 `code-review` 收口。

`project-readme` 会在你要求新建、重写、改进或审计项目 README 时触发。加载后先区分 solo/public 与 create/improve/audit，再扫描仓库证据，重点保证 Quick Start 可运行、命令和配置真实存在、安全/数据边界与排错信息不虚构；已有 README 会先确认处理方式，不会直接覆盖。

## 约定

- **语言**：skill 正文以中文为主，专业术语保留英文原词（Issue / PR / CI / ADR / Conventional Commits / SemVer 等）。
- **粒度**：一个 skill 只负责一条正交的能力线，不把无关内容揉进同一个 `SKILL.md`。
- **按需加载**：主流程写进 `SKILL.md`；细则、模板放 `references/`，agent 按需读取，不默认全文加载。
- **可裁剪**：每个 skill 都区分场景强度，个人/一次性项目从轻，团队/多迭代项目按门禁。

## License

[MIT](LICENSE) © 2026 Weiki

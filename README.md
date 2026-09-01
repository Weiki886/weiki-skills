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
安装 Weiki886/weiki-skills 仓库里的 github-engineering-workflow skill
```

或直接调用安装脚本（等价命令）：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo Weiki886/weiki-skills \
  --path skills/github-engineering-workflow
```

安装后 skill 落到 `~/.codex/skills/`，重启 Codex（或下一个会话）后可用。

### 方式二：git clone + 复制到目标目录

通用做法，适用于任意工具：

```bash
git clone https://github.com/Weiki886/weiki-skills.git

# Codex
cp -R weiki-skills/skills/github-engineering-workflow ~/.codex/skills/

# Claude Code
cp -R weiki-skills/skills/github-engineering-workflow ~/.claude/skills/

# Cursor（全局）
cp -R weiki-skills/skills/github-engineering-workflow ~/.cursor/skills/

# Trae（国内版全局）
cp -R weiki-skills/skills/github-engineering-workflow ~/.trae-cn/skills/
```

### 方式三：下载 zip 后解压

下载仓库 zip、解压后，把 `skills/github-engineering-workflow` 放到上表对应工具的目录即可。

## 使用

skill 由 `SKILL.md` 的 `description` 负责被触发，不需要手动「调用」。例如在做以下事情时，agent 会自动加载 `github-engineering-workflow`：

- 搭建或完善一个仓库的 GitHub 开发规范
- 把一轮迭代拆成 Issue、PR 并设计门禁
- 写/审一个 Proposal、ADR、Milestone、Issue 或 PR
- 准备一次可复现的 Release，或复盘一次交付/事故

加载后，它会先让你确认「个人 / 团队」交付场景，再按协作规模裁剪流程强度，避免用统一重量级流程压到个人小项目上。

## 约定

- **语言**：skill 正文以中文为主，专业术语保留英文原词（Issue / PR / CI / ADR / Conventional Commits / SemVer 等）。
- **粒度**：一个 skill 只负责一条正交的能力线，不把无关内容揉进同一个 `SKILL.md`。
- **按需加载**：主流程写进 `SKILL.md`；细则、模板放 `references/`，agent 按需读取，不默认全文加载。
- **可裁剪**：每个 skill 都区分场景强度，个人/一次性项目从轻，团队/多迭代项目按门禁。

## License

[MIT](LICENSE) © 2026 Weiki

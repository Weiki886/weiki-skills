# weiki-skills

面向 Codex 的可复用 skills 仓库，沉淀可复现的软件开发方法论。

![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Codex](https://img.shields.io/badge/Codex-skills-6C48D7.svg)

本仓库把「怎么把一个项目从想法交付到生产再复盘」这类高频、可复用的工程经验，封装成 Codex 可以直接加载的 skill。每个 skill 都是自包含的指令包：`SKILL.md` 负责触发与主流程，`references/` 放按需加载的细则与模板，`agents/` 放可选 subagent 配置。

与一次性 Prompt 不同，skill 会长期维护、随场景裁剪强度，目标是在不同项目里反复得到一致、可验证的结果。

## 为什么是「工程交付」而不是「代码片段」

代码片段到处都有，但「交付过程」的决策更值钱：什么时候该上 Proposal、Issue 和 Milestone，什么时候直接 Commit；PR 到底该小到什么程度；一次发布要绑哪些证据才能可回溯；出了事故先回滚还是先修。这类判断一旦写清楚并被模型稳定执行，比任何单一函数的实现都更能影响项目质量。

本仓库目前从**软件工程交付规范**起步，按需逐步补齐调试、架构、需求等正交能力。

## 已收录 Skills

| Skill | 定位 | 语言 | 安装路径 |
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
    └── openai.yaml             # 可选 subagent 配置
```

## 目录结构

```
weiki-skills/
├── README.md                   # 本文件
├── LICENSE                     # MIT
├── .gitignore                  # 忽略 .DS_Store 等
└── skills/                     # 每个 skill 一个目录
    └── <skill-name>/
        ├── SKILL.md            # 入口：name + description + 主流程
        ├── references/         # 按需加载的细则与模板，不默认全文加载
        └── agents/             # 可选 subagent 配置
```

## 安装

### 方式一：Codex 内置 `skill-installer`（推荐）

在 Codex 中对它说：

```
安装 Weiki886/weiki-skills 仓库里的 github-engineering-workflow skill
```

或直接调用 skill-installer 的安装脚本（等价命令）：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo Weiki886/weiki-skills \
  --path skills/github-engineering-workflow
```

安装后 skill 会落到 `~/.codex/skills/`，重启 Codex（或下一个会话）后可用。

### 方式二：git clone + 手动复制

```bash
git clone https://github.com/Weiki886/weiki-skills.git
cp -R weiki-skills/skills/github-engineering-workflow ~/.codex/skills/
```

### 方式三：下载单个目录后解压

也可以直接下载仓库 zip、解压后只把 `skills/github-engineering-workflow` 放进 `~/.codex/skills/`。

## 使用

skill 由 `SKILL.md` 的 `description` 负责被触发，不需要手动「调用」。例如在做以下事情时，Codex 会自动加载 `github-engineering-workflow`：

- 搭建或完善一个仓库的 GitHub 开发规范
- 把一轮迭代拆成 Issue、PR 并设计门禁
- 写/审一个 Proposal、ADR、Milestone、Issue 或 PR
- 准备一次可复现的 Release，或复盘一次交付/事故

加载后，它会先让你确认「个人 / 团队」交付场景，再按协作规模裁剪流程强度，避免用统一重量级流程压到个人小项目上。

## 约定

- **语言**：skill 正文以中文为主，专业术语保留英文原词（Issue / PR / CI / ADR / Conventional Commits / SemVer 等）。
- **粒度**：一个 skill 只负责一条正交的能力线，不把无关内容揉进同一个 `SKILL.md`。
- **按需加载**：主流程写进 `SKILL.md`；细则、模板放 `references/`，模型按需读取，不默认全文加载。
- **可裁剪**：每个 skill 都区分场景强度，个人/一次性项目从轻，团队/多迭代项目按门禁。

## Roadmap

- [x] `github-engineering-workflow` — 软件工程交付规范
- [ ] `dev-debugging`（候选）— 复现、假设、二分定位、日志/指标到根因的调试方法论
- [ ] `project-resume-writer`（候选）— 从项目源码、文档与工作笔记生成简历向材料

> Roadmap 只是方向记录，不代表当前已有这些 skill；以 [已收录 Skills](#已收录-skills) 列表为准。

## License

[MIT](LICENSE) © 2026 Weiki

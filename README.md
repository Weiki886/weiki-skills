# weiki-skills

面向 AI 编码 agent 的可复用 skills 仓库，覆盖产品设计、GitHub 工程交付、代码实现、代码审查、内容写作与项目 README。

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**目录：** [快速开始](#快速开始) · [已收录 Skills](#已收录-skills) · [目录结构](#目录结构) · [支持的工具](#支持的工具) · [安装](#安装) · [使用](#使用) · [设计取舍](#设计取舍) · [安全边界](#安全边界) · [已知限制](#已知限制) · [开发与验证](#开发与验证) · [贡献](#贡献) · [许可证](#许可证)

---

仓库目前收录 6 个 `SKILL.md` skill。每个 skill 都是自包含目录：入口 `SKILL.md` 决定何时加载，`references/` 放按需读取的细则，`agents/openai.yaml` 只供 Codex 使用。安装时把需要的 skill 目录复制或安装到目标 agent 的 skills 目录，不需要为不同工具复制不同版本。

## 快速开始

以安装 `project-readme` 到 Codex 为例：

```sh
# 1. 使用 Codex 内置安装脚本，把 skill 安装到 Codex 的 skills 目录
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo Weiki886/weiki-skills \
  --path skills/project-readme

# 2. 验证入口文件存在
test -f ~/.codex/skills/project-readme/SKILL.md && echo "project-readme installed"
```

看到 `project-readme installed` 后，重启 Codex 或开始新会话；之后提出「为这个项目新建/改进/审计 README」一类请求时，agent 就可以加载 `project-readme`。其他 agent 的安装目录见[支持的工具](#支持的工具)。

## 已收录 Skills

| Skill | 定位 | 主要入口 |
| --- | --- | --- |
| [product-design](skills/product-design) | 产品设计决策规范，覆盖调研、共识、两步法（先文字后原型）与 Proposal 定稿，按有无界面与 MiniSpec/FullSpec 裁剪详略 | `skills/product-design/SKILL.md` |
| [github-engineering-workflow](skills/github-engineering-workflow) | 以 GitHub 为载体的软件工程交付规范，先区分个人/团队场景，再按协作规模裁剪 Issue、PR、Review/CI、Release 与复盘流程 | `skills/github-engineering-workflow/SKILL.md` |
| [content-writing](skills/content-writing) | 内容写作规范，先区分技术文章与营销文案，再按读者、目的与平台决定结构、语气、事实核查与反 AI 味校对 | `skills/content-writing/SKILL.md` |
| [code-review](skills/code-review) | 代码审查规范，先钉住 diff 基线并区分 self/peer 场景，再按规范符合度与需求符合度双轴审查 | `skills/code-review/SKILL.md` |
| [code-implementation](skills/code-implementation) | 代码实现规范，覆盖澄清需求、实现计划、TDD 红绿重构、系统化调试与交付前验证 | `skills/code-implementation/SKILL.md` |
| [project-readme](skills/project-readme) | 项目 README 撰写与审计规范，强调仓库证据、可运行 Quick Start、任务化用法、安全/数据边界、排错与已知限制 | `skills/project-readme/SKILL.md` |

## 特性

- **按能力线拆分** —— 每个 skill 只负责一个可复用决策域，避免把写作、实现、审查和发布规范混成一个入口。
- **入口轻、细则按需加载** —— 每个目录保留 `SKILL.md` 主流程，细节放在 `references/`，agent 只在任务需要时读取。
- **先定场景再执行** —— `github-engineering-workflow` 区分个人/团队，`content-writing` 区分技术/营销，`code-implementation` 区分一次性/需维护，`code-review` 区分 self/peer，`project-readme` 区分 solo/public 与 create/improve/audit，`product-design` 区分有无界面与 MiniSpec/FullSpec。
- **强调可验证证据** —— 各 skill 都要求命令、测试、diff 基线、仓库事实或文档链接真实存在，不把猜测写成已完成。
- **跨工具复用同一份内容** —— skill 使用开放的 Agent Skills 目录格式；不同 agent 只需要各自的安装目录，不需要多套正文。

## 目录结构

```text
weiki-skills/
├── README.md
├── LICENSE
├── .gitignore
└── skills/
    ├── <skill-name>/
    │   ├── SKILL.md
    │   ├── references/
    │   └── agents/openai.yaml
    └── ...
```

各 skill 的资源清单：

```text
skills/github-engineering-workflow/
├── SKILL.md
├── references/
│   ├── practices.md
│   └── templates.md
└── agents/openai.yaml

skills/content-writing/
├── SKILL.md
├── references/
│   ├── checklist.md
│   ├── marketing.md
│   ├── seo.md
│   └── technical.md
└── agents/openai.yaml

skills/code-review/
├── SKILL.md
├── references/
│   ├── checklist.md
│   ├── feedback.md
│   ├── overengineering.md
│   └── smells.md
└── agents/openai.yaml

skills/code-implementation/
├── SKILL.md
├── references/
│   ├── clarify.md
│   ├── debugging.md
│   ├── ladder.md
│   ├── planning.md
│   ├── tdd.md
│   └── verification.md
└── agents/openai.yaml

skills/project-readme/
├── SKILL.md
├── references/
│   ├── checklist.md
│   ├── evidence.md
│   └── structure.md
└── agents/openai.yaml

skills/product-design/
├── SKILL.md
├── references/
│   ├── process.md
│   └── proposal-template.md
└── agents/openai.yaml
```

## 支持的工具

| 工具 | 全局目录 | 项目目录 |
| --- | --- | --- |
| Codex | `~/.codex/skills/` | — |
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| Cursor | `~/.cursor/skills/` | `.cursor/skills/` |
| Trae | `~/.trae/skills/`；国内版 `~/.trae-cn/skills/` | `.trae/skills/` |

不同工具读取的是同一套 `SKILL.md` 与 `references/`；`agents/openai.yaml` 只由 Codex 使用，其他工具可以忽略该文件。

## 安装

### 方式一：Codex 内置 `skill-installer`

安装全部 6 个 skill：

```sh
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo Weiki886/weiki-skills \
  --path skills/product-design skills/github-engineering-workflow skills/content-writing skills/code-review skills/code-implementation skills/project-readme
```

只安装其中一个时，把 `--path` 改成对应目录即可。安装完成后重启 Codex 或开始新会话。

### 方式二：clone 后复制

```sh
git clone https://github.com/Weiki886/weiki-skills.git
cd weiki-skills

# 示例：把 project-readme 安装到 Codex 全局 skills 目录
cp -R skills/project-readme ~/.codex/skills/
```

Claude Code、Cursor、Trae 只需要把 `~/.codex/skills/` 换成上表中的对应目录。只想安装部分 skill 时，逐条复制需要的目录；不要复制整个 `skills/` 后再手工删除，以免误装未确认内容。

### 方式三：下载仓库压缩包

下载仓库 zip，解压后把 `skills/` 下需要的 skill 目录复制到目标工具的 skills 目录。

## 使用

skill 由 `SKILL.md` 的 `description` 决定何时加载，不需要手动记住命令。安装完成后，可以用这些任务触发对应 skill：

- `product-design`：新功能或较大需求在开发前做产品设计、撰写或评审 Proposal/PRD、做竞品调研、文字定稿后补原型。
- `github-engineering-workflow`：搭建仓库开发规范、拆解 Issue/PR、准备 Release、复盘一次交付或事故。
- `content-writing`：写技术文章、教程、源码解读、产品介绍或营销文案。
- `code-review`：审查 PR、分支、未提交改动，或处理别人给你的审查意见。
- `code-implementation`：实现新功能、修复缺陷、重构或做行为变更。
- `project-readme`：新建、重写、改进或审计项目 README。

需要显式指定时，也可以在请求中写 skill 名，例如 `使用 project-readme 审计这个仓库的 README`。

## 设计取舍

- 以 `SKILL.md` 为单一事实来源，不为每个 agent 维护多份正文，避免规则漂移。
- 规则优先覆盖「什么时候用什么流程、什么证据才算完成」，不替代具体项目的语言规范、框架约定或已有 `AGENTS.md`。
- 个人项目可以裁剪流程强度，但 Quick Start、证据、已知限制和安全边界不应被裁掉。
- 本仓库不追求 skill 数量；新增 skill 应覆盖一条新的正交能力线，而不是把已有 skill 改写成更长清单。

## 安全边界

skill 是文本指令包，不携带可执行安装脚本或独立依赖。风险主要来自安装动作与目标 agent 的执行权限：

| 动作 | 读取 | 写入 |
| --- | --- | --- |
| 浏览或 clone 仓库 | GitHub 上的本仓库文件 | 本地 clone 目录 |
| Codex 安装脚本 | GitHub 仓库中的指定 skill | 指定的目标 skills 目录 |
| `cp -R` 手动安装 | 本仓库中的指定 skill 目录 | 目标工具的 skills 目录 |
| agent 加载 skill | skill 指令与项目上下文 | 取决于具体任务与 skill 授权范围 |

不要把密钥、私有配置或受限数据写入 skill、Issue、PR 或日志。安装前确认目标目录；agent 使用 skill 执行任务时，仍应遵守对应项目自己的安全与授权边界。

## 已知限制

- 当前收录 6 个 skill；不是覆盖所有开发场景的完整 skill 市场。
- 内容以中文为主，专业术语保留英文；需要英文读者协作时，应单独维护英文版本，不要在同一文件混写两套正文。
- 本仓库验证的是 `SKILL.md` 结构和安装路径，不保证第三方 agent 对 skill 的加载行为完全一致。
- `agents/openai.yaml` 只对 Codex 有效；其他工具不会使用这份元数据。
- 仓库当前没有独立 `CONTRIBUTING.md`、社区渠道或 CI 状态页；README 不会虚构这些入口。

## 开发与验证

本仓库的主要验证对象是每个 skill 目录的必需文件和 frontmatter。添加或修改 skill 后，至少检查：

```sh
# 确认入口文件存在
find skills -mindepth 2 -maxdepth 2 -name SKILL.md -print

# 在 macOS 上用 Ruby 校验 frontmatter 可解析
ruby -e 'require "yaml"; ARGV.each { |p| t = File.read(p); m = t.match(/\A---\n(.*?)\n---\n/m) or abort("missing frontmatter: #{p}"); y = YAML.safe_load(m[1], permitted_classes: [Symbol]); abort("bad name: #{p}") unless y["name"] == File.basename(File.dirname(p)); abort("missing description: #{p}") if y["description"].to_s.empty? }; puts "skill frontmatter OK"' skills/*/SKILL.md
```

本地通过 `~/.codex/skills/<name>` 指向仓库内 skill 目录时，修改会直接影响本机 Codex 行为；验证后再提交。

## 贡献

欢迎 Issue 与 Pull Request；除很小的修复外，请先开 Issue 对齐要新增的能力线、证据来源与是否会与现有 skill 重叠。提交信息与 PR 标题使用英文。

## 许可证

[MIT](LICENSE)

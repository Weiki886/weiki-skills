# weiki-skills

一组可复用的 Agent Skills，覆盖产品设计、前端设计、GitHub 工程流程、代码实现、测试、审查、技术写作和项目文档。

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**语言：** [English](README.md) | 简体中文

**目录：** [快速开始](#快速开始) · [Skills 列表](#skills-列表) · [仓库结构](#仓库结构) · [支持的工具](#支持的工具) · [安装](#安装) · [使用](#使用) · [设计取舍](#设计取舍) · [安全边界](#安全边界) · [已知限制](#已知限制) · [开发与校验](#开发与校验) · [贡献](#贡献) · [许可证](#许可证)

本仓库包含 10 个 `SKILL.md` skill。每个 skill 都是独立目录：`SKILL.md` 说明适用场景，`references/` 存放按需读取的细则，`assets/` 可存放复用资源，`agents/openai.yaml` 提供 Codex 专用元数据。只需安装需要的 skill 目录；同一份内容可供支持 skills 的工具使用。

## 快速开始

以在 Codex 中安装 `project-readme` 为例：

```sh
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo Weiki886/weiki-skills \
  --path skills/project-readme

test -f ~/.codex/skills/project-readme/SKILL.md && echo "project-readme installed"
```

看到 `project-readme installed` 后，重启 Codex 或开启新会话。之后可以在创建、改进或审查项目 README 时使用这个 skill。其他工具的安装目录见[支持的工具](#支持的工具)。

## Skills 列表

| Skill | 用途 | 入口文件 |
| --- | --- | --- |
| [architecture-design](skills/architecture-design) | 将已定稿需求转化为可演进的模块边界、数据归属、接口契约、风险决策，以及可直接进入实现的系统骨架。 | `skills/architecture-design/SKILL.md` |
| [product-design](skills/product-design) | 从调研和共识开始，以文字规格和可选原型完成产品决策，并按 MiniSpec/FullSpec 控制提案详略。 | `skills/product-design/SKILL.md` |
| [frontend-design](skills/frontend-design) | 创建、打磨或审查前端界面，建立符合产品主题的视觉方向，完成无障碍实现并验证实际渲染。 | `skills/frontend-design/SKILL.md` |
| [github-engineering-workflow](skills/github-engineering-workflow) | 面向个人或团队的 GitHub 工程交付流程，覆盖 Issue、PR、Review/CI、Release 和复盘。 | `skills/github-engineering-workflow/SKILL.md` |
| [content-writing](skills/content-writing) | 根据读者、目的和发布平台撰写技术内容或营销内容。 | `skills/content-writing/SKILL.md` |
| [code-review](skills/code-review) | 固定变更基线，从工程规范和需求符合度两个方面审查代码，并提供有证据的发现。 | `skills/code-review/SKILL.md` |
| [production-risk-review](skills/production-risk-review) | 合并前生产风险审查，重点关注数据丢失、数据库迁移、崩溃、故障扩散、发布顺序和回滚。 | `skills/production-risk-review/SKILL.md` |
| [code-implementation](skills/code-implementation) | 通过澄清、计划、适当的 TDD、调试和验证实现功能、修复缺陷、重构或行为变更。 | `skills/code-implementation/SKILL.md` |
| [code-testing](skills/code-testing) | 从需求和契约推导有效测试，覆盖边界和异常，验证测试能发现缺陷，并运行规定的测试套件。 | `skills/code-testing/SKILL.md` |
| [project-readme](skills/project-readme) | 基于仓库证据撰写或改进项目 README，包含可运行的 Quick Start、任务化用法、安全边界、排错和限制。 | `skills/project-readme/SKILL.md` |

## 仓库结构

```text
skills/
├── <skill-name>/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   └── references/
└── ...
```

入口文件保持聚焦，只有任务需要时才加载配套细则。

## 支持的工具

Skill 内容使用 Markdown 编写，可适配支持指令文件的 Agent。`agents/openai.yaml` 是 Codex 专用元数据；其他工具可能会忽略它。不同工具的安装路径和自动发现方式可能不同，请以目标工具当前的 skill 规范为准。

## 安装

### 使用 Codex 安装器

安装单个 skill：

```sh
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo Weiki886/weiki-skills \
  --path skills/<skill-name>
```

将 `<skill-name>` 替换为 `skills/` 下的目录名。

### 克隆并复制

```sh
git clone https://github.com/Weiki886/weiki-skills.git
cp -R weiki-skills/skills/<skill-name> ~/.codex/skills/
```

使用其他 Agent 时，将选中的 skill 目录复制到该工具文档所述的 skills 目录。安装多个 skill 时，只复制选中的目录，不要复制整个 `skills/` 文件夹。

### 下载压缩包

下载并解压仓库压缩包，再将 `skills/` 下选中的 skill 目录复制到目标工具的 skills 目录。

## 使用

每个 skill 会通过 `SKILL.md` 中的描述被发现。常见使用场景：

- `product-design`：开发前设计新功能或较大需求、撰写或评审 Proposal/PRD、调研替代方案，并在文字方案定稿后制作原型。
- `architecture-design`：将已定稿的产品需求转化为模块边界、数据归属、接口契约、技术风险决策，并在适用时搭建可编译的系统骨架。
- `frontend-design`：创建、重构、审查或打磨网站、仪表盘、产品界面、组件或表单，并验证实际渲染效果。
- `github-engineering-workflow`：建立仓库规范、拆分 Issue/PR、准备 Release，或复盘交付流程和事故。
- `content-writing`：撰写技术文章、教程、源码解读、产品介绍或营销文案。
- `code-review`：审查 PR、分支、未提交改动，或处理收到的审查意见。
- `production-risk-review`：评估 PR 是否可能导致数据库删除、数据损坏、服务崩溃、故障扩散或无法恢复的发布事故。
- `code-implementation`：实现新功能、修复缺陷、重构或变更行为。
- `code-testing`：为生成的代码补测试、增强薄弱用例、补充回归测试，或在提交前运行规定测试套件。
- `project-readme`：创建、改进或审查项目 README。

如需显式指定，可在请求中写出 skill 名，例如：`使用 project-readme 审查这个仓库的 README。`

测试示例：`使用 $code-testing 为本次改动补齐边界和异常测试，确认关键测试能够发现回归，并运行提交前规定的测试套件。`

生产风险审查示例：`使用 $production-risk-review 审查 PR #123 是否适合合并并发布，重点检查数据库迁移、批量删除、服务崩溃、故障扩散和回滚。`

## 设计取舍

- `SKILL.md` 是唯一事实来源；本仓库不为不同 Agent 维护多份重复正文。
- Skills 聚焦决策点、工作流和完成证据，不替代项目自己的语言规范、框架约定或 `AGENTS.md` 指令。
- 个人项目可以降低流程强度，但不应省略必要的验证、证据、已知限制或安全边界。
- 本仓库优先维护彼此独立的能力范围，不通过重述现有能力来增加 skill 数量。

## 安全边界

Skills 是指令文本，不包含可执行安装脚本或独立依赖。主要风险来自安装操作和目标 Agent 的执行权限。

| 操作 | 读取 | 写入 |
| --- | --- | --- |
| 浏览或克隆 | GitHub 上的仓库文件 | 本地克隆目录 |
| Codex 安装器 | 本仓库中选定的 skill | 指定的目标 skills 目录 |
| 手动 `cp -R` 安装 | 选定的 skill 目录 | 目标 Agent 的 skills 目录 |
| Agent 加载 skill | Skill 指令和项目上下文 | 取决于任务及其授权范围 |

不要把密钥、私有配置或受限数据写入 skills、Issue、PR 或日志。安装前确认目标目录。Agent 使用 skill 时，仍需遵守目标项目的安全和授权规则。

## 已知限制

- 本仓库目前包含 10 个 skills，并非覆盖所有工程场景的完整市场。
- README 提供英文和简体中文两个版本；产品名、代码标识和其他专有名词保留其规范写法。
- 仓库校验可以检查 skill 结构和元数据，但不能保证所有第三方 Agent 的加载行为完全一致。
- `agents/openai.yaml` 仅供 Codex 使用，其他工具可能忽略它。
- 仓库目前没有单独的 `CONTRIBUTING.md`、社区渠道或 CI 状态页面；本文档不会虚构这些入口。

## 开发与校验

至少应确认每个 skill 都有必需的入口文件，且 frontmatter 有效：

```sh
find skills -mindepth 2 -maxdepth 2 -name SKILL.md -print

ruby -e 'require "yaml"; ARGV.each { |p| t = File.read(p); m = t.match(/\A---\n(.*?)\n---\n/m) or abort("missing frontmatter: #{p}"); y = YAML.safe_load(m[1], permitted_classes: [Symbol]); abort("bad name: #{p}") unless y["name"] == File.basename(File.dirname(p)); abort("missing description: #{p}") if y["description"].to_s.empty? }; puts "skill frontmatter OK"' skills/*/SKILL.md
```

如果本地 `~/.codex/skills/<name>` 指向本仓库中的 skill，修改会直接影响本机 Codex 行为。提交前请先完成验证。

## 贡献

欢迎提交 Issue 和 Pull Request。除小型修复外，建议先通过 Issue 对齐要新增的能力范围、证据来源，以及是否与现有 skills 重叠。Commit 标题和 PR 标题必须使用英文。

## 许可证

[MIT](LICENSE)

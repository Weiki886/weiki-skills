# weiki-skills

我的个人 Codex skills 集合。内容以中文为主，专业术语保留英文原词（Issue / PR / CI / ADR / Conventional Commits 等）。

## 已收录 Skills

| Skill | 说明 |
| --- | --- |
| [github-engineering-workflow](skills/github-engineering-workflow) | 以 GitHub 为载体的软件工程交付规范：Proposal/ADR → Milestone → Issue → PR → Review/CI → Merge → Release → 复盘，先区分个人/团队交付场景再按协作规模裁剪流程强度。 |

## 安装

对 Codex 说「安装 skill」，或使用 `skill-installer` 从本仓库安装：

```
--repo Weiki886/weiki-skills --path skills/github-engineering-workflow
```

安装后进入 `~/.codex/skills/`，下一个会话可用。

## 目录约定

- 每个 skill 一个目录，`SKILL.md` 为入口。
- `references/` 放按需加载的细则与模板，不默认全文加载。
- `agents/` 放可选 subagent 配置。

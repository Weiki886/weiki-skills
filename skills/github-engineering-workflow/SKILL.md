---
name: github-engineering-workflow
description: 规划、执行或审计以 GitHub 为载体的软件工程交付流程，覆盖 Proposal/ADR、Milestone、Issue、分支、Commit、PR、CODEOWNERS、CI/CD、Review、Merge、不可变构建、渐进式部署、Release、可观测性与复盘。用于搭建或完善仓库开发规范、拆解迭代任务、实施代码变更、编写或评审 Issue/PR、设计质量与安全门禁、准备版本发布或复盘交付效能；先区分个人/团队交付场景，再按协作规模裁剪流程强度。
---

# GitHub 工程交付规范

把 GitHub 作为工程事实来源，保持 `Proposal → Milestone → Issue → PR → Review/CI → Merge → Build → Deploy → Observe/Learn` 全链路可追溯。

## 核心原则

1. 先完成产品取舍与架构设计，再进入开发；每轮交付走完「产品 → 架构 → 开发 → 验收 → 发布」。
2. 改动关联 Issue，每次合并经过 PR 与人工 Review；团队协作或多迭代交付时，让 Issue 归属 Milestone。
3. 主干保持可编译、可测试、可运行；小步提交、小 PR、及时合并。
4. 追加记录决策与设计变更，不覆盖旧判断。
5. AI 可辅助工程活动，但设计、评审、理解与最终交付责任在人；AI 生成代码须可解释、可验证。
6. 先确认仓库公开性与信息边界，密钥、凭据、受限数据不得进入公开仓库、Issue、PR、日志或 AI Prompt。
7. 构建一次，同一不可变产物逐环境晋级；不在部署阶段重新构建未经验证内容。
8. 流程强度随风险与协作规模调整；个人项目从轻，团队项目按门禁，不以统一阈值替代判断。

## 先定场景

动手前先判断交付形态，据此决定流程强度：

- **个人 / 一次性（solo）**：轻量。直接 Commit + 可选 Issue + Tag/Release 即可；Proposal/ADR、Milestone、CODEOWNERS、强制人工 Review、CI 门禁、DORA 度量均可裁掉。底线是主干可用、提交可回溯、发布可复现。
- **团队 / 多迭代（team）**：完整。启用 Proposal → Milestone → Issue → PR → Review/CI → Release 全链路，以及仓库治理与 DORA 度量。

缺省判定：多人协作、多轮迭代、共享发布或合规要求 → 按 team；单人自用、一次性 → 按 solo。混合场景按风险上探、不向下放松；后续「标准链路」默认按 team 描述，solo 项目按本节裁剪。

## 风险分级

给每个 Issue/PR 标风险并写明依据；沿用仓库已有分级，缺省四级：

- `low`：文档、测试、内部重构或不改变运行行为的机械性改动。
- `standard`：边界清楚、可独立回滚的一般功能或缺陷修复。
- `high`：涉及身份权限、资金、隐私、公开 API、数据迁移、基础设施、供应链、生产权限或较大爆炸半径。
- `emergency`：为恢复服务或处置正在发生的安全事件而必须加速的改动。

所有级别都保留「Issue → 改动 → 验证 → 人工 Review」审计链；仅获授权的 `emergency` 可临时压缩部分门禁，恢复后补齐。具体门禁矩阵见 `references/practices.md`。

## 开始与完成

开始实现前满足 Definition of Ready；关闭 Issue/Milestone 前满足 Definition of Done（清单见 `references/templates.md`）。

## 执行前建立上下文

1. 读仓库 `AGENTS.md`、`CONTRIBUTING*`、`README*`、`SECURITY*`、Issue/PR 模板、分支保护、CI、标签与发布约定。
2. 确认默认分支、当前 Milestone、关联 Issue、仓库可见性、测试命令与发布方式。
3. 确认系统关键性、数据敏感性、部署环境、风险分级与事故响应方式。
4. 沿用已有命名、标签、Commit 与合并策略；仅仓库无约定时采用本 Skill 默认。
5. 审计任务保持只读并输出证据；实施任务仅执行授权范围内的变更。

## 标准链路

以下默认按团队（team）场景描述；个人（solo）项目按「先定场景」裁剪到 Commit + 可选 Issue + Tag/Release。配套模板见 `references/templates.md`，细则见 `references/practices.md`。

1. **Proposal**：编码前写清用户、问题、价值、方案取舍、非目标、边界与验收标准；未被接受的 Proposal 不进入实现。
2. **架构**：由人主导模块边界、依赖、接口契约、数据模型与技术风险；接口和调用关系须真实存在，可留空或用 mock/桩。不以目录树或技术栈清单冒充架构。
3. **Milestone**：把一批 Issue 聚成一轮可验收交付，写清目标、范围、非目标、验收标准与预期 Release。个人或一次性项目可省略 Milestone，直接用 Release/Tag 作为交付单元；团队协作或多迭代交付时才建立。
4. **Issue**：把 Issue 作为过程基本单元，禁止空泛标题；含背景、目标、非目标、关联、风险、验收标准、子任务与验证计划。
5. **Commit**：从已接受且可执行的 Issue 开始；短生命周期分支频繁同步；每个 Commit 表达一个完整意图并保持可构建。缺省遵循 Conventional Commits 的 `<type>(<scope>): <summary>`，并与 SemVer 关联以自动算版本与 changelog。
6. **PR**：关联并关闭对应 Issue，范围与 Issue 一致；说明问题、关键改动、风险、验证证据、兼容性、部署与回滚，保持小而频繁。
7. **Review**：先自审，再请至少一名合适的人类 Reviewer；AI 检查只补充、不替代。检查方向、逻辑、边界、测试、CI 与可部署性；区分 `Blocking` 与 `Nit/Optional/FYI`。
8. **CI 与构建**：每提交触发可重复构建与快速检查，主干合并后再次验证；快速层放格式/静态检查/单测/关键安全，慢层放 e2e/性能/动态安全。flaky test 视为缺陷。
9. **Merge**：Review 通过且必需检查成功后按仓库策略合并；保持主干可用，合并后删除短生命周期分支，禁止绕过 PR 直接落主干。
10. **部署与 Release**：同一自动化流程把同一不可变产物逐环境晋级，部署与开放功能分开控制；高风险用 canary/渐进/Feature Flag。Milestone 结束创建可复现的 Release 与 Tag。

## 事故与紧急修复

- 先恢复用户服务，再保存时间线、告警、部署、Commit 与决策证据。
- 优先回滚到已知良好产物，或经正常流水线提交最小前滚修复。
- 紧急路径复用日常自动化，只压缩已授权等待；恢复后开展无责复盘，产出带 Owner 与期限的改进 Issue。

## 输出要求

完成后简洁输出：① 当前 Milestone 与交付目标；② 已创建/修改/检查的 Proposal、Issue、PR、Commit、Release 及链接；③ 已执行的测试/CI/人工验证与结果；④ 风险级别、产物、部署状态与健康证据；⑤ 未满足的门禁、例外与阻塞；⑥ 下一项最小可执行动作。无证据不得声称流程完整，不得以本地测试通过推断远程 CI/Review/Merge/Release 已完成。

## 参考资源

按需读取，不要全文加载：

- `references/practices.md`：风险门禁矩阵、仓库治理、CI 分层、Actions 与供应链安全、部署与渐进式发布、事故、DORA 度量、一手资料。
- `references/templates.md`：Definition of Ready/Done、Proposal/Milestone/Issue/PR/Release/紧急变更模板、Review 检查表、审计证据矩阵。

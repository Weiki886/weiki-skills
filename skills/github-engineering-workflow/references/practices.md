# 工程实践细则

校准日期：2026-08-14。把通用原则映射为仓库级策略；遇到平台功能、许可证、合规或团队约定差异时，以当前仓库与官方文档为准。

建议分三类：`MUST`（最低可追溯/可验证/有人负责基线）、`SHOULD`（默认采用，可有记录理由调整）、`CONDITIONAL`（规模/许可证/平台匹配时启用）。每项例外至少记录：被豁免规则、原因、影响范围、批准人、补偿控制、责任人与复查日期。

## 风险门禁矩阵

评估维度（任一明显升高即调高）：① 是否触及身份/权限/资金/隐私/合规/安全边界；② 是否修改公开 API、持久化数据、基础设施、IAM、密钥或构建发布链；③ 是否难回滚或回滚有损失；④ 是否扩大到多服务/地域/租户/大量用户；⑤ 是否缺自动测试、预发布环境、健康信号或成熟维护者。不要用代码行数降低风险判断。

| 门禁 | low | standard | high | emergency |
| --- | --- | --- | --- | --- |
| Issue、风险依据、可验证验收 | MUST | MUST | MUST | MUST，可事后补全 |
| 人类 Review 与必需 CI | MUST | MUST | MUST | SHOULD 走正常路径；绕过须授权留痕 |
| 领域 CODEOWNER | 按路径 | SHOULD | MUST | 可事后复核 |
| 威胁建模/隐私合规评审 | 不需要或精简 | 按影响 | MUST | 恢复后补 |
| 部署与回滚/前滚计划 | 可标不适用 | MUST | MUST 且演练关键步骤 | MUST，优先最小恢复路径 |
| 渐进式发布与自动健康门禁 | 可选 | 按爆炸半径 | MUST（平台允许） | 恢复后验证 |
| SBOM、来源证明、签名验证 | 按交付物 | SHOULD | MUST（可执行发布物） | 事后补齐 |
| 复盘与改进 Issue | 可选 | 失败时 | 失败或重大近失时 | MUST |

平台不支持某门禁时，用其他平台或人工审计控制替代并标 `UNKNOWN`/已接受风险，不得伪称已启用。

## 仓库治理

- 用 ruleset/分支保护禁止默认分支被删除或强推，要求经 PR、必需检查通过与适当审批合并。
- 关键路径（`.github/workflows/`、`CODEOWNERS`、发布脚本、IaC、身份权限、支付、数据迁移）要求领域 Owner Review，并保护这些敏感文件本身。
- 给必需检查使用唯一稳定名称，避免同名检查被错误满足或阻塞；条件工作流要对每个适用 PR 都报告结果，否则会永久 pending。
- 审批后差异实质变化时要求重新批准。
- 高吞吐主干用 Merge Queue，让候选在最新主干组合上重新通过必需检查（Actions 需处理 `merge_group` 事件）。
- 用 GitHub Environments 限制部署分支、审批、并发与环境凭据；保护规则通过前不释放生产凭据。
- 选择并记录一致合并策略，不擅自改变既有历史策略。
- 缺省采用 trunk-based / GitHub Flow：改到主干或极短生命周期分支（数小时～一天）；仅在需要长期维护多个受支持版本时才使用 Git Flow。
- 公共/开源仓库提供 `SECURITY.md` 说明漏洞报告渠道与响应期限，并配合默认安全标签、依赖与凭据扫描；纯私有内部仓库可省略。
- 开启签名提交与签名标签（GPG/SSH），视合规/供应链要求对公共仓库强制 Verified，个人项目可选。

## Labels、Tag 与 Topics

三者的职责必须分开：Labels 管理工作项，Git Tag 标记版本，Topics 帮助仓库被理解和发现。不要用一个对象承担另一个对象的职责。

### GitHub Labels

仓库没有现成约定时，采用小写、短横线分隔的分组命名，例如 `type:bug`；冒号前是维度，冒号后是值。建议目录如下：

| 维度 | 示例 | 规则 |
| --- | --- | --- |
| `type` | `type:feature`、`type:bug`、`type:docs`、`type:chore`、`type:security` | Issue 至少一个；通常只选一个主要类型 |
| `area` | `area:api`、`area:web`、`area:docs`、`area:ci` | 按影响范围选零个或多个；值应来自仓库模块 |
| `priority` | `priority:p0`、`priority:p1`、`priority:p2`、`priority:p3` | 需要排序时最多一个；先定义响应含义，不把 P0 当“重要”的装饰 |
| `risk` | `risk:low`、`risk:standard`、`risk:high`、`risk:emergency` | 与 Issue/PR 的风险分级一致，最多一个 |
| `status` | `status:blocked`、`status:needs-info`、`status:ready` | 只表示需要跨工具保留的特殊状态；常规 `open/closed`、看板状态和 Assignee 不重复做成 Label |

默认职责：Issue 创建者填写 `type`、已知 `area` 与风险；维护者在 triage 时校正并补充 `priority`。PR 至少继承关联 Issue 的 `area` 和 `risk`，改动范围变化时同步更新。安全、隐私、依赖漏洞和紧急修复使用专门类型或风险 Label，不能只写在正文里。

个人项目采用最小集合：`type` + 必要时的 `risk`；不为只有一个值的维度建立一整组 Labels。团队项目再按实际筛选、统计和责任边界增加 `area`、`priority`，并把目录写入仓库贡献指南或 `.github/labels.yml` 等可审查位置。颜色只用于视觉分组，不承载流程语义；同一维度的颜色保持一致。

Label 维护规则：

- 创建前先搜索同义 Label，避免同时出现 `bug`、`defect`、`type:bug`。
- 新 Label 必须有名称、含义、适用对象和 Owner；不再使用的 Label 先迁移现有 Issue/PR，再删除或标记废弃。
- 每个季度或每轮重要迭代清理重复、无 Owner、长期未使用的 Label；删除前检查搜索、自动化和报表规则。
- Label 只能辅助筛选，不能替代验收标准、风险依据、Milestone 或正文中的决策证据。

### Git Tag 与 Release

- 正式版本统一使用 `vMAJOR.MINOR.PATCH`，遵循 SemVer；预发布使用 `vMAJOR.MINOR.PATCH-rc.1`、`-beta.1` 等明确的预发布标识。
- Tag 必须指向已验证的源 Commit，正式 Tag 使用 annotated tag；公共发布或供应链风险较高时启用 GPG/SSH 签名并验证 `Verified`。
- Tag 创建后不移动、不删除后重建、不让同一个版本号指向不同 Commit。发现问题时发布新的 patch 版本；未公开且未被消费的预发布 Tag 才可按仓库约定废弃，并留下记录。
- Release 绑定 Tag、源 Commit、Milestone/交付范围、Issue/PR、变更说明、已知问题、验证结果和升级/回滚方式；可执行产物按风险附摘要、SBOM、来源证明或签名。
- 不给每个 Commit 随意打版本 Tag；个人项目在可用里程碑或对外发布时创建 Tag，团队项目按 Release 节奏创建 Tag。
- 部署和回滚按产物摘要执行，不重新解析可移动 Tag；Tag 是版本入口，不是部署状态或环境指针。

### GitHub Topics

- Topics 使用小写、短横线分隔，例如 `ai-agents`、`developer-tools`、`typescript`；不放 `v1.0.0`、`in-progress`、某个 Issue 编号或个人状态。
- 只保留能长期描述仓库的 3～10 个主题，优先产品领域、主要技术和使用场景；不要为了搜索流量堆同义词。
- README、仓库描述和 Topics 应相互一致；项目方向改变时同步更新，至少每次大版本或季度治理时检查一次。

## 决策记录（ADR）

- 对难以逆转、跨模块或影响长期演进的决策写 ADR，模板见 `templates.md`。
- 状态流转 Proposed → Accepted → Superseded / Deprecated；不修改已接受记录，决策变化时新建并引用被取代记录。

## 小批量开发与 Review

- 一个 PR 一个自洽目标并带相关测试；行为变化、实质重构、依赖升级、大规模格式化拆开。
- 每个中间状态可构建、可合并、可部署或用 Feature Flag 安全隐藏。
- 拆分按规模、概念数量、文件分散度、风险与 Reviewer 上下文判断，不设全局行数硬门槛。
- Review 审查设计、功能、复杂度、测试、命名、注释、文档与每行人工维护代码；隐私/安全/并发/无障碍/数据库/基础设施按需加专家 Reviewer。
- 标注 `Blocking` 与 `Nit/Optional/FYI`，说明原因并对代码不对人；明确改善整体健康度的改动允许合并，不以个人偏好追求完美。
- 给首次 Review 响应设可实现 SLO（Google 工程实践参考为 1 个工作日内，按团队实际调整），并尽早暴露方向性/架构性阻塞意见。

## CI 与测试反馈层

| 层 | 触发 | 内容 | 处置 |
| --- | --- | --- | --- |
| 快速 PR 层 | 每提交/PR | 格式、lint、类型、单测、快速构建、凭据检查 | 必需门禁，保持几分钟反馈 |
| 完整验证层 | PR 或按路径 | 集成、契约、e2e、兼容性、安全 | 按风险设必需或异步阻塞 |
| 主干层 | 合并后 | 重建/验证、冒烟、产物发布 | 失败立即修复或回退 |
| 发布层 | 产物晋级 | 环境验证、迁移、性能、动态安全、canary | 由环境与健康门禁控制 |
| 定期层 | 定时 | 全量回归、长时性能、灾备、依赖与基线审计 | 创建有责任人的风险项 |

- 让同一构建脚本尽量本地可用；主干至少每日集成。
- 主干破坏是团队最高优先级之一，短时间内修不好就回退致错改动。
- 慢测试拆分/并行/后移，不删除；flaky test 作为缺陷，隔离只能是带 Owner、Issue、期限的临时手段。

## Actions 与供应链安全

- `GITHUB_TOKEN` 默认只读、按 job 增权；不足时用最小权限 GitHub App token，避免宽权限 PAT。
- 云资源优先 OIDC 短期凭据，不持有长期密钥；敏感值不写入工作流，泄漏时删除日志并轮换。
- 不可信输入作为数据传给受控程序，不拼进 shell。
- 避免在 `pull_request_target`/`workflow_run` 特权上下文执行不可信 PR 代码；公共仓库外部 PR 用 GitHub 托管临时 Runner，私有自托管 Runner 隔离信任域、网络、密钥与 Runner Group。
- 第三方 Action 与可复用工作流固定到完整 Commit SHA 并验证来源，用自动更新工具提交升级 PR。
- 启用依赖图、依赖审查、漏洞更新与凭据扫描；依赖更新 PR 走正常 CI 与 Review，不因 Dependabot 创建就自动合并。
- 按风险运行 SAST/SCA/IaC/容器/DAST 扫描，规则与误报例外须版本化。
- 供应链目标按风险递进：先有来源证明，再托管构建签名与硬化构建平台，不无依据要求最高 SLSA；SBOM、provenance、VEX 分别说明组成/构建来源/可利用性，均不证明产物无漏洞。

## 构建、部署与 Release

- Build once：CI 构建一个权威、不可变、带版本与摘要的产物，测试/预发布/生产使用同一产物；环境差异只来自受版本控制的配置或密钥引用。
- 同一套自动化覆盖各环境，部署脚本到生产前反复验证；记录哪个产物、谁、何时部署到哪、smoke 与健康结果。
- 按加密摘要晋级与回滚，不依赖可移动 Tag 重新解析，不重复构建“同一版本”。
- Release 至少绑定：Tag、源 Commit、Milestone、Issue/PR、产物版本与摘要、测试/安全/部署证据、用户可感知变更、已知问题、升级与回滚方式，按风险附 SBOM/来源证明/签名。
- 用 SemVer 打版本：`feat`→minor、`fix`→patch、breaking（`!` 或 `BREAKING CHANGE:`）→major；用 Keep a Changelog 维护 `CHANGELOG.md`，按版本分组、按 Added/Changed/Fixed/Removed/Security 归类。

## API、数据库与 Feature Flag

- 用扩展—迁移—收缩（parallel change）交付库表与接口变化：先加兼容，再迁移读写，最后删旧。
- 迁移脚本纳入版本控制并在类生产环境验证；大型迁移提供进度、暂停、重试与恢复。
- 避免应用部署与不可逆 Schema 变更必须原子同步；无法解耦时把停机、备份、验证、前滚设为显式门禁。
- Feature Flag 要有 Owner、用途、默认值、有效环境、测试组合、观测指标与清理日期；不留永久 Flag 或隐藏分支。

## 可观测性与渐进式交付

上线前定义成功/失败信号：关键路径成功率与延迟、错误率、资源饱和、与目标相关的业务信号，并写清阈值、观察窗口、负责人与停止/回滚动作。

高风险改动：① 部署 + smoke；② 小比例实例/流量/租户开放；③ 与控制组/历史基线比对；④ 逐阶段扩大并保留观察窗口；⑤ 触发停止条件自动或授权回滚；⑥ 记录每阶段产物、流量、指标与决策。canary/blue-green/滚动/Flag 按系统架构与回滚特性选，不为形式全上。

## 事故与紧急变更

- 建或补 incident/hotfix Issue，记录用户影响、时间线、负责人与恢复目标。
- 优先用日常流水线部署已知良好产物或最小修复，避免临时手工步骤。
- 绕过 Review/检查/环境审批须有权限人授权，记录精确 bypass、原因、证据与失效时间；恢复后补齐测试、Review、文档、Release 与被绕过控制。
- 无责复盘区分触发/放大/检测缺口/恢复阻力，不把根因简化为个人失误；产出少量可验证、带 Owner/优先级/期限的改进 Issue，并追踪到关闭。

## DORA 度量

| 维度 | 指标 | 口径 |
| --- | --- | --- |
| 吞吐 | 变更前置时间 | 从提交到生产可用 |
| 吞吐 | 部署频率 | 一段时间的生产部署次数/间隔 |
| 吞吐 | 失败部署恢复时间 | 生产故障后恢复耗时 |
| 不稳定性 | 变更失败率 | 需回滚/hotfix 的部署占比 |
| 不稳定性 | 部署返工率 | 因事故产生的非计划部署占比 |

一次只度量一个服务并固定口径，先建基线、找最大瓶颈、改一处、再看趋势；不跨系统排名、不绑定个人绩效、不美化数据。采集成本高于改进价值时用抽样与团队复盘。

## 一手资料

- [Managing labels](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/managing-labels)
- [Classifying your repository with topics](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics)
- [About tags](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases/about-releases#about-tags)
- [Protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [Secure use of GitHub Actions](https://docs.github.com/en/actions/reference/security/secure-use)
- [Supply-chain security](https://docs.github.com/en/code-security/concepts/supply-chain-security/supply-chain-security)
- [Artifact attestations](https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/use-artifact-attestations)
- [Google: Small CLs](https://google.github.io/eng-practices/review/developer/small-cls.html)
- [DORA: delivery metrics](https://dora.dev/guides/dora-metrics/)
- [NIST SP 800-218](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-218.pdf)
- [SLSA v1.2](https://slsa.dev/spec/v1.2/)
- [OpenSSF Scorecard](https://github.com/ossf/scorecard)
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [Semantic Versioning](https://semver.org/)
- [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
- [Trunk-Based Development](https://trunkbaseddevelopment.com/)
- [Architecture Decision Records](https://adr.github.io/)

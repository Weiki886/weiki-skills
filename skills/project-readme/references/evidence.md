# 证据阶梯与仓库扫描清单

## 证据阶梯速查

| 级别 | 来源 | 写法 |
| --- | --- | --- |
| 1 | 仓库原生（源码/测试/CI/配置/已有文档） | 直接写，引用来源 |
| 2 | 用户确认（定位/指标/截图/案例） | 直接写，标注「用户确认」 |
| 3 | 外部可验证（包管理器/Release/benchmark） | 写 + 链接 |
| 4 | 无来源 | 不写。重要就问，不重要的删 |

## 仓库扫描清单

create 和 improve 模式的第一步，用这个清单收集事实：

### 元数据
- [ ] 包管理文件（`package.json`、`pyproject.toml`、`Cargo.toml`、`go.mod` 等）：提取 name、version、description、license
- [ ] 包 scope、推荐固定版本、运行时版本要求与未发布状态
- [ ] 远程 URL（`git remote -v`）
- [ ] 已有 Tag 与 Release
- [ ] `LICENSE*` 文件内容
- [ ] Logo、双语 README、badge 指向的真实资源

### 代码与入口
- [ ] 可执行入口（CLI 的 `bin`、`main`、`entry point`）
- [ ] 公开模块 / 导出 API
- [ ] CLI 子命令、参数、默认值、退出码与帮助文本
- [ ] MCP server 名称、传输方式、工具名、参数 schema、注册入口
- [ ] 示例 / examples 目录
- [ ] 测试覆盖的信号（`tests/`、`__tests__/`、`*.test.*`）

### 配置面
- [ ] `.env.example` 或样例配置
- [ ] 配置文件（`*.yml`、`*.toml`、`*.json`、`*.config.*`）
- [ ] 环境变量名、默认值、合法值、是否必需
- [ ] 已验证宿主的配置方式（如 Claude Code、Codex 的 MCP 配置）
- [ ] Dockerfile / docker-compose（如果有）

### 安全与数据面
- [ ] 读取的文件、数据库、目录、剪贴板或系统权限
- [ ] 写入路径、数据库表、临时文件、缓存与备份
- [ ] dry-run、确认、回滚、权限位、路径规范化、符号链接检查
- [ ] 凭据读取与存放方式（Keychain、环境变量、配置文件等）
- [ ] 网络端点、Provider、请求载荷、什么数据会离开本机
- [ ] 日志脱敏、失败即关闭、重试、超时与错误码
- [ ] 安全承诺对应的实现与测试证据

### 已有文档
- [ ] 现有 README（如果有，逐条核对声明）
- [ ] `CONTRIBUTING.md`
- [ ] `CHANGELOG.md`
- [ ] `docs/` 中的提案、ADR、规范、安全、测试与发布文档
- [ ] `AGENTS.md`、`CLAUDE.md`（如果有）

### Git 与 CI
- [ ] `.github/workflows/`：CI 配置
- [ ] 分支保护规则
- [ ] 贡献者数（`git shortlog -sn`）

### 素材
- [ ] 截图 / GIF / 视频（`assets/`、`images/`、`screenshots/`）
- [ ] benchmark 数据
- [ ] 论文 / 预印本链接
- [ ] 包管理器页面（npm、PyPI、crates.io、Go 等）

## 禁止编造项（无证据一律不写）

- 安装命令（`npm install`、`pip install`、`brew install` 等）
- CLI 参数名与作用
- API 函数签名与返回值
- MCP 工具名、参数、宿主配置、传输方式、服务发现命令
- 配置项与环境变量名
- 错误码、错误可重试性、退出码与异常行为
- 文件路径
- Logo、截图、GIF、双语 README、badge、社区链接、设计文档链接
- 性能数字（延迟、吞吐量、内存占用）
- 兼容性声明（支持的平台、语言版本、浏览器）
- License 条款
- 社区链接（Discord、Twitter、Slack）
- 贡献流程
- 依赖版本
- 用户量、下载量、star 数（除非外部可验证）
- 架构图中的组件与调用关系
- 安全与隐私承诺（只读、无日志、不上传、失败即关闭、最小权限、自动备份等）
- Provider/第三方服务的数据保留、训练、合规或 SLA 承诺
- 包名防混淆或「非官方包」提醒（除非仓库或包管理器有明确依据）

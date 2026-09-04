# 常见问题

本页回答 pg-skills 的常见概念和使用问题。遇到具体报错时，请查看[故障排查](troubleshooting.md)。

## 基础问题

### pg-skills 是一个 Skill 吗？

不是单个 Skill。它是一套工作流包，包含 Commands、多个 Skills、Agent 角色、Python Runner、Hook 协议、项目配置和开发工具适配器。

### 每个项目都需要安装吗？

当前模式是项目级安装。每个项目把 pg-skills 放入自己的 `.pg/skills/`，再运行 `pg init`。这样项目配置、工作流版本和运行证据可以随业务仓库一起版本化。

### `pg init` 会下载 pg-skills 吗？

不会。必须先通过 Git subtree、手动复制或其他方式把源码放入 `.pg/skills/`。

### `.pg/` 和工具目录有什么区别？

`.pg/` 保存公共项目配置、change 和运行记录；`.opencode/`、`.mobile-coder/` 保存特定工具的加载文件。

## 命令与工作流

### 为什么 `/3-pg-build` 不能在 PowerShell 执行？

它是 AI 工作流入口，应输入开发工具对话框。终端只运行 `pg init`、`pg doctor`、`pg upgrade` 等 CLI。

### Command、Skill 和 Agent 有什么区别？

Command 是用户入口，Skill 是完整流程说明和脚本，Agent 是被派送执行某类任务的角色。Runner 决定派送顺序并记录结果。

### 一定要先 define 再 propose 吗？

重要变更推荐如此。边界已经非常清楚时可以直接 propose；局部小任务可以 quick-build。需求模糊时不要跳过 define。

### propose 完成后能直接 build 吗？

技术上可以，但不建议。先审查 proposal、design、tasks 和 manifest，错误方案在写代码前修正成本最低。

### build 成功后还要手动 archive 吗？

通常不需要。标准 build 成功后会自动归档，再进入验证合并。`/6-pg-archive` 用于失败后、取消或主动放弃。

### 工作流中断后要重新开始吗？

不要立即重新开始。保留对应 change 的 `2-build/`，特别是 `pipeline.events`、`pipeline.snapshot.json` 和阶段产物；再次调用同一个 build，Runner 会根据这些状态判断能否 resume。`.pg/runs/` 在当前版本只是预留目录。

## 项目配置与环境

### `pg init` 已经生成 `project.yaml`，为什么还要 pg-init-project？

`pg init` 首次只保证骨架存在，可能是 placeholder。`pg-init-project` 会扫描真实仓库并生成 modules、environments、tracks、stages 和测试命令。

### 环境一定要配置吗？

纯编译或单元测试任务可以不需要真实服务环境。涉及集成测试、端到端场景或运行服务时，应在 `.pg/project.yaml` 声明 Environment、Role 和 Hook。

### pg-skills 会自动启动项目服务吗？

只有项目配置声明了对应 Hook，并且当前工作流需要该环境时，Runner 才会按计划调用。pg-skills 不会凭空推断启动命令。

## 工具与模型

### 能同时生成 OpenCode 和 Mobile Coder 配置吗？

可以生成多个适配目录，但最后一次 `pg init` 的工具会写入 `.pg/tool-integration.json`，升级自动刷新以该选择为准。

### `associate`、`expert`、`master` 是模型名称吗？

它们是稳定的工作流路由层级，不是固定厂商模型。OpenCode 将三档渲染为 `pg-router/pg-associate`、`pg-router/pg-expert` 和 `pg-router/pg-master`；Mobile Coder 当前都继承会话模型。

### 主会话已经选择模型，为什么工作流仍有三级路由？

页面模型通常是主会话模型。pg-build 派送的 Test、Dev、Review 等 Sub-agent 可以按角色使用 associate、expert 或 master，二者不是同一层配置。

## Git 与升级

### pg-skills 会自动 commit 或 push 吗？

不同入口的行为不同：

- `pg init`、`pg doctor` 和手动 archive 不会 commit 或 push。
- quick-build 会创建本地 commit，但不 push。
- fix-issue 不自动 commit 或 push。
- 标准 build 成功后会调用 verify-and-merge，可能提交、rebase、push、合并并推送默认分支。
- regression 会把测试脚本修复提交并直接推送到 `master`；生产代码修复走独立分支和 PR。

因此，运行 build 或 regression 前必须检查当前分支、工作树、默认分支和远端权限。

### 能修改 `.pg/skills/` 里的源码吗？

业务项目不应为了完成需求修改 pg-skills 公共逻辑。适配或框架缺陷应在 pg-skills 仓库单独修复，再通过版本升级同步。

### 为什么推荐固定 tag？

固定 tag 让团队使用同一版本并能够复现运行。master 可能随时变化，更适合开发 pg-skills 本身。

## 获取帮助

提交问题时附上：

- `.pg/skills/VERSION`。
- 操作系统和 Python 版本。
- AI 开发工具及版本。
- 执行命令和完整输出。
- `pg doctor` 结果。
- 能公开的 `.pg/project.yaml` 相关片段。

# 命令如何工作

pg-skills 同时包含终端 CLI 和 AI 工作流入口。两者都叫“命令”，但运行位置和职责不同。

> **最常见的错误：** 把 `/3-pg-build` 输入 PowerShell，或者把 `pg doctor` 输入 AI 对话框。前者应在 AI 对话框运行，后者应在终端运行。

## 两个入口

### 终端 CLI

终端命令负责安装、检查和升级项目级 pg-skills：

```text
TERMINAL  python .pg/skills/src/runtime/bin/pg init --tool opencode
TERMINAL  python .pg/skills/src/runtime/bin/pg doctor
TERMINAL  python .pg/skills/src/runtime/bin/pg upgrade --list
```

它们由 `.pg/skills/src/runtime/bin/pg` 执行，不会替代 AI 开发工具。

### AI 工作流命令

工作流命令在 OpenCode 或 Mobile Coder 的对话框中运行：

```text
AI CHAT  /1-pg-define
AI CHAT  /2-pg-propose add-health-summary
AI CHAT  /3-pg-build add-health-summary
```

开发工具读取项目中的 Commands 或 Skills，将说明交给当前会话，再通过 Skill、Sub-agent、用户问答和任务列表等原生能力执行。

## 命令是如何安装的

pg-skills 的规范源只有一份：

```text
.pg/skills/src/core/workflows/
├── commands/
├── skills/
└── agents/
```

运行 `pg init --tool <tool>` 后，适配器读取公共模板，替换工具能力占位符，并写入工具能够识别的项目目录：

```text
公共工作流
   ↓ adapter render
OpenCode          .opencode/
Mobile Coder      .mobile-coder/
```

这不是只改几个名称。适配器还负责输出目录、权限、Skill 加载语义、Sub-agent 派送参数、用户提问机制和模型路由配置的映射。

## Command、Skill 和 Agent 的关系

以 `/3-pg-build add-health-summary` 为例：

```text
用户调用 Command
      ↓
Command 加载 pg-build Skill
      ↓
Skill 启动 Python Runner
      ↓
Runner 返回 dispatch action
      ↓
主 Agent 调用 Test / Dev / Review 等 Sub-agent
      ↓
结果交回 Runner record
      ↓
进入下一轮，直到 done 或 failed
```

- Command 是入口。
- Skill 定义完整工作流。
- Agent 执行被派送的角色任务。
- Runner 保存确定性状态并决定下一步。

## 命令名称为什么可能不同

公共文件和不同工具的用户界面可能采用不同调用语法。本文档使用当前项目适配器的规范形式：

```text
/1-pg-define
/2-pg-propose
/3-pg-build
```

命令源文件使用 `pg-1-define.md` 等文件名，但 frontmatter 中的 `name` 决定用户看到的命令名，例如 `1-pg-define`。如果工具命令列表显示不同形式，以 `pg init` 生成的文件内容和工具实际命令列表为准。

## 如何确认安装成功

1. 运行 `pg init --list-tools`，确认适配器存在。
2. 运行 `pg init --tool <tool>`，刷新项目适配文件。
3. 重启 AI 开发工具，因为部分工具只在启动时扫描命令。
4. 在工具界面查看命令或 Skill 列表。
5. 检查工具目录中是否存在 `pg-1-define`、`pg-2-propose`、`pg-3-build` 对应文件。

OpenCode 示例：

```powershell
dir .opencode\commands
dir .opencode\skills
dir .opencode\agents
```

## 第一次干净运行

```text
TERMINAL  pg init --tool <tool>
APP       重启开发工具并打开项目根目录
AI CHAT   请加载 pg-init-project skill，扫描仓库并初始化项目配置
TERMINAL  pg doctor
AI CHAT   /1-pg-define
```

如果命令仍未出现，请转到[故障排查](troubleshooting.md#工作流命令没有出现)。

## 相关文档

- 每个工作流命令的行为：[命令参考](commands.md)。
- 终端参数：[CLI 参考](cli.md)。
- 工具目录和差异：[支持的开发工具](supported-tools.md)。

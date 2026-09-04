# pg-skills 文档

欢迎来到 pg-skills 文档中心。这里说明如何把 pg-skills 接入一个真实项目，如何选择工作流，以及如何检查 AI 生成的方案、代码和验证结果。

pg-skills 通过 **SEA-Driven Development** 把方案（Spec）、真实环境（Environment）和验收（Acceptance）组织成一条可追踪的开发链路。它不只是生成代码，而是把需求定界、方案设计、任务执行、测试、审查、环境验证和归档连接起来。

本页按使用场景组织文档。开发工具能力以当前源码和 `pg init --list-tools` 的结果为准。

如果你只读两篇文档，请先读：

1. [入门指南](getting-started.md)：使用虚构示例，完成第一次标准变更。
2. [命令如何工作](how-commands-work.md)：分清终端命令、AI 对话命令和不同开发工具的调用方式。

> **最重要的使用习惯：** `pg init`、`pg doctor` 等命令在终端运行；`/1-pg-define`、`/2-pg-propose`、`/3-pg-build` 等工作流在 AI 开发工具的对话框中运行。

> **执行 build 前先检查 Git。** 标准 `pg-build` 的成功链路会调用 `pg-verify-and-merge`，可能自动提交、rebase、push、合并并推送默认分支。请先确认当前位于正确的功能分支、工作树干净、`git.default_branch` 正确，并且你允许该仓库被推送。

## 选择你的道路

**我想先快速了解 pg-skills 是什么。** 阅读[pg-skills 简介](pg-skills.md)，先认识项目定位、标准工作流和项目级接入方式。

**我是新手。** 从[入门指南](getting-started.md)开始，然后快速浏览[核心概念概览](overview.md)。遇到不熟悉的术语时查看[常见问题](faq.md)。

**我还没有安装。** 阅读[安装指南](installation.md)，完成项目级安装、工具适配和安装验证。

**我不知道应该使用哪个工作流。** 阅读[工作流指南](workflows.md)，在标准开发、快速构建、回归测试、问题修复和手动归档之间选择。

**我通过实例学习。** [示例与教程](examples.md)提供常见任务配方；[celer-route 实战教程](tutorials/celer-route.md)展示真实仓库中的端到端过程。

**我正在接入一个已有代码库。** 阅读[在现有项目中使用 pg-skills](existing-projects.md)，了解如何核对模块、环境和测试配置，再从一个小变更开始。

**我要调整项目运行方式。** 阅读[配置指南](configuration.md)，了解 modules、environments、tracks、stages、Hooks 和回归套件。

**我不清楚三级模型如何配置。** 阅读[模型路由指南](model-routing.md)，区分 OpenCode 和 Mobile Coder 的真实能力边界。

**我想知道生成的目录和文件有什么作用。** 阅读[项目目录与产物](project-structure.md)。

**我需要某个开发工具的完整步骤。** 阅读 [OpenCode](tutorials/opencode.md) 或 [Mobile Coder](tutorials/mobile-coder.md) 教程。

**我要开发或维护工具适配器。** 阅读[开发工具适配器说明](tool-integrations.md)，了解公共工作流、占位符映射、Adapter 注册和测试边界。

**我不知道命令应该输入在哪里。** 阅读[命令如何工作](how-commands-work.md)和[支持的开发工具](supported-tools.md)。

**我只想查询某个命令。** AI 工作流命令见[命令参考](commands.md)，终端命令见[CLI 参考](cli.md)。

**出现错误。** 先按现象查看[故障排查](troubleshooting.md)；如果只是概念疑问，查看[常见问题](faq.md)。

## 完整文档地图

### 从这里开始

| 文档 | 你会得到什么 |
|---|---|
| [pg-skills 简介](pg-skills.md) | 项目定位、标准工作流和项目级接入方式 |
| [安装指南](installation.md) | 安装、初始化、验证、升级和移除 |
| [入门指南](getting-started.md) | 使用虚构需求完成第一次端到端变更 |
| [在现有项目中使用 pg-skills](existing-projects.md) | 将 pg-skills 安全接入已有代码库或 Monorepo |
| [核心概念概览](overview.md) | SEA、Change、Stage、Track、Agent 和运行记录的整体认识 |
| [项目目录与产物](project-structure.md) | `.pg/`、工具目录、变更产物和运行记录的所有权 |
| [命令如何工作](how-commands-work.md) | 终端与 AI 对话框的边界，以及命令如何被工具加载 |

### 日常使用

| 文档 | 你会得到什么 |
|---|---|
| [工作流指南](workflows.md) | 常见任务应该选择哪条工作流 |
| [配置指南](configuration.md) | 配置 modules、environments、tracks、stages、Hooks 和回归套件 |
| [模型路由指南](model-routing.md) | 配置并验证 associate、expert 和 master 三级路由 |
| [示例与教程](examples.md) | 可以照着选择和改写的通用任务配方 |
| [celer-route 实战教程](tutorials/celer-route.md) | 在真实 celer-route 仓库中完成端到端演练 |
| [命令参考](commands.md) | 每个 pg 工作流命令的用途、输入和产物 |
| [CLI 参考](cli.md) | `pg init`、`pg doctor`、`pg upgrade` 的参数 |

### 工具教程

| 文档 | 你会得到什么 |
|---|---|
| [OpenCode 完整教程](tutorials/opencode.md) | 从初始化到 Commands、Skills、Agents 和标准工作流验证 |
| [Mobile Coder 完整教程](tutorials/mobile-coder.md) | 原生项目目录、当前模型行为和完整运行过程 |

### 适配与帮助

| 文档 | 你会得到什么 |
|---|---|
| [开发工具适配器说明](tool-integrations.md) | 公共工作流、占位符映射、Adapter 注册和适配边界 |
| [支持的开发工具](supported-tools.md) | OpenCode、Mobile Coder 的目录和调用差异 |
| [常见问题](faq.md) | 安装、工作流、产物和模型路由方面的常见疑问 |
| [故障排查](troubleshooting.md) | 按具体报错和现象定位问题 |

## 三十秒版本

```text
TERMINAL  把 pg-skills 放入项目的 .pg/skills/
TERMINAL  python .pg/skills/src/runtime/bin/pg init --tool opencode
AI CHAT   加载 pg-init-project skill，初始化项目配置
TERMINAL  python .pg/skills/src/runtime/bin/pg doctor
AI CHAT   /1-pg-define
AI CHAT   /2-pg-propose <change-name>
AI CHAT   /3-pg-build <change-name>
```

Windows 示例使用 `python`；Linux 和 macOS 通常使用 `python3`。开发工具名称可以替换为当前版本实际支持的适配器，使用 `pg init --list-tools` 查询。

## 获取帮助

- 项目仓库：[pin-gou/pg-skills](https://github.com/pin-gou/pg-skills)
- 使用问题与缺陷：[GitHub Issues](https://github.com/pin-gou/pg-skills/issues)
- 提交问题时请附上 pg-skills 版本、操作系统、开发工具、执行命令和完整错误输出。

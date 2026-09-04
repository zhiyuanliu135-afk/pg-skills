# pg-skills 简介

> 本页是项目简介。可复制的安装与操作步骤以[文档中心](README.md)为准。

pg-skills 是一套项目级 AI 开发工作流。它把需求定界、方案设计、任务执行、测试、审查、真实环境验证和归档组织在同一个 change 中，并通过适配器接入 OpenCode 和 Mobile Coder。

**SEA（Spec、Environment、Acceptance）是 pg-skills 的方法论：** Spec 由 `proposal.md`、`design.md` 和 `tasks.md` 描述方案；Environment 由 `describe_env` 只读探测并生成 `env-description.yaml`；Acceptance 通过 `scenario-*.yaml` 定义可执行验收条件。

## 标准工作流

```text
/1-pg-define
      ↓
/2-pg-propose <change-id>
      ↓
人工审查 proposal、design、tasks 和 execution-manifest
      ↓
/3-pg-build <change-id>
      ↓
archive → verify-and-merge
```

- `define` 调查代码、澄清需求并确定边界。
- `propose` 生成方案、设计、任务和执行清单。
- `build` 依据项目配置派送 Test、Dev、Review、Verify 和 Gate 等角色。
- `verify-and-merge` 可能自动执行 Git 提交、rebase、push、合并和默认分支推送。
- `archive` 保存完成或放弃的变更记录。

运行时间和人工介入次数取决于变更规模、模型、项目测试、环境稳定性和失败情况。pg-skills 不承诺固定耗时、固定通过率或完全无人值守。

## 项目级接入

pg-skills 源码位于业务项目的 `.pg/skills/`，一般通过 Git subtree 引入。`pg init --tool <tool>` 创建公共项目骨架并生成目标工具的 Commands、Skills、Agents 或桥接配置，但不会下载 pg-skills，也不会替用户配置模型和 API 密钥。

首次初始化的正确顺序是：

1. 把 pg-skills 放入 `.pg/skills/`。
2. 运行 `pg init --tool <tool>`。
3. 在 AI 开发工具中加载 `pg-init-project`，生成真实 `.pg/project.yaml`。
4. 在终端运行 `pg doctor`。
5. 从一个边界清楚的小变更开始。

## 继续阅读

- [安装指南](installation.md)
- [入门指南](getting-started.md)
- [核心概念概览](overview.md)
- [工作流指南](workflows.md)
- [支持的开发工具](supported-tools.md)
- [celer-route 实战教程](tutorials/celer-route.md)

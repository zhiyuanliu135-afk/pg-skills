# 支持的开发工具

pg-skills 的核心工作流与开发工具无关。适配器负责把公共 Commands、Skills、Agents 和能力占位符渲染成目标工具能够加载的项目文件。

官方 `v0.9.2` 注册了以下适配器：

| 工具 | `--tool` 值 | 项目目录 | 安装形态 |
|---|---|---|---|
| OpenCode | `opencode` | `.opencode/` | 渲染 Commands、Skills、Agents |
| Mobile Coder | `mobile-coder` | `.mobile-coder/` | 渲染 Commands、Skills、Agents |

使用源码查询实际注册的工具，不要根据旧文档猜测：

```powershell
python .pg\skills\src\runtime\bin\pg init --list-tools
```

## 公共边界

无论选择哪个工具，以下内容保持一致：

- `.pg/project.yaml`；
- `.pg/changes/` 和工作流产物；
- Python Runner、Event、Reducer、Hook 和 Doctor；
- Pipeline 阶段、失败处理、结果记录、验证和归档语义。

适配器只负责“如何加载和调用”，不应改变 pg-build 等公共工作流逻辑。

## OpenCode

初始化：

```powershell
python .pg\skills\src\runtime\bin\pg init --tool opencode
```

生成：

```text
.opencode/
├── commands/
├── skills/
├── agents/
└── .pg-adapter-manifest.json
```

OpenCode 从该目录加载项目级 Commands、Skills 和 Agents。适配器将公共占位符映射为 OpenCode 的 Skill、Task、question 和 TodoWrite 等能力。

模型路由使用：

```text
pg-router/pg-associate
pg-router/pg-expert
pg-router/pg-master
```

适配器不会修改 `opencode.json` 或安装模型 Provider。用户需要保证这些路由在自己的 OpenCode 配置中可解析。初始化后重启 OpenCode，再检查命令列表。

## Mobile Coder

初始化：

```powershell
python .pg\skills\src\runtime\bin\pg init --tool mobile-coder
```

生成：

```text
.mobile-coder/
├── commands/
├── skills/
├── agents/
└── .pg-adapter-manifest.json
```

适配器把公共 Skill 加载、Sub-agent 派送、用户提问和任务跟踪语义转换为 Mobile Coder 的能力。当前模板把 associate、expert、master 三档都映射为 `current`，因此角色职责仍然分层，但实际继承当前会话模型。适配器不会修改 `mobile-coder.json`。

## 多个工具能否共存

可以先后运行不同的 `--tool`，两个适配目录可以共存。但 `.pg/tool-integration.json` 只记录最后一次成功选择，`pg upgrade` 自动刷新时以该记录为准。

团队仓库最好明确一个默认工具。确认新工具可用后，再通过 Git diff 清理不需要的旧适配文件，不要直接删除可能包含用户自定义内容的整个目录。

## 相关文档

- 完整操作教程：[OpenCode](tutorials/opencode.md)、[Mobile Coder](tutorials/mobile-coder.md)。
- 三级模型映射：[模型路由指南](model-routing.md)。
- 生成目录和文件所有权：[项目目录与产物](project-structure.md)。
- 不清楚在哪输入命令：[命令如何工作](how-commands-work.md)。
- 初始化失败：[故障排查](troubleshooting.md)。
- 适配器开发边界：[tool-integrations.md](tool-integrations.md)。

# 模型路由指南

pg-skills 使用 `associate`、`expert` 和 `master` 三档逻辑路由，让不同角色可以选择不同成本和能力的模型。这三个名称是稳定的工作流等级，不是某个厂商的固定模型。

## 三档分别做什么

| 路由 | 典型职责 | 当前角色示例 |
|---|---|---|
| `associate` | 状态读取、简单验证、管理和探索 | `explore`、`pg-manager`、部分 verify/simple |
| `expert` | 实现、测试、修复和常规 Gate | dev、test、fix、gate、regression |
| `master` | 需求定界、方案设计和高风险审查 | define、propose、review、fix-gate |

角色文档通过 `model` frontmatter 引用逻辑路由。适配器在执行 `pg init --tool <tool>` 时把公共占位符渲染为目标工具能够理解的值。

模型更强并不保证结果正确。任务边界、测试、Review 和 Verify 仍由工作流负责。

## OpenCode

OpenCode 生成的角色使用：

```text
pg-router/pg-associate
pg-router/pg-expert
pg-router/pg-master
```

这些值遵循 `provider/model` 形式，其中 `pg-router` 必须是 OpenCode 能够解析的 provider 或路由层。适配器不会修改 `opencode.json`，也不会安装模型 provider。

因此用户需要在 OpenCode 配置中完成两件事：

1. 配置能够提供 `pg-router` 的 provider 或路由插件。
2. 让三个 model id 分别解析到实际模型。

具体 JSON 结构取决于采用的 provider 或路由插件，不能把某个第三方插件的 schema 当作 pg-skills 固定格式。修改后重启 OpenCode，并确认生成的 `.opencode/agents/*.md` 中三个路由都可被调用。

## Mobile Coder

当前 Mobile Coder 模板把三档全部渲染为：

```text
current
```

这意味着：

- 工作流仍保留 associate、expert、master 的角色分层。
- 所有角色实际继承当前会话模型。
- `pg init` 不会修改 `mobile-coder.json`。
- 当前适配器没有提供三档独立真实模型映射。

不要仅通过修改公共 Agent 文档伪造三档路由，否则下一次初始化可能造成适配层漂移。若未来 Mobile Coder 提供稳定的项目级路由接口，应在适配器模板中统一实现并补充测试。

## 怎样验证

先做静态检查：

```powershell
# OpenCode
Get-ChildItem .opencode\agents -Recurse -Filter *.md |
  Select-String -Pattern "pg-router/pg-"

# Mobile Coder
Get-ChildItem .mobile-coder\agents -Recurse -Filter *.md |
  Select-String -Pattern "^model:"
```

再做真实调用验证：要求工具分别派送三个等级的简单只读任务，并检查运行日志中的实际 provider/model。只让 Agent 回复 `ROUTE_OK` 只能证明调用返回，不能单独证明后端使用了预期模型。

## 常见误区

- `master` 是工作流路由等级，不等于某个固定模型。
- 三个路由可以临时指向同一模型，但这不等于已经实现成本分层。
- `.pg/project.yaml` 不保存模型路由。
- API 密钥不应写入 `.pg/`、`.opencode/` 或 `.mobile-coder/` 的可提交文件。

## 相关文档

- 各工具完整操作：[OpenCode](tutorials/opencode.md)、[Mobile Coder](tutorials/mobile-coder.md)。
- 工具能力对比：[支持的开发工具](supported-tools.md)。

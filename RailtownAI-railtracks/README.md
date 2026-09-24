# RailtownAI/railtracks 情报档案

**一句话定位**：Railtracks 是 Railtown AI 开源的纯 Python Agent 框架，让开发者用普通 Python 对象（无 YAML/DSL）组装自己的 agent harness——工具调用循环、工具面、上下文管理、权限/预算控制、可回放的运行记录（README.md）。

## 档案索引

- [tech-stack.md](tech-stack.md) —— 语言、框架、构建、测试、文档工具链清单
- [architecture.md](architecture.md) —— 组件架构图与职责说明
- [business-logic.md](business-logic.md) —— 核心流程（agent 构建、调用、中间件、MCP、检索、评估）
- [changelog/2026-09-25-b28115b.md](changelog/2026-09-25-b28115b.md) —— 首次建档快照记录
- [changelog/2026-09-25-2f1626e.md](changelog/2026-09-25-2f1626e.md) —— 增量：function_node 中间件类型推断修复（#1538）
- [changelog/2026-09-25-75c3b17.md](changelog/2026-09-25-75c3b17.md) —— 增量：Middleware 泛型第三参数引发 guardrails 导入修复（#1591）；agent-facing 文档改写（#1583）
- [tripwires.md](tripwires.md) —— 持续盯防事项

## 同步信息

- 模式：incremental（本次）
- 本次同步游标：`2f1626e` → `75c3b17`（2026-09-25）
- 数据来源：仓库 diff（2 个提交，7 个文件变更）
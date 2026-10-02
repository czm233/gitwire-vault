# RailtownAI/railtracks 情报档案

**一句话定位**：Railtracks 是 Railtown AI 开源的纯 Python Agent 框架，让开发者用普通 Python 对象（无 YAML/DSL）组装自己的 agent harness——工具调用循环、工具面、上下文管理、权限/预算控制、可回放的运行记录（README.md）。

## 档案索引

- [tech-stack.md](tech-stack.md) —— 语言、框架、构建、测试、文档工具链清单
- [architecture.md](architecture.md) —— 组件架构图与职责说明
- [business-logic.md](business-logic.md) —— 核心流程（agent 构建、调用、中间件、MCP、检索、评估）
- [changelog/2026-09-25-b28115b.md](changelog/2026-09-25-b28115b.md) —— 首次建档快照记录
- [changelog/2026-09-25-2f1626e.md](changelog/2026-09-25-2f1626e.md) —— 增量：function_node 中间件类型推断修复（#1538）
- [changelog/2026-09-25-75c3b17.md](changelog/2026-09-25-75c3b17.md) —— 增量：Middleware 泛型第三参数引发 guardrails 导入修复（#1591）；agent-facing 文档改写（#1583）
- [changelog/2026-09-25-ee5d84d.md](changelog/2026-09-25-ee5d84d.md) —— 增量：LLM 响应全链路透出 reasoning/thinking（#1431/#1563）
- [changelog/2026-09-25-1c3ab8c.md](changelog/2026-09-25-1c3ab8c.md) —— 增量：verifier 事件透出 + viz 分类（#1425/#1569）
- [changelog/2026-09-26-e8b0590.md](changelog/2026-09-26-e8b0590.md) —— 增量：litellm 升级至 <=1.102.1 支持 gpt-6 reasoning_effort（#1596）；anyio 4.15.1
- [changelog/2026-10-01-9b894a3.md](changelog/2026-10-01-9b894a3.md) —— 增量：context 事件流可观测性（`context.*` 事件 + `RAILTRACKS_CONTEXT_EVENTS`）（#1601）；PEP 563 工具 schema 修复 + Literal 参数处理器（#1580）；Conductr 托管评估文档（#1606）；AGENTS.md 技能确认机制（#1595）
- [changelog/2026-10-02-8ff5a4c.md](changelog/2026-10-02-8ff5a4c.md) —— 增量：docstring 解析支持 Google/NumPy/reST 三风格（#1452）；CLI `add --force` 位置修复（#1610）；文档口径统一 `.content` + 示例模型名/命名规范刷新（#1611）
- [tripwires.md](tripwires.md) —— 持续盯防事项

## 同步信息

- 模式：incremental（本次）
- 本次同步游标：`9b894a3` → `8ff5a4c`（2026-10-02）
- 数据来源：仓库 diff（3 个提交，53 个文件变更）
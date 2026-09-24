# 情报档案 · browser-use/jev-ultrafast

**一句话定位**：一个由 TypeSafe Jev 驱动的浏览器 Agent——从索引化、动态生成的动作空间中「选择」操作与目标元素，而非生成动作文本；仅在 `TYPE_TEXT` 时调用小型 LLM 生成字段文本（信源：README.md、pyproject.toml description）。

## 档案索引

| 文件 | 内容 |
| --- | --- |
| [tech-stack.md](tech-stack.md) | 语言、依赖、构建、测试工具链 |
| [architecture.md](architecture.md) | 组件架构图与职责说明 |
| [business-logic.md](business-logic.md) | 核心决策-执行循环、快照、守卫等业务流程 |
| [tripwires.md](tripwires.md) | 持续盯防的风险点 |
| [changelog/2026-09-25-1231850.md](changelog/2026-09-25-1231850.md) | 本次全量建档记录 |

## 最近同步

- 游标：`∅（无游标）` → `1231850`（2026-09-25），init 模式首次全量建档
- 版本：v0.1.0，MIT License（Copyright 2026 Browser Use）
- 仓库规模：约 12 个 Python/JS 核心模块 + 本地 inspector 前端 + 离线测试

## 关键事实速览

- 决策循环：每次观测发出**一次** TypeSafe 请求，同时携带 operation 与投机性 target heads；仅被选中操作对应的 target head 可执行（README.md、jev_ultrafast/model.py）
- 性能声称：Google Flights Zürich→London 7.073 s；六次交替对比中位数 9.450 s→7.092 s（-25%），CDP 调用 1,092→101；作者自述非通用可靠性基准（README.md、docs/performance.md 未在快照中给出内容，标注未核实）
- 明确 MVP 边界：不支持 Shadow roots、iframe、canvas、上传、弹出标签页等（README.md）
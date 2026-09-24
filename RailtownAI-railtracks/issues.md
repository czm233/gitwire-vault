# Issue 雷达 · RailtownAI/railtracks

> 全景扫描：2026-09-25 · 开放 issue 93 个 · 机会 49 · 被占 25 · 困难 19

## 机会榜（按值得做排序，top 10）

| # | 难度 | 信号 | 标题 → 一句话 |
| --- | --- | --- | --- |
| [#1577](https://github.com/RailtownAI/railtracks/issues/1577) | 简单 | 新鲜·good first issue+help wanted | docs: web search tool page doesn't document installing the ` —— websearch 文档页缺 optional extra 安装说明，纯文档补全 |
| [#1547](https://github.com/RailtownAI/railtracks/issues/1547) | 简单 | good first issue+help wanted | Remove/soften model specific instruction in docs —— 文档去模型特定表述（Claude Code、OpenAI 示例），纯文档编辑 |
| [#873](https://github.com/RailtownAI/railtracks/issues/873) | 简单 | good first issue+help wanted | [Docs] Unclear Usage of Context —— Context 用法文档前置，纯文档改动，适合入门 |
| [#940](https://github.com/RailtownAI/railtracks/issues/940) | 简单 | good first issue+help wanted | [Docs] Update BYFA/RYFA to be a bit better in terms of narra —— BYFA/RYFA 教程叙事重构，纯文档改写 |
| [#1053](https://github.com/RailtownAI/railtracks/issues/1053) | 简单 | help wanted | [Feature] Add Length Requirement Guardrail —— 长度护栏 Guardrail，接口已定范围明确 |
| [#1150](https://github.com/RailtownAI/railtracks/issues/1150) | 简单 | help wanted | [Feature] [Retrieval] Batch write for stores —— 存储层批量写入 EmbeddedChunk，接口扩展明确 |
| [#1167](https://github.com/RailtownAI/railtracks/issues/1167) | 简单 | good first issue+help wanted | [Bug] [Retrieval] same `id` for all rows in `HuggingFaceData —— HF 数据集所有行 id 相同，破坏 upsert 语义 |
| [#1381](https://github.com/RailtownAI/railtracks/issues/1381) | 简单 | good first issue+help wanted | Add llms.txt / llms-full.txt —— 添加 llms.txt / llms-full.txt，范围明确的文档任务 |
| [#1573](https://github.com/RailtownAI/railtracks/issues/1573) | 简单 | 较新 | railtracks add --force before <tool>:<skill> fails, despite  —— `railtracks add --force` 前置位置解析顺序 bug，定位明确的 CLI 修复 |
| [#1534](https://github.com/RailtownAI/railtracks/issues/1534) | 简单 | — | Delete legacy install detection —— 删除 legacy 安装检测代码，前置条件已满足，范围清晰的清理任务 |

## 已被占（不必再看）
- #1562 Tool.from_function silently degrades unmapped parameter types to "object" (bypasses #1552's strict validation) —— 被 assignee rajathpatel23 占
- #1474 We need a ticket assign max duration or PR open duration —— 被 assignee CoronRing 占
- #1458 Tool schemas silently degrade to `{"type": "object"}` under `from __future__ import annotations` —— 被 assignee CoronRing 占
- #1451 Skill Infrastructure: Directories Support —— 被 assignee Pooria90 占
- #1433 Precise request cost with cache hit info —— 被 PR#1519 占
- #1431 Surface thinking token in LLM response. —— 被 assignee Pooria90 占
- #1430 LLM finish_reason  is not respected —— 被 assignee CoronRing 占
- #1425 Emit verifier decisions as observability events —— 被 assignee Aryan-Railtown 占
- #1393 More insightful error message for bad schema —— 被 assignee CoronRing 占
- #1357 Agent response do not include tool calls. —— 被 assignee CoronRing 占
- #1347 [Epic] Rich media support —— 被 assignee CoronRing 占
- #1343 Thorough Review of Coding Assistant support —— 被 assignee Pooria90 占
- #1266 General Verifier —— 被 assignee Aryan-Railtown 占
- #1265 Implement Naive Human in the loop —— 被 assignee Aryan-Railtown 占
- #1239 Rethinking observability into an events stream —— 被 assignee Amir-R25 占
- #1233 Prebuilt Tools —— 被 assignee Pooria90 占
- #1228 [Feature] Json should store in UTF-16 instead of ASC-II —— 被 assignee CoronRing 占
- #1217 [Feature] [Middleware] Thinking model support — streaming, storage, and round-tripping of reasoning blocks —— 被 assignee CoronRing 占
- #1179 [Docs] Define of keywords —— 被 PR#1566 占
- #1156 [Feature]  Auto populate agent manifest by ingesting system message. —— 被 assignee CoronRing 占
- #1145 [Feature] General Issue for Supported Add Ons —— 被 assignee soulFood5632 占
- #1114 [Docs][Feature] Minor Doc Grammar and content fixes —— 被 assignee CoronRing 占
- #947 [Tech Debt] LLM submodule needs its own loggers —— 被 assignee Aryan-Railtown 占
- #881 [Feature] Add Support for non-google docstring formats —— 被 PR#1452 占
- #486 [Feature] Documentation App —— 被 assignee Aryan-Railtown 占

## 近期关闭
- #1538 Type hints collapse when you use a list of pre-built middlewares —— PR#1541（2026-09-24）

## 分析详情（最新分析在前）
### #1588 [中等|🟢机会] viz: match middleware failures by exception id, not message
- 将 viz 中间件失败匹配从异常消息改为异常 id，避免误匹配
- 问题：viz 中间件表用异常 message 判断是阻断还是仅被打断，消息相同会导致误判。大概率在 `cli/viz_api/queries/middleware.py` 的 `raised_here` 判定逻辑，需追踪 `callee_failures` 数据结构是否已携带异常 id/类型信息（未核实），可能需上游记录点配合。
- 方案：在异常记录处附带异常标识（类型名或唯一 id）并持久化到 session 数据，查询时按 id 匹配替代 message 匹配；若数据层无此字段则需小范围跨层改动。工作量小时级；风险：数据结构变更影响历史 session 兼容性与 #1569 修复逻辑回归。（工作量级：小时级）
- 分析于 2026-09-25

### #1584 [中等|🟢机会] Aggregate cost SUMs (sessions/nodes) silently drop unresolved-cost calls, unlike the fixed row-level endpoint
- 聚合SUM掩盖未定价调用的null成本，与已修的单行端点行为不一致，站点明确
- 问题：裸 `SUM(total_cost)` 跳过 NULL 行，混合定价/未定价的 session/node 成本被静默低估，与 #1553/#1568 修复的行级端点不一致；站点已明确列出 sessions.py/nodes.py 等（第三处被截断，需核实完整清单）。
- 方案：改用 `SUM` + NULL 检测（如 `COUNT(*) FILTER (WHERE total_cost IS NULL)` 配合输出 null 或部分标记），或用 `SUM(...) 保持null语义` 的条件聚合；保持与行级端点语义一致。小时级。风险：前端对 null 聚合值的展示处理；需同步更新查询测试。
- 分析于 2026-09-25

### #1582 [中等|🟢机会] Heterogeneous tuples do not constrain element position
- 异构元组需改用prefixItems按位约束，修法明确但涉及schema版本兼容
- 问题：定长异构 tuple 生成的 items 用 anyOf，元素位置不受约束，`["a","b"]` 可通过 `Tuple[str, int]`；大概率在 tuple→array 的 schema 转换逻辑（未核实具体路径）。
- 方案：按成员顺序生成 `prefixItems`（保留 minItems/maxItems），需处理 JSON Schema draft 版本兼容及所有成员同类型时回退 items。小时级到天级。风险：老客户端不支持 prefixItems；单元素 tuple 等边界情况。
- 分析于 2026-09-25

### #1581 [中等|🟢机会] Optional[X] with no default is reported as not required
- Union处理错误放宽required，修复明确且影响函数调用正确性
- 问题：`UnionParameterHandler` 在联合类型含 None 时错误地将参数标记为非必填，混淆了"值可为 None"与"键可省略"；大概率在 schema 生成模块的 union 处理器（未核实具体路径）。
- 方案：移除 union handler 对 `required` 的覆盖，沿用 `Tool.from_function` 的 default 判断；或区分 presence 与 nullability。小时级。风险：可能影响下游依赖旧 schema 行为的测试/兼容性。
- 分析于 2026-09-25

### #1577 [简单|🟢机会] docs: web search tool page doesn't document installing the `websearch` extra
- websearch 文档页缺 optional extra 安装说明，纯文档补全
- 问题：Web Search Tool 文档未说明需安装 `websearch` extra（`tavily-python`、`trafilatura` 在 `pyproject.toml` 的 optional extra 中），照文档操作会 ImportError。源文件：`docs/documentation/agent_design/tools/prebuilt/websearch.md`。
- 方案：在文档开头加 `pip install "railtracks[websearch]"` 及 API key 说明；顺带检查其他 prebuilt 工具页是否有同类缺失。工作量级：小时级。风险：无。
- 分析于 2026-09-25

### #1576 [困难|🟢机会] Follow-up on OpenAI Responses-API `reasoning_items`
- OpenAI Responses-API reasoning_items 支持，前置依赖未就绪
- 问题：litellm 1.89 的 `reasoning_items` 是 Responses-API 字段，railtracks 当前走 `litellm.completion`（Chat Completions），根本不暴露该数据——issue 自己也承认"今天不需要"。真正落地需先把 OpenAI 路由切到 Responses API（大前置工程，信息不足于该切换的方案）。
- 方案：短期在 `Message`/`Delta` 模型上预留 `reasoning_items` 字段与序列化透传；长期依赖 Responses API 路由工作。工作量级：字段透传为天级，完整支持为周级。风险：现在实现属投机性 API，litellm 行为可能变动；建议挂起等待路由能力。
- 分析于 2026-09-25

### #1574 [中等|🟢机会] Session flow_name warning fires on railtracks' own internal call paths
- 内部调用路径的 Session 触发 flow_name 警告，误扰正常用户
- 问题：`Session.__init__`（`_session.py:90-94`）对无 `flow_name` 的构造发警告，但 `interaction/_call.py` 与 `_astream.py` 的内部路径同样开 `with Session()`，使文档推荐的顶层 `await rt.call(...)` 也告警。本质是"内部路径 vs 用户路径"无法区分。
- 方案：给 `Session.__init__` 加内部标志参数（如 `_internal=True`）由 `_call.py`/`_astream.py` 传入跳过警告；或仅当用户显式写 `with rt.Session()` 才警告（难以检测，标志方案更实际）。工作量级：小时级。风险：低；注意不要连带静默其他有价值的告警。
- 分析于 2026-09-25

### #1573 [简单|🟢机会] railtracks add --force before <tool>:<skill> fails, despite usage text showing that order
- `railtracks add --force` 前置位置解析顺序 bug，定位明确的 CLI 修复
- 问题：`_run_add` 在解析 `--force` 之前检查 `args[0].startswith("-")`，导致 usage 文本承诺的 `railtracks add --force <tool>:<skill>` 顺序实际报错退出。位置：`packages/railtracks/src/railtracks/cli/__init__.py`。
- 方案：把 `force = "--force" in args` 提前到 usage 检查前，或改为先过滤 flag 再校验剩余参数；顺带补一条顺序无关的测试（作者指出测试缺失）。工作量级：小时级。风险：极低。
- 分析于 2026-09-25

### #1572 [困难|🟢机会] Feature Request: Expose current node and flow metadata via ambient execution context (rt.context.get_current_node())
- 通过环境上下文暴露当前节点/flow 元数据，需架构设计的新能力
- 问题：现有 `rt.context` 只暴露 session 级身份与 `get_parent_id()` 裸 UUID；节点名称、类型、元数据未入 scope 栈，middleware/工具无法感知所在节点。涉及 `railtracks.context.central` 的 `ContextVarScopeManager`/`SessionContext.scope` 与节点执行路径，源自 #1570/#1571 系列设计（正文截断，部分信息不足）。
- 方案：在节点入栈时推送 `{name, type, metadata}`，新增 `rt.context.get_current_node()`；需设计与 session identity 的关系、性能开销与跨 await 安全。工作量级：天级到周级。风险：ContextVar 生命周期与嵌套节点边界易出错，API 设计需先评审。
- 分析于 2026-09-25

### #1562 [中等|🔒认领] Tool.from_function silently degrades unmapped parameter types to "object" (bypasses #1552's strict validation)
- Tool.from_function 把未映射类型静默降级为 object，绕过 #1552 严格校验
- 问题：`ParameterType.from_python_type` 的 `mapping.get(py_type, cls.OBJECT)` 默认兜底，使 `DefaultParameterHandler.create_parameter` 在调用 `Parameter()` 前就把注解坍缩为合法字符串 `"object"`，#1552 的严格校验永不触发。涉及 `packages/railtracks/src/railtracks/llm/tools/parameters/_base.py` 与 `parameter_handlers.py`。
- 方案：移除/收紧 `from_python_type` 的默认兜底使其抛错，或在 `create_parameter` 处对未映射类型显式校验；评估现有用户函数中未映射类型的实际存在量，决定 raise 还是 warn+跳过。工作量级：小时级到天级。风险：可能使此前静默可用的工具函数开始报错（行为破坏），需文档说明与灰度策略。
- 分析于 2026-09-25

### #1547 [简单|🟢机会] Remove/soften model specific instruction in docs
- 文档去模型特定表述（Claude Code、OpenAI 示例），纯文档编辑
- 问题：agent.md 多处假定用户使用 Claude Code，文档高层指令嵌入 OpenAI 具体示例，应泛化为 "agents" 或移入代码注释。涉及 `agent.md` 与 docs 目录（未核实全部命中位置，需全文扫描）。
- 方案：全文检索模型/工具专名，高层文本泛化，具体示例迁至代码段注释或脚本。工作量级：小时级。风险：极低，注意保留必要的技术准确性（如 API 名称不可省略处）。
- 分析于 2026-09-25

### #1534 [简单|🟢机会] Delete legacy install detection
- 删除 legacy 安装检测代码，前置条件已满足，范围清晰的清理任务
- 问题：#1522/#1525 已落地 manifest 与 detector，`find_legacy_installs` 及 `.github/copilot-instructions.md` 标记块、`.cursor/rules/*.mdc` 的 legacy 感知只剩清理工作；需确认 Copilot+Cursor directory handlers 已落地且迁移窗口已过。
- 方案：删除 `find_legacy_installs` 及 CLI/tests 中的 legacy 形状处理，清理 `ai_setup.md` 迁移期说明。工作量级：小时级。风险：低；仅注意是否有外部用户仍依赖 legacy 检测的隐性兼容诉求。
- 分析于 2026-09-25

### #1509 [中等|🟢机会] Args used to throw a new error is not clear.
- 统一错误类的 reason/message 术语并上提 notes+reason 到 RTError，需设计决策
- 问题：错误体系中 Reason/Message 混用（同义遗留），Notes 表可行动建议。作者提议把 notes+reason 收敛为 `RTError` 共享属性，并统一 reason→message，但明确说"两个都超出本 ticket 范围，征求意见"——即范围本身未定，需维护者先拍板（信息不足于最终决定）。
- 方案：讨论定型后：在 `RTError` 基类统一 `message`/`notes` 属性，子类删除重复 `__init__` 定义；全局替换 reason→message 并保留兼容别名。工作量级：天级。风险：公共 API 破坏性变更，需 deprecation 周期与全量调用点排查。
- 分析于 2026-09-25

### #1503 [中等|🟢机会] Optional markdown render on visualizer
- visualizer 增加 AI 输入/输出的 markdown raw/preview 切换，范围明确的前端增强
- 问题：visualizer 中 raw markdown（含表格、图片）不可读，需 GitHub 式渲染预览。涉及 visualizer 前端渲染组件（未核实技术栈，可能是 web/本地 UI）。
- 方案：为 input/output 面板加 raw/preview 切换，接入 markdown 渲染库（如 marked/markdown-it）。工作量级：小时级到天级。风险：XSS（需 sanitize）、大输出渲染性能、与现有 UI 风格一致性。
- 分析于 2026-09-25

### #1496 [中等|🟢机会] Let input guards fire once per agent call instead of per model round-trip
- 给 InputGuard 加 once 开关避免每次工具循环重复触发，方案已明确
- 问题：Guard 是 `ModelInvoker` 中间件，按 model round-trip 触发；`OutputGuard` 已跳过中间 tool turn（`concrete.py:184`），`InputGuard` 无等价机制，guard 内含 LLM 调用时开销大。涉及 `railtracks` guard/middleware 层。
- 方案：`InputGuard.__init__` 加 `once: bool = False`，穿透 `input_guard(...)` 装饰器与 `_make_guard`；`once=True` 且本次 agent 调用已触发时 `_middleware_fn` 直接转发。依赖异步 guardrail task 先落地。工作量级：小时级到天级。风险：与 async guard 任务的状态生命周期管理（"本次调用"判定）、默认值兼容性低风险。
- 分析于 2026-09-25

### #1488 [困难|🟢机会] Command Line Assistant Module
- 提供开箱即用的终端 Assistant 模块，是较大新功能设计
- 问题：用户搭建终端助手需自行胶水组合 base agent、shell 工具、python 执行、记忆、clarification、实时输出与 CLI。需要设计 `AssistantPro` + `TerminalUI` 的新 API 面，且作者明确不放 `prebuilt`——归属与分层需维护者决策，正文被截断（信息不足）。
- 方案：先与维护者确认模块归属与 API 形状，再实现 agent 装配 + 终端 TUI（可能复用 rich/prompt_toolkit，未核实）。工作量级：周级。风险：API 设计定型过早、依赖新增、与未来 prebuilt 体系冲突。
- 分析于 2026-09-25

### #1474 [中等|🔒认领] We need a ticket assign max duration or PR open duration
- 为 ticket/PR 设置最长停留时长并自动延期机制，属流程规范类
- 问题：缺少 ticket assign 与 PR open 的时长上限，防止数月停滞。本质是仓库流程/自动化策略而非代码缺陷；"7 天、任何评论/commit 延期、紧急修复豁免"规则需设计决策。
- 方案：用 GitHub Action（stale bot 定制或自写 workflow）按 label/活动时间自动提醒、降级或关闭，配置豁免机制。工作量级：小时级到天级。风险：误关活跃但低活动 PR、与现有 bot 冲突、豁免规则难界定。
- 分析于 2026-09-25

### #1471 [困难|🟢机会] Optimize framework import time
- 优化框架导入时间，需重构大量动态导入，收益明确但工程量大
- 问题：litellm subtree 占导入耗时 ~75%（2165 模块，外部依赖），MCP 服务栈 ~300 模块为 railtracks 自身可避免的 eager import；本质是包级初始化策略问题。大概率涉及 `railtracks/__init__.py`、各子包顶层 import、litellm 引入方式（未核实具体懒加载基建是否存在）。
- 方案：将 100+ 处 import 改为函数级/`__getattr__` 惰性导入，重点先处理 MCP 栈与 requests 相关 provider；litellm subtree 需评估是否可延迟或按 provider 拆分。工作量级：周级。风险：动态导入易引发循环导入、API 兼容性（顶层符号暴露）破坏，需完整回归测试。
- 分析于 2026-09-25

### #1468 [中等|🟢机会] MessageHistory serializable and loadable
- MessageHistory/消息对象支持 JSON 序列化与反序列化加载
- 问题：UserMessage/ToolCall/ToolResponse 等均不可 JSON 序列化，无法保存/加载会话。本质是消息模型缺 encode/decode 全覆盖，替代 #1353。
- 方案：为全部消息内容类型实现 to/from JSON（如 discriminator tag），提供 MessageHistory 级别的 dump/load API，补测试（工作量级：天级）。风险：序列化格式是公共 API，需考虑向前兼容与 ToolResponse 内嵌对象（未核实其结构复杂度）。
- 分析于 2026-09-25

### #1463 [中等|🟢机会] Long file cleanup: _litellm_wrapper.py
- 重构超长文件 _litellm_wrapper.py 及测试，抽公共逻辑
- 问题：三个文件 1000+ 行，_litellm_wrapper.py 需抽父类、拆分流逻辑、用 dataclass 去重复。纯重构，无行为变更，但涉及较大面积代码搬移。
- 方案：提取 CommonHyperparameters dataclass、拆分 stream 逻辑到独立文件、精简 mixin，靠现有 1000+ 行测试保障回归（工作量级：天级）。风险：重构期间与并行开发冲突，测试本身也需同步拆分。
- 分析于 2026-09-25

## 全量总表

<details><summary>展开全部开放 issue</summary>

| # | 难度 | 状态 | 一句话 |
| --- | --- | --- | --- |
| #486 | 困难 | 🔒认领 | 文档应用+MCP服务，架构级新子系统，价值高但范围大 |
| #853 | 困难 | 🟡困难 | MCP 协议补全缺失能力，量大且涉及协议设计 |
| #873 | 简单 | 🟢机会 | Context 用法文档前置，纯文档改动，适合入门 |
| #881 | 中等 | 🔒PR占 | docstring 多格式解析，范围明确需改解析逻辑 |
| #940 | 简单 | 🟢机会 | BYFA/RYFA 教程叙事重构，纯文档改写 |
| #947 | 中等 | 🔒认领 | LLM 子模块独立 logger 配置，单模块日志改造 |
| #1004 | 困难 | 🟡困难 | 多种 Agent 架构文档父票，范围未定信息不足 |
| #1014 | 中等 | 🟢机会 | 评估指标增加 shots/examples 支持，DX 设计待定 |
| #1053 | 简单 | 🟢机会 | 长度护栏 Guardrail，接口已定范围明确 |
| #1065 | 中等 | 🟢机会 | 被拦截护栏的重试机制，需设计决策 |
| #1114 | 简单 | 🔒认领 | 文档语法内容修缺父票，长期跟踪，零散小改 |
| #1131 | 困难 | 🟡困难 | RetrievalRuntime 替换 RAGConfig，架构级重设计 |
| #1142 | 困难 | 🟡困难 | LLM 层缓存式上下文压缩，跨模块且依赖未完成项 |
| #1145 | 困难 | 🔒认领 | 默认插件批量父票，范围未拆分信息不足 |
| #1150 | 简单 | 🟢机会 | 存储层批量写入 EmbeddedChunk，接口扩展明确 |
| #1156 | 困难 | 🔒认领 | 用 system message 自动生成 agent manifest，自动化程度需设计决策 |
| #1160 | 中等 | 🟢机会 | 把 pdoc API 参考嵌入主文档站，提升一致性 |
| #1167 | 简单 | 🟢机会 | HF 数据集所有行 id 相同，破坏 upsert 语义 |
| #1172 | 中等 | 🟢机会 | 将 _Session 等内部类标记为 internal，防误用 |
| #1173 | 中等 | 🟢机会 | 新增 Turbovec 向量后端集成 |
| #1179 | 简单 | 🔒PR占 | 为文档核心术语定义 glossary 并互链 |
| #1184 | 中等 | 🟢机会 | ingest 逐 chunk 单次写库，需增加批量写接口 |
| #1185 | 中等 | 🟢机会 | 用户 metadata 可覆盖保留 payload 键导致数据损坏 |
| #1186 | 中等 | 🟢机会 | Chroma 后端 chunk 内容双份存储，浪费成本 |
| #1187 | 困难 | 🟡困难 | 部分失败的重新 ingest 会删掉旧好版本 |
| #1193 | 困难 | 🟡困难 | 用户 context 是无锁普通 dict，并发不安全 |
| #1203 | 中等 | 🟢机会 | 新增 Thoughts 工具：脑洞/键值/向量三种暂存草稿 |
| #1204 | 困难 | 🟡困难 | 文件系统工具：受控文件/shell 操作，设计问题未决 |
| #1209 | 中等 | 🟢机会 | pgvector 后端把所有 metadata 塞单 payload 列 |
| #1216 | 困难 | 🟡困难 | 键值存储后端支持（tracking issue，无具体范围） |
| #1217 | 困难 | 🔒认领 | 全链路思考块支持，跨流式/解析/序列化架构级改造 |
| #1223 | 简单 | 🟢机会 | 给预构建工具打标签以便可视化器区分展示 |
| #1228 | 简单 | 🔒认领 | JSON 存储改 UTF-16 以下提升人类/代理可读性 |
| #1233 | 困难 | 🔒认领 | 预构建工具集（内存/文件/TODO/搜索），tracking 票范围大 |
| #1239 | 困难 | 🔒认领 | 可观测性重构为事件流，架构级替换现有状态导出 |
| #1248 | 中等 | 🟢机会 | 引入 ModelRequest 类型统一模型调用入参 |
| #1251 | 中等 | 🟢机会 | viz 命令 --dir 标志形式化并修复子目录感知 bug |
| #1265 | 困难 | 🔒认领 | HIL 人机协同作为节点中间件，父票含设计决策 |
| #1266 | 中等 | 🔒认领 | 通用验证器中间件：callable 校验节点输入并抛异常 |
| #1306 | 中等 | 🟢机会 | ScopeGuard 护栏拦截超出范围的提问 |
| #1316 | 简单 | 🟢机会 | 改进自定义函数作为工具的文档说明 |
| #1320 | 中等 | 🟢机会 | 附件在消息中的位置交错，媒体块位置信息丢失 |
| #1321 | 困难 | 🟡困难 | 大附件走文件 API 自动上传，跨层功能 |
| #1343 | 困难 | 🔒认领 | 编码助手支持全面评估与改进，研究+实现混合票 |
| #1347 | 困难 | 🔒认领 | 富媒体支持 Epic，多方向大范围架构工作 |
| #1348 | 困难 | 🟡困难 | 让工具原生返回富内容，需重构类型与调用链，架构级改动 |
| #1349 | 中等 | 🟢机会 | Attachment 支持音频/视频/文件模态，单模块功能扩展 |
| #1353 | 中等 | 🟢机会 | OpenAI 格式无损导入导出 API，范围明确 |
| #1354 | 中等 | 🟢机会 | 通用 token 估算器，范围清晰但需选型设计 |
| #1355 | 困难 | 🟡困难 | 清除 539 个 Pyright 错误，量大且需逐一定位 |
| #1356 | 中等 | 🟢机会 | 统一 LLM 子类初始化接口与 from_string 工厂 |
| #1357 | 中等 | 🔒认领 | 让 Agent 响应暴露工具调用信息，需接口设计决策 |
| #1362 | 困难 | 🟡困难 | 重实现结构化工具调用 LLM，此前被移除、信息不足 |
| #1375 | 中等 | 🟢机会 | 清理文档页重复与命名冲突，量大但机械 |
| #1381 | 简单 | 🟢机会 | 添加 llms.txt / llms-full.txt，范围明确的文档任务 |
| #1393 | 简单 | 🔒认领 | 坏 schema 报错不友好，已在手的明确小修 |
| #1395 | 中等 | 🟢机会 | 为 agent-as-tool 类场景增加 Custom Tools 抽象 |
| #1398 | 中等 | 🟢机会 | post-init 赋值守卫，涉及大量字段迁移、规范类任务 |
| #1409 | 中等 | 🟢机会 | 成本限额中间件，设计已有雏形的功能 |
| #1413 | 中等 | 🟢机会 | 彩色日志迁移到事件驱动方案，需设计验证 |
| #1414 | 困难 | 🟡困难 | 节点创建事件需补充工具模型信息，设计导向、范围未定 |
| #1415 | 中等 | 🟢机会 | 为 ctrl+c 中断注册信号处理器，发送终止事件并刷新 |
| #1425 | 中等 | 🔒认领 | 把 verifier 决策结构化为可观测性事件而非日志 |
| #1430 | 中等 | 🔒认领 | LLM finish_reason 被丢弃，max token 截断时返回空串 |
| #1431 | 中等 | 🔒认领 | 在 LLM 响应/历史对象中透出 thinking tokens |
| #1433 | 简单 | 🔒PR占 | 成本计算纳入 cache hit 折扣 token，显著提升准确度 |
| #1435 | 中等 | 🟢机会 | railtracks 导入时全局修改 litellm 参数，应局部化 |
| #1446 | 中等 | 🟢机会 | 新增 context 创建/读写/完成四类可观测事件 |
| #1451 | 中等 | 🔒认领 | skill 安装从单文件改为目录，元数据从磁盘 SKILL.md 派生 |
| #1454 | 困难 | 🟡困难 | 节点创建时发布静态边检测事件，作为可视化结构图 |
| #1456 | 困难 | 🟡困难 | 重试时切换/降级 LLM 模型的 router 式接口，需设计审批 |
| #1458 | 中等 | 🔒认领 | from __future__ import annotations 下工具参数 schema 静默退化为 object |
| #1462 | 简单 | 🟢机会 | publisher 关闭后任务仍发布导致 RuntimeError，需条件保护 |
| #1463 | 中等 | 🟢机会 | 重构超长文件 _litellm_wrapper.py 及测试，抽公共逻辑 |
| #1468 | 中等 | 🟢机会 | MessageHistory/消息对象支持 JSON 序列化与反序列化加载 |
| #1471 | 困难 | 🟡困难 | 优化框架导入时间，需重构大量动态导入，收益明确但工程量大 |
| #1474 | 中等 | 🔒认领 | 为 ticket/PR 设置最长停留时长并自动延期机制，属流程规范类 |
| #1488 | 困难 | 🟡困难 | 提供开箱即用的终端 Assistant 模块，是较大新功能设计 |
| #1496 | 中等 | 🟢机会 | 给 InputGuard 加 once 开关避免每次工具循环重复触发，方案已明确 |
| #1503 | 中等 | 🟢机会 | visualizer 增加 AI 输入/输出的 markdown raw/preview 切换，范围明确的前端增强 |
| #1509 | 中等 | 🟢机会 | 统一错误类的 reason/message 术语并上提 notes+reason 到 RTError，需设计决策 |
| #1534 | 简单 | 🟢机会 | 删除 legacy 安装检测代码，前置条件已满足，范围清晰的清理任务 |
| #1547 | 简单 | 🟢机会 | 文档去模型特定表述（Claude Code、OpenAI 示例），纯文档编辑 |
| #1562 | 中等 | 🔒认领 | Tool.from_function 把未映射类型静默降级为 object，绕过 #1552 严格校验 |
| #1572 | 困难 | 🟡困难 | 通过环境上下文暴露当前节点/flow 元数据，需架构设计的新能力 |
| #1573 | 简单 | 🟢机会 | `railtracks add --force` 前置位置解析顺序 bug，定位明确的 CLI 修复 |
| #1574 | 中等 | 🟢机会 | 内部调用路径的 Session 触发 flow_name 警告，误扰正常用户 |
| #1576 | 困难 | 🟡困难 | OpenAI Responses-API reasoning_items 支持，前置依赖未就绪 |
| #1577 | 简单 | 🟢机会 | websearch 文档页缺 optional extra 安装说明，纯文档补全 |
| #1581 | 中等 | 🟢机会 | Union处理错误放宽required，修复明确且影响函数调用正确性 |
| #1582 | 中等 | 🟢机会 | 异构元组需改用prefixItems按位约束，修法明确但涉及schema版本兼容 |
| #1584 | 中等 | 🟢机会 | 聚合SUM掩盖未定价调用的null成本，与已修的单行端点行为不一致，站点明确 |
| #1588 | 中等 | 🟢机会 | 将 viz 中间件失败匹配从异常消息改为异常 id，避免误匹配 |

</details>

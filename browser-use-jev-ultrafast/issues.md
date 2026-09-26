# Issue 雷达 · browser-use/jev-ultrafast

> 全景扫描：2026-09-27 · 开放 issue 32 个 · 机会 0 · 未入榜 23 · 被占 9

## 未入榜（23）
- #149 Could the visible page state preserve more semantic structur —— 仓库从未合并外部 PR
- #145 Community project built on this: jev-browse (agent-callable  —— 难度困难
- #140 ioio —— 难度困难
- #115 Add a theme switch button —— 仓库从未合并外部 PR
- #100 qr-menu —— 难度困难
- #85 Latency from Japan: 12-17s per task - it's the client's link —— 难度困难
- #77 Create main.ym. —— 难度困难
- #67 Jev ultra fast Can't complete simple task —— 难度困难
- #55 Model-call budget (MAX_STEPS * 2) is undocumented and untest —— 仓库从未合并外部 PR
- #54 HIGH — README clone URL points to the wrong repository File: —— 仓库从未合并外部 PR
- #53 agent.py is minified and violates the project's own line-len —— 仓库从未合并外部 PR
- #51 Windows: owned tab is backgrounded, modal menus do not paint —— 仓库从未合并外部 PR
- #47 Minin —— 难度困难
- #46 Min —— 难度困难
- #26 Question about the Runtime/JeV boundary in jev-ultrafast —— 仓库从未合并外部 PR
- #25 [Feature Request]: Decouple Jev inference provider to suppor —— 仓库从未合并外部 PR
- #23 Element table misses clickable non-native elements, so sugge —— 难度困难
- #21 How can I get the TYPESAFE_API_KEY? —— 仓库从未合并外部 PR
- #16 Setup friction: SOCKS proxy crash on startup; browser-harnes —— 仓库从未合并外部 PR
- #14 app not functioning —— 难度困难
- #11 Support LLMTR as a text-helper provider —— 仓库从未合并外部 PR
- #5 Fix Windows static asset decoding in demo server —— 仓库从未合并外部 PR
- #1 Library API: first observation can return an empty action sp —— 难度困难

## 已被占（不必再看）
- #133 choose() can return raw KeyError for an off-envelope HTTP-200 response —— 被 PR#135 占
- #132 Stale action ids can select a different live control without an error —— 被 PR#148 占
- #129 MDN wrong-waypoint actions —— 被 PR#151 占
- #125 Feature Request: Support local resident decision backend (SemIf) and Model Context Protocol (MCP) —— 被 PR#126 占
- #120 Expose a confidence gate for ordinary browser actions —— 被 PR#127 占
- #94 A failed post-action observation (page_changed: null) disables the three-repeat no-progress check, so a stalled run keeps spending model calls —— 被 PR#131 占
- #93 The bundled Google Flights demo can no longer succeed: its goal date (2026-09-20) is in the past and past days are not indexed —— 被 PR#109 占
- #87 Model responses with invalid JSON leak decoder errors —— 被 PR#103 占
- #36 Missing TEXT_MODEL_API_KEY crashes mid-run; code default base URL differs from .env.example —— 被 PR#147 占

## 分析详情（最新分析在前）
### #149 [中等|🟢机会] Could the visible page state preserve more semantic structure?
- 建议保留页面快照语义结构，单模块改进，值得做
- 问题：`snapshot.js` 用 TreeWalker 收集可见文本后 `join('\n').slice(0, 6000)` 扁平化，丢失 DOM 层级/分组关系；`model.py` 将其作为 page.text 连同索引元素表发给 Jev。本质是观测表征质量与 6000 字符预算的权衡（未核实：具体截断策略细节）。
- 方案：在快照中保留轻量结构标记（如标题/区块边界缩进或标签），或分层截断；同时评估 token 成本与 Jev 理解收益。工作量级：天级。风险点：破坏现有 prompt 格式兼容性、结构化文本变长挤压 6000 预算，需基准验证。
- 分析于 2026-09-27

### #145 [困难|🟢机会] Community project built on this: jev-browse (agent-callable loop, MCP, benchmark harness), with ideas for #16, #93, #120, #132
- 社区衍生项目投稿，涉及上游集成决策，需维护者定方向
- 问题：本质是展示外部项目 jev-browse 并提议将 MCP、speculative targets 等能力回馈上游（关联 #16/#93/#120/#132），非具体 bug。信息不足：正文截断，仅见 #16 开头，各映射点未展开，需读其仓库代码核实。
- 方案：先审 jev-browse 源码，逐项评估哪些能力（MCP server、fast_run 单调用循环、后端抽象）值得合入上游；涉及架构取舍与多模块改动，建议拆分子 issue。工作量级：周级（评估+集成）。风险点：范围蔓延、许可证/署名合规、与现有 Jev 接口设计冲突。
- 分析于 2026-09-27

### #140 [困难|🟢机会] ioio
- 正文为空的"ioio"，无任何信息，无法分析
- 问题：标题"ioio"无实义，正文为空，无上下文。信息不足。
- 方案：需向提交人澄清意图，澄清前无法评估。（工作量级：无法评估）
- 分析于 2026-09-27

### #133 [中等|🔒PR占] choose() can return raw KeyError for an off-envelope HTTP-200 response
- choose() 三处裸索引响应信封，HTTP 200 异常体抛原始 KeyError
- 问题：model.py choose() 三处直接索引 result["answers"]/result["model"]，异常信封导致裸 KeyError，与 #87 同类问题但位置不同；报告者诚实标注为"live-corroborated-not-proven"，另一可疑源是 os.environ["TYPESAFE_API_KEY"] 直接索引。
- 方案：对三处信封索引加守卫，缺失时抛带上下文的 RuntimeError（与既有错误风格一致）；顺带审查 API key 索引。位置已确认，逻辑清晰，需补 mocked 响应测试。（工作量级：小时级到天级）
- 分析于 2026-09-27

### #132 [中等|🔒PR占] Stale action ids can select a different live control without an error
- 快照 id 按位置分配，陈旧 id 可静默命中错误控件，属正确性缺陷
- 问题：snapshot.js 中 e-id 在 splice(250) 后按位置赋值，动作解析仅按 id 匹配（agent.py 的 next(...)），不校验 node/role/label，页面变化后旧 id 可能指向新控件，liveness 检查通过但点错目标，无异常。核心是标识与解析不一致。
- 方案：在解析时额外比对 WeakMap node 或 role+label；不一致时报错或重新观察。需改动 snapshot.js 与 agent.py 的解析路径，注意对 scroll/wait 等稳定 id 的豁免，及误报率控制。（工作量级：天级）
- 分析于 2026-09-27

### #129 [困难|🔒PR占] MDN wrong-waypoint actions
- MDN 用例路径断言失败，正文只有数据缺问题描述
- 问题：给出一个 MDN 多路点用例配置（HTML→CSS→JavaScript 依次点击），但未说明失败现象、期望与实际差异；标题"wrong-waypoint actions"暗示执行偏离路径，但无日志无复现细节。信息不足。
- 方案：需先复现该 scenario，判断是 id 漂移（可能与 #132 相关）、页面结构变化还是模型决策问题，才能定位修复。（工作量级：天级，且可能无法复现）
- 分析于 2026-09-27

### #125 [困难|🔒PR占] Feature Request: Support local resident decision backend (SemIf) and Model Context Protocol (MCP)
- 支持本地决策后端与 MCP 集成，跨模块架构级功能
- 问题：当前仅支持云端 TypeSafe API，请求支持本地 SemIf 后端（SEMIF_BASE_URL）及 stdio MCP server。涉及模型后端抽象层、配置体系、新增 MCP 服务器模块，需架构设计与接口决策。
- 方案：抽象决策后端接口 → 实现 SemIf 适配器（两阶段层级决策循环）→ 独立 stdio MCP server 包；工作量大，需维护双后端兼容与协议设计。（工作量级：周级）
- 分析于 2026-09-27

### #120 [中等|🔒PR占] Expose a confidence gate for ordinary browser actions
- 为普通浏览器动作暴露置信度门槛，涉及决策执行核心逻辑
- 问题：agent.py 已记录 confidence/target_confidence，但只有 DONE/BLOCKED 等终态决策有置信度门控（#99 相关），普通动作直接执行，低置信度误点无防护。涉及 agent 决策-执行主链路与配置面设计。
- 方案：在 act 执行路径加可配置阈值（低于阈值时拒绝执行/降级为 blocked），需设计默认值、配置暴露方式（env/config）、与 #99 终态门控的一致性；报告者附有测量数据但正文截断。存在行为变更风险（阈值过严导致 run 频繁卡死）。（工作量级：天级）
- 分析于 2026-09-27

### #115 [简单|🟢机会] Add a theme switch button
- 落地页增加主题切换按钮，范围明确的前端小功能
- 问题：官网无明暗主题切换，提议在导航栏加按钮，附带截图，需求清晰，纯前端 UI 功能。
- 方案：加主题状态（localStorage 持久化）、切换按钮、CSS 变量/暗色样式；需注意与现有样式体系统一。标准前端任务。（工作量级：小时级）
- 分析于 2026-09-27

### #100 [困难|🟢机会] qr-menu
- 正文为空的"qr-menu"，无任何信息，无法分析
- 问题：标题仅"qr-menu"，正文为空，无描述、无复现步骤、无明确诉求。信息不足。
- 方案：需向提交人澄清需求（二维码菜单相关功能？bug？），澄清前无法评估。（工作量级：无法评估）
- 分析于 2026-09-27

### #94 [中等|🔒PR占] A failed post-action observation (page_changed: null) disables the three-repeat no-progress check, so a stalled run keeps spending model calls
- 观察失败时 page_changed=None 使停滞检测失效，run 持续烧调用
- 问题：agent.py 153-158 行的三次无进展检查要求 page_changed is False，但观察异常时该值为 None，False/None 交替使检查永不触发，停滞 run 持续消耗模型调用。核心是状态判定逻辑不健壮。
- 方案：修改停滞检测条件，将 None（观察失败）视为无进展信号之一（如 `h["page_changed"] is not True`），或区分观察失败与成功不动；同时补对应测试。逻辑集中在一处但需考虑误判风险（暂时性观察超时不应立即算停滞）。（工作量级：小时级到天级）
- 分析于 2026-09-27

### #93 [简单|🔒PR占] The bundled Google Flights demo can no longer succeed: its goal date (2026-09-20) is in the past and past days are not indexed
- 演示目标日期硬编码为过去日期导致 demo 必然失败，需更新或参数化
- 问题：flights demo 的目标日期 2026-09-20 已过期，Google Flights 不再索引过去日期，导致所有演示路径 blocked。涉及 static/app.js、index.html、examples/flights.py、README、docs 多处硬编码。
- 方案：短期更新日期为未来；长期将日期参数化/相对化（如"下周日"）。跨 5+ 文件但均为配置/文案级改动；注意 examples/flights.py 中 verify() 断言需同步。（工作量级：小时级）
- 分析于 2026-09-27

### #87 [简单|🔒PR占] Model responses with invalid JSON leak decoder errors
- HTTP 200 非 JSON 响应泄漏解码错误，应包装为清晰 RuntimeError
- 问题：post_json() 在 HTTP 成功后直接调 response.json()，非 JSON 体导致 ValueError/JSONDecodeError 直接逃逸，与既有错误路径（"no action executed"）风格不一致。
- 方案：在 post_json() 中捕获 JSON 解码异常，包装为 RuntimeError("Model provider returned invalid JSON; no action executed.")，并补单测。范围明确，单函数修改。（工作量级：小时级）
- 分析于 2026-09-27

### #85 [困难|🟢机会] Latency from Japan: 12-17s per task - it's the client's link, not the region (3-location measurement)
- 日本延迟 12-17s，报告者已证明非区域问题，属性能排查
- 问题：从日本访问延迟高，报告者三地测量后认为瓶颈在客户端链路而非区域；正文被截断，结论与建议不完整。这不是明确缺陷，更像性能数据分享，优化点不明（可能涉及连接复用、超时配置等）。信息不足。
- 方案：需基于报告者完整数据定位客户端/网络路径瓶颈，再决定是否做连接优化或文档化部署建议。（工作量级：天级）
- 分析于 2026-09-27

### #77 [困难|🟢机会] Create main.ym.
- 请求创建 main.yml workflow，正文不完整，信息不足
- 问题：要求创建 GitHub Actions workflow 文件，但正文极不完整：无文件名后缀（"main.ym."）、无具体 job 内容、触发场景不明（"Windows Cloud Pc amydesk" 含义不明）。信息不足。
- 方案：需与提交人澄清意图后才可判断；若只是加 workflow_dispatch 的空壳 yml 则极简单，但当前无法确认。（工作量级：无法评估）
- 分析于 2026-09-27

### #67 [困难|🟢机会] Jev ultra fast Can't complete simple task
- 简单公交查询任务多处失败，根因不明，需排查定位
- 问题：多步任务（地名转地址、公交路线查询）在多个环节出错，报告者自己也未定位单一根因；截图与描述信息有限，可能涉及模型能力、上下文构造、页面交互等多方面。信息不足，无法定位具体模块。
- 方案：需先复现并逐层定位（模型决策日志、页面快照、上下文构造），可能是 prompt/状态管理改进或能力边界问题。（工作量级：天级，且可能不可修）
- 分析于 2026-09-27

### #55 [简单|🟢机会] Model-call budget (MAX_STEPS * 2) is undocumented and untested
- 模型调用预算（MAX_STEPS*2=120）未文档化未测试，值得补文档与边界测试
- 问题：agent.py 中预算为动作预算的 2 倍（120 次），README 未说明，错误信息含糊，tests/test_agent.py 缺边界测试。改动集中在文档与测试。
- 方案：README 补充说明 + 错误信息写明具体数字 + 添加预算边界测试；范围明确，纯文档/测试级修改。（工作量级：小时级）
- 分析于 2026-09-27

### #54 [简单|🟢机会] HIGH — README clone URL points to the wrong repository File: ```README.md``` (line 32)
- README 克隆 URL 指向错误仓库，文档级修正
- 问题：README "Try it" 的 git clone 地址与实际仓库不符，误导用户。
- 方案：改为正确 clone URL（未核实真实正确地址，需确认）。一行文档修改。（工作量级：小时级）
- 分析于 2026-09-27

### #53 [中等|🟢机会] agent.py is minified and violates the project's own line-length configuration ```File: jev_ultrafast/agent.py```
- agent.py 被压缩成超长行，违反项目自身 ruff line-length 配置
- 问题：agent.py 82 行全被压缩，多处超 300 字符，违反 pyproject.toml line-length=120。
- 方案：用 ruff/black 按配置重新格式化并跑测试确认无行为变更。机械性工作。（工作量级：小时级）
- 分析于 2026-09-27

### #51 [中等|🟢机会] Windows: owned tab is backgrounded, modal menus do not paint in time, Flights demo always returns BLOCKED
- Windows 后台标签页不绘制致模态遮挡判 BLOCKED，加 bringToFront 即可修
- 问题：owned tab 后台创建，setFocusEmulationEnabled 不恢复前台绘制，元素表为空致 BLOCKED。与 #1 根因不同。
- 方案：创建后调用 Page.bringToFront；提交者已验证 3/3 通过。单点修复，风险低。（工作量级：小时级）
- 分析于 2026-09-27

## 全量总表

<details><summary>展开全部开放 issue</summary>

| # | 难度 | 状态 | 一句话 |
| --- | --- | --- | --- |
| #1 | 困难 | 🟡困难 | 初始化竞态致空动作空间即BLOCKED，需修导航等待逻辑，值得做 |
| #5 | 简单 | 🟢机会 | Windows 下 read_text 未指定编码致静态资源加载失败，明确值得修 |
| #11 | 中等 | 🟢机会 | 为 LLMTR 提供 OpenAI 兼容适配，需协商 reasoning 字段行为 |
| #14 | 困难 | 🟡困难 | 截图守护 5 秒超时、页面索引失败，根因信息不足 |
| #16 | 中等 | 🟢机会 | SOCKS 代理导致导入期崩溃，需补 socks 依赖或代理处理，值得做 |
| #21 | 简单 | 🟢机会 | 用户询问 TYPESAFE_API_KEY 获取方式，属文档/支持问题 |
| #23 | 困难 | 🟡困难 | 元素表漏掉非原生可点击元素，需可点击性启发式设计，价值高但难 |
| #25 | 中等 | 🟢机会 | 推理 provider 解耦功能请求，架构抽象需设计，值得评估 |
| #26 | 中等 | 🟢机会 | 关于 Runtime/Jev 边界的讨论，非缺陷，属设计交流 |
| #36 | 中等 | 🔒PR占 | TEXT_MODEL_API_KEY 缺失在运行中途才报错，且默认 base URL 与文档不一致，值得修 |
| #46 | 困难 | 🟡困难 | 内容为继电器电路 ASCII 图，与本仓库无关，疑似误投 |
| #47 | 困难 | 🟡困难 | 内容同 #46 的继电器电路图重复贴，疑似误投或垃圾内容 |
| #51 | 中等 | 🟢机会 | Windows 后台标签页不绘制致模态遮挡判 BLOCKED，加 bringToFront 即可修 |
| #53 | 中等 | 🟢机会 | agent.py 被压缩成超长行，违反项目自身 ruff line-length 配置 |
| #54 | 简单 | 🟢机会 | README 克隆 URL 指向错误仓库，文档级修正 |
| #55 | 简单 | 🟢机会 | 模型调用预算（MAX_STEPS*2=120）未文档化未测试，值得补文档与边界测试 |
| #67 | 困难 | 🟡困难 | 简单公交查询任务多处失败，根因不明，需排查定位 |
| #77 | 困难 | 🟡困难 | 请求创建 main.yml workflow，正文不完整，信息不足 |
| #85 | 困难 | 🟡困难 | 日本延迟 12-17s，报告者已证明非区域问题，属性能排查 |
| #87 | 简单 | 🔒PR占 | HTTP 200 非 JSON 响应泄漏解码错误，应包装为清晰 RuntimeError |
| #93 | 简单 | 🔒PR占 | 演示目标日期硬编码为过去日期导致 demo 必然失败，需更新或参数化 |
| #94 | 中等 | 🔒PR占 | 观察失败时 page_changed=None 使停滞检测失效，run 持续烧调用 |
| #100 | 困难 | 🟡困难 | 正文为空的"qr-menu"，无任何信息，无法分析 |
| #115 | 简单 | 🟢机会 | 落地页增加主题切换按钮，范围明确的前端小功能 |
| #120 | 中等 | 🔒PR占 | 为普通浏览器动作暴露置信度门槛，涉及决策执行核心逻辑 |
| #125 | 困难 | 🔒PR占 | 支持本地决策后端与 MCP 集成，跨模块架构级功能 |
| #129 | 困难 | 🔒PR占 | MDN 用例路径断言失败，正文只有数据缺问题描述 |
| #132 | 中等 | 🔒PR占 | 快照 id 按位置分配，陈旧 id 可静默命中错误控件，属正确性缺陷 |
| #133 | 中等 | 🔒PR占 | choose() 三处裸索引响应信封，HTTP 200 异常体抛原始 KeyError |
| #140 | 困难 | 🟡困难 | 正文为空的"ioio"，无任何信息，无法分析 |
| #145 | 困难 | 🟡困难 | 社区衍生项目投稿，涉及上游集成决策，需维护者定方向 |
| #149 | 中等 | 🟢机会 | 建议保留页面快照语义结构，单模块改进，值得做 |

</details>

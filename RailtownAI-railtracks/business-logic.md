# 核心业务流程

## 1. 构建并调用第一个 Agent

主路径：函数 → 工具节点 → agent → Flow.invoke。

```mermaid
flowchart TD
    A[开发者定义普通函数] --> B[rt.function_node 装饰/包装]
    B --> C[rt.agent_node 组装: tools + llm + system_message]
    C --> D[rt.Flow 构建 entry_point]
    D --> E[flow.invoke 输入]
    E --> F[返回 result.text 或 result.structured]
```

docstring 即工具描述；`output_schema`（pydantic）时返回 `result.structured`（README.md、docs/scripts/first_agent.py）。

## 2. Agent 工具调用循环（多步协作）

agent 内部循环调用模型，模型可发起工具调用，工具间用 `rt.call` 组合（可为子 agent），同步/异步统一。

```mermaid
sequenceDiagram
    participant U as 用户
    participant F as Flow
    participant A as AgentNode
    participant M as LLM
    participant T as 工具/子Agent
    U->>F: invoke(prompt)
    F->>A: 运行
    loop 工具调用循环
        A->>M: message_history
        M-->>A: 文本 或 工具调用请求
        A->>T: rt.call(tool, args)
        T-->>A: 结果
    end
    A-->>F: 最终 Response
    F-->>U: result.text / result.structured
```

顺序、分支、循环直接用 Python 控制流表达；并行用 `asyncio.gather(*rt.call(...))`（docs/scripts/flows.py、async_await.py）。

## 3. 中间件与控制（Controls）

两层中间件在调用链上洋葱式包裹；verifier 是人审/HIL 的统一形态。

```mermaid
flowchart TD
    Call[节点调用] --> NodeMW[middleware: 外层 wrap_node / pre_verifier / post_verifier / Retry / Timeout / MaxCalls]
    NodeMW --> IsAgent{是 agent 节点?}
    IsAgent -->|是| ModelMW[model_middleware: pre_llm / ContextInjection / PII Guard / wrap_llm / post_llm]
    IsAgent -->|否| Exec[执行函数体]
    ModelMW --> LLMCall[模型调用]
    LLMCall --> Exec
    NodeMW -->|pre_verifier 拒绝| Rej[抛 VerifierRejectedError]
```

`MaxCalls` 必须放 `model_middleware` 槽位才生效（issue #1560，examples/harness/README.md）；`post_verifier` 可改写结果（如脱敏），`couple()` 后加的中间件位于最外层（docs/scripts/middleware.py ordering 示例）。

## 4. Flow / Connection 生命周期与上下文

```mermaid
sequenceDiagram
    participant Dev as 开发者
    participant Flow as Flow
    participant Conn as FlowConnection
    participant Ctx as rt.context
    Dev->>Flow: Flow(name, entry_point, context, timeout, save_state)
    Dev->>Flow: connect() 或 invoke()
    Flow->>Conn: 每次运行一个 Connection
    Conn->>Ctx: 注入共享/运行级上下文
    Conn->>Conn: ainvoke（可多连接并发 gather）
    Conn-->>Dev: 结束后仍可读 context 与 message_histories
    Note over Conn: 失败时 context 仍可读<br/>save_state=True 落盘 .railtracks/*.json
```

`.update_context()` 派生不同上下文的运行；`end_on_error` 控制异常语义（docs/scripts/flows_sessions.py、session.py）。

## 5. MCP 双向集成

```mermaid
flowchart LR
    RT[railtracks] -->|connect_mcp HTTP/Stdio| Remote[外部 MCP server]
    Remote -->|server.tools| RTAgent[agent 的 tool_nodes]
    RT -->|create_mcp_server| Export[把 RT 节点导出为 MCP server]
    Export -->|mcp.run streamable-http| 外部客户端
```

多 server 工具可拼接为 `all_tools` 供一个 agent 使用（docs/scripts/MCP_tools_in_RT.py、RTtoMCP.py）。

## 6. 检索（RAG）管道

ingestion（chunk + embed + 写入）→ query（embed + scope + top_k + 过滤）→ 结果带 rank/score；后端可换 InMemory（可 snapshot）/Pgvector/Chroma/ChromaCloud，语义或词法搜索可插拔到记忆工具集（docs/retrieval/、docs/scripts/retrieval/store.py、key_value_memory.py）。

## 非核心流程（文字带过）

- **评估**：`rt.evaluations.evaluate` 用 judge/llm_inference/tool_use evaluator + metric 打分并可视化（docs/evaluations/）。
- **错误处理**：LLMError 族细分（Timeout/RateLimit/Auth），推荐指数退避重试可瞬时错误、跨 provider fallback、`err.format_verbose()` 带完整 message_history（docs/scripts/error_handling.py）。
- **观测**：`enable_logging` + broadcast_callback + `railtracks viz` 回放。
- **CLI skillkit**：向 Claude/Codex/Copilot/Cursor 安装代码风格技能。
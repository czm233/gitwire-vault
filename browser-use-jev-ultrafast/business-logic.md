# 核心业务流程 · jev-ultrafast

## 1. 决策循环（predict → act）

```mermaid
sequenceDiagram
    participant AG as Agent
    participant BR as Browser
    participant TS as TypeSafe API
    participant EX as Chrome CDP
    AG->>BR: fresh(page)? 否则重新 observe
    AG->>TS: 一次请求（operation + click/type_text/select_target heads）
    TS-->>AG: 各 head 概率分布
    AG->>AG: validate_choice（仅校验被选操作对应的 target head）
    AG->>AG: 消费 decision（置空，防重试双击）
    AG->>BR: act(action, page, text?)
    BR->>BR: fresh + 命中测试 + 可见/禁用守卫
    BR->>EX: 鼠标事件 / insertText / select change
    AG->>BR: observe（记录执行后才读）
    AG->>AG: 更新 history / 判定 blocked（3次无变化非wait）
```

说明：DONE 仍需调用方独立验证结果（examples/flights.py verify），模型答案本身不作数（agent.py、README.md）。

## 2. 原子 DOM 快照与指纹

```mermaid
flowchart TD
    A[Runtime.evaluate snapshot.js 一次读出] --> B[可见控件 列表 + 节点身份]
    A --> C[可见文本 ≤6000字符]
    A --> D[page_key + per-node guards]
    B --> E[fingerprint = sha256 url+text+actions+scroll]
    C --> E
    D --> E
```

说明：指纹比较语义与节点身份，几何不参与——动画不触发重新预测，输入前实时解析几何并 hit-test（snapshot.js、browser.py fingerprint、check_guards.py）。

## 3. TYPE_TEXT 文本生成与复用

```mermaid
flowchart TD
    A[action.kind == fill] --> B{fresh?}
    B -- 否 --> C[StalePage 重新选择]
    B -- 是 --> D[field_context 构造]
    D --> E{pending_text 上下文完全相同?}
    E -- 是 --> F[复用已生成文本 不再调用LLM]
    E -- 否 --> G[调用 OpenAI 兼容模型]
    G --> H{仅含 text 键 且为非空字符串 ≤2000?}
    H -- 否 --> I[ValueError 什么都不输入]
    H -- 是 --> J[insertText 输入]
```

说明：陈旧页重试仅在文本助手完整输入不变时复用生成值；无 `TEXT_MODEL_API_KEY` 时直接报错、绝不猜测（agent.py、model.py、tests）。

## 4. 目标空间构造（一次一索引，按操作分头）

```mermaid
flowchart LR
    A[观测动作列表] --> B[按 DOM 节点合并为元素]
    B --> C[元素.operations: TYPE_TEXT / CLICK / SELECT]
    B --> D[targets per operation: 仅兼容元素与索引]
    A --> E[controls: SCROLL / WAIT]
    D --> F[SELECT 目标带 option 索引 e.g. 3:2]
```

说明：一个节点可同时提供 fill 与 click 两个动作（`Open <label>`），但只占一个索引；未选中的投机 head 结果不会被执行（model.py action_space / choose）。

## 5. 执行前守卫与陈旧页处理

```mermaid
flowchart TD
    A[act 请求] --> B{fresh page, action?}
    B -- 否 --> C[StalePage 不发浏览器输入]
    B -- 是 --> D[node 解析 + 禁用/只读/可见/遮挡检查]
    D -- 失败 --> E[select: RuntimeError; 其他: StalePage]
    D -- 通过 --> F[CDP 鼠标/键盘/insertText]
    F --> G[after_input 等待 建议/动画帧]
```

说明：下拉选择若被导航中断（Execution context destroyed）不可作为陈旧页重试，必须人工检查（browser.py、tests/test_agent.py）。

## 非核心流程（文字带过）

- **demo inspector**：本地 HTTP 服务 + Token 鉴权，`reset/predict/act/tick` 命令转发到 Agent，串行锁防并发步骤。
- **录制/测量/渲染**：record_flights.py 连续 CDP screencast 保留原始时间戳；measure_flights.py 包装 cdp 统计调用数与耗时；render_demo.py 用 ffmpeg 输出 1× mp4/gif 并裁掉 Google 账号条。
- **check_guards.py**：本地 data: URL 页面无模型调用地回归全部执行守卫。
# 核心业务流程

## 1. 语音回合：说话 → 转写 → LLM → 开口

```mermaid
sequenceDiagram
    participant U as 用户(网页)
    participant A as AgentSession
    participant S as STT(本地)
    participant G as llm_gateway
    participant L as 远端 LLM
    U->>A: 麦克风音频
    A->>S: VAD 收口(0.4s 静音)+语义轮次检测
    S-->>A: final 转写
    A->>G: chat(messages)
    G->>L: 唯一上游请求(流式)
    L-->>G: 增量 chunk
    G-->>A: delta
    A-->>U: 字幕先行(sync_transcription=false)+say 合成音频
```

preemptive_generation 关闭，请求只在用户轮次确认后发起（agent/app/main.py、agent/app/tutor_agent.py）。

## 2. 开口打断

```mermaid
sequenceDiagram
    participant U as 用户
    participant A as Agent
    participant G as SerialRequests
    participant L as 远端 LLM
    U->>A: 打断时开口(VAD mode, min_duration 0.15s, resume=False)
    A->>A: 立即停止旧回答播放，丢弃已合成结果
    A->>G: input_started() abandon 旧请求读者
    G->>L: 旧请求后台继续读到 EOF(不取消)
    U->>A: 说完新内容
    A->>G: 新请求排队，仅保留最新 pending
    Note over G,L: 旧请求结束后发送含完整上下文(A+B+C+D)的新请求
```

打断即停绝不续播；迟到旧字幕不得覆盖定稿（agent/app/llm_gateway/serial.py、README.md）。

## 3. 改错重发（vt.control / vt.events）

```mermaid
sequenceDiagram
    participant U as 用户
    participant A as amend.py
    participant S as AgentSession
    U->>A: user_amend(replace_last=true, text)
    A->>S: interrupt(force=True) 先打断再改历史
    A->>S: 定位上一条 user 消息→替换→截断其后内容
    A->>S: generate_reply()
    A-->>U: amend_ack(replaced_segment_id) / amend_error
```

先打断后截断的原因：interrupt 会把半截 assistant 回答写回上下文，先截断会以 assistant 结尾被 Anthropic 协议拒绝（agent/app/amend.py）。

## 4. LLM 渠道即时切换与 429 处理

```mermaid
flowchart TD
    A[客户端 llm_switch via vt.control] --> B{provider 可用且 enabled?}
    B -- 否 --> E[llm_error 事件]
    B -- 是 --> C[create_provider 热替换 set_provider]
    C --> F[llm_switched + llm_providers 事件]
    G[请求遇 429 且尚无输出] --> H[按 retry-after/退避最多重试 2 次]
    H --> I[重试期间有新输入则放弃旧快照改发最新]
    J[超时/断流等无法确认远端结束] --> K[fail-closed: 暂停该通话后续请求并页面提示]
```

## 5. 中英混合 TTS

flowchart 文字描述：LLM 流式出句 → SentenceTokenizer 按中英标点切句 → 每句 split_language_runs 分段（中文 voice_zh / 英文 voice_en）→ _SYNTH_LOCK 串行 say 合成、WAV 格式校验（单声道/16bit/采样率/长度）→ 按原顺序 50ms 块推流。打断时已开始的合成允许结束但丢弃结果并跳过剩余片段（agent/app/plugins/macos_tts.py、prefetch_tts.py）。

## 6. 按键说话（PTT）

按住：ptt_start → 开麦 + 老师音频静音（零回声）+ stt.begin_hold；松开：ptt_end → stt.end_hold 整段终结，不依赖 VAD 静音判定；迟到临时识别不能串入下一句（server/web-test.html、agent/app/main.py）。

**非核心流程**（文字带过）：语速运行时调节（tts_rate 控制消息，set_rate 影响后续句子）；麦克风选择与虚拟设备过滤（VIRTUAL_MIC_RE）及 4 秒无声警告；token 失效自动重取；acceptance.py 服务生命周期管理（端口归属校验、进程身份/cwd 绑定）。
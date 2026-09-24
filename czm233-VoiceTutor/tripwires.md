# Tripwires · 持续盯防

1. **隐私底线：音频不得出机器**。main.py 显式禁用 adaptive 打断检测（会连 LiveKit 云端）。任何引入云端 STT/TTS/turn detection 的改动都违背核心卖点。依据：agent/app/main.py 注释。
2. **单请求串行约束易被破坏**。README 承诺"同一通话最多 1 个上游 LLM 请求"，由 serial.py 保证（fail-closed、preemptive_generation 关闭）。若有人改回框架默认并发生成会重现 429 累积并发问题。依据：agent/app/llm_gateway/serial.py、main.py。
3. **amend 协议顺序敏感**。必须先 interrupt 再截断历史，否则上下文以 assistant 结尾被 Anthropic 拒绝——重构 amend.py 时高危。依据：agent/app/amend.py。
4. **Anthropic 反代 base_url 硬编码公网 IP**：`http://192.204.60.97:8317`（gemini_proxy，test_config.py 断言）——明文 HTTP 且 IP 会漂移，属临时配置，需确认是否有意长期保留。依据：agent/tests/test_config.py。未核实其用途稳定性。
5. **验收脚本硬编码本机用户路径**：acceptance.py 读 `/Users/czm/.config/local-deploy/ports.yaml`，换环境即失效；也算轻度个人信息暴露。依据：scripts/acceptance.py。
6. **dev 模式任意房间自动 dispatch + 固定 dev key**（vt-dev-secret-…）：仅限本机回环尚可，进入分发/联网阶段必须替换随机密钥（livekit.dev.yaml 注释已自知）。依据：server/livekit.dev.yaml、main.py。
7. **字幕定序是回归重灾区**：乱序/迟到流/空流/纯标点等边界已有 8 个 node 测试守护（server/tests/transcript-stream.test.cjs），改 STT 转发逻辑时必须跑 `node --test`。
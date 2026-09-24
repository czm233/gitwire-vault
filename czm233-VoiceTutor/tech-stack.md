# 技术栈

| 类别 | 选型 | 用途 |
|---|---|---|
| 语言 | Python 3（agent/）、JavaScript（server/ 临时客户端） | Agent 管线 / 网页验证客户端 |
| 核心框架 | LiveKit Agents（`livekit.agents`：AgentSession、VAD、turn_detector、RoomIO） | 实时语音对话管线、打断、轮次检测 |
| 实时通信 | livekit-server（本地回环，端口 10230–10234）+ livekit-client.js（同源伺服，594KB 不走 CDN） | WebRTC 房间、音频轨、text stream 控制通道 |
| STT | SenseVoice（sherpa-onnx，默认）或 MLX whisper（medium/small，interim 字幕） | 本机语音转写（agent/app/plugins/sensevoice_stt.py、whisper_stt.py） |
| TTS | macOS `say` 系统语音（中英分段选音色 Tingting/Samantha，串行合成 + PrefetchStreamAdapter） | 本机语音合成（agent/app/plugins/macos_tts.py） |
| LLM | 自研 llm_gateway 适配层：openai_compatible / anthropic / mock 三类 provider（Gemini、GLM Coding Plan、智谱、OpenAI、Claude、本机反代） | 可独立单测的 LLM 抽象，SerialRequests 单请求串行 + 429 显式重试（agent/app/llm_gateway/） |
| 配置 | config/config.yaml（pydantic 校验）+ .env（dotenv），api_key 只存环境变量名 | 客户端与 Agent 共享唯一配置源 |
| 测试 | pytest（agent/tests/，7 个文件）、node --test（server/tests/） | gateway/config/TTS/segment-STT/serial 单测、字幕流乱序与打断回归 |
| 部署 | 本机脚本（bootstrap.sh / dev.sh / acceptance.py up-down，PID 登记 .runtime/，无 Docker 无数据库） | 本机验收服务生命周期管理 |
| 文档 | docs/requirements.md、docs/architecture.md | 需求规格与架构设计 |

信源：README.md、agent/pyproject.toml（未核实内容，按文件名与引用推断）、config/config.yaml、scripts/acceptance.py。
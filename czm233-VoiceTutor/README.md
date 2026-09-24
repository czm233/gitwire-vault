# VoiceTutor 情报档案

**一句话定位**：macOS 本地语音英语老师——LiveKit Agent 管线跑全链路，STT/TTS 全在本机（隐私底线：音频不出机器），仅 LLM 文本出网。（README.md）

## 档案索引

- [tech-stack.md](tech-stack.md) —— 语言 / 框架 / 测试 / 工具链清单
- [architecture.md](architecture.md) —— 组件架构图与职责
- [business-logic.md](business-logic.md) —— 核心业务流程（对话回合、打断、改错重发、渠道切换等）
- [tripwires.md](tripwires.md) —— 持续盯防问题
- [changelog/2026-09-24-263d783.md](changelog/2026-09-24-263d783.md) —— 首次全量建档

## 项目状态

- 当前阶段：M0 —— Agent 侧全链路（临时网页客户端验证，Swift 壳就绪后 web-test.html 废弃）（README.md）
- 最近同步：`∅ → 263d783`（2026-09-24，init 全量建档）
- 仓库规模：~30 文件；Python Agent（agent/）+ Node/HTML 临时客户端（server/）+ 脚本与文档
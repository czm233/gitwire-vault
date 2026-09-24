# 技术栈 · jev-ultrafast

| 类别 | 选型 | 版本/约束 | 用途 | 信源 |
| --- | --- | --- | --- | --- |
| 语言 | Python | >=3.12 | 核心库与全部脚本 | pyproject.toml |
| 语言 | JavaScript | ES（Node 校验语法） | DOM 快照（snapshot.js）与 inspector 前端（static/app.js） | jev_ultrafast/snapshot.js |
| 外部模型 API | TypeSafe（`jev-latest`） | via HTTPS（api.typesafe.ai/v1/systemone） | 操作/目标选择决策头 | .env.example、model.py |
| 外部模型 API | OpenAI 兼容文本模型（默认 `inception/mercury-2.5` via OpenRouter） | TEXT_MODEL_BASE_URL 可配 | `TYPE_TEXT` 字段值生成 | .env.example、model.py |
| 浏览器驱动 | browser-harness | ==0.1.13 | Chrome CDP 守护进程、连接与执行（cdp-use/websockets 传递依赖） | pyproject.toml、uv.lock |
| HTTP 客户端 | httpx[http2] | >=0.28,<1 | 模型 API 调用（含重试 429/503/529） | pyproject.toml、model.py |
| 构建 | hatchling | — | 打包，console script `jev = jev_ultrafast.demo:main` | pyproject.toml |
| 包管理 | uv + uv.lock | revision 2 | 锁定依赖与 dev 组 | uv.lock |
| Lint | ruff | >=0.14,<1，line-length 120，select E/F/I | 代码检查 | pyproject.toml |
| 测试 | pytest | >=8.4,<9，testpaths=tests，离线 | 契约测试（mock 模型，无付费 API） | pyproject.toml、tests/test_agent.py |
| 图像/视频 | Pillow、ffmpeg | dev 组；ffmpeg 外部二进制 | demo 录制渲染（render_demo.py / render_fixture.py） | pyproject.toml、scripts/ |
| 前端 | 原生 HTML/CSS/JS（无框架） | — | 本地回环 inspector（127.0.0.1:8766） | jev_ultrafast/static/、demo.py |
| 部署形态 | 本地 CLI / 库 / 本地 Web 服务 | 仅回环绑定 | 无服务端部署，凭证留在本地 .env | demo.py、.env.example |

**安全相关约束**：文本模型响应必须解析为仅含 `text` 键的小 JSON 对象才可输入；模型输出永不变成选择器、坐标、shell 命令或可执行 JS（model.py field_text、README.md）。
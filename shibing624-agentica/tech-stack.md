# 技术栈

结论先行：纯 Python 核心（3.10+，async-first）+ Vite/React SPA 前端 + Electron 桌面壳 + Node 18+ TS SDK，交付形态为 PyPI 包（`agentica` / `agentica[gateway]`）、npm 包（`@agentica-ai/sdk`）与三平台桌面安装包。

## 语言与运行时

| 选型 | 用途 |
|---|---|
| Python 3.10+ | 核心框架、CLI、Gateway（`requirements.txt` badge，README.md） |
| TypeScript / Node 18+ | Web SPA（web/）、TS SDK（sdk-ts/）、桌面壳（desktop/） |

## 核心框架与库（依据模块目录与文档，未核实逐项依赖版本）

| 选型 | 用途 |
|---|---|
| asyncio | Async-First 核心：并行工具执行、Agentic Loop（examples/README.md） |
| Pydantic | 结构化输出（`response_model`） |
| OpenAI / Anthropic / ZhipuAI / Moonshot / Ollama / LiteLLM 等模型客户端 | 多厂商模型接入（agentica/model/） |
| LanceDB / text2vec | RAG 向量检索（examples/README.md LLM OS 依赖） |
| LangChain / LlamaIndex | 知识库集成（agentica/knowledge/） |
| Langfuse | 可观测性集成（examples/observability/） |
| MySQL / PostgreSQL / SQLite / Redis | 会话/记忆持久化后端（agentica/db/） |
| MCP SDK（仅 2.x） | Model Context Protocol（v1.4.16 News） |
| FastAPI 推测（gateway/main.py + uvicorn 部署形态） | Gateway HTTP 服务——具体框架未核实 |

## 构建与发布

| 选型 | 用途 |
|---|---|
| pip / uv | Python 打包；产品推荐 `uv tool install`（README.md） |
| `python -m build` + MANIFEST.in | PyPI wheel；`npm run build` 产物写入 `agentica/gateway/ui/` 随 wheel 分发（web/README.md） |
| Vite + React | Web SPA 构建到 `agentica/gateway/ui/`（web/README.md） |
| Electron + electron-builder | 桌面安装包 dmg/NSIS/AppImage/deb（desktop/README.md） |
| npm publish | `@agentica-ai/sdk` 手动发布（sdk-ts/README.md） |
| Docker / docker compose | 自托管 Gateway（Dockerfile、.env.docker.example） |

## CI / 测试

| 选型 | 用途 |
|---|---|
| GitHub Actions：ubuntu.yml / desktop.yml / docker.yml / docs.yml | 测试、桌面构建矩阵、镜像、文档（.github/workflows/） |
| pytest | 单测（evaluation/code_benchmark/tests/ 等）；主仓测试目录未在快照中，未核实 |
| flake8 | Python lint（.flake8） |

## 部署形态

| 形态 | 说明 |
|---|---|
| CLI（`agentica`） | uv tool 隔离安装的交互终端 |
| Gateway（`agentica-gateway`） | 本机 Web 服务，默认 `127.0.0.1:8881` |
| Desktop App | Electron 壳，attach 或 spawn 本机 gateway，首次启动可托管安装 Python runtime（desktop/README.md） |
| Docker | 自托管，镜像内预编译 UI，运行时无需 Node（README.md） |
```

```
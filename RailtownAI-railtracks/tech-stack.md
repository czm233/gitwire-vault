# 技术栈清单

结论：单语言 Python 项目（monorepo / uv workspace），核心包 `railtracks` 位于 `packages/railtracks`，文档用 MkDocs Material + pdoc 生成并输出 llms.txt 供 AI 助手消费，MIT 协议（LICENSE、pyproject.toml）。

| 类别 | 选型 | 版本约束 | 用途 | 信源 |
|---|---|---|---|---|
| 语言 | Python | >=3.10 | 唯一实现语言 | pyproject.toml |
| 包管理/构建 | uv workspace | — | 根 workspace 管理成员包 `packages/railtracks`，发布到 PyPI（包名 `railtracks`） | pyproject.toml、.github/workflows/release_package.yaml |
| LLM 统一接入 | litellm | >= 1.101.0, <= 1.102.1（e8b0590 起；原 >=1.84.0,<=1.89.0） | 多家模型统一调用，支持 gpt-6 reasoning_effort（#1596）；锁定版新增 boto3、pydantic-settings 依赖并改为平台 wheel | packages/railtracks/pyproject.toml、uv.lock |
| 结构化数据/校验 | pydantic | >= 2.11, <3 | 结构化输出与 schema | packages/railtracks/pyproject.toml |
| 配置 | pydantic-settings | 2.15.0（uv.lock 锁定，随 litellm 升级引入） | litellm 传递依赖 | uv.lock |
| MCP | mcp | >= 1.23.0, <2 | MCP 协议集成 | packages/railtracks/pyproject.toml |
| 并发原语 | anyio | 4.15.1（uv.lock 锁定） | async 基础设施（4.15.1 起移除 sniffio 直接依赖） | uv.lock（e8b0590） |
| 其他核心依赖 | colorama、python-dotenv、httpx、trafilatura 等 | 未核实 | 终端输出、环境变量、HTTP、websearch 抓取 | packages/railtracks/pyproject.toml |
| 文档示例依赖 | railtownai | >=2.1.2（9b894a3 起，原 >=2.0.3） | docs/scripts 评估示例；2.1.2 提供 `HostedEvaluationResponse` / `upload_agent_evaluation`（Conductr 托管评估） | docs/scripts/requirements.txt |
| 托管评估端点 | FastAPI + pydantic（docs/scripts 示例） | 未核实 | Hosted Evaluations 示例：`/evals/run` 端点接收 `agent_run_id` 并回传评估结果 | docs/scripts/evaluations/conductr_hosted.py（689c3fe） |
| LLM 文档输出 | mkdocs-llmstxt | >=0.5.0（5b385d2 起，docs extra） | 构建 `llms.txt` 分节索引、逐页 Markdown 副本与 `llms-full.txt` 全量单文件（传递依赖 markdownify / mdformat / mdformat-tables，markdown-it-py 因此锁回 3.0.0） | mkdocs.yml、pyproject.toml、uv.lock（5b385d2） |
| Lint/格式化 | ruff | >=0.11.13，target py310，line-length 88 | 代码风格与静态检查 | pyproject.toml |
| 类型检查 | mypy | >=1.19.1 | 类型校验 | pyproject.toml |
| 测试 | pytest + pytest-asyncio + pytest-cov + pytest-timeout | pytest>=9.0.3 | asyncio_mode=auto；CI 忽略 `tests/llm_live_tests` 与 `tests/end_to_end/retrieval` | pyproject.toml、.github/workflows/pr_tests.yaml |
| 文档站点 | mkdocs-material >=9.7.0 + mermaid2 + pymdown-extensions >=11.0.2,<12 | — | 文档站（docs.railtracks.org，CNAME） | pyproject.toml、mkdocs.yml、docs/CNAME |
| API 文档 | pdoc >=15.0.4 | — | 由 `scripts/mkdocs_hooks.py` 在构建前自动生成 API reference（google 风格，含未文档化成员） | scripts/mkdocs_hooks.py |
| CI/CD | GitHub Actions | — | pr_tests、push_main、release_package、release_docs、e2e 五条流水线 | .github/workflows/*.yaml |
| 可视化/观测 | 自带 `railtracks viz` 本地可视化器；可选 railtownai、Loggly、Sentry handler；context 事件流（`RAILTRACKS_CONTEXT_EVENTS`，9b894a3 起） | — | 运行记录回放、日志外发与 context 调用记录（级别可调） | README.md、docs/scripts/_logging.py、docs/documentation/advanced/context.md |
| AI 辅助开发 | Claude Code skills（.claude/skills/code-style）、AGENTS.md、CLAUDE.md；5b385d2 起文档站输出 llms.txt 系列文件供助手直接引用 | — | 贡献者代码风格约束注入；9b894a3 起 AGENTS.md 要求 agent 显式确认规则列表在上下文中；ai_setup.md 引导助手读取 docs.railtracks.org/llms.txt | .claude/、AGENTS.md（73b83aa）、docs/documentation/getting_started/ai_setup.md（5b385d2） |

补充：文档示例脚本集中在 `docs/scripts/`，通过 pymdownx.snippets 的 `--8<--` 标记嵌入 md，CI 有类型校验（docs/scripts/verifiers.py 头注，提及 scripts/docs_validation.sh，该脚本本身未在快照中，未核实）。
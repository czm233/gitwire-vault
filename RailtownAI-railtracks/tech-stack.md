# 技术栈清单

结论：单语言 Python 项目（monorepo / uv workspace），核心包 `railtracks` 位于 `packages/railtracks`，文档用 MkDocs Material + pdoc 生成，MIT 协议（LICENSE、pyproject.toml）。

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
| Lint/格式化 | ruff | >=0.11.13，target py310，line-length 88 | 代码风格与静态检查 | pyproject.toml |
| 类型检查 | mypy | >=1.19.1 | 类型校验 | pyproject.toml |
| 测试 | pytest + pytest-asyncio + pytest-cov + pytest-timeout | pytest>=9.0.3 | asyncio_mode=auto；CI 忽略 `tests/llm_live_tests` 与 `tests/end_to_end/retrieval` | pyproject.toml、.github/workflows/pr_tests.yaml |
| 文档站点 | mkdocs-material >=9.7.0 + mermaid2 + pymdown-extensions >=11.0.2,<12 | — | 文档站（docs.railtracks.org，CNAME） | pyproject.toml、mkdocs.yml、docs/CNAME |
| API 文档 | pdoc >=15.0.4 | — | 由 `scripts/mkdocs_hooks.py` 在构建前自动生成 API reference（google 风格，含未文档化成员） | scripts/mkdocs_hooks.py |
| CI/CD | GitHub Actions | — | pr_tests、push_main、release_package、release_docs、e2e 五条流水线 | .github/workflows/*.yaml |
| 可视化/观测 | 自带 `railtracks viz` 本地可视化器；可选 railtownai、Loggly、Sentry handler | — | 运行记录回放与日志外发（均可选） | README.md、docs/scripts/_logging.py |
| AI 辅助开发 | Claude Code skills（.claude/skills/code-style）、AGENTS.md、CLAUDE.md | — | 贡献者代码风格约束注入 | .claude/、AGENTS.md |

补充：文档示例脚本集中在 `docs/scripts/`，通过 pymdownx.snippets 的 `--8<--` 标记嵌入 md，CI 有类型校验（docs/scripts/verifiers.py 头注，提及 scripts/docs_validation.sh，该脚本本身未在快照中，未核实）。
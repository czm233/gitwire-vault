# Tripwires — 持续盯防

1. **`MaxCalls` 槽位陷阱**：必须放 `model_middleware=`，放 node `middleware=` 槽不生效（issue #1560，examples/harness/README.md）。上游若修复/改变该语义需跟踪。注意：`MaxCalls` 现已被类型系统标为槽位无关 `Middleware[Any, Any, Never]`（2f1626e），但静态类型不约束运行时槽位语义，该陷阱仍在。
2. **`run_shell` 非沙箱**：官方 coding harness 明确 shell 工具只是 cwd 约束 + 人工审批，allowlist 内解释器可触达全盘（examples/harness/README.md）。任何「沙箱化」相关 PR 是高价值信号。
3. **1.5.x 弃用窗口 + `Middleware` 泛型签名持续变动**：pytest filterwarnings 把 1.5.0 弃用告警当错误（pyproject.toml），API 仍在收敛。`Middleware` 第三参数（constraint）引入后已产生一次真实下游破裂：Python 3.11+ 下 `BaseGuardrail` 隐式继承导入失败，75c3b17 以显式 `Middleware[_P, _R, _MiddlewareSignature[_P, _R]]` 修复（guardrails/core/interfaces.py，#1591）。任何自定义 `Middleware` 子类都应显式传 constraint 参数；后续再变签名仍可能破坏生态代码，盯 docs/documentation/upgrading/。
4. **PDF URL 抓取的信任边界**：`trust_urls=True` 才会进程内抓取 URL PDF，文档强调仅限开发者可控 URL（docs/scripts/multimodal.py）——SSRF 相关设计变更需关注。
5. **文档基础设施脆弱点**：pdoc 生成物位于被 watch 的 `docs/` 下，mtime 判断不慎会引发 `mkdocs serve` 无限重载（scripts/mkdocs_hooks.py 注释）。AGENTS.md 现明确验证口径为 `mkdocs build --strict`、`mkdocs serve` 只是阻塞式预览（4726cf2）。`scripts/docs_validation.sh` 已确认存在，且 2f1626e 起以 `--warn-unused-ignores` 校验 docs/scripts——docs 示例里的 `# type: ignore` 若失效会直接挂 CI。
6. **模型版本命名超前**：示例中大量使用 `gpt-5.4-mini`、`claude-sonnet-5`、`gemini-3.7-flash` 等模型名，疑为文档未来化写法或笔误，实际可用性未核实；若为文档错误可向下游反馈。
7. **技能文档是产品面的一部分**：bundled skills（agent-builder/middleware/rag-pipeline）随包分发并有注册表单测锁定 description（tests/unit_tests/cli/skillkit/test_registry.py）——改技能文案必须同步测试；rag-pipeline 已确认 embedder 混用会抛 `EmbeddingModelMismatchError`，检索相关集成需注意建库与查询模型一致性。
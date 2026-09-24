# Tripwires — 持续盯防

1. **`MaxCalls` 槽位陷阱**：必须放 `model_middleware=`，放 node `middleware=` 槽不生效（issue #1560，examples/harness/README.md）。上游若修复/改变该语义需跟踪。注意：`MaxCalls` 现已被类型系统标为槽位无关 `Middleware[Any, Any, Never]`（2f1626e），但静态类型不约束运行时槽位语义，该陷阱仍在。
2. **`run_shell` 非沙箱**：官方 coding harness 明确 shell 工具只是 cwd 约束 + 人工审批，allowlist 内解释器可触达全盘（examples/harness/README.md）。任何「沙箱化」相关 PR 是高价值信号。
3. **1.5.x 弃用窗口**：pytest filterwarnings 把 1.5.0 弃用告警当错误（pyproject.toml），提示 API 仍在收敛；后续 1.6/2.0 可能再有破坏性变更，盯 docs/documentation/upgrading/。`Middleware` 泛型刚加第三参数（2f1626e），进一步签名变动可能性高。
4. **PDF URL 抓取的信任边界**：`trust_urls=True` 才会进程内抓取 URL PDF，文档强调仅限开发者可控 URL（docs/scripts/multimodal.py）——SSRF 相关设计变更需关注。
5. **文档基础设施脆弱点**：pdoc 生成物位于被 watch 的 `docs/` 下，mtime 判断不慎会引发 `mkdocs serve` 无限重载（scripts/mkdocs_hooks.py 注释）。`scripts/docs_validation.sh` 已确认存在，且 2f1626e 起以 `--warn-unused-ignores` 校验 docs/scripts——docs 示例里的 `# type: ignore` 若失效会直接挂 CI，写文档示例需同步跑 mypy。
6. **模型版本命名超前**：示例中大量使用 `gpt-5.4-mini`、`claude-sonnet-5`、`gemini-3.7-flash` 等模型名，疑为文档未来化写法或笔误，实际可用性未核实；若为文档错误可向下游反馈。
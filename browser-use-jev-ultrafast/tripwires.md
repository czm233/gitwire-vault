# Tripwires · 持续盯防

1. **性能声称的可复现性**：README 承认「三 repeats × 单任务 × 单浏览器 profile，非通用可靠性基准」（README.md）。盯防：后续是否出现更大规模基准，以及 docs/performance.md 数据是否与代码同步更新。当前判定：标注为营销级证据，不作可靠性结论。

2. **独立验证依赖 URL 编码解析**：flights.py verify 解码 `tfs` 参数 base64 检查日期，Google Flights URL 结构变更会静默使验证失效。盯防 examples/flights.py 的 `verify` 是否跟进。

3. **模型响应校验的健壮性**：validate_choice 已覆盖 NaN/负值/非最大项/概率和偏差（容差 0.02），但未校验 usage/latency 字段。盯防 TypeSafe API 契约变更（api.typesafe.ai/v1/systemone 端点硬编码于 model.py）。

4. **DOM 快照 MVP 边界**：明确不支持 Shadow roots、frames、canvas、上传、弹出标签页、嵌套滚动、键盘组件（README.md）。盯防这些限制是否在文档与 snapshot.js 实现间保持一致。

5. **demo 服务安全面**：仅绑定 127.0.0.1 且有 Token/Host/Origin 校验，但 goal 会被注入模型上下文；页面文本已声明为不可信数据（questions.py）。盯防 prompt-injection 相关指令是否随 questions.py 演进被削弱。

6. **依赖锁定**：browser-harness 钉死 0.1.13（pyproject.toml），上游 API 变更需显式升级。盯防 uv.lock 与 pyproject 约束漂移。

7. **凭证处理**：.env 中 TYPESAFE_API_KEY / TEXT_MODEL_API_KEY，仓库承诺凭证与原始 trace 不入库（.gitignore、README.md）。盯防 artifacts/ 泄漏风险。
# Tripwires — 持续盯防

1. **未签名桌面安装包的信任链**：三平台安装包均未签名，依赖用户手动解除 macOS quarantine / Windows SmartScreen 拦截（desktop/README.md、README.md）。盯防：后续是否引入签名与公证；伪装成 Agentica 安装包的供应链风险。
2. **MCP 2.x-only 的兼容断点**：v1.4.16 起不再支持 mcp 1.x，生态用户升级可能报障。盯防：issue 区 MCP 相关报错密度。
3. **CWE-22 类路径安全问题复发**：v1.4.7 修过 `/api/upload` 路径穿越；Gateway 暴露文件/工作区路由（routes/workspace.py、channels.py）。盯防：新增 REST 路由的输入校验。
4. **压缩正确性承诺**："零 LLM 摘要、原文可回取" 是核心卖点，依赖超大输出落盘 + JSONL 完整性。盯防：session_log.py（约 87KB 核心文件）的写入路径变更、淘汰占位符与实际文件的对应关系是否被破坏。
5. **桌面托管 runtime 的版本漂移**：首次启动安装 Python 3.12 + `agentica[gateway]` 到 Application Support，与用户自装版本可能不一致。盯防：runtime 解析顺序（登录 shell → managed venv）变化引发的"改了没生效"类 issue。
6. **评测口径的可比性**：benchmark 数字来自本机 harness（无 Docker），README 明确"可对表但不贴官方 leaderboard"。盯防：对外传播中被断章取义的风险，以及 `results/` 产物与文档数字的一致性。

> 以上判定均基于快照内文档；代码级验证未做，标为未核实。
```

```
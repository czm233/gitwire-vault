# Gitwire Vault

> 由 [Gitwire](https://github.com/czm233/Gitwire) 自动维护的开源项目情报档案。

本仓库是 [Gitwire](https://github.com/czm233/Gitwire)（个人开源情报站）的情报产出仓库：每个被监控的 GitHub 项目对应一个 `owner-repo/` 文件夹，内含技术栈清单、架构图、业务逻辑时序图、逐版本 changelog 与哨兵（tripwire）记录。同步游标存放在各项目的 `meta.yml` 中——档案反映到的提交，就是游标指向的提交。

## 目录约定

```
owner-repo/
├── README.md         # 档案索引
├── tech-stack.md     # 技术栈清单
├── architecture.md   # mermaid 架构图
├── business-logic.md # 核心业务时序图/流程图（非核心用文字）
├── changelog/        # 每次同步一份变更简报
├── tripwires.md      # 哨兵问题与历次判定（可选）
└── meta.yml          # 同步游标（repo / last_synced）
```

---

# Gitwire 态势板

- 最近一轮同步：2026-09-24；2 个监控目标，2 个已发布

| 项目 | 本轮 | 状态 | 同步到 | 摘要 |
| --- | --- | --- | --- | --- |
| [czm233/CC-Balancer](./czm233-CC-Balancer) | 建档 | 已发布 | dfecb1f | CC-Balancer 是纯前端「额度规划实验室」单页应用（TypeScript + React 19 + Vite 7 + Tailwind v4，仅 845 行核心源码，零后端零数据库，GitHub Pages 托管），模拟 Coding Plan 的 5 小时额度窗口并生成交给外部 AI 配置定时调用的 Prompt。产出技术栈、三层架构图、5 条核心业务流程图与首版 changelog；实测 tests / lint / build 全部通过，并核实 HEAD 重构遗留——time.ts 多个导出与 ClockFace 拖拽创建忙时代码已无调用方/未接线。 |
| [farion1231/cc-switch](./farion1231-cc-switch) | 建档 | 已发布 | f878871 | cc-switch（v3.20.4）是 Tauri 2 桌面应用，统一管理 10 个 AI CLI/桌面应用（Claude Code、Codex、Gemini CLI 等）的供应商/MCP/skills/prompts 配置，核心是 SQLite 配置库物化写入各 CLI live 文件，并内建带熔断与故障转移的本地 axum 代理。档案厘清「切换=写 live 文件」与「接管=live 指向本地代理」两条主线，产出 5 条核心流程图；哨兵确认 Profile 为手动命名快照、不按目录自动应用。 |

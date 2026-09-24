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

<!-- board:start -->

- 最近一轮同步：2026-09-24；1 个监控目标，3 个已发布

| 项目 | 本轮 | 状态 | 同步到 | 摘要 |
| --- | --- | --- | --- | --- |
| [czm233-CC-Balancer](./czm233-CC-Balancer) | — | 已发布 | dfecb1f | — |
| [farion1231-cc-switch](./farion1231-cc-switch) | — | 已发布 | f878871 | — |
| [shibing624-agentica](./shibing624-agentica) | 建档 | 已发布 | e84fc48 | Agentica 为 async-first Python Agent 框架+CLI/Web/桌面三入口产品，主打零 LLM 摘要压缩与自进化 Skill；本次 init 建档至 e84fc48，近期主线是 MCP 2.x 收敛与空窗换窗压缩。 |

<!-- board:end -->

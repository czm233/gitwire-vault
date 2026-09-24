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

- 最近一轮同步：2026-09-25；4 个监控目标，7 个已发布

| 项目 | 本轮 | 状态 | 同步到 | 摘要 |
| --- | --- | --- | --- | --- |
| [RailtownAI-railtracks](./RailtownAI-railtracks) | 增量 | 已发布 | 2f1626e | 追踪 2 个 issue |
| [browser-use-jev-ultrafast](./browser-use-jev-ultrafast) | 建档 | 已发布 | 1231850 | jev-ultrafast v0.1.0 首次建档：TypeSafe 驱动的选择式浏览器 Agent，单请求决策+原子DOM快照+严格执行守卫，性能声称 25% 提速但样本小，已列三项盯防点。<<<END>>> |
| [czm233-CC-Balancer](./czm233-CC-Balancer) | — | 已发布 | dfecb1f | — |
| [czm233-VoiceTutor](./czm233-VoiceTutor) | 建档 | 已发布 | 263d783 | macOS 本地语音英语老师，M0 全链路跑通：LiveKit Agent + 本地 STT/TTS + 自研 LLM 网关（单请求串行防 429），音频不出机器，仅 LLM 文本出网。<<<END>>> |
| [farion1231-cc-switch](./farion1231-cc-switch) | — | 已发布 | f878871 | — |
| [rakkasei0810-notion-plugin-notion-openai-curated-remote](./rakkasei0810-notion-plugin-notion-openai-curated-remote) | 建档 | 已发布 | 0b2db47 | GetToIt 是画师/单主委托排单计时双端工作台：Next.js+Koa+Prisma/MySQL，含 RBAC 后台、邮箱/QQ 登录、COS 图片直传；本次为首次全量建档，需盯防仓库名错位与 prod MySQL 端口暴露。 |
| [shibing624-agentica](./shibing624-agentica) | 增量 | 已发布 | e84fc48 | 追踪 1 个 issue |

<!-- board:end -->

# DanceBattler 情报档案

**一句话定位**：DanceBattler 是一个围绕“圣华娱乐”跳舞机外部系统（ShApi）构建的全栈第三方辅助平台，提供玩家数据查询、排行榜、机台地图/排队、成绩统计、头像框/称号资源管理等功能，基于 Node.js + Koa + Prisma + PostgreSQL + Redis + React。（信源：README.md、backend/utils/ShapiClient.js、CLAUDE.md）

## 档案索引

| 文件 | 内容 |
|------|------|
| [tech-stack.md](tech-stack.md) | 技术栈清单 |
| [architecture.md](architecture.md) | 架构与组件职责 |
| [business-logic.md](business-logic.md) | 核心业务流程 |
| [tripwires.md](tripwires.md) | 持续追踪的风险点 |
| [changelog/2026-09-27-019bec5.md](changelog/2026-09-27-019bec5.md) | 本次同步变更 |

## 最近同步

- 本次同步：`∅（无游标）` → `019bec5`（2026-09-27），**init 首次全量建档**
- 当前版本：package.json `1.0.29`（信源：package.json）
- 许可证：MIT

## 关键背景（摘要）

- 项目本质是对外部“圣华”（shenghuayule.com）系统的非官方数据聚合层：歌曲/玩家/舞团搜索、排行榜、成绩、机台分布均通过 `ShapiClient` 代理调用外部 API（信源：backend/utils/ShapiClient.js、backend/routes/songs.js、search.js、players.js）。
- 自身维护 PostgreSQL（Prisma）存储用户、机台点（machine_places）、资源（avatar_frames/titles）、扫描结果等；Redis 做缓存（信源：prisma/schema.prisma、backend/config/redis.js）。
- 包含资源 ID 扫描工具链（scripts/scanResourceIds.js 等），通过“装备前后对比”探测圣华头像框/称号 ID 有效性（信源：scripts/README.md）。
- 含大量 AI 协作痕迹（CLAUDE.md 41KB、.agent/、.claude/），开发流程高度依赖 Claude Code。
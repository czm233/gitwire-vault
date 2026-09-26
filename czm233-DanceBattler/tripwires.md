# Tripwires — 持续追踪

## 1. 合规/法律风险：对圣华系统的非官方爬取与代理

**判定**：项目核心数据全部来自第三方“圣华”系统的逆向接口（ShapiClient 23KB、test/http/抓包.md、资源 ID 全范围扫描 0-3000 万）。装备验证扫描本质是自动化枚举攻击面。持续追踪：圣华侧是否封禁/改版接口（token 2-3 天过期已是信号）。信源：backend/utils/ShapiClient.js、scripts/README.md。

## 2. 敏感凭据泄露风险

**判定**：scripts/README.md 中明文粘贴了一个 JWT token（`eyJhbGci...`，payload 含 userId `504_8602`）与用户 ID 8602。虽是自建系统 token，但已构成凭据入库。追踪：后续 commit 是否清理；登录圣华的 dancebattle_token 明文存于 users 表。信源：scripts/README.md、prisma/schema.prisma。

## 3. 真实地理位置数据

**判定**：machine_places 存储全国 1000+ 跳舞机机台的真实地址与 GCJ02 坐标（含商户名如 PartyKing 等），且公开 API `/api/places` 无认证（backend/routes/places.js 无 auth 中间件）。追踪：是否被要求下架数据。信源：backend/routes/places.js、scripts/README.md（1086 台）。

## 4. 架构不一致与坏味道

**判定**：
- README 描述（MySQL 提及、AuthController 等）与实际代码（PostgreSQL、无 AuthController）不符，README 疑为模板残留。
- search/songs/players 路由内联业务逻辑、大量 `console.log` 调试输出（含敏感返回数据）绕过 logger。
- 双锁文件（package-lock.json + pnpm-lock.yaml）并存，依赖解析不确定。
- scores 路由的 best/`/:scoreId` 为 TODO 空实现。
信源：README.md、backend/routes/search.js、backend/routes/scores.js。

## 5. 资源 ID 扫描的可维护性

**判定**：默认扫描范围 0-3000 万、200ms 延迟，完整扫完理论上需约 70 天/类型；token 2-3 天过期意味着需人工反复续期。追踪 ResourceSyncService 与扫描脚本是否收敛为增量同步。信源：scripts/README.md。

## 6. 部署内存脆弱性

**判定**：React 构建 OOM 历史问题靠 swap + 部署锁 + PM2 限制三重兜底而非根治（如升级构建机或改 Vite）。PM2 配置需 `delete + start` 才生效的坑已记录（错误教训 11，CLAUDE.md）。信源：scripts/README.md、CLAUDE.md。
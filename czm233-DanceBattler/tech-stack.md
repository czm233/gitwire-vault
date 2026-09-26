# 技术栈

> 信源：package.json、frontend/package.json、docker-compose.yml、ecosystem.api.config.js、deploy.sh

## 后端

| 选型 | 版本 | 用途 |
|------|------|------|
| Node.js | >=16 | 运行时 |
| Koa.js | 2.14 | Web 框架（分层：routes → controllers → services → prisma） |
| Prisma | 5.6 | ORM，PostgreSQL 访问与迁移 |
| PostgreSQL | 15（docker-compose） | 主数据库，端口映射 5433 |
| Redis (ioredis) | 5.3 | 缓存（用户信息缓存 24h 等），7-alpine 容器，端口 6380 |
| jsonwebtoken | 9 | JWT 认证，httpOnly Cookie + Bearer 双通道 |
| bcryptjs | 2.4 | 密码加密 |
| koa-helmet / helmet | 7/8 | 安全头 + CSP（白名单含 *.aliyuncs.com、*.shenghuayule.com） |
| koa-multer | 1.0 | 文件上传（按日期分目录，MD5 命名） |
| winston | 3.11 | 日志（error/combined/exceptions/rejections，5MB 轮转） |
| node-cron | 4.2 | 定时任务（机台同步、每日重置） |
| joi | 17 | 参数校验 |
| nodemailer | 7 | 邮件（未核实具体使用点） |

## 前端

| 选型 | 用途 |
|------|------|
| React 18 + CRA（react-scripts） | 前端框架与构建 |
| antd-mobile 5 | 移动端 UI |
| Zustand | 状态管理（useAuthStore / useAppStore） |
| React Router 6 | 路由（含 ProtectedRoute） |
| 高德地图 JS API（useAmapLoader） | 机台地图展示 |
| SCSS / CSS | 样式 |

## 工具与部署

| 选型 | 用途 |
|------|------|
| PM2（ecosystem.api/webhook.config.js） | 进程管理，API 限 500MB、webhook 限 200MB 内存 |
| Docker Compose | 本地 PG + Redis |
| scripts/webhook-server.js | GitHub push webhook 触发自动部署，带内存部署锁（5min 超时） |
| scripts/add-swap.sh | 4GB swap 配置，防并发构建 OOM |
| Jest + Supertest | 测试（仓库内仅见 scripts/utils/test.js，测试覆盖情况未核实） |
| Puppeteer（dev 依赖） | 疑似用于抓包/验证码（scripts/ocr/），未核实 |
| pnpm 10.15（packageManager 字段） | 包管理（同时存在 npm package-lock 与 pnpm-lock，双锁文件并存） |

## 外部依赖

| 服务 | 用途 |
|------|------|
| 圣华 ShApi（shenghuayule.com / message.shenghuayule.com） | 歌曲、玩家、舞团、成绩、资源、机台数据源头 |
| 高德地图 REST API（AMAP_API_KEY） | 机台地址地理编码（GCJ02） |
| 阿里云 OSS（*.aliyuncs.com，CSP 白名单推断） | 资源图片，未核实 |
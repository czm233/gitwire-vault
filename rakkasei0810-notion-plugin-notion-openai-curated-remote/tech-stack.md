# 技术栈清单

| 类别 | 选型 | 版本/约束 | 用途（信源 package.json / README.md） |
|---|---|---|---|
| 语言 | TypeScript | strict, ES2017 target | 全栈类型安全 |
| 前端框架 | Next.js（App Router） | 16.2.6 | Web/App 与管理后台页面 |
| UI 库 | React / react-dom | 19.2.6 | 界面渲染 |
| 图标 | @phosphor-icons/react | ^2.1.10 | 工作台图标 |
| 二维码 | qrcode.react | ^4.2.0 | 推测用于邀请/分享链接二维码（未核实具体使用点） |
| 后端框架 | Koa + @koa/router | koa ^3.0.0 | 独立 Node API（backend/src/app.ts，约 154KB 单文件） |
| 校验 | Zod | ^4.1.5 | API 请求 schema 校验 |
| ORM | Prisma + @prisma/client | ^6.15.0 | MySQL 数据访问 |
| 数据库 | MySQL 8 | Docker Compose（本机 10152 端口） | 项目独立数据资源 |
| 对象存储 | 腾讯云 COS（cos-nodejs-sdk-v5 ^3.0.0） | 浏览器直传 + 短时签名 URL | 原图/预览图存储（backend/src/lib/cos.ts） |
| 邮件 | nodemailer ^9.0.5 / Resend HTTP API | EMAIL_PROVIDER=console/smtp/resend | 邮箱验证码发送（backend/src/lib/email.ts） |
| 密码哈希 | node:crypto scrypt（自实现 scrypt-v1） | N=16384, r=8 | backend/src/lib/password.ts |
| 进程管理 | concurrently ^9.2.1 / tsx ^4.20.3 | — | 本地并行启动 Web+API、TS 直接运行 |
| 测试 | node:test（契约/集成测试 .mjs/.ts） | npm test / test:smoke | 渲染测试、身份访问、核心业务、QQ OAuth、图片资产等（tests/） |
| Lint | ESLint 9.39.4（eslint-config-next） | — | 代码规范 |
| 部署 | Docker Compose（local/prod）、deploy.sh、.github/workflows/deploy.yml | MySQL 8.0 容器 | 本机与生产 MySQL 提供；CI 独立 MySQL Service |
| 运行时 | Node.js | >=22.13.0 | engines 约束 |

**端口约定**：Web 10150 / API 10151 / MySQL 10152（README.md、.env.example）。Next.js 将 `/api/*` 同源转发到 Koa API。
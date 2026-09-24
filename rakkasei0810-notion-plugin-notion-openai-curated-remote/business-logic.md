# 核心业务流程

## 1. 邮箱验证码登录/注册

验证码 6 位、10 分钟有效、一次性；首次验证成功即建号并引导选身份（README.md、backend/src/app.ts）。

```mermaid
sequenceDiagram
  participant U as 用户
  participant W as Next Web
  participant A as Koa API
  participant E as 邮件服务
  U->>W: 输入邮箱
  W->>A: 申请验证码
  A->>A: 生成6位码, 存hash, 10分钟TTL
  A->>E: sendVerificationEmail
  E-->>U: 验证码（dev 模式打印到 API 日志）
  U->>W: 提交验证码
  W->>A: 验证
  A->>A: 首次则创建账号, 签发 APP 会话
  A-->>W: 重定向 /select-identity 或 next
```

## 2. 双身份模型与身份选择

角色 ARTIST/CLIENT（scope=APP）决定可用模式；画师注册默认立即获得永久授权；单身份账号可自助追加缺失身份；双身份登录时选择模式，两模式独立路由（README.md、backend/src/constants.ts）。

```mermaid
flowchart TD
  L[登录成功] --> C{角色包含哪些模式}
  C -->|仅 artist| A1[进入 /app/artist]
  C -->|仅 client| A2[进入 /app/client]
  C -->|双身份| S[/select-identity 选择模式/]
  S --> A1
  S --> A2
  P[已登录单身份] --> Add[POST /api/app/account/identities 追加身份] --> C
```

## 3. 委托邀请绑定（Form Invite）

画师从单笔委托生成专属邀请链接；单主登录/注册后确认领取，之后单主视图只显示明确绑定到当前账号的委托，不做姓名或普通分享链接自动匹配（README.md）。表单邀请支持自定义字段（含 file 类型图片直传 COS）与幂等领取（迁移 form_invite_idempotency）。

```mermaid
sequenceDiagram
  participant AR as 画师
  participant A as API
  participant CL as 单主
  AR->>A: 为委托创建表单邀请（token 32-64位）
  A-->>AR: /form-invite/:token 链接
  CL->>A: 打开链接, 填表提交（含图片资产）
  CL->>A: 确认领取（幂等）
  A->>A: 委托绑定 clientUserId
  CL->>A: 之后每次进入 /app/client 看到该委托进度
```

## 4. 管理后台 RBAC

管理员经 `/admin/login`（独立 ADMIN 会话 cookie），权限按 `permission.code` 树校验；接口目录（ENDPOINTS）定义 method+pathPattern+protectionType（RBAC/SESSION_ONLY），seed 幂等同步；后台支持用户管理、启停、角色分配（含画师有效期）、自定义角色与权限树、审计日志（backend/src/constants.ts、seed.ts、schema.prisma）。

```mermaid
flowchart TD
  Req[请求 /api/admin/*] --> S{readSession ADMIN 有效?}
  S -->|否| R401[401]
  S -->|是| P{命中 ENDPOINTS 且角色具备所需 permission?}
  P -->|否| R403[403]
  P -->|是| H[处理并写 AuditLog]
```

## 5. 画师工作台排单与计时

委托状态含 pending_review（待审核）等，支持拖拽排序（position，PATCH 不继承 create 默认值避免字段被重置）、审核、稿酬（Decimal 12,2）、绘制计时（WorkSession + ActiveWorkTimer）、公开进度（publicProgress + 公开排单 token 分享）（features/workbench/、app.ts schema、queue-preview 页面）。

## 6. QQ 登录

首次 QQ 登录走 onboarding 选择一个工作台身份，不自动开通双身份；state 经 `gettoit_qq_oauth_state` httpOnly cookie 防护；openid/unionid 落 QqAccount 表（backend/src/lib/qq-oauth.ts、schema.prisma、README.md）。

**非核心流程（文字带过）**：密码修改需 PASSWORD_CHANGE 挑战 cookie；图片资产未绑定残留由 cleanup:images 每 30 分钟清理；公开排单预览页 `queue-preview` 仅供预览不索引。
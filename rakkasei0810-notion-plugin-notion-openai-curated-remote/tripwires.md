# Tripwires 盯防清单

1. **仓库名与内容不符**：仓库路径为 notion-plugin-*，内容实为 gettoit-workbench。后续同步若出现真正的 notion 插件内容需警惕快照错位。判定：命名不一致（未核实原因）。
2. **backend/src/app.ts 单文件 154KB**：全部业务路由集中一处，合并冲突与审查成本高，需关注是否拆分。
3. **docker-compose.prod.yml 映射了 MySQL 宿主端口**（`"${MYSQL_HOST_PORT:-10152}:3306"`），与 README「生产 MySQL 不暴露公网端口」的声明存在张力，依赖部署环境防火墙兜底（信源：docker-compose.prod.yml）。
4. **限流为进程内存实现**（rateBuckets Map，上限 1 万桶）：多实例/重启即失效，横向扩容后滥用防护退化（backend/src/app.ts）。
5. **默认 QQ_APP_ID/REDIRECT_URI 硬编码于代码与 .env.example**（含生产域名 rakkasei.createcat.cn）：属作者自有配置入库，换环境必须覆盖（app.ts、.env.example）。
6. **.runtime/ 日志文件被纳入快照**（api.log/web.log）：虽小，但运行时产物入库可能是 .gitignore 疏漏，需确认。
7. **app.proxy = true**：Koa 信任所有转发头，实际客户端 IP 判定靠 TRUSTED_PROXY_IPS 白名单兜底；若部署方漏配，限流键可被伪造（app.ts clientIp 注释）。
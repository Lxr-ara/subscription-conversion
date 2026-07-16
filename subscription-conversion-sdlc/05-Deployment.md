# SubLink — 部署与运维指南

## 容器化

主部署不需要容器。仓库保留上游 Dockerfile 供自托管静态站使用；GitHub Pages 直接部署 `dist/` 更简单。

## 基础设施即代码

无独立云资源。`.github/workflows/deploy.yml` 即部署定义，GitHub Pages 设置由仓库 API/控制台管理。

## CI/CD

1. 推送 `main`。
2. `actions/checkout@v4` 拉取代码。
3. Node.js 20 + Yarn 缓存。
4. `yarn install --frozen-lockfile`。
5. `yarn build`。
6. `actions/upload-pages-artifact@v3` 上传 `dist/`。
7. `actions/deploy-pages@v4` 发布并输出 URL。

PR 阶段建议补充 build/test 工作流；合并后发布；后端健康探测可设为每日计划任务。

## 回滚策略

- 页面故障：在 GitHub 上对问题提交执行 `Revert`，推送 main 触发重新部署。
- 紧急回滚：重新运行最后一个成功提交对应的 workflow，或建立回滚提交。
- 无数据库迁移和服务端状态，因此回滚仅涉及静态产物。
- 发布后以 Pages URL HTTP 200、标题和关键静态资源为退出条件。

## 环境管理

| 环境 | 配置 | 说明 |
| --- | --- | --- |
| 开发 | `.env` + dev server | 本地验证，不使用真实订阅 |
| 预览 | 本地 `dist/` 或 PR 构建产物 | 检查资源基路径 |
| 生产 | main + GitHub Pages | HTTPS 静态站 |

环境变量均为公开构建配置，不应放入秘密信息。若未来出现密钥，使用 GitHub Actions Secrets，并确保它不会以 `VUE_APP_*` 注入前端包。

## 网络与安全

- GitHub Pages 提供 TLS、CDN 和基础 DDoS 防护。
- 浏览器出站仅需 Pages 静态资源、远程规则和用户选择的后端。
- 静态 Pages 无 VPC、入站端口、防火墙或服务端 CORS 配置。
- 推荐后续通过自定义域/Cloudflare 增加 CSP、HSTS、`X-Content-Type-Options` 等响应头控制。

## 可观测性

- 构建/部署：GitHub Actions 日志与 Pages deployment 状态。
- 可用性：Uptime Kuma、Better Stack 或 GitHub 定时工作流监控 Pages URL。
- 错误跟踪：如接入 Sentry，必须关闭用户输入和 URL query 采集。
- 指标/追踪：MVP 无服务端，不部署 Prometheus/OpenTelemetry。

## 数据库运维

无数据库、备份、连接池、只读副本或迁移流程。源码和发布历史由 Git/GitHub 保留。

## 生产检查清单

- [ ] Pages Source 设置为 **GitHub Actions**
- [ ] workflow 具有 `pages: write` 与 `id-token: write`
- [ ] `yarn build` 成功且资源路径包含 `/subscription-conversion/`
- [ ] 线上首页与核心 JS/CSS 返回 HTTP 200
- [ ] 默认后端 `/version` 可达
- [ ] 页面未暴露 Token、真实订阅或测试数据
- [ ] 明暗主题和 375px 移动端布局通过
- [ ] 依赖扫描已启用
- [ ] 回滚流程已记录并演练

## 扩展性

- 静态资源由 Pages CDN 横向扩展，无需应用实例扩容。
- 转换吞吐由第三方/自建 Subconverter 决定；高流量时部署多后端并做健康排序。
- 远程规则可迁移到稳定 CDN；前端包按内容哈希缓存。


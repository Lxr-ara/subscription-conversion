# SubLink — 实施蓝图

## 项目初始化

```bash
yarn install --frozen-lockfile
yarn serve
yarn build
```

## 文件级任务清单

1. `.env`：设置仓库、默认转换/短链/配置后端。
2. `vue.config.js`：把 `publicPath` 固定为 `/subscription-conversion/`。
3. `public/index.html`：设置中文语言、标题与 SEO 元数据。
4. `src/views/Subconverter.vue`：保留转换流程，替换品牌和无关社交入口，展示数据流说明。
5. `.github/workflows/deploy.yml`：使用官方 Pages artifact + deploy 工作流。
6. `README.md`：补充在线地址、本地开发、配置、部署和上游归属。

## 核心模块实现

### URL 构造

- 读取 `sourceSubUrl`，把换行转换为 `|`。
- 对源 URL、规则 URL 和文本参数执行 `encodeURIComponent`。
- 仅在用户启用对应选项时追加高级参数。
- 输出写入 `customSubUrl`，由剪贴板插件复制。

```js
const source = form.sourceSubUrl.replace(/(\n|\r|\n\r)/g, "|");
const result = `${backend}/sub?target=${form.clientType}&url=${encodeURIComponent(source)}`;
```

### 后端探测

- 切换后端时请求 `{backend}/version`。
- 成功展示版本/类型信息；网络或 CORS 失败时展示“后端不可用”。
- 不因探测失败清空用户输入。

### 明暗主题

- `localStorage.localTheme` 优先于系统主题。
- `prefers-color-scheme` 变化时同步主题。
- 手动切换写回 `localStorage`。

## 前端状态管理

单页面、单表单，继续使用组件内 `data()`；不引入 Vuex。只有主题偏好持久化，订阅内容不写入持久存储。

## 认证流程

无账号和业务认证。部署身份由 GitHub Actions OIDC 和仓库权限处理，不在前端暴露 Token。

## 数据库与迁移

无数据库、无 ORM、无迁移。若 V2 增加用户预设，再引入后端与正式迁移工具；MVP 不提前设计。

## 错误处理与日志

- 用户输入错误：Element UI Message 显示可操作提示。
- 第三方服务错误：区分转换后端、短链、配置托管三类错误。
- 基础设施错误：由 GitHub Actions 日志和 Pages 状态暴露。
- 静态站默认不采集订阅内容或请求 URL；若接入 Sentry，必须过滤 query string。

## 代码质量

- 当前门禁：`yarn install --frozen-lockfile`、`yarn build`。
- 建议 V2：ESLint + Prettier、Vitest/Jest、Playwright、Dependabot。
- PR 检查：构建通过；无秘密信息；不记录订阅 URL；移动端布局无回归。

## 第三方集成

- Subconverter：`GET /version` 与 `/sub`。
- 短链：`POST /short`，表单字段 `longUrl`、可选 `shortKey`。
- 配置托管：`POST /sub.php` 或 `/api.php`。
- 不需要 OAuth、支付、邮件、对象存储。

## 环境变量示例

```dotenv
VUE_APP_PROJECT="https://github.com/OWNER/subscription-conversion"
VUE_APP_SUBCONVERTER_DEFAULT_BACKEND="https://BACKEND"
VUE_APP_MYURLS_DEFAULT_BACKEND="https://SHORT_BACKEND"
VUE_APP_CONFIG_UPLOAD_BACKEND="https://CONFIG_BACKEND"
```

## 开发启动

1. 使用 Node.js 20 和 Yarn 1.22。
2. 安装依赖并运行 `yarn serve`。
3. 用不含真实凭据的测试节点验证 URL 构造。
4. 运行 `yarn build`，检查 `dist/index.html` 的资源基路径。

## Agent 执行提示

> 在现有 Vue 2 + Element UI 项目中做最小改动：维护 `Subconverter.vue` 的 URL 构造、后端探测和主题切换；环境差异集中在 `.env` 与 `vue.config.js`；每次修改后运行冻结锁文件安装与生产构建；不要引入账号、数据库或服务端；部署只能使用官方 GitHub Pages Actions，且不得把订阅内容写入日志或持久存储。


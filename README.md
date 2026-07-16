# SubLink · 订阅转换

一个部署在 GitHub Pages 上的纯前端订阅转换工具，支持 Clash、Sing-Box、Surge、Quantumult X、Loon 等常见客户端格式。

- 在线地址：<https://lxr-ara.github.io/subscription-conversion/>
- 源码仓库：<https://github.com/Lxr-ara/subscription-conversion>

## 功能

- 多订阅或单节点链接合并转换
- 多种目标客户端格式
- 可选远程规则配置与高级转换参数
- 自定义 Subconverter 后端
- 长链接解析、短链接生成、亮色/暗色主题
- 响应式页面，适配桌面端与移动端

## 数据流

本站是静态页面，没有在 GitHub Pages 上运行服务器。默认转换后端为部署在个人 Oracle Cloud 主机上的 `https://weekeebu.top/subconverter`，订阅转换不会再经过公共 Subconverter。

“生成短链接”和“自定义配置”仍分别使用 `.env` 中配置的短链与配置托管服务，页面已明确标记为第三方功能；敏感订阅只使用“生成订阅链接”即可。

自建 Subconverter 只监听服务器的 `127.0.0.1:25500`，通过 Caddy 的 `/subconverter/` 路径对外提供转换接口，容器日志已关闭。

## 本地开发

```bash
yarn install --frozen-lockfile
yarn serve
```

构建静态文件：

```bash
yarn build
```

构建结果位于 `dist/`。

## 配置

主要配置位于 `.env`：

| 变量 | 用途 |
| --- | --- |
| `VUE_APP_PROJECT` | GitHub 仓库地址 |
| `VUE_APP_SUBCONVERTER_DEFAULT_BACKEND` | 默认转换后端 |
| `VUE_APP_MYURLS_DEFAULT_BACKEND` | 默认短链接后端 |
| `VUE_APP_CONFIG_UPLOAD_BACKEND` | 自定义配置托管后端 |

GitHub Pages 的仓库子路径配置位于 `vue.config.js` 的 `publicPath`。

## 部署

推送到 `main` 分支后，`.github/workflows/deploy.yml` 会自动构建并发布到 GitHub Pages，无需额外 Token。

## 上游与许可

本项目基于以下 MIT 开源项目继续定制：

- [kukuqi666/Subscription-conversion](https://github.com/kukuqi666/Subscription-conversion)
- [CareyWang/sub-web](https://github.com/CareyWang/sub-web)
- [tindy2013/subconverter](https://github.com/tindy2013/subconverter)

项目遵循 [MIT License](LICENSE)。

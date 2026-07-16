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

本站是静态页面，没有自建业务服务器。订阅链接由浏览器直接发送给页面中选定的 Subconverter 后端；如对第三方后端有要求，可在“后端地址”中填写自建服务。

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

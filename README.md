# SubLink · 订阅转换

一个部署在 GitHub Pages 上的纯前端订阅转换工具，支持 Clash、Sing-Box、Surge、Quantumult X、Loon 等常见客户端格式。

- 在线地址：<https://lxr-ara.github.io/subscription-conversion/>
- 源码仓库：<https://github.com/Lxr-ara/subscription-conversion>

## 功能

- 多订阅或单节点链接合并转换
- 多种目标客户端格式
- 可选远程规则配置与高级转换参数
- 自定义 Subconverter 后端
- 长链接解析、私有订阅短链生成、亮色/暗色主题
- 响应式页面，适配桌面端与移动端

## 数据流

本站是静态页面，没有在 GitHub Pages 上运行服务器。默认转换后端为私有部署的 `SubLink 私有增强后端`，订阅转换不会经过公共 Subconverter 或第三方短链服务。

点击“生成私有订阅短链”后，页面把转换参数提交给私有网关。网关使用 AES-GCM 加密保存参数，令牌只保存 HMAC 哈希，并返回 `/s/<随机令牌>` 链接；短链本身不包含原始订阅地址。“自定义配置”仍是独立的第三方功能，页面已明确标记。

Subconverter 只监听服务器的 `127.0.0.1:25500`，私有网关监听 `127.0.0.1:25501` 并在访问令牌链接时调用它，容器日志已关闭。

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
| `VUE_APP_SUBCONVERTER_DEFAULT_BACKEND` | 默认私有网关地址 |
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

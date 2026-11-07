# 欧小新の站

个人博客，基于 [Hugo](https://gohugo.io/) 与主题 `themes/xeu`，构建为纯静态站点，并部署在 Cloudflare Workers（Static Assets）。

- 线上地址：https://shiue.<你的 workers.dev 子域>.workers.dev/
- 旧博客：https://blog.112133.xyz/

## 写作

```bash
npm run post:new
```

交互式创建 `content/post/<slug>/index.md`，默认 `draft: true`；写完后改成 `false` 再提交。

也可以直接手写 Markdown，front matter 参考现有文章：

```yaml
---
title: 标题
slug: slug
date: 2026-11-07T12:00:00+08:00
description: 摘要
draft: false
tags: [随笔]
---
```

## 本地构建（可选）

需要 Node.js 22.12+ 或 24，以及 `.hugo-version` 指定版本的 Hugo Extended。构建入口会通过 `scripts/hugo.sh` 调用 Hugo，因此需要 bash（Linux/macOS 自带，Windows 需要 WSL 或 Git Bash）。

```bash
npm ci
npm run dev     # 本地预览
npm run build   # 完整构建，产物在 public/
```

## 部署

通过 Cloudflare Workers Builds 自动构建：

| 设置 | 值 |
| --- | --- |
| Build command | `npm run build` |
| Deploy command | `npx wrangler deploy` |
| Root directory | `/` |

`wrangler.jsonc` 固定了 Worker 入口 `functions/cloudflare/worker.js`、静态资源目录 `public/` 与 `/api/*` 的优先路由。仓库里没有提交 `public/`，每次构建重新生成。

换域名时：修改 `hugo.toml` 与 `config/_default/config.toml` 里的 `baseURL`，推送后自动重建。

## 评论系统

当前在 `config/_default/params.toml` 中关闭（`[comments] enabled = false`），站点不渲染评论 UI，`/api/*` 仅保留 Worker 路由。启用前需要配置 Resend、Cloudflare Turnstile 与 GitHub Token，详见 `docs/comments.md` 与 `docs/serverless.md`。

## 许可证

- 主题代码：[MIT](themes/xeu/LICENSE)
- 本站文字：CC BY-NC-SA 4.0

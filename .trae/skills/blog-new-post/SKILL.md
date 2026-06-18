---
name: "blog-new-post"
description: "为 Wizard23333 博客（Jekyll + Chirpy）新建文章，生成符合命名与 frontmatter 规范的 Markdown 骨架。当用户要写新博客、新增文章、创建 post 时调用。"
---

# 新建博客文章（New Post）

在本仓库 `_posts/` 下创建符合 Chirpy 规范的文章。

## 文件命名
- 路径：`_posts/`（可按主题放入子目录，如 `_posts/translation/`，不影响 URL）
- 文件名：`YYYY-MM-DD-title-in-kebab-case.md`
- 站点 permalink 为 `/posts/:title/`（见 `_config.yml`）

## Frontmatter 模板
```yaml
---
title: 文章标题
author: wizard
date: YYYY-MM-DD HH:MM:SS +0800
categories: [一级分类, 二级分类]
tags: [标签1, 标签2]
---
```

字段说明：
- `date`：使用 `+0800`（站点 `timezone: Asia/Shanghai`）。
- `categories`：建议最多两级（一级、二级），Chirpy 会据此生成分类层级。
- `tags`：小写/自由文本均可，用于 `/tags/` 聚合。
- 可选：`pin: true`（置顶）、`math: true`、`mermaid: true`、`image:`（封面）。

## 正文常用语法（Chirpy）
- 提示框：在引用块后加 `{: .prompt-info }`（也支持 `tip`/`warning`/`danger`）。
- 图片：站点配置了 `img_cdn`，以 `/` 开头的图片路径会自动加 CDN 前缀。
- 代码块默认显示行号（`_config.yml` 中 `block.line_numbers: true`）。

## 创建后
1. 本地预览确认渲染：见 `blog-local-preview` skill（`bundle exec jekyll serve -l`）。
2. 确认无误后发布：见 `blog-publish-deploy` skill。

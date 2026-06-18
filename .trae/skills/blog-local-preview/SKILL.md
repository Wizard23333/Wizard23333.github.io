---
name: "blog-local-preview"
description: "在本地启动 Wizard23333 博客（Jekyll + Chirpy）的预览服务器。当用户要本地预览/调试博客、运行 jekyll serve、或修改后想看效果时调用。"
---

# 博客本地预览（Local Preview）

为本仓库（Jekyll 4.3 + Chirpy 主题 5.5.2，Windows + PowerShell）提供一键本地预览。

## 环境要求（本机已验证）
- Ruby 3.3.x（x64-mingw-ucrt）、Bundler、Node.js + npm
- `Gemfile` 已锁定 `wdm ~> 0.2.0`、`tzinfo-data`，兼容 Ruby 3.3

## 步骤

1. 安装依赖（首次或 Gemfile/package.json 变更后执行）：
```powershell
bundle install
npm install
```

国内网络可选镜像加速：
```powershell
bundle config set mirror.https://rubygems.org https://gems.ruby-china.com
npm config set registry https://registry.npmmirror.com
```

2. 启动本地服务器（带 LiveReload）：
```powershell
bundle exec jekyll serve -l
```

3. 终端出现 `Server running... press ctrl-c to stop.` 后，浏览器打开：
```
http://127.0.0.1:4000/
```

## 常见问题
- **构建慢**：Windows 下首次生成约 15–20s 属正常。
- **wdm 编译失败**：确认 `Gemfile` 中 `wdm` 为 `~> 0.2.0`。
- **文章不显示**：检查 `_posts` 文件名为 `YYYY-MM-DD-title.md`，且 frontmatter 的 `date` 正确。
- **改动未刷新**：`-l`（livereload）已开启；必要时 Ctrl-C 重启。

## 生产构建校验（可选，等同 CI）
```powershell
$env:JEKYLL_ENV="production"; bundle exec jekyll b
bundle exec htmlproofer _site --disable-external --check-html --allow_hash_href
```

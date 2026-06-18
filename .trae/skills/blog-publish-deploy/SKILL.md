---
name: "blog-publish-deploy"
description: "把 Wizard23333 博客的本地改动发布到线上 GitHub Pages。当用户要部署/发布博客、上线新文章、push 到生产时调用。"
---

# 发布部署（Publish & Deploy）

将本地改动发布到 `https://Wizard23333.github.io/`。

## 部署链路（重要）
- 本博客的**源分支是 `gh-pages`**（不是 master）。
- push 到 `gh-pages` → 触发 `.github/workflows/pages-deploy.yml`：
  - `bundle exec jekyll b`（`JEKYLL_ENV=production`）构建
  - `htmlproofer` 校验
  - 部署到 GitHub Pages
- `production` / `docs` 分支触发 `cd.yml`（向外部 builder 仓库发 dispatch），属上游主题发布链路，**日常发文不用**。
- 其他分支与 PR 会触发 `ci.yml`（运行 `tools/test` 做构建+校验）。

## 发布步骤

1. 本地校验（推荐，等同 CI，避免线上构建失败）：
```powershell
$env:JEKYLL_ENV="production"; bundle exec jekyll b
bundle exec htmlproofer _site --disable-external --check-html --allow_hash_href
```

2. 确认当前在 `gh-pages` 分支：
```powershell
git branch --show-current
```

3. 提交并推送（仅添加相关文件，避免 `git add .` 带入敏感/无关文件）：
```powershell
git add _posts/ _tabs/ assets/ _config.yml
git commit -m "docs: <本次改动说明>"
git push origin gh-pages
```

4. 验证部署：打开仓库 GitHub Actions 页，确认 "Build and Deploy" 工作流成功；随后访问线上站点确认改动生效。

## 注意
- 不要对 `gh-pages` 做 force push。
- commit message 遵循约定式提交（仓库启用了 commitlint / husky）。
- 改动只涉及 `.gitignore` / `README.md` / `LICENSE` 时不会触发部署（workflow 已 paths-ignore）。

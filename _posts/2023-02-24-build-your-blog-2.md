---
title: 使用 Jeklly + Github Pages 搭建个人博客(2)
author: wizard
date: 2023-02-24 16:19:00 +0800
categories: [博客搭建]
tags: [Jekyll, GitHub Pages]
---

> 本文所涉及开发环境默认为macOS，如在其他环境下，部分内容可能稍有差异，请自行甄别判断
> {: .prompt-info }

## 部署到 GitHub Pages

> 在上一篇文章中我们完成了 Jekyll 本地环境的配置，并运行了模版代码。接下来我们将我们的项目部署到 GitHub Pages

在上一篇博客中提到了模板作者提供了[详尽的文档](https://chirpy.cotes.page/)，本部分是以文档中的[部署部分](https://chirpy.cotes.page/posts/getting-started/#deployment)展开的

### 创建新分支用于部署

如果使用默认的 main（master）分支部署的话，可能不便于后续的版本更新操作，所以这里创建一个分支用来部署页面，博客内容也在这个分支上提交。main（master）分支用于同步原仓库内容。

```shell
# 基于一个 tag 创建分支
$ git branch gh-pages v5.5.2
$ git checkout gh-pages
```

### 创建并修改 workflow 文件

关于 workflow 的用法可以阅读 GitHub 官方文档：[GitHub Actions Documentation](https://docs.github.com/en/actions)

简单来讲，workflow 就是一系列的自动化操作，以`YAML` 文件的形式存储在项目的`.github/workflows` 目录下，这些自动化操作也可以设置不同的触发条件

```shell
$ cd your_project_path/.github/workflows/
# 使用锁存文件创建新的 workflow 文件
$ cp pages-deploy.yml.hook pages-deploy.yml
$ vim pages-deploy.yml
```

修改文件中的下列配置：

```yaml
....
on:
# 在 push 时触发
  push:
    branches:
# 设置触发 workflow 的分支，
# 这里删除了 main 和 master 分支，选择一个自定义分支来触发对应操作
      - gh-pages
    paths-ignore:
      - .gitignore
      - README.md
      - LICENSE
....
name: Setup Pages
   id: pages
   uses: actions/configure-pages@v3 # 使用v1的话会出现warning，这里修改成v3
....
```

### Push 修改内容到 GitHub

```shell
$ git add .
$ git commit -m "your commit message."
$ git push
```

## Github Pages 设置



   

## 如何开始博客写作

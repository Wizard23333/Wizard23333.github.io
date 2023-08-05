---
title: 使用 Jeklly + Github Pages 搭建个人博客(2)
author: wizard
date: 2023-02-24 16:19:00 +0800
categories: [博客搭建]
tags: [Jekyll, GitHub Pages]
---

> 本文所涉及开发环境默认为macOS，如在其他环境下，部分内容可能稍有差异，请自行甄别判断
{: .prompt-info }

## 部署到 GitHub Pages

> 在上一篇文章中我们完成了 Jekyll 本地环境的配置，并运行了模版代码。接下来我们将我们的项目部署到 GitHub Pages

在上一篇博客中提到了模板作者提供了[详尽的文档](https://chirpy.cotes.page/)，本部分是以文档中的[部署部分](https://chirpy.cotes.page/posts/getting-started/#deployment)展开的

### **创建新分支用于部署**

如果使用默认的 main（master）分支部署的话，可能不便于后续的版本更新操作，所以这里创建一个分支 : gh-pages 用来部署页面，博客内容也在这个分支上提交。main（master）分支用于同步原仓库内容。

```shell
# 基于一个 tag 创建分支
$ git branch gh-pages v5.5.2
$ git checkout gh-pages
```

### **创建并修改 workflow 文件**

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

### **Push 修改内容到 GitHub**

```shell
$ git add .
$ git commit -m "your commit message."
$ git push
```

## Github Pages 设置

### **Pages 设置**

打开项目设置，选择 Pages 设置，Source 选择 GitHub Action

![2023-02-24-build-your-blog-2-2023-02-28-12-59-28](https://cdn.jsdelivr.net/gh/Wizard23333/BlogPictures/archive_1/2023-02-24-build-your-blog-2-2023-02-28-12-59-28.png)

> 选择完成后不需要点击下面的 Configure 按钮，因为用于部署 GitHub Pages 的workflow 文件就是上文中的 pages-deploy.yml 文件
{: .prompt-warning }

### **触发 workflow**

触发 workflow 的方式在 pages-deploy.yml 文件中已经声明：手动触发 or 通过在 gh-pages 分支上 push 触发。下面介绍手动触发方式：

1. 进入到仓库的 Actions 页面，选择 Build and Deploy 这个 workflow 
   
   >  如果 Actions 显示为未启用，则启用 Actions
   
   ![2023-02-24-build-your-blog-2-2023-02-28-13-20-21](https://cdn.jsdelivr.net/gh/Wizard23333/BlogPictures/archive_1/2023-02-24-build-your-blog-2-2023-02-28-13-20-21.png)

2. 点击右方的 Run workflow 按钮，触发部署流程
   ![2023-02-24-build-your-blog-2-2023-02-28-13-23-33](https://cdn.jsdelivr.net/gh/Wizard23333/BlogPictures/archive_1/2023-02-24-build-your-blog-2-2023-02-28-13-23-33.png)
   
   任务完成后，点击链接，查看部署情况，没有问题的话，可以看到博客主页

## 如何开始博客写作

### **Markdown 语法**

每一篇博客都是通过对应的 MarkDown 文件生成的，如果你还不了解 MarkDown 语法，建议简单学习一下，上手成本很低

通过编辑器编写 MarkDown 文件会更加直观清晰，常见的 MarkDown 编辑器有：Typora（2021.11开始收费）、VSCode + 插件、markText（开源） 等编辑器

### **阅读模版作者的教程**

作为模板的作者，自然没有人比他更懂如何使用自己的模板，作者的教程也非常详细清晰，建议仔细阅读

作者到目前为止一共有 5 篇教程：

1. [Getting Started](https://chirpy.cotes.page/posts/getting-started/)：主要介绍如何使用该模板

2. [Writing a New Post](https://chirpy.cotes.page/posts/write-a-new-post/)：介绍如何开始写一篇博客

3. [Customize the Favicon](https://chirpy.cotes.page/posts/customize-the-favicon/)：介绍如何更改博客的图标

4. [Text and Typography](https://chirpy.cotes.page/posts/text-and-typography/)：介绍 markdown 在该模板上的渲染

5. [Enable Google Page Views](https://chirpy.cotes.page/posts/enable-google-pv/)：介绍启用页面浏览量

### **博客中的图片**

对于一般的 .md 文件来说，其中的图片可以应用本地路径的图片；或者像 GitHub 仓库中的 README.md 一样，引用同仓库中的内容。但是对于博客来说需要引用稳定的网络位置图片，因此需要一个服务能将博客中的图片转换为一个网络位置的 URL ，也就是图床。

图床的选择也有很多，我选择了一种免费的方案：GitHub + JsDelivr + picGo VSCode 插件。这个方案的教程也非常多，善用搜索引擎可以很快查到。

### **博客写作**

编写博客 MarkDown 文件的时候，在本地运行项目，可以实时查看渲染情况

博客 MarkDown 文件编写完成后，push commits 到远程仓库，即可以触发自动部署流程



> 如果博客中有任何的错误或者纰漏，欢迎在下面评论区指出
> 
{: .prompt-info }

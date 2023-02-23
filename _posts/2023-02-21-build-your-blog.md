---
title: 使用 Jeklly + Github Pages 搭建个人博客(1)
author: wizard
date: 2023-02-21 17:04:00 +0800
categories: [博客搭建]
tags: [Jekyll, GitHub Pages]
---

> 本文所涉及开发环境默认为macOS，如在其他环境下，部分内容可能稍有差异，请自行甄别判断
> {: .prompt-info }

## 技术选型

> 本部分介绍笔者为什么选择这样的技术搭配搭建博客，可自行判断跳过～

* 现成平台 VS 自己搭建
  
  如果只是想要发布自己的某些博客文章，可以选择现成的内容发布平台，这里仅仅列举一些国内常见的平台：**知乎**、**简书**、**CSDN**、**segmentfault**、**掘金**、**博客园**、**bilibili专栏**等等。使用这些平台能够便捷的发布文章内容，也有良好的文本编辑功能，但是不提供博客的个性化功能。如果希望能够对博客有完全的管理，并且自己有一定的技术基础，可以选择自建博客

* 静态页面 VS 动态页面
  
  作为个人博客网站，如果提供文章访问，使用静态页面就足够了，管理起来会更加简单

* 云服务器 VS Github Pages
  
  如果不考虑国内访问因素，Github Pages 是较优选择，主要优点如下：
  
  * 完全免费（要什么自行车）
    
    使用云服务器的话，即使是最低配的也需要几百每年
  
  * 不限时间
    
    使用云服务器的话，服务器过期后需要重新配置部署博客
  
  * 免备案
    
    使用国内服务器搭建博客均需要备案，使用GitHub Pages不需备案
  
  但也有缺点：
  
  * 仓库大小限制
    
    熟悉 Github 的话会知道，作为免费用户或者付费用户，每一个代码仓库的大小是有限制的：
    ![2023-02-21-build-your-blog-2023-02-21-19-16-05](https://cdn.jsdelivr.net/gh/Wizard23333/BlogPictures/archive_1/2023-02-21-build-your-blog-2023-02-21-19-16-05.png)
    
    但是作为个人博客站点，这个大小和带宽限制已经足够使用
  
  * 访问速度
    
    使用国内网络访问速度会受限，这一点无法避免

* Jekyll vs Hexo
  
  笔者对两种框架没有深刻研究，对于主流博客框架的各种优劣对比可自行搜索，对于在 Github Pages 部署，两者都可以实现便捷部署。

## 本地环境配置

### Git

> 网络上关于 git 的教程很多，所以这里的介绍会很简略
> {: .prompt-tip }

安装 git 主要有两种方式，可以从[官网下载安装包](https://git-scm.com/download/)，也可以使用 homebrew 安装（个人推荐，更加简洁，安装一些其他软件包也会使用到 homebrew ）：

1. 首先需要[安装homebrew](https://www.jianshu.com/p/e0471aa6672d)

2. 使用 homebrew 安装 git
   
   ```console
   # 安装最新版 git
   $ brew install git
   # 查看 homebrew 已安装内容
   $ brew list
   ```

> 这里可能会因为国内网络环境的原因遇到一些问题，解决这些问题可以参考我的另一篇[博客TODO]()
> {: .prompt-warning }

### Ruby

在所有的 macOS 上都会预装 ruby，但是由于系统权限原因，使用 RubyGems 安装软件包会出现如下错误：

```console
$ gem install jekyll
Fetching google-protobuf-3.22.0-x86_64-darwin.gem
Fetching sass-embedded-1.58.3-arm64-darwin.gem
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.6.0 directory.
```

具体原因可以参考这篇[博客](https://www.rubyonmac.dev/you-dont-have-write-permissions-for-the-library-ruby-gems-2-6-0-directory)，主要原因是系统 ruby 不提供写入权限；除此之外也因为系统 ruby 的一些原因，不建议强行使用。

对此，通用解决方法是使用 ruby 版本管理工具来管理包括系统 ruby 在内的多个 ruby 环境。常见的 ruby 管理工具有很多：rvm，rbenv，chruby等，这里选择了 rbenv 来管理包括系统 ruby 在内的多个环境

1. 使用 homebrew 安装 rbenv，ruby-build
   
   ```console
   $ brew install rbenv ruby-build
   ```

2. 配置环境变量
   
   ```console
   $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
   $ echo 'eval "$(rbenv init -)"' >> ~/.bashrc
   $ echo 'export RUBY_BUILD_MIRROR_URL=https://cache.ruby-china.com' >> ~/.bashrc
   ```
   
   或者打开 ~./zshrc or ~./bashrc 将文本添加在配置文件最后，两者等效
   
   ```textile
   # rbenv path
   export PATH="$HOME/.rbenv/bin:$PATH"
   eval "$(rbenv init -)"
   export RUBY_BUILD_MIRROR_URL=https://cache.ruby-china.com
   ```

3. 安装 ruby
   
   查看可用 ruby 版本
   
   ```console
   $ rbenv install --list
   2.6.10
   2.7.6
   3.0.4
   3.1.2
   jruby-9.3.4.0
   mruby-3.0.0
   rbx-5.0
   truffleruby-22.1.0
   truffleruby+graalvm-22.1.0
   ```
   
   这里选择了最新的 ruby 版本 `3.1.2`
   
   ```console
   # 安装 ruby 3.1.2
   $ rbenv install 3.1.2
   # 查看已安装的 ruby 版本
   $ rbenv versions
   # 设置全局的 ruby版本
   $ rbenv global 3.1.2
   ```

### Jekyll & Bundler

```console
# 使用 gem 安装 jekyll 和 bundler
$ gem install jekyll bundler
```

> 到这里为止，本地调试所需要的环境基本配置完成
> {: .prompt-tip}

---

## 选择 Jekyll 博客模版

可以在 [jekyllthemes](http://jekyllthemes.org/) 上选择免费的模版，也可以自行选择其他的模版。这里我选择了这样的一个模版：[Chirpy](http://jekyllthemes.org/themes/jekyll-theme-chirpy/)，模版的项目地址：[GitHub - cotes2020/jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy/)

该模版的demo地址：<https://chirpy.cotes.page/>，在其中有详细的项目的相关使用方法

---

> 下文步骤均以选择上述模板为前提
> {: .prompt-warning }

## 本地运行项目

1. fork 模板仓库
   
   > 除了 fork 之外也可以直接下载代码，或者使用 [template](https://github.com/cotes2020/chirpy-starter/)
   
   fork 时候需要注意将仓库名称改为 `your_github_username.github.io`

2. 克隆到本地
   
   > 克隆之前请先完成 [GitHub ssh 配置](https://blog.csdn.net/weixin_42310154/article/details/118340458) 或者 使用 GitHub desktop (不推荐，有些git 功能无法使用) 或者使用 https（同样不推荐，每次需要输入账号密码）

![Desktop View](https://cdn.jsdelivr.net/gh/Wizard23333/BlogPictures/archive_1/2023-02-21-build-your-blog-2023-02-23-14-14-21.png){:width="700" }

```console
# ！！使用自己的仓库地址！！
$ git clone git@github.com:Wizard23333/Wizard23333.github.io.git
```

3. 运行项目
   
   ```console
   $ cd your_project_path
   $ bundle install
   $ bundle exec jekyll server
   ```

       如果出现下面输出，说明成功运行
![2023-02-21-build-your-blog-2023-02-23-14-36-24](https://cdn.jsdelivr.net/gh/Wizard23333/BlogPictures/archive_1/2023-02-21-build-your-blog-2023-02-23-14-36-24.png){:width="700"}

    打开<http://localhost:4000/>，即可以看到运行中网页

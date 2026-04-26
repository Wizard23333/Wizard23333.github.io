# Wizard23333's Blog

> Powered by [Chirpy Jekyll Theme](https://github.com/cotes2020/jekyll-theme-chirpy)
>
> Deployed with [Github Pages](https://pages.github.com/)

## 线上地址
访问 [wizard23333.github.io](https://wizard23333.github.io/) 查看我的博客。

## 本地开发与预览

这个博客基于 Jekyll 引擎构建，使用了 Chirpy 主题。以下是在本地搭建环境并预览的步骤。

### 1. 环境准备
确保你的 Windows 设备上安装了以下工具：
- **Ruby 3.3+** (推荐使用 RubyInstaller with DevKit)
- **Bundler** (Ruby 的依赖管理器)
- **Node.js** (用于前端资源处理)

### 2. 初始化项目
在项目根目录下，运行以下命令安装依赖：

```powershell
# 设置 Ruby 环境变量（如果尚未在全局 PATH 中）
$env:PATH = "C:\Ruby33-x64\bin;" + $env:PATH

# 配置国内镜像源（可选，加速下载）
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
bundle config set mirror.https://rubygems.org https://gems.ruby-china.com
npm config set registry https://registry.npmmirror.com

# 安装依赖
bundle install
npm install
```

### 3. 启动预览
使用以下命令启动本地预览服务器：

```powershell
bundle exec jekyll serve --livereload
```

启动成功后，在浏览器访问 [http://127.0.0.1:4000/](http://127.0.0.1:4000/)。

### 常见问题
- **Windows 下 wdm 编译失败**：本项目已将 `wdm` 版本锁定为 `0.2.0` 以兼容 Ruby 3.3。
- **文章不显示**：检查 `_posts` 中的文章文件名格式是否为 `YYYY-MM-DD-title.md`，且 `date` 属性是否正确。

---

<div align="center">

  # Chirpy Jekyll Theme

  A minimal, responsive, and powerful Jekyll theme for presenting professional writing.

  [![Gem Version](https://img.shields.io/gem/v/jekyll-theme-chirpy?color=brightgreen)](https://rubygems.org/gems/jekyll-theme-chirpy)
  [![CI](https://github.com/cotes2020/jekyll-theme-chirpy/actions/workflows/ci.yml/badge.svg)](https://github.com/cotes2020/jekyll-theme-chirpy/actions/workflows/ci.yml)
  [![Codacy Badge](https://app.codacy.com/project/badge/Grade/4e556876a3c54d5e8f2d2857c4f43894)](https://www.codacy.com/gh/cotes2020/jekyll-theme-chirpy/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=cotes2020/jekyll-theme-chirpy&amp;utm_campaign=Badge_Grade)
  [![GitHub license](https://img.shields.io/github/license/cotes2020/jekyll-theme-chirpy.svg)](https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/LICENSE)
  [![996.icu](https://img.shields.io/badge/link-996.icu-%23FF4D5B.svg)](https://996.icu)

  [**Live Demo →**](https://cotes2020.github.io/chirpy-demo)

  [![Devices Mockup](https://chirpy-img.netlify.app/commons/devices-mockup.png)](https://cotes2020.github.io/chirpy-demo)

</div>

## Features

- Dark/Light Theme Mode
- Localized UI language
- Pinned Posts
- Hierarchical Categories
- Trending Tags
- Table of Contents
- Last Modified Date of Posts
- Syntax Highlighting
- Mathematical Expressions
- Mermaid Diagram & Flowchart
- Dark/Light Mode Images
- Embed Videos
- Disqus/Utterances/Giscus Comments
- Search
- Atom Feeds
- Google Analytics
- Page Views Reporting
- SEO & Performance Optimization

## Quick Start

Before starting, please follow the instructions in the [Jekyll Docs](https://jekyllrb.com/docs/installation/) to complete the installation of `Ruby`, `RubyGems`, `Jekyll`, and `Bundler`. In addition, [Git](https://git-scm.com/) is also required to be installed.

### Step 1. Creating a New Site

Create a new repository from the [**Chirpy Starter**](https://github.com/cotes2020/chirpy-starter/generate) and name it `<GH_USERNAME>.github.io`, where `GH_USERNAME` represents your GitHub username.

### Step 2. Installing Dependencies

Before running for the first time, go to the root directory of your site, and install dependencies as follows:

```console
$ bundle
```

### Step 3. Running Local Server

Run the following command in the root directory of the site:

```console
$ bundle exec jekyll s
```

Or run with Docker:

```console
$ docker run -it --rm \
    --volume="$PWD:/srv/jekyll" \
    -p 4000:4000 jekyll/jekyll \
    jekyll serve
```

After a while, navigate to the site at <http://localhost:4000>.

## Documentation

For more details on usage, please refer to the tutorial on the [demo website](https://cotes2020.github.io/chirpy-demo/) / [wiki](https://github.com/cotes2020/jekyll-theme-chirpy/wiki). Note that the tutorial is based on the [latest release](https://github.com/cotes2020/jekyll-theme-chirpy/releases/latest), and the features of the default branch are usually ahead of the documentation.

## Contributing

Welcome to report bugs, improve code quality or submit a new feature. For more information, see [contributing guidelines](.github/CONTRIBUTING.md).

## Credits

This theme is mainly built with [Jekyll](https://jekyllrb.com/) ecosystem, [Bootstrap](https://getbootstrap.com/), [Font Awesome](https://fontawesome.com/) and some other wonderful tools (their copyright information can be found in the relevant files). The avatar and favicon design come from [Clipart Max](https://www.clipartmax.com/middle/m2i8b1m2K9Z5m2K9_ant-clipart-childrens-ant-cute/).

:tada: Thanks to all the volunteers who contributed to this project, their GitHub IDs are on [this list](https://github.com/cotes2020/jekyll-theme-chirpy/graphs/contributors). Also, I won't forget those guys who submitted the issues or unmerged PR because they reported bugs, shared ideas, or inspired me to write more readable documentation.

Last but not least, thank [JetBrains][jb] for providing the OSS development license.

## Sponsoring

If you like this theme or find it helpful, please consider sponsoring me, because it will encourage and help me better maintain the project, I will be very grateful!

[![Ko-fi](https://img.shields.io/badge/-Buy%20Me%20a%20Coffee-ff5f5f?logo=ko-fi&logoColor=white)](https://ko-fi.com/coteschung)
[![Wechat Pay](https://img.shields.io/badge/-Tip%20Me%20on%20WeChat-brightgreen?logo=wechat&logoColor=white)][cn-donation]
[![Alipay](https://img.shields.io/badge/-Tip%20Me%20on%20Alipay-blue?logo=alipay&logoColor=white)][cn-donation]

## License

This work is published under [MIT](https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/LICENSE) License.

<!-- ReadMe links -->

[jb]: https://www.jetbrains.com/?from=jekyll-theme-chirpy
[cn-donation]: https://sponsor.cotes.page/

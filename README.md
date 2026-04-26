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

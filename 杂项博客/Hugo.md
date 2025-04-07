# Hugo

## 前置

在 Windows 10 上运行 Hugo 有以下几种方式，具体要求如下：

1. **预编译二进制文件**
   - Hugo 提供了适用于 Windows 平台的预编译二进制文件。下载后无需额外安装 Go 环境，直接在命令行（如 PowerShell 或 CMD）中运行即可。
   - 请确保操作系统为 64 位，因为 Hugo 的 Extended 版本（支持 Sass/SCSS 编译）通常仅适用于 64 位系统。
2. **从源码构建**
   - 如果需要从源码编译 Hugo，则需要预先安装 Go 编程环境（建议使用 Go 1.16 或更高版本）。
   - 构建完成后，同样可以通过命令行来运行 Hugo。
3. **额外功能支持**
   - 若需要使用 Hugo 的 Sass/SCSS 处理等高级功能，请选择 Hugo Extended 版本，该版本已内置相关依赖，无需另外安装其他工具。

综上所述，若仅运行 Hugo 并生成静态网站，下载适用于 Windows 的预编译二进制文件即可；而从源码构建则需要安装 Go 开发环境。选择 Hugo Extended 版本则有助于更方便地处理样式文件。

## 快速入门安装必要环境

[快速入门](https://hugo.opendocs.io/getting-started/quick-start/) 安装了GO,Dart Sass（通过windows下Scoop包管理器），在通过Scoop安装Hugo的扩展版。

```
hugo new site MyBlog --format yaml
# replace MyFreshWebsite with name of your website
```

## 添加主题

如果您已经在您的网站中使用 Git，则可以通过在 Hugo 网站的根目录中运行以下命令将主题添加为子模块：

```bash
git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
```


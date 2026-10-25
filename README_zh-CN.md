<p align="center">
  <img width="500" alt="osu! logo" src="assets/lazer.png">
</p>

# osu!

[![Build status](https://github.com/ppy/osu/actions/workflows/ci.yml/badge.svg?branch=master&event=push)](https://github.com/ppy/osu/actions/workflows/ci.yml)
[![GitHub release](https://img.shields.io/github/release/ppy/osu.svg)](https://github.com/ppy/osu/releases/latest)
[![CodeFactor](https://www.codefactor.io/repository/github/ppy/osu/badge)](https://www.codefactor.io/repository/github/ppy/osu)
[![dev chat](https://discordapp.com/api/guilds/188630481301012481/widget.png?style=shield)](https://discord.gg/ppy)
[![Crowdin](https://d322cqt584bo4o.cloudfront.net/osu-web/localized.svg)](https://crowdin.com/project/osu-web)

一个免费胜利的节奏游戏。节奏只是*一次点击*之遥！

这是 [osu!](https://osu.ppy.sh) 游戏客户端的未来——也是最终——迭代版本，标志着开放时代的开始！目前以发布代号“*lazer*”为人所知并发布。如同比尖端还锋利。

## 状态

本项目在持续开发中，但我们尽力保持稳定。鼓励玩家从发布版安装，与稳定的 *osu!* 客户端一起使用。本项目将持续演进，直到最终达到多数用户更偏好它而非旧的“osu!stable”发布的程度。

这里有一些可帮助你参与并理解项目的资源：

- 详细的发布更新日志在[官方 osu! 网站](https://osu.ppy.sh/home/changelog/lazer)。
- 你可以更多了解我们对[项目管理](https://github.com/ppy/osu/wiki/Project-management)的方法。
- 追踪我们[改进游戏](https://github.com/orgs/ppy/projects/7/views/6)的当前工作。

## 运行 osu!

如果你只是想试一试这个游戏，可以为你的平台获取最新版本：

### 最新版本：

| [Windows 10+ (x64)](https://github.com/ppy/osu/releases/latest/download/install.exe) | macOS 12+（[Intel](https://github.com/ppy/osu/releases/latest/download/osu.app.Intel.zip)、[Apple Silicon](https://github.com/ppy/osu/releases/latest/download/osu.app.Apple.Silicon.zip)） | [Linux (x64)](https://github.com/ppy/osu/releases/latest/download/osu.AppImage) | [iOS 13.4+](https://osu.ppy.sh/home/testflight) | [Android 7+](https://github.com/ppy/osu/releases/latest/download/sh.ppy.osulazer.apk) |
|--------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ------------- | ------------- | ------------- |

你也可以通常从 [osu! 网站](https://osu.ppy.sh/home/download)下载适合当前设备的版本。

如果你的平台不受支持或未列在上面，你仍有可能运行发布版或按照下面的说明手动构建。

**对于 iOS/iPadOS 用户**：iOS TestFlight 链接很快会满（Apple 有 10,000 用户的硬性限制）。我们会偶尔重置。请不要询问此事。请定期回来查看链接重置，或在 Twitter 关注 [peppy](https://twitter.com/ppy) 获取公告。我们的目标是将游戏尽快上架移动应用商店，这样就不必忍受这个限制。

## 开发自定义规则集

osu! 设计为允许用户创建的玩法变体，称为“规则集”（rulesets）。构建一个这样的规则集可以让开发者利用 osu! 谱面库、游戏引擎和通用 UX 的力量，用于新的玩法风格。要开始开发规则集，我们有一些模板[在这里](https://github.com/ppy/osu/tree/master/Templates)。

你可以通过访问[自定义规则集目录](https://github.com/ppy/osu/discussions/13096)查看一些自定义规则集的示例。

## 开发 osu!

### 前置要求

请确保你有以下前置要求：

- 一个装有正确 [.NET SDK](https://dotnet.microsoft.com/download) 的桌面平台。

处理代码库时，我们推荐使用具有智能代码补全和语法高亮的 IDE，例如最新版的 [Visual Studio](https://visualstudio.microsoft.com/vs/)、[JetBrains Rider](https://www.jetbrains.com/rider/)，或安装了 [EditorConfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) 和 [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) 插件的 [Visual Studio Code](https://code.visualstudio.com/)。

### 下载源码

克隆仓库：

```shell
git clone https://github.com/ppy/osu
cd osu
```

要将源码更新到最新提交，在 `osu` 目录内运行：

```shell
git pull
```

### 构建

#### 从 IDE

你应该通过平台特定的 `.slnf` 文件加载解决方案，而不是主 `.sln`。这会减少依赖并隐藏你不关心的平台。有效的 `.slnf` 文件有：

- `osu.Desktop.slnf`（最常见）
- `osu.Android.slnf`
- `osu.iOS.slnf`

已包含推荐 IDE（上面所列）的运行配置。你应该使用 IDE 提供的构建/运行功能来开始。测试或构建新组件时，强烈建议使用 `osu! (Tests)` 项目/配置。更多信息见[下面](#contributing)。

要为移动平台构建，你可能需要运行 `sudo dotnet workload restore`（如果之前没做过）。这会安装完成构建所需的 Android/iOS 工具。

#### 从命令行

你也可以用单个命令从命令行构建并运行 *osu!*：

```shell
dotnet run --project osu.Desktop
```

本地运行做任何性能测试时，务必在构建命令中加入 `-c Release`，因为默认 `Debug` 配置的开销可能很大（尤其是按下面方式测试本地框架改动时）。

如果构建失败，尝试用 `dotnet restore` 恢复 NuGet 包。

### 使用资源/框架改动测试

有时可能需要在 [osu-resources](https://github.com/ppy/osu-resources) 或 [osu-framework](https://github.com/ppy/osu-framework) 中交叉测试改动。这可以用包含的命令快速实现：

Windows：

```ps
UseLocalFramework.ps1
UseLocalResources.ps1
```

macOS / Linux：

```ps
UseLocalFramework.sh
UseLocalResources.sh
```

注意这些命令假设你已在相邻目录中检出相关项目：

```
|- osu            // 本仓库
|- osu-framework
|- osu-resources
```

### 代码分析

提交代码前，请运行代码格式化工具。可以通过在命令行运行 `dotnet format`，或在 IDE 中使用 `Format code` 命令。

我们采用了一些跨平台、编译器集成的分析器。它们在编辑、在 IDE 或命令行构建时提供警告，如同编译器本身提供的一样。

JetBrains ReSharper InspectCode 也用于更广泛的规则集。你可以用 PowerShell 运行 `.\InspectCode.ps1`。或者，你可以安装 ReSharper 或使用 Rider 在你选择的 IDE 中获得内联支持。

## 贡献

贡献项目时，你能提供帮助的两件主要事情是报告问题和提交拉取请求。请参考[贡献指南](CONTRIBUTING.md)了解如何最有效地提供帮助。

如果你想帮助本地化工作，请前往 [crowdin](https://crowdin.com/project/osu-web)。

我们的团队相信**人的贡献**。任何贡献——无论是问题报告还是拉取请求——如果是通过 AI/LLM 创建、记录或辅助的，通常会被**关闭并锁定，不再讨论**。

## 许可证

*osu!* 的代码和框架采用 [MIT 许可证](https://opensource.org/licenses/MIT)。更多信息请见[许可证文件](LICENCE)。[tl;dr](https://tldrlegal.com/license/mit-license) 只要在任何软件/源代码副本中包含原始版权和许可证声明，你可以做任何想做的事。

请注意，这*不包括*在任何软件、资源、广告或推广中使用“osu!”或“ppy”品牌，因为这受商标法保护。

还请注意，游戏资源由单独的许可证覆盖。请参见 [ppy/osu-resources](https://github.com/ppy/osu-resources) 仓库了解说明。

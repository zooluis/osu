<p align="center">
  <img width="500" alt="osu! logo" src="assets/lazer.png">
</p>

# osu!

[![Build status](https://github.com/ppy/osu/actions/workflows/ci.yml/badge.svg?branch=master&event=push)](https://github.com/ppy/osu/actions/workflows/ci.yml)
[![GitHub release](https://img.shields.io/github/release/ppy/osu.svg)](https://github.com/ppy/osu/releases/latest)
[![CodeFactor](https://www.codefactor.io/repository/github/ppy/osu/badge)](https://www.codefactor.io/repository/github/ppy/osu)
[![dev chat](https://discordapp.com/api/guilds/188630481301012481/widget.png?style=shield)](https://discord.gg/ppy)
[![Crowdin](https://d322cqt584bo4o.cloudfront.net/osu-web/localized.svg)](https://crowdin.com/project/osu-web)

一個免費勝利（free-to-win）的節奏遊戲。節奏只是*一次點擊*之遙！

這是 [osu!](https://osu.ppy.sh) 遊戲用戶端的未來——也是最終——迭代版本，標誌著開放時代的開始！目前以發布代號“*lazer*”為人所知並發布。如同比尖端還鋒利。

## 狀態

本專案在持續開發中，但我們盡力保持穩定。鼓勵玩家從發布版安裝，與穩定的 *osu!* 用戶端一起使用。本專案將持續演進，直到最終達到多數使用者更偏好它而非舊的“osu!stable”發布的程度。

這裡有一些可幫助你參與並理解專案的資源：

- 詳細的發布更新日誌在[官方 osu! 網站](https://osu.ppy.sh/home/changelog/lazer)。
- 你可以更多了解我們對[專案管理](https://github.com/ppy/osu/wiki/Project-management)的方法。
- 追蹤我們[改進遊戲](https://github.com/orgs/ppy/projects/7/views/6)的目前工作。

## 執行 osu!

如果你只是想試一試這個遊戲，可以為你的平台取得最新版本：

### 最新版本：

| [Windows 10+ (x64)](https://github.com/ppy/osu/releases/latest/download/install.exe) | macOS 12+（[Intel](https://github.com/ppy/osu/releases/latest/download/osu.app.Intel.zip)、[Apple Silicon](https://github.com/ppy/osu/releases/latest/download/osu.app.Apple.Silicon.zip)） | [Linux (x64)](https://github.com/ppy/osu/releases/latest/download/osu.AppImage) | [iOS 13.4+](https://osu.ppy.sh/home/testflight) | [Android 7+](https://github.com/ppy/osu/releases/latest/download/sh.ppy.osulazer.apk) |
|--------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ------------- | ------------- | ------------- |

你也可以通常從 [osu! 網站](https://osu.ppy.sh/home/download)下載適合目前裝置的版本。

如果你的平台不受支援或未列在上面，你仍有可能執行發布版或按照下面的說明手動建置。

**對於 iOS/iPadOS 使用者**：iOS TestFlight 連結很快會滿（Apple 有 10,000 使用者的硬性限制）。我們會偶爾重置。請不要詢問此事。請定期回來查看連結重置，或在 Twitter 關注 [peppy](https://twitter.com/ppy) 獲取公告。我們的目標是將遊戲盡快上架行動應用程式商店，這樣就不必忍受這個限制。

## 開發自訂規則集

osu! 設計為允許使用者建立玩法變體，稱為「規則集」（rulesets）。建置一個這樣的規則集可以讓開發者利用 osu! 譜面庫、遊戲引擎和通用 UX 的力量，用於新的玩法風格。要開始開發規則集，我們有一些範本[在這裡](https://github.com/ppy/osu/tree/master/Templates)。

你可以透過造訪[自訂規則集目錄](https://github.com/ppy/osu/discussions/13096)查看一些自訂規則集的範例。

## 開發 osu!

### 前置需求

請確保你有以下前置需求：

- 一個裝有正確 [.NET SDK](https://dotnet.microsoft.com/download) 的桌面平台。

處理程式碼庫時，我們推薦使用具有智慧型程式碼補全和語法反白的 IDE，例如最新版的 [Visual Studio](https://visualstudio.microsoft.com/vs/)、[JetBrains Rider](https://www.jetbrains.com/rider/)，或安裝了 [EditorConfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) 和 [C# Dev Kit](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) 外掛的 [Visual Studio Code](https://code.visualstudio.com/)。

### 下載原始碼

克隆倉庫：

```shell
git clone https://github.com/ppy/osu
cd osu
```

要將原始碼更新到最新提交，在 `osu` 目錄內執行：

```shell
git pull
```

### 建置

#### 從 IDE

你應該透過平台特定的 `.slnf` 檔案載入解決方案，而不是主 `.sln`。這會減少相依性並隱藏你不關心的平台。有效的 `.slnf` 檔案有：

- `osu.Desktop.slnf`（最常見）
- `osu.Android.slnf`
- `osu.iOS.slnf`

已包含推薦 IDE（上面所列）的執行設定。你應該使用 IDE 提供的建置/執行功能來開始。測試或建置新元件時，強烈建議使用 `osu! (Tests)` 專案/設定。更多資訊見[下面](#contributing)。

要為行動平台建置，你可能需要執行 `sudo dotnet workload restore`（如果之前沒做過）。這會安裝完成建置所需的 Android/iOS 工具。

#### 從命令列

你也可以用單一命令從命令列建置並執行 *osu!*：

```shell
dotnet run --project osu.Desktop
```

在本機執行做任何效能測試時，務必在建置命令中加入 `-c Release`，因為預設 `Debug` 設定的開銷可能很大（尤其是按下面方式測試本機框架變更時）。

如果建置失敗，嘗試用 `dotnet restore` 還原 NuGet 套件。

### 使用資源/框架變更測試

有時可能需要在 [osu-resources](https://github.com/ppy/osu-resources) 或 [osu-framework](https://github.com/ppy/osu-framework) 中交叉測試變更。這可以用包含的命令快速實現：

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

注意這些命令假設你已在相鄰目錄中檢出相關專案：

```
|- osu            // 本倉庫
|- osu-framework
|- osu-resources
```

### 程式碼分析

提交程式碼前，請執行程式碼格式化工具。可以透過在命令列執行 `dotnet format`，或在 IDE 中使用 `Format code` 命令。

我們採用了一些跨平台、編譯器整合的分析器。它們在編輯、在 IDE 或命令列建置時提供警告，如同編譯器本身提供的一樣。

JetBrains ReSharper InspectCode 也用於更廣泛的規則集。你可以用 PowerShell 執行 `.\InspectCode.ps1`。或者，你可以安裝 ReSharper 或使用 Rider 在你選擇的 IDE 中獲得內嵌支援。

## 貢獻

貢獻專案時，你能提供幫助的兩件主要事情是報告問題和提交拉取請求。請參考[貢獻指南](CONTRIBUTING.md)了解如何最有效地提供幫助。

如果你想幫助在地化工作，請前往 [crowdin](https://crowdin.com/project/osu-web)。

我們的團隊相信**人的貢獻**。任何貢獻——無論是問題報告還是拉取請求——如果是透過 AI/LLM 建立、記錄或輔助的，通常會被**關閉並鎖定，不再討論**。

## 授權條款

*osu!* 的程式碼和框架採用 [MIT 授權條款](https://opensource.org/licenses/MIT)。更多資訊請見[授權條款檔案](LICENCE)。[tl;dr](https://tldrlegal.com/license/mit-license) 只要在任何軟體/原始碼副本中包含原始版權和授權聲明，你可以做任何想做的事。

請注意，這*不包括*在任何軟體、資源、廣告或推廣中使用「osu!」或「ppy」品牌，因為這受商標法保護。

還請注意，遊戲資源由單獨的授權條款涵蓋。請參見 [ppy/osu-resources](https://github.com/ppy/osu-resources) 倉庫了解說明。

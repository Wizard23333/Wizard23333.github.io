---
title: Apple文档翻译（4）——Describing data use in privacy manifests
author: wizard
date: 2026-05-03 20:20:00 +0800
categories: [文档翻译]
tags: [文档翻译, Apple, 隐私]
---

> 本文基于 Apple 官方文档进行中文翻译与整理。
{: .prompt-info }

原文地址：[Apple 官方文档](https://developer.apple.com/documentation/bundleresources/describing-data-use-in-privacy-manifests)

# Describing data use in privacy manifests

> 声明你的 App 或第三方 SDK 收集的数据。

## 概览

在隐私清单中，你需要记录 App 或第三方 SDK 收集了哪些与 App 使用者相关的数据类别，以及收集这些数据的原因。

App 开发者可以使用 Xcode 创建隐私报告。该报告会汇总 App 以及 App 链接的第三方 SDK 中有关数据收集的信息。

> **重要：**
>
> 第三方 SDK 需要提供自己的隐私清单文件，记录它们收集的数据类型。你的 App 的隐私清单不需要覆盖 App 链接的第三方 SDK 所收集的数据。
{: .prompt-warning }

## 描述 App 或第三方 SDK 收集的数据

对于你的 App 或第三方 SDK 收集的每一种数据类型，都需要在隐私信息文件的 `NSPrivacyCollectedDataTypes` 数组中添加一个字典。

这个字典需要包含以下键。

### `NSPrivacyCollectedDataType`

一个字符串，用于标识你的 App 或第三方 SDK 收集的数据类型。

你需要从 Apple 文档列出的数据类型中选择一个与实际收集行为匹配的值。不要自定义数据类型。

### `NSPrivacyCollectedDataTypeLinked`

一个布尔值，用于表示你的 App 或第三方 SDK 是否会将该数据类型与用户身份关联。

关于“与用户关联的数据”的具体含义，可以参考 App Store 中 App 隐私详情的相关说明。

### `NSPrivacyCollectedDataTypeTracking`

一个布尔值，用于表示你的 App 或第三方 SDK 是否会使用该数据类型进行跟踪。

这里的“跟踪”应按 Apple 的 App Tracking Transparency 和 App 隐私说明中的定义理解。

### `NSPrivacyCollectedDataTypePurposes`

一个字符串数组，列出你的 App 或第三方 SDK 收集该数据的原因。

你需要从 Apple 文档列出的用途值中选择与实际收集原因匹配的值。不要自定义收集原因。

> **注意：**
>
> 如果为 `NSPrivacyCollectedDataType` 自定义数据类型，或为 `NSPrivacyCollectedDataTypePurposes` 自定义原因，Xcode 将无法正确生成隐私报告。请使用 Apple 文档为这些键列出的值。
{: .prompt-info }

## 数据声明的判断顺序

实际填写时，可以按以下顺序审查每一种数据：

1. 这项数据是否真的由 App 或 SDK 收集。
2. 它属于 Apple 列表中的哪一种数据类型。
3. 它是否会与用户身份关联。
4. 它是否会被用于跟踪。
5. 它的收集用途是什么，是否与代码和产品功能一致。

这样整理后，再把对应值写入 `NSPrivacyCollectedDataTypes` 数组中的字典。

## 创建 App 的隐私报告

Xcode 可以把你的 App 和它链接的第三方 SDK 中的隐私清单聚合起来，生成一份隐私报告。你可以用这份报告理解 App 收集的所有数据，以及是否存在跟踪行为。

创建隐私报告的步骤如下：

1. 在 Xcode 中打开项目。
2. 选择 **Product > Archive**。Xcode 会创建 archive，并在 organizer 中显示。
3. 在 organizer 中按住 Control 点击 archive，选择 **Generate Privacy Report**。
4. 选择保存隐私报告的位置。
5. 切换到 Finder。
6. 打开保存位置，双击报告，用 Preview 查看。

隐私报告的组织方式与 Privacy Nutrition Labels 类似。你在 App Store Connect 中填写 App 隐私详情时，可以参考这份报告。

## 常见错误

- 在隐私清单里声明了自定义数据类型或自定义用途，导致 Xcode 无法正确生成报告。
- App 的隐私清单试图覆盖第三方 SDK 收集的数据。第三方 SDK 应提供自己的隐私清单。
- 数据用途、跟踪状态或是否与用户关联的判断与实际代码行为不一致。
- 隐私清单与 App Store Connect 中填写的 App 隐私详情不一致。

---
title: Apple文档翻译（2）——Privacy manifest files
author: wizard
date: 2026-05-03 20:00:00 +0800
categories: [文档翻译]
tags: [文档翻译, Apple, 隐私]
---

> 本文基于 Apple 官方文档进行中文翻译与整理，尽量保持术语一致与语义准确。
{: .prompt-info }

原文地址：[Apple 官方文档](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files)

# Privacy manifest files（隐私清单文件）

> 描述你的 App 或第三方 SDK 收集的数据，以及它使用的 required reason API。

## 概览

App 和第三方 SDK 可以包含一个名为 `PrivacyInfo.xcprivacy` 的隐私清单文件。这里的第三方 SDK 可以通过 XCFramework、Swift package 或 Xcode project 等形式分发。

隐私清单是一个属性列表文件，用来记录以下信息：

- 你的 App 或第三方 SDK 收集了哪些类型的数据。无论在哪个平台上，都需要为 App 或第三方 SDK 提供这类信息。
- 你的 App 或第三方 SDK 使用了哪些 required reason API。在 iOS、iPadOS、tvOS、visionOS 和 watchOS 上，需要提供这类信息。

对于 App 或第三方 SDK 收集的每一种数据类型，以及它使用的每一种 required reason API 类别，都需要在随包提供的隐私清单文件中记录对应原因。

> **重要：**
>
> 如果你的第三方 SDK 位于 Apple 的 “SDKs that require a privacy manifest and signature” 列表中，就需要包含隐私清单文件。即使不在该列表中，只要 SDK 使用 required reason API、收集使用该 SDK 的 App 用户的数据、使 App 能够收集用户数据，或者会连接跟踪域名，也应提供隐私清单文件。提供隐私清单有助于 App 开发者理解第三方 SDK 的 API 使用情况和数据收集实践。
{: .prompt-warning }

如果需要编辑隐私清单文件，可以参考 Apple 的属性列表编辑文档。

## 创建隐私清单

在 Xcode 中为 App 或第三方 SDK 添加隐私清单，可以按以下步骤操作：

1. 选择 **File > New File**。
2. 滚动到 **Resource** 区域，选择 **App Privacy** 文件类型。
3. 点击 **Next**。
4. 在 **Targets** 列表中勾选你的 App 或第三方 SDK target。
5. 点击 **Create**。

默认情况下，Xcode 会将文件命名为 `PrivacyInfo.xcprivacy`。对于随包提供的隐私清单来说，这是必需的文件名。

> **注意：**
>
> 你需要把隐私清单文件加入 target 的资源中，Xcode 才能在生成隐私报告时使用它。若要在 `NSPrivacyTrackingDomains` 中提供互联网域名列表，需要同时把 `NSPrivacyTracking` 设为 `true`。
{: .prompt-info }

## 隐私清单中的顶层键

### `NSPrivacyTracking`

一个布尔值，用于表示你的 App 或第三方 SDK 是否按照 App Tracking Transparency 框架中的定义使用数据进行跟踪。

### `NSPrivacyTrackingDomains`

一个字符串数组，列出你的 App 或第三方 SDK 会连接的、参与跟踪的互联网域名。

如果用户没有通过 App Tracking Transparency 框架授予跟踪权限，发往这些域名的网络请求会失败，App 会收到错误。若 `NSPrivacyTracking` 设置为 `true`，就至少需要在 `NSPrivacyTrackingDomains` 中提供一个域名；否则可以提供零个或多个域名。

### `NSPrivacyCollectedDataTypes`

一个字典数组，用于描述你的 App 或第三方 SDK 收集的数据类型。每个字典中的键和值应参考 “Describing data use in privacy manifests”。

### `NSPrivacyAccessedAPITypes`

一个字典数组，用于描述你的 App 或第三方 SDK 访问了哪些被 Apple 标记为 required reason API 的 API 类别。每个字典中的键和值应参考 “Describing use of required reason API”。

## 相关主题

Apple 在隐私清单主题下还提供了以下核心文档：

- **Adding a privacy manifest to your app or third-party SDK**：说明如何把隐私清单放到 App、framework、Swift package、静态库或 XCFramework 中。
- **Describing data use in privacy manifests**：说明如何声明 App 或第三方 SDK 收集的数据。
- **Describing use of required reason API**：说明如何声明 required reason API 的使用原因。
- **Editing property list files**：说明如何编辑结构化的属性列表文件。
- **App Privacy Configuration**：隐私清单根对象对应的数据结构。

## 与属性列表的关系

隐私清单属于 bundle resources 中的属性列表资源。与它相关的常见资源还包括：

- `Entitlements`：授予可执行文件使用特定服务或技术的键值对。
- `Information Property List`：用于识别和配置 bundle 的键值资源。

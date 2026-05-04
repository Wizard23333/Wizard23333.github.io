---
title: Apple文档翻译（3）——Adding a privacy manifest to your app or third-party SDK
author: wizard
date: 2026-05-03 20:10:00 +0800
categories: [文档翻译]
tags: [文档翻译, Apple, 隐私]
---

> 本文基于 Apple 官方文档进行中文翻译与整理。
{: .prompt-info }

原文地址：[Apple 官方文档](https://developer.apple.com/documentation/bundleresources/adding-a-privacy-manifest-to-your-app-or-third-party-sdk)

# Adding a privacy manifest to your app or third-party SDK

> 在你的 App 或第三方 SDK 中报告收集的数据，以及使用的 required reason API。

## 概览

隐私清单是一个名为 `PrivacyInfo.xcprivacy` 的属性列表文件，需要加入 target 的资源中。它描述 App 或第三方 SDK 的隐私实践。

如果使用 Xcode 为 App 或第三方 SDK 添加隐私清单，可以参考 “Privacy manifest files” 文档中的创建步骤。你的 App 中包含的所有隐私清单都必须有效，只能包含 Apple 预期的键和值。关于数据收集字段的填写方式，可以参考 “Describing data use in privacy manifests” 中的相关说明。

> **重要：**
>
> App Store Connect 会拒绝包含无效隐私清单文件的 App 提交。如果你上传的 App 包含带有非预期键或值的隐私清单文件，Apple 会发送邮件，指出 App bundle 中无效文件的名称和路径。
>
> 如果无效文件来自第三方 SDK，请联系 SDK 开发者获取包含有效隐私清单的更新版本。排查无效隐私清单时，可以参考 TN3181。自 2025 年 2 月 12 日起，提交到 App Store Connect 审核的 App 必须为若干常用第三方 SDK 包含有效隐私清单。详情可参考 Apple 的第三方 SDK 要求。
{: .prompt-warning }

当你在 Xcode 中把隐私清单加入构建 App 或第三方 SDK 的 target 时，Xcode 会根据产品类型和目标平台自动把文件放到正确位置。如果你不使用 Xcode 构建产品，需要按下面的位置手动放置。即使使用 Xcode，这些位置说明也可以帮助你验证最终产物中的隐私清单是否放对。

## 向 App 添加隐私清单

如果构建的是 iOS、iPadOS、tvOS、visionOS 或 watchOS App，应将隐私清单放在 App bundle 的根目录。

例如，一个名为 `Sample` 的 iOS App，其结构应包含：

```text
Sample.app/
    Info.plist
    Sample
    PrivacyInfo.xcprivacy
    ...
```

对于 macOS 和 Mac Catalyst App，应将隐私清单放在 App bundle 的 `Contents/Resources/` 目录下。

例如，一个名为 `MacSample` 的 macOS App，其隐私清单路径应为：

```text
MacSample.app/
    Contents/
        Info.plist
        MacOS/
            MacSample
        Resources/
            PrivacyInfo.xcprivacy
        ...
```

## 向 framework 添加隐私清单

framework 中隐私清单的位置取决于目标平台。

对于 iOS、iPadOS、tvOS、visionOS 或 watchOS framework，应将隐私清单放在 framework bundle 的根目录。

例如，一个名为 `SampleFramework` 的 iOS framework，其结构应包含：

```text
SampleFramework.framework/
    Info.plist
    SampleFramework
    PrivacyInfo.xcprivacy
    ...
```

对于 macOS 和 Mac Catalyst framework，应将隐私清单放在 framework bundle 的 `Versions/A/Resources/` 中。

例如，一个名为 `MacSampleFramework`、只支持 `A` 版本的 macOS framework，其隐私清单路径应为：

```text
MacSampleFramework.framework/
    MacSampleFramework -> Versions/Current/MacSampleFramework
    Resources -> Versions/Current/Resources
    Versions/
        Current -> A
        A/
            MacSampleFramework
            Resources/
                Info.plist
                PrivacyInfo.xcprivacy
            ...
```

## 向 Swift package 添加隐私清单

Swift package 的资源可以包含隐私清单文件。源文件通常放在 package 的 `Sources` 目录下，并且属于哪个 target，就位于与 target 同名的子目录中。默认情况下，资源也可以放在这个目录里。

如果没有指定其他资源目录，可以把隐私清单放在 `Sources/SomeLibrary` 下：

```text
SamplePackage/
    README.md
    Package.swift
    Sources/
        SomeLibrary/
            SomeLibrary.swift
            Product.swift
            PrivacyInfo.xcprivacy
            ...
    Tests/
        ...
```

如果资源文件放在源文件之外的单独子目录中，例如 `Sources/SomeLibrary/Resources`，则可以把隐私清单放在该资源目录中：

```text
SamplePackage/
    README.md
    Package.swift
    Sources/
        SomeLibrary/
            SomeLibrary.swift
            Product.swift
            ...
            Resources/
                PrivacyInfo.xcprivacy
                ...
    Tests/
        ...
```

Xcode 默认不会把隐私清单识别为 Swift package 资源。添加清单文件后，需要在 package manifest 中显式声明这个文件，或声明包含它的目录。

显式声明单个清单文件：

```swift
targets: [
    .target(
        name: "SomeLibrary",
        resources: [
            .process("PrivacyInfo.xcprivacy")]
    ),
]
```

显式声明 `Resources` 目录：

```swift
targets: [
    .target(
        name: "SomeLibrary",
        resources: [
            .process("Resources")]
    ),
]
```

如果你把二进制 framework 作为 Swift package 分发，需要确认 XCFramework bundle 为每个支持的平台都包含隐私清单。

## 为静态库分发隐私清单

静态库是以 `.a` 扩展名结尾的目标文件归档，本身不支持包含隐私清单这类资源。

如果你当前以静态库形式分发 SDK，并希望在 Xcode 中加入隐私清单，可以创建一个新的 static framework target，把静态库的源文件和资源都纳入其中，再把隐私清单加入该 target 的资源。

如果不使用 Xcode 构建静态库，则需要手动为静态库创建 static framework bundle，并把隐私清单作为资源加入 bundle。隐私清单在 static framework 中的位置可参考前文的 framework 放置规则。

## 验证 XCFramework bundle 中是否包含所需清单

XCFramework bundle 是一种二进制包，可以包含多个平台变体的 framework 或 library，并支持分发 Swift 与 C 系代码。

为 framework 加入隐私清单后，可以考虑将其作为签名过的 XCFramework bundle 分发。生成 XCFramework 时，每个平台变体都应包含自己的隐私清单文件。清单所在位置由该 framework 变体的目标平台决定。

签名 XCFramework bundle 后，集成该 framework 的开发者可以判断 framework 更新后的代码签名是否兼容。

例如，如果 `SampleFramework` 支持 iOS、iOS Simulator、Mac Catalyst 和 macOS，可以用 `xcodebuild -create-xcframework` 将各平台归档合并为一个 XCFramework。生成后的结构中，每个变体都应在对应平台要求的位置包含 `PrivacyInfo.xcprivacy`：

- iOS 与 iOS Simulator framework：清单位于 framework 根目录。
- macOS 与 Mac Catalyst framework：清单位于 `Versions/A/Resources/`。

## 定位 App 归档中的无效隐私清单

如果需要在 App archive 中查找无效隐私清单，可以按以下步骤操作：

1. 从 Apple 邮件中获取无效文件的位置。
2. 在 Xcode 中选择 **Window > Organizer**，然后点击 **Archives**。
3. 在 organizer 中按住 Control 点击 App archive，选择 **Show in Finder**。
4. 在 Finder 中按住 Control 点击 archive，选择 **Show Package Contents**。
5. 导航到邮件中指出的隐私清单路径。

## 处理无效隐私清单

如果你无法从第三方 SDK 开发者那里获得有效隐私清单，但又需要提交 App，可以在 App archive 中定位无效隐私清单，将其删除后使用 Xcode 提交归档。另一种做法是定位 App 中的无效隐私清单，修复其中的验证问题，然后再通过 Xcode 提交归档。

> **重要：**
>
> 删除或修改无效隐私清单会破坏已归档 App 的签名。当你从 Archives organizer 分发归档时，Xcode 会重新为 App 签名。
{: .prompt-warning }

## 相关文档

- **Describing data use in privacy manifests**：声明 App 或第三方 SDK 收集的数据。
- **Describing use of required reason API**：确保 covered API 的使用符合政策。
- **Editing property list files**：编辑结构化属性列表文件。
- **App Privacy Configuration**：隐私清单根对象的数据结构。

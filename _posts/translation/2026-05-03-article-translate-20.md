---
title: Apple文档翻译（5）——TN3183: Adding required reason API entries to your privacy manifest
author: wizard
date: 2026-05-03 20:30:00 +0800
categories: [文档翻译]
tags: [文档翻译, Apple, 隐私, Technote]
---

> 本文基于 Apple Technote 文档进行中文翻译与整理。
{: .prompt-info }

原文地址：[Apple 官方文档](https://developer.apple.com/documentation/technotes/tn3183-adding-required-reason-api-entries-to-your-privacy-manifest)

# TN3183: Adding required reason API entries to your privacy manifest

> 在隐私清单中声明你的 App 或第三方 SDK 中可能被用于设备指纹识别的 API。

## 概览

当你构建的 App 或第三方 SDK 使用任何 required reason API 时，需要在隐私清单文件 `PrivacyInfo.xcprivacy` 中完成以下工作：

1. 添加 `NSPrivacyAccessedAPITypes` 键，并将其值设置为数组。
2. 对于 App 或第三方 SDK 使用的每一种 required reason API，在 `NSPrivacyAccessedAPITypes` 数组中添加一个字典。这个字典包含 API 类别，以及使用该 API 的原因列表。

关于隐私清单和这些键的更多背景，可以参考 “Privacy manifest files” 和 “Describing use of required reason API”。

本文说明如何在 Xcode 中向隐私清单添加 `NSPrivacyAccessedAPIType`、`NSPrivacyAccessedAPITypeReasons` 和 `NSPrivacyAccessedAPITypes`。如果你不使用 Xcode，也可以通过本文了解这些键的预期结构。

> **注意：**
>
> 在开始添加键之前，建议在 Xcode 中启用 raw keys and values，以便查看原始键名并隐藏人类可读名称。点击隐私清单中的任意位置，然后选择 **Xcode > Editor > Raw Keys and Values**。重复该操作可以关闭此功能。
{: .prompt-info }

## 选择访问的 API 类别

privacy accessed API category 用于标识你的 App 或第三方 SDK 使用了哪一类 required reason API。

你需要将 `NSPrivacyAccessedAPIType` 键的值设置为一个 privacy accessed API category。可用类别包括：

- `NSPrivacyAccessedAPICategoryActiveKeyboards`
- `NSPrivacyAccessedAPICategoryDiskSpace`
- `NSPrivacyAccessedAPICategoryFileTimestamp`
- `NSPrivacyAccessedAPICategorySystemBootTime`
- `NSPrivacyAccessedAPICategoryUserDefaults`

## 添加 accessed API type 键

`NSPrivacyAccessedAPIType` 键的结构如下：

```xml
<key>NSPrivacyAccessedAPIType</key>
<string>NS_PRIVACY_ACCESSED_API_CATEGORY_VALUE</string>
```

其中 `NS_PRIVACY_ACCESSED_API_CATEGORY_VALUE` 表示一个 privacy accessed API category。

在 Xcode 的属性列表编辑器中，可以按以下步骤添加该键：

1. 选择 privacy accessed API type and reasons 字典。
2. 点击字典左侧的展开三角形。
3. 点击字典旁的 Add 按钮（+）添加新项目。
4. 在弹出的菜单中选择 `NSPrivacyAccessedAPIType`。
5. 确认 Type 列中的值为 `String`。
6. 在 Value 列的弹出菜单中选择一个 privacy accessed API category。
7. 确认该值与 App 或第三方 SDK 实际使用的 required reason API 类别完全一致。

## 添加 accessed API type reasons 键

`NSPrivacyAccessedAPITypeReasons` 键的结构如下：

```xml
<key>NSPrivacyAccessedAPITypeReasons</key>
<array>
    <string>NS_PRIVACY_ACCESSED_API_TYPE_REASON_VALUE</string>
    ...
</array>
```

数组中的每个 `NS_PRIVACY_ACCESSED_API_TYPE_REASON_VALUE` 字符串都表示 App 或第三方 SDK 使用某类 required reason API 的一个原因。这些原因必须与同一字典中的 `NSPrivacyAccessedAPIType` 值相匹配。

在 Xcode 中添加该键：

1. 选择对应字典。
2. 点击字典左侧的展开三角形。
3. 确认字典中已有 `NSPrivacyAccessedAPIType`，且值是正确 API 类别。
4. 点击字典旁的 Add 按钮（+）添加新项目。
5. 在弹出菜单中选择 `NSPrivacyAccessedAPITypeReasons`。
6. 确认 Type 列中的值为 `Array`。
7. 展开 `NSPrivacyAccessedAPITypeReasons`。
8. 点击该数组旁的 Add 按钮（+）添加原因。
9. 在 Value 列中选择一个原因值。
10. 确认该原因值确实适用于第 3 步中声明的 API 类别。

## 添加 API type and reasons 字典

privacy accessed API type and reasons 字典包含一类 required reason API，以及与它相关的原因列表。这个字典只应包含两个键：

- `NSPrivacyAccessedAPIType`
- `NSPrivacyAccessedAPITypeReasons`

结构如下：

```xml
<dict>
    <key>NSPrivacyAccessedAPIType</key>
    <string>NS_PRIVACY_ACCESSED_API_CATEGORY_VALUE</string>

    <key>NSPrivacyAccessedAPITypeReasons</key>
    <array>
        <string>NS_PRIVACY_ACCESSED_API_TYPE_REASON_VALUE</string>
        ...
    </array>
</dict>
```

在 Xcode 中把这个字典添加到 `NSPrivacyAccessedAPITypes`：

1. 在 Project navigator 中选择 `PrivacyInfo.xcprivacy`。
2. 在属性列表编辑器中找到 `NSPrivacyAccessedAPITypes` 键。
3. 展开 `NSPrivacyAccessedAPITypes`。
4. 点击 `NSPrivacyAccessedAPITypes` 旁的 Add 按钮（+）插入新项目。
5. 确认新项目的 Type 为 `Dictionary`。
6. 按前文步骤为该字典添加 `NSPrivacyAccessedAPIType`。
7. 按前文步骤为该字典添加 `NSPrivacyAccessedAPITypeReasons`。

## 添加 accessed API types 键

`NSPrivacyAccessedAPITypes` 是一个数组，数组中的每一项都是一个 privacy accessed API type and reasons 字典。

结构如下：

```xml
<key>NSPrivacyAccessedAPITypes</key>
<array>
    <dict>
        <key>NSPrivacyAccessedAPIType</key>
        <string>NS_PRIVACY_ACCESSED_API_CATEGORY_VALUE</string>
        <key>NSPrivacyAccessedAPITypeReasons</key>
        <array>
            <string>NS_PRIVACY_ACCESSED_API_TYPE_REASON_VALUE</string>
            ...
        </array>
    </dict>
    ...
</array>
```

在 Xcode 中添加该键：

1. 在 Project navigator 中选择 `PrivacyInfo.xcprivacy`。
2. 点击属性列表编辑器中 `App Privacy Configuration` 键旁的 Add 按钮（+）。
3. 在弹出菜单中选择 `NSPrivacyAccessedAPITypes`。
4. 确认 Type 列中的值为 `Array`。
5. 按前文步骤向数组中添加 API type and reasons 字典。

## 示例：声明磁盘空间 API

下面的示例表示名为 `Sample` 的 App 使用了 Disk Space 类 required reason API，并声明了两个原因代码：

```xml
<plist>
<dict>
    <key>NSPrivacyAccessedAPITypes</key>
    <array>
        <dict>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryDiskSpace</string>
            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>B728.1</string>
                <string>E174.1</string>
            </array>
        </dict>
    </array>
</dict>
</plist>
```

如果 App 或第三方 SDK 使用了更多 required reason API，需要为每个额外类别重复添加字典。

## 示例：同时声明 Disk Space 和 User Defaults

下面的示例在前一个基础上，又声明了 User Defaults 类 required reason API：

```xml
<plist>
<dict>
    <key>NSPrivacyAccessedAPITypes</key>
    <array>
        <dict>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryDiskSpace</string>
            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>B728.1</string>
                <string>E174.1</string>
            </array>
        </dict>
        <dict>
            <key>NSPrivacyAccessedAPIType</key>
            <string>NSPrivacyAccessedAPICategoryUserDefaults</string>
            <key>NSPrivacyAccessedAPITypeReasons</key>
            <array>
                <string>CA92.1</string>
            </array>
        </dict>
    </array>
</dict>
</plist>
```

## 修订历史

- 2024-12-17：首次发布。

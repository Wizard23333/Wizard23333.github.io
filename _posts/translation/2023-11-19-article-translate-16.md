---
title: Apple文档翻译（1）——Describing use of required reason API
author: wizard
date: 2023-11-19 17:00:00 +0800
categories: [文档翻译]
tags: [文档翻译]

---

原文地址：[链接](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_use_of_required_reason_api)

# Describing use of required reason API（描述 requiried reason 的使用）

> 确保您使用的涉及到的 API 符合政策的要求

## 概览

你的应用程序使用一些 API 来实现其核心的功能 —— 在你所编写的代码中，或者包含在第三方的 SDK 中 —— 这些 API 可能会被滥用，比如通过获取设备信息来识别用户，也就是所谓的指纹识别（fingerprinting）。无论用户是否允许你的应用跟踪用户，指纹识别都是不被允许的。因此描述你的应用程序或者三方 SDK 使用这些 API 的原因，不管它在iOS、iPadOS、tvOS、visionOS 还是 watchOS 上。同时也需要检查，检查你的应用程序或者三方 SDK 是否仅仅使用这些 API 用于预期的目的。

> 从 2023 年秋季开始，如果您上传到 App Store Connect 的应用程序使用了 required reason API，但是没有在隐私清单文件中描述原因，您将收到一封来自 Apple 的电子邮件。从 2024 年春季开始，如果应用程序在其隐私清单文件中没有对 required reason API 使用的描述，这些应用程序将不会被 App Store Connect 接受。
{: .prompt-warning }

对于在您的应用程序中或三方 SDK 中使用的每个 required reason API 类别，请您在您的应用程序或者三方 SDK 的隐私清单文件（privacy manifest file）中`NSPrivacyAccessedAPITypes` 数组中添加一个字典，这个字典用于报告您使用这类 API 的原因。如果您在您的应用程序的代码中使用这些 API ，则需要在您的应用程序的隐私清单文件中报告它们。如果您在您的三方 SDK 的代码中使用这些 API，则同样需要在你的三方 SDK 的隐私清单文件中报告它们。您的三方 SDK 不能仅仅依赖使用它的 App，或者其他的该 App 使用过的 SDK来报告你的三方SDK对 required reason API 的使用

> 你的应用程序或者三方 SDK，必须声明一个或多个被 Apple 允许的理由来准确的反映您对每个 API，以及调用这些 API 获得数据的使用。您只能将这些 API 和通过这些 API 获得的数据用于已经声明的理由。这些声明的理由必须与您应用程序呈现给用户的功能一致，并且您不得使用这些内容用于跟踪用户。
{: .prompt-warning }

`NSPrivacyAccessedAPITypes` 数组中的每个键值对都需要包含以下键和值：`NSPrivacyAccessedAPIType`

一个字符串，用于标识您的应用程序使用的 required reason API 的类别。您提供的值必须是下面各节中列出的值之一。

`NSPrivacyAccessedAPITypeReasons`

一个字符串数组，用于标识您的应用程序使用该 API 的原因。您提供的值必须是下面各节中与所访问的 API 类型相关联的值。

各种 required reason API 的类别、包含在每个类别中的 API，以及在隐私清单中可以包含的原因在下面各节中有描述。

> 请注意：Apple 将不断审查所需的 reason API 和使用原因列表，并会不定期更新本文。若您的应用程序使用的 required reason API 可以给使用该应用程序的用户某种好处，但该原因未在此列出，请[提交一个新的批准请求](https://developer.apple.com/contact/request/privacy-manifest-reason/)。
{: .prompt-info }

## File timestamp APIs

以下用于访问文件时间戳的 API 需要使用原因。请在 `NSPrivacyAccessedAPITypes` 字典中使用字符串 `NSPrivacyAccessedAPICategoryFileTimestamp` 作为 `NSPrivacyAccessedAPIType` 键的值。

* `[creationDate](https://developer.apple.com/documentation/foundation/fileattributekey/1418187-creationdate)`

* `[modificationDate](https://developer.apple.com/documentation/foundation/fileattributekey/1410058-modificationdate)`

* `[fileModificationDate](https://developer.apple.com/documentation/uikit/uidocument/1619952-filemodificationdate)`

* `[contentModificationDateKey](https://developer.apple.com/documentation/foundation/urlresourcekey/1408803-contentmodificationdatekey)`

* `[creationDateKey](https://developer.apple.com/documentation/foundation/urlresourcekey/1410073-creationdatekey)`

* `getattrlist(_:_:_:_:_:)`

* `getattrlistbulk(_:_:_:_:_:)`

* `fgetattrlist(_:_:_:_:_:)`

* `[stat](https://developer.apple.com/documentation/kernel/stat)`

* `fstat(_:_:)`

* `fstatat(_:_:_:_:)`

* `lstat(_:_:)`

* `getattrlistat(_:_:_:_:_:_:)`

在 `NSPrivacyAccessedAPITypeReasons` 数组中，提供此列表中相关值的声明。

`DDA9.1`

声明此原因是为了向使用设备的人显示文件时间戳。

以此目的来访问得到的信息或任何派生信息都不得发送到设备外。

`C617.1`

声明此原因是为了访问应用程序容器、应用程序组容器或应用程序的 CloudKit 容器中的文件的时间戳。

`3B52.1`

声明此原因是为了访问用户明确授权访问的文件或目录的时间戳，例如使用文档选择器视图控制器。如需有关创建隐私清单文件的更多信息，请参阅[创建隐私清单](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files#4284009)。

## System boot time APIs

以下访问系统启动时间的 API 需要使用原因。在您的 `NSPrivacyAccessedAPITypes` 字典中，将 `NSPrivacyAccessedAPIType` 键的值设为字符串 `NSPrivacyAccessedAPICategorySystemBootTime`。

* `[systemUptime](https://developer.apple.com/documentation/foundation/processinfo/1414553-systemuptime)`

* `mach_absolute_time()`

在您的 `NSPrivacyAccessedAPITypeReasons` 数组中，提供以下列表中相关的值。

`35F9.1`

声明此原因是为了访问系统启动时间，用于测量在应用程序内发生事件的时间间隔，或执行计算以启用计时器。

出于此原因访问得到的信息或任何派生信息不能被发送到设备之外。关于在应用程序内发生的事件之间的时间间隔，是一个例外，可以发送到设备之外。

## Disk space APIs

以下访问可用磁盘空间的 API 需要使用原因。在您的 `NSPrivacyAccessedAPITypes` 字典中，将 `NSPrivacyAccessedAPIType` 键的值设为字符串 `NSPrivacyAccessedAPICategoryDiskSpace`。

* `[volumeAvailableCapacityKey](https://developer.apple.com/documentation/foundation/urlresourcekey/1412898-volumeavailablecapacitykey)`

* `[volumeAvailableCapacityForImportantUsageKey](https://developer.apple.com/documentation/foundation/urlresourcekey/2887126-volumeavailablecapacityforimport)`

* `[volumeAvailableCapacityForOpportunisticUsageKey](https://developer.apple.com/documentation/foundation/urlresourcekey/2887125-volumeavailablecapacityforopport)`

* `[volumeTotalCapacityKey](https://developer.apple.com/documentation/foundation/urlresourcekey/1415933-volumetotalcapacitykey)`

* `[systemFreeSize](https://developer.apple.com/documentation/foundation/fileattributekey/1410126-systemfreesize)`

* `[systemSize](https://developer.apple.com/documentation/foundation/fileattributekey/1415888-systemsize)`

* `statfs(_:_:)`

* `statvfs(_:_:)`

* `fstatfs(_:_:)`

* `fstatvfs(_:_:)`

* `getattrlist(_:_:_:_:_:)`

* `fgetattrlist(_:_:_:_:_:)`

* `getattrlistat(_:_:_:_:_:_:)`

在您的 `NSPrivacyAccessedAPITypeReasons` 数组中，提供以下列表中相关的值。

`85F4.1`

声明此原因是表示为了向使用设备的人显示磁盘空间信息。磁盘空间可以显示为信息单位（例如字节）或时间单位与媒体类型的组合（例如高清视频的分钟数）。

为此目的访问的信息或任何派生信息不能被发送到设备之外。

`E174.1`

声明此原因表示为了检查是否有足够的磁盘空间来写入文件，或者检查磁盘空间是否不足，以便应用程序在磁盘空间不足时删除文件。应用程序必须根据可观察到的方式基于磁盘空间执行不同的操作。

为此目的访问的信息或任何派生信息不能被发送到设备之外。但有一个例外，为了允许应用程序在磁盘空间不足时避免从服务器下载文件。

## Active keyboard APIs

下面是访问活跃键盘列表的API，需要提供使用原因。请将字符串`NSPrivacyAccessedAPICategoryActiveKeyboards`作为`NSPrivacyAccessedAPIType`键的值，放入您的`NSPrivacyAccessedAPITypes`字典中。

* `[activeInputModes](https://developer.apple.com/documentation/uikit/uitextinputmode/1614522-activeinputmodes)`

在`NSPrivacyAccessedAPITypeReasons`数组中，提供以下列表中相关的值。

`3EC4.1`

如果您的应用是一个自定义键盘应用，并且您调用此API类别以确定设备上活跃的键盘，请声明此原因。

向用户提供系统范围的自定义键盘必须是应用的主要功能。

基于此原因访问的信息或任何派生信息不得发送到设备外。

`54BD.1`

声明此原因以访问活跃键盘信息，以向使用设备的人呈现正确的自定义用户界面。应用程序必须具有用于输入或编辑文本的文本字段，并且必须根据活跃键盘以对用户可见的方式进行不同行为。

基于此原因访问的信息或任何派生信息不得发送到设备外。

## User defaults APIs

下面是访问 user defaults 的API，需要提供使用原因。请将字符串`NSPrivacyAccessedAPICategoryUserDefaults`作为`NSPrivacyAccessedAPIType`键的值，放入您的`NSPrivacyAccessedAPITypes`字典中。

* `[UserDefaults](https://developer.apple.com/documentation/foundation/userdefaults)`

在`NSPrivacyAccessedAPITypeReasons`数组中，提供以下列表中相关的值。

`CA92.1`

声明此原因以访问用户默认设置，读取和写入仅对应用本身可访问的信息。

此原因不允许读取其他应用程序或系统写入的信息，也不允许写入其他应用程序可以访问的信息。

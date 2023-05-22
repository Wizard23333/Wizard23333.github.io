---
title: UIKit文档翻译（4）——Requesting access to protected resources
author: wizard
date: 2023-05-20 12:49:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/requesting_access_to_protected_resources)

# Requesting access to protected resources (请求访问受保护的资源)

> 请提供一个目的字符串，以向用户解释为什么您需要访问他们设备上的受保护资源。

## 概览

现代设备收集并存储了关于使用者的大量敏感信息。许多应用程序依赖于此类数据及生成它的设备硬件来执行有用的工作。例如，导航应用程序需要一个人的GPS坐标来在地图上定位该人。但是，并不是所有应用程序都需要访问所有数据。同样的导航应用程序不需要访问一个人的健康历史记录、相机接口或蓝牙外设。

确保您的应用程序只访问它需要执行其工作的内容。为了支持这一原则，苹果的操作系统默认限制对受保护数据和资源的访问。应用程序可以根据具体情况请求访问权限，同时提供为何需要访问的说明。使用应用程序的人决定是否授予或拒绝请求。

> **提示信息：**
> 
> 除了向用户请求访问资源的权限外，在某些情况下，您还需要通过向应用程序添加授权来单独声明您的意图，如[授权](https://developer.apple.com/documentation/bundleresources/entitlements)所述。
{: .prompt-info }

## 提供目的字符串

当您的应用程序首次尝试访问受保护资源时，系统会提示应用程序使用者授予权限。以下是一个示例，一个名为FoodDeliveryApp的iOS应用程序，提供食品送货服务，生成了一个请求访问用户的位置的提示：

<img title="" src="https://docs-assets.developer.apple.com/published/463f770ecd/renderedDark2x-1667327329.png" alt="" data-align="center" width="234">

如果授权被允许，系统会记住使用者的选择，并且不会再次提示。如果使用者拒绝了授权，访问尝试将以特定于资源的方式失败。对于访问位置数据的特定情况，用户可以通过点击“仅允许一次”来允许仅在一次会话中使用位置数据。

系统会自动生成提示的标题，其中包括您的应用程序的名称。您需要提供一个称为“目的字符串（*purpose string*）”或“用途说明（*usage description*）”的消息 - 在本例中是“您的位置允许您查看送餐范围内的餐厅。” - 以指示您的应用程序需要访问的原因。准确而简洁地向用户解释为什么您的应用程序需要访问敏感数据通常需要一个完整的句子，这有助于用户做出知情决策并提高他们授权的几率。

您可以通过为应用程序的[`Information Property List`](https://developer.apple.com/documentation/bundleresources/information_property_list)文件的特定资源的键（key）设置字符串值来提供目的字符串。例如，上面图像中的消息是与[`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/documentation/bundleresources/information_property_list/nslocationwheninuseusagedescription)键相关联的字符串。使用Xcode中的属性列表编辑器修改您的`Info.plist`文件。

![](https://docs-assets.developer.apple.com/published/4ec12be08e/renderedDark2x-1667327328.png)

如果您的应用程序使用受保护资源，请始终在Info.plist文件中提供有效的目的字符串。如果您没有提供有效的目的字符串，访问资源的尝试将失败，甚至可能导致您的应用程序崩溃。

如果您的应用程序支持多个语言环境，请在Info.plist文件中提供目的字符串的本地化版本，并将本地化字符串放置在每个语言环境的InfoPlist.strings文件中以进行支持。

## 遵守目的字符串的要求

为了向用户提供有用、简洁的关于为什么请求访问受保护资源的信息，请确保您提供的每个目的字符串都是有效的，检查以下内容：

* 目的字符串不为空，并且不仅由空格字符组成。

* 目的字符串长度小于4,000字节。典型的目的字符串是一句完整的句子，但您可以提供附加信息以帮助用户做出正确的有关共享个人信息的决定。

* 目的字符串具有相应键所需的正确类型，通常是字符串。

* 目的字符串提供的描述准确、有意义且具体，说明应用程序为何需要访问受保护资源。

请遵守这些要求，对`Info.plist`文件和特定语言环境的`InfoPlist.strings`文件中的每个目的字符串都要进行检查。

App Review会检查是否使用了受保护资源，并拒绝包含未经目的字符串授权的访问这些资源的代码的应用程序。例如，访问位置的应用程序可能会从App Review接收到有关需要存在[`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/documentation/bundleresources/information_property_list/nslocationwheninuseusagedescription)键的要求的以下信息：

```textile
ITMS-90683: Missing purpose string in Info.plist. 
Your app’s code references one or more APIs that access sensitive user 
data, or the app has one or more entitlements that permit such access. 
The Info.plist file for the "{app-bundle-path}" bundle should contain a 
NSLocationWhenInUseUsageDescription key with a user-facing purpose string 
explaining clearly and completely why your app needs the data.
If you’re using external libraries or SDKs, they may reference APIs that 
require a purpose string. While your app might not use these APIs, a 
purpose string is still required. For details, visit: 
https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/requesting_access_to_protected_resources.

ITMS-90683: Info.plist文件中缺少目的字符串。
您的应用程序代码引用了一个或多个访问敏感用户数据的API，或应用程序具有一个或多个允许此类访问的授权。
"{app-bundle-path}"包的Info.plist文件应包含一个NSLocationWhenInUseUsageDescription键，
其中包含一个面向用户的目的字符串，清楚而完整地解释您的应用程序为何需要这些数据。
如果您正在使用外部库或SDK，它们可能引用需要目的字符串的API。
尽管您的应用程序可能不使用这些API，但仍需要提供目的字符串。有关详细信息，请访问：
https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/requesting_access_to_protected_resources。
```

要解决此问题，请提供一个目的字符串，说明应用程序为何需要访问这些敏感信息，或删除访问该资源的代码。

> **提示：**
> 
> 如果您正在使用外部库或SDK，则它们可能引用需要目的字符串的API。尽管您的应用程序可能不使用这些API，但仍需要提供目的字符串以供App Review审核。您可以联系库或SDK的开发人员，请求有关开发人员使用的受保护资源及其目的的信息，或请求开发人员发布不包含这些API的代码版本。您负责所有受保护资源的访问，包括外部SDK和库的访问。
{: .prompt-info }

## 检查授权

许多提供访问受保护资源的系统框架都有专用的API，用于检查和请求授权以使用这些资源。该模型允许您根据当前访问权限调整应用程序的行为。例如，如果用户拒绝了您的应用程序的权限，您可以从用户界面中删除相关元素。

由于用户可以随时使用“设置”更改授权状态，请在访问受保护资源之前始终检查其特征的授权状态。在没有专用API的情况下，准备您的应用程序以优雅地处理访问失败。

## 重置授权访问

当您的应用程序在第一次尝试后再次访问受保护资源时，系统将记住用户的权限选择，不会再次提示。要再次提示用户，您需要在设备或系统上重置对这些资源的访问。

要在iOS应用程序中重置对受保护资源的权限访问，请在设备上点击“设置”>“通用”>“还原”>“重置位置与隐私”。

> **重要信息：**
> 
> 使用“重置位置与隐私”将重置您设备上所有服务的位置和隐私设置。
{: .prompt-warning }

要在 macOS 应用程序中重置特定服务的权限，请在终端中运行`tccutil reset <服务名称>`命令。例如，要重置 AppleEvents 的所有权限，请输入：

```shellsession
$ tccutil reset AppleEvents
```

这个命令会重置所有使用受保护资源的应用程序的授权访问。您也可以单独指定摄像头、日历、提醒事项或其他服务来重置它们。



---
title: UIKit文档翻译（7）——Managing your app’s life cycle
author: wizard
date: 2023-05-26 13:05:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
> {: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)

# Managing your app’s life cycle（管理您的应用程序生命周期）

> 在您的应用程序在前台或后台时响应系统通知，并处理其他重要的与系统相关的事件。

## 概览

您的应用程序的当前状态决定了它在任何时候可以或不可以做什么。例如，前台应用程序拥有用户的注意力，因此它优先使用系统资源，包括 CPU。相比之下，后台应用程序应尽可能少地工作，并且最好什么也不做，因为它不在屏幕上。随着您的应用程序从一种状态转换到另一种状态，您必须相应地调整其行为。

当您的应用程序状态发生变化时，UIKit 通过调用适当的委托对象的方法来通知您：

* 在 iOS 13 及更高版本中，使用[`UISceneDelegate`](https://developer.apple.com/documentation/uikit/uiscenedelegate)对象来响应基于场景（scene-based）的应用程序的生命周期事件。

* 在 iOS 12 及更早版本中，使用[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)对象来响应生命周期事件。

> 如果您在应用程序中启用场景支持，在 iOS 13 及更高版本中 iOS 总是使用您的场景委托对象（scene delegates）。在 iOS 12 及更早版本中，系统使用您的应用委托对象（app delegate）。
> 
> {: .prompt-info }

## 响应基于场景的生命周期事件

如果您的应用程序支持场景，则 UIKit 为每个场景提供单独的生命周期事件。场景（scene）代表在设备上运行的应用程序 UI 的一个实例。用户可以为每个应用程序创建多个场景，并单独显示和隐藏它们。由于每个场景都有自己的生命周期，因此每个场景可以处于不同的执行状态。例如，一个场景可能在前台，而其他场景可能在后台或者处于暂停状态。

> 场景支持是一项自愿选择的功能。要启用基本支持，请按照[指定您的应用程序支持的场景](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/specifying_the_scenes_your_app_supports)中所述，在您的应用程序的`Info.plist`文件中添加[`UIApplicationSceneManifest`](https://developer.apple.com/documentation/bundleresources/information_property_list/uiapplicationscenemanifest)键。

下图显示了场景的状态转换。当用户或系统请求您的应用程序的新场景时，UIKit 会创建它并将其放置在未连接的状态。用户请求的场景快速移动到前台，出现在屏幕上。系统请求的场景通常移动到后台，以便它可以处理事件。例如，系统可能在后台启动场景以处理位置事件。当用户关闭您的应用程序 UI 时，UIKit 将相关场景移动到后台状态，最终转换为暂停状态。UIKit 可以随时断开（disconnect）后台或暂停的场景以回收其资源，并将该场景返回到未连接状态。

<img title="" src="https://docs-assets.developer.apple.com/published/8e113a7266/scene-state~dark@2x.png" alt="" data-align="center" width="400">

使用场景转换来执行以下任务：

* 当 UIKit 将一个场景连接到您的应用程序时，配置您场景的初始 UI 并加载场景所需的数据。

* 当转换到前台活动状态时，配置您的 UI 并准备与用户交互。请参见[准备您的 UI 在前台运行](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_foreground)。

* 离开前台活动状态时，保存数据并使您的应用程序行为保持安静。请参见[准备您的 UI 在后台运行](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)。

* 进入后台状态时，完成关键任务，释放尽可能多的内存，并为您的应用程序快照做好准备。请参见[准备您的 UI 在后台运行](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)。

* 在场景断开连接时，清理与场景相关联的任何共享资源。

* 除了场景相关事件外，您还必须使用您的[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)对象响应应用程序的启动。有关应用程序启动时要执行的操作，请参见[响应应用程序的启动](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app)。

## 响应基于应用程序的生命周期事件

在 iOS 12 及更早版本中，以及不支持场景的应用程序中，UIKit 将所有生命周期事件传递给[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)对象。应用委托管理您应用程序的所有窗口，包括在单独屏幕上显示的窗口。因此，应用程序状态转换会影响您应用程序的整个 UI，包括外部显示器上的内容。

以下图示显示了涉及应用委托对象的状态转换。启动后，系统将应用程序置于非活动或后台状态，这取决于 UI 是否即将出现在屏幕上。当启动到前台时，系统会自动将应用程序转换为活动状态。之后，状态在活动和后台之间波动，直到应用程序终止。

<img title="" src="https://docs-assets.developer.apple.com/published/e6ac158845/app-state~dark@2x.png" alt="" data-align="center" width="360">

使用应用程序转换来执行以下任务：

* 在启动时，初始化您的应用程序的数据结构和 UI。请参见[响应应用程序的启动](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app)。

* 在激活时，完成配置您的 UI，并准备与用户交互。请参见[准备您的 UI 在前台运行](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_foreground)。

* 在停用时，保存数据并使您的应用程序行为保持安静。请参见[准备您的 UI 在后台运行](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)。

* 进入后台状态时，完成关键任务，释放尽可能多的内存，并为您的应用程序快照做好准备。请参见[准备您的 UI 在后台运行](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)。

* 在终止时，立即停止所有工作并释放任何共享资源。请参见[`applicationWillTerminate(_:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623111-applicationwillterminate)。

## 响应其他重要事件

除了处理生命周期事件之外，应用程序还必须准备好处理以下表格中列出的事件。使用您的[`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)对象来处理大多数这些事件。在某些情况下，您还可以使用通知来处理它们，从而允许您从应用程序的其他部分进行响应。

| 内存警告         | 当您的应用程序内存使用过高时会收到此事件。减少应用程序使用的内存量；请参见[响应内存警告](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle/responding_to_memory_warnings)。                                                                                                                                                                                  |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 受保护的数据可用/不可用 | 当用户锁定或解锁他们的设备时会收到此事件。请参见[`applicationProtectedDataDidBecomeAvailable(_:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623044-applicationprotecteddatadidbecom)和[`applicationProtectedDataWillBecomeUnavailable(_:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623019-applicationprotecteddatawillbeco)。 |
| Handoff 任务   | 当需要处理[`NSUserActivity`](https://developer.apple.com/documentation/foundation/nsuseractivity)对象时会收到此事件。请参见[`application(_:didUpdate:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622963-application)。                                                                                                                                |
| 时间更改         | 当发生多种不同的时间更改时，例如当电话运营商发送时间更新时会收到此事件。请参见[`applicationSignificantTimeChange(_:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622992-applicationsignificanttimechange)。                                                                                                                                                                  |
| 打开 URL       | 当您的应用程序需要打开资源时会收到此事件。请参见[`application(_:open:options:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623112-application)。                                                                                                                                                                                                              |
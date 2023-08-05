---
title: UIKit文档翻译（10）——About the app launch sequence
author: wizard
date: 2023-08-05 20:17:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app/about_the_app_launch_sequence)

# About the app launch sequence （关于应用程序启动顺序）

> 了解系统在应用程序启动时执行代码的顺序。

## 概览

启动应用程序涉及到一个复杂的步骤序列，其中大部分由系统自动处理。在启动序列中，UIKit会调用您的应用程序委托中的方法，以便您可以为用户交互准备您的应用程序，并执行任何您的应用程序要求的特定任务。以下是此启动序列的各个步骤，从应用程序启动到序列完成的时间：

![](https://docs-assets.developer.apple.com/published/d83df251b7/renderedDark2x-1635759036.png)

1. 用户或系统启动了您的应用程序，或系统预热了您的应用程序。有关预热的更多信息，请参见[准备应用程序进行预热](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app/about_the_app_launch_sequence#3894431)。
  
2. 系统执行Xcode提供的`main()`函数。
  
3. `main()`函数调用[`UIApplicationMain(_:_:_:_:)`](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)，创建`UIApplication`和您的应用程序委托的实例。
  
4. UIKit加载默认的故事板，该故事板在应用程序的`Info.plist`文件或Xcode项目编辑器的目标的Custom iOS Target Properties选项卡中指定；不使用默认故事板的应用程序将跳过此步骤。
  
5. UIKit调用您的应用程序委托中的[`application(_:willFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623032-application)方法。
  
6. UIKit执行状态恢复（State Restoration），从而导致在您的应用程序委托和应用程序的视图控制器中执行其他方法。有关更多信息，请参见[关于UI恢复过程](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches/about_the_ui_restoration_process)。
  
7. UIKit调用您的应用程序委托中的[`application(_:didFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application)方法。
  

启动序列完成后，系统将使用您的应用程序或场景委托（app or scene delegates）来显示您的应用程序的用户界面并管理其生命周期。

## 做好预热的准备

在iOS 15及更高版本中，根据设备状况，系统可能会*预热*您的应用程序，即启动非运行的应用程序进程以减少用户等待应用程序可用的时间。预热会执行应用程序的启动序列，直到`main()`调用`UIApplicationMain(_:_:_:_:)`之前，但不包括此步骤。这为系统提供了构建和缓存任何低级结构的机会，以预期完全启动。

> 有关系统在应用程序启动期间所需的低级结构的更多信息，请参见WWDC会议视频 [App Startup Time: Past, Present, and Future](https://developer.apple.com/videos/play/wwdc2017/413)。
{: .prompt-info }

系统在预热应用程序后，其启动序列将保持暂停状态，直到应用程序启动并序列恢复，或系统删除预热的应用程序用于回收资源。系统可以在设备重新启动后和系统条件允许的情况下周期性地预热您的应用程序。

如果您的应用程序在调用 `UIApplicationMain(_:_:_:_:)` 之前执行代码，例如在静态初始化器中，比如 `load()`，请不要做出有关可用服务和资源的假定（注：这些服务可能无法正常使用）。例如，钥匙串可能无法使用，因为它们的数据保护策略要求解锁设备，而预热即使在设备处于锁定状态时也会发生。如果您的代码依赖于访问特定的服务或资源，请将该代码迁移到启动序列的较后阶段。

预热应用程序会导致在 预热阶段完成 和 用户或系统完全启动应用程序 之间存在不确定的时间（原文：Prewarming an app results in an indeterminate amount of time between when the prewarming phase completes and when the user, or system, fully launches the app.）。因此，使用 [MetricKit](https://developer.apple.com/documentation/metrickit) 来准确地测量用户驱动的启动和恢复时间，而不是手动标记启动序列的各个点。


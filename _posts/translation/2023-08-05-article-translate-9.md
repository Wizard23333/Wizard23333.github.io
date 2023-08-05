---
title: UIKit文档翻译（9）——Responding to the launch of your app
author: wizard
date: 2023-08-05 17:10:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle/responding_to_memory_warnings)

# Responding to the launch of your app（响应应用程序的启动）

> 初始化应用程序的数据结构，准备应用程序运行，并响应系统的任何启动请求。

## 概览

当用户在主屏幕上点击您的应用程序图标时，系统会启动您的应用程序。如果您的应用程序请求了特定的事件，系统也可能在后台启动您的应用程序来处理这些事件。对于基于场景（scene）的应用程序，当您的场景需要出现在屏幕上或执行一些工作时，系统同样会启动应用程序。

所有应用程序都有一个关联的进程，由 [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication) 对象表示。应用程序还有一个 *应用程序委托对象（app delegate object）* - 一个符合 [`UIApplicationDelegate`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate) 协议的对象 - 用于响应该进程中发生的重要事件。即使是基于场景的应用程序也使用应用程序委托（app delegate）来管理诸如启动和终止等基本事件。在启动时，UIKit自动创建 `UIApplication` 对象和您的应用程序委托对象。然后它启动您的应用程序的主事件循环（main event loop）。

## 提供 Launch StoryBoard

当用户第一次在设备上启动您的应用程序时，系统会显示您的启动画面故事板（launch storyboard），直到您的应用程序准备好显示其用户界面为止。显示启动画面故事板可以确保用户知道您的应用程序已启动并正在执行某些操作。如果您的应用程序快速地初始化并准备好其用户界面，用户可能只会短暂地看到您的启动画面故事板。

Xcode项目会自动为您包含一个默认的启动画面故事板，您可以对其进行自定义，并根据需要添加更多的启动画面故事板。要向项目添加新的启动画面故事板，请执行以下操作：

1. 在Xcode中打开您的项目。
2. 选择文件 > 新建 > 文件。
3. 向您的项目添加启动画面资源。

向您的启动画面故事板（launch storyboard）添加视图，并使用自动布局约束来调整它们的大小和位置，使它们适应底层环境。UIKit会根据您提供的内容精确显示，并使用您的约束将您的视图适应可用空间。有关设计指南，请参见[人机界面指南](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/launch-screen)。

> 不要使用静态图像作为您的启动画面。在iOS 14及更高版本中，启动画面的大小限制为25 MB。
{: .prompt-warning }

## 初始化应用程序的数据结构

将应用程序的启动初始化代码放在以下方法中的一个或两个中：

* `application(_:willFinishLaunchingWithOptions:)`
* `application(_:didFinishLaunchingWithOptions:)`

UIKit在应用程序启动周期的开始调用这些方法。使用它们来：

* 初始化应用程序的数据结构。
* 验证应用程序具有运行所需的资源。
* 当您的应用程序首次启动时，执行任何一次性设置。例如，在可写目录中安装模板或用户可修改文件。请参见[为您的应用程序执行一次性设置](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app/performing_one-time_setup_for_your_app)。
* 连接应用程序使用的任何关键服务。例如，如果您的应用程序支持远程通知，则连接到Apple Push Notification服务。
* 检查启动选项字典以获取有关应用程序为何启动的信息。请参见[确定应用程序启动的原因](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app#2922740)。

对于非基于场景（scene-based）的应用程序，在该界面出现在屏幕上之前，使用 `application(_:didFinishLaunchingWithOptions:)` 方法对该界面进行其他更改。例如，您可以安装一个不同的视图控制器来反映用户上一次使用应用程序时正在进行的操作。

## 将耗时的任务移出主线程

当用户使用您的应用程序时，启动快速会给用户留下好的印象。UIKit在[`application(_:didFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application)方法返回后才呈现应用程序的界面。在该方法或[`application(_:willFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623032-application)方法中执行耗时的任务可能会使您的应用程序对用户显示迟缓。在后台时快速返回也很重要，因为系统限制了应用程序的后台执行时间。

将不是应用程序初始化关键的任务移出启动时间序列。例如：

* 推迟初始化应用程序暂时不需要的功能。
* 将重要的长时间运行任务从应用程序的主线程中移出。例如，在全局调度队列上异步运行它们。

## 确定应用程序启动的原因

当UIKit启动您的应用程序时，它会向您的[`application(_:willFinishLaunchingWithOptions:)` ](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623032-application)和[`application(_:didFinishLaunchingWithOptions:)` ](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application)方法传递一个启动选项字典，其中包含有关您的应用程序启动原因的信息。该字典中的键表示需要立即执行的重要任务。例如，它们可能反映了用户在其他地方启动，并希望在您的应用程序中继续执行的操作。请始终检查启动选项字典的内容以查找您预期的键，并根据其出现情况做出适当的响应。

> 对于基于场景（scene-based）的应用程序，请检查UIKit传递给`application(_:configurationForConnecting:options:)`方法的选项，以确定它创建场景的原因。
{: .prompt-info }

以下代码显示了处理后台位置更新的应用程序委托方法。当位置键存在时，该应用程序立即启动位置更新，而不是将其推迟到以后。启动位置更新允许Core Location框架传递新的位置事件。

```swift
class AppDelegate: UIResponder, UIApplicationDelegate, 
               CLLocationManagerDelegate {

   let locationManager = CLLocationManager()
   func application(_ application: UIApplication,
              didFinishLaunchingWithOptions launchOptions:
              [UIApplicationLaunchOptionsKey: Any]?) -> Bool {

      // 如果是因为新的位置数据而启动，则立即启动位置服务
      if let keys = launchOptions?.keys {
         if keys.contains(.location) {
            locationManager.delegate = self
            locationManager.startMonitoringVisits()
         }
      }

      return true
   }
   // 其他方法...
}
```

除非您的应用程序支持相应的功能，否则系统不会包含对应的键。例如，对于不支持远程通知的应用程序，系统不会包含[`remoteNotification`](https://developer.apple.com/documentation/uikit/uiapplication/launchoptionskey/1622967-remotenotification)键。

有关启动选项键的列表以及如何处理它们的信息，请参见[`UIApplication.LaunchOptionsKey`](https://developer.apple.com/documentation/uikit/uiapplication/launchoptionskey)。

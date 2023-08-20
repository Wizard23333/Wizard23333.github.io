---
title: UIKit文档翻译（13）——About the UI preservation process
author: wizard
date: 2023-08-20 17:46:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches/about_the_ui_preservation_process)

# About the UI preservation process（关于UI保留过程）

> 了解如何自定义UIKit的状态保留过程

## 概览

下面的图表展示了界面保留过程中发生的调用顺序。在询问应用程序委托是否要保留应用程序状态后，UIKit会对当前应用程序的视图控制器层次结构中的对象进行编码。只有具有有效 [`restorationIdentifier`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621499-restorationidentifier)的视图控制器才会被保留。

![](https://docs-assets.developer.apple.com/published/8041cdac90/af587d2a-5d12-4a16-826b-4a7e2d398dae.png)

保留过程会遍历您的视图控制器层次结构，并递归地对找到的对象进行编码。该过程从应用程序窗口的根视图控制器开始，将他们的数据写入提供的存档中。如果根视图控制器的数据包含对其他视图控制器的引用，UIKit会要求每个新视图控制器在存档的不同部分中编码其数据。这些子视图控制器可能会进一步编码其自己的子视图控制器，以此类推。

UIKit视图控制器会自动对其子视图控制器进行适当的编码。如果您定义了一个自定义容器视图控制器，您的视图控制器的 [encodeRestorableState(with:)](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches/about_the_ui_preservation_process#:~:text=your%20view%20controller%E2%80%99s-,encodeRestorableState(with%3A),-method%20must%20similarly) 方法必须类似地将任何子视图控制器写入提供的存档中。

## 排除不需要保留的视图控制器

有两种方法可以排除视图控制器（及其视图）不参与状态恢复过程：

* 将其 [`restorationIdentifier`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621499-restorationidentifier) 属性设置为 `nil`。
  
* 提供一个恢复类，并在 [`viewController(withRestorationIdentifierPath:coder:)`](https://developer.apple.com/documentation/uikit/uiviewcontrollerrestoration/1616859-viewcontroller) 方法返回 `nil`。
  

排除视图控制器将阻止该视图控制器被保存在存档（archive）中。它还会排除该视图控制器的子视图控制器不进行保留。

## 在您的应用程序中对任何对象进行编码

状态恢复不仅限于您的应用程序的视图和视图控制器。任何采用 [`UIStateRestoring`](https://developer.apple.com/documentation/uikit/uistaterestoring) 协议的对象也可以包含在恢复存档中。例如，您可以在存储应用程序的全局配置数据的对象中采用此协议。要将这样的对象添加到存档中，请按照以下步骤进行操作：

1. 在应用程序运行时，通过调用 `UIApplication` 的 [`registerObject(forStateRestoration:restorationIdentifier:)`](https://developer.apple.com/documentation/uikit/uiapplication/1623027-registerobject) 方法来注册对象。例如，您可以在创建配置对象后立即注册它。
  
2. 在您的 [`encodeRestorableState(with:)`](https://developer.apple.com/documentation/uikit/uistaterestoring/1616866-encoderestorablestate) 方法之一中，将对象编码到恢复存档中。您还可以在应用程序委托的 [`application(_:willEncodeRestorableStateWith:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623099-application) 方法中对其进行编码。
  

您可以对自定义对象编码任何您想要的数据，只要这些数据足以在下一次启动周期中将该对象恢复到先前的状态。编码那些对您的应用程序行为并不关键的数据，绝不要编码应以其他方式持久化的数据。例如，不要对应用程序的设置和用户数据进行编码。
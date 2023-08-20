---
title: UIKit文档翻译（14）——About the UI restoration process
author: wizard
date: 2023-08-20 21:03:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches/about_the_ui_restoration_process)

# About the UI restoration process（关于 UI 恢复过程）

> 了解如何自定义 UIKit 的状态恢复过程

## 概览

以下图表显示了从应用程序启动到恢复完成这一时间段的函数调用顺序。恢复过程发生在应用程序初始化的中间阶段，并且只在存在*状态恢复存档*而且应用程序委托的 [`application(_:shouldRestoreApplicationState:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622987-application) 方法返回 `true` 时才进行。

![](https://docs-assets.developer.apple.com/published/9fd35a9f67/92b434a7-dc7e-479c-b668-dbb70bb1dd2e.png)

恢复过程的第一步是创建界面的视图控制器对象（显式或隐式）。第二步是解码和恢复这些对象的状态。这两个步骤都是为了重新创建视图控制器的层级结构。例如，在创建导航控制器及其子视图控制器之后，这些对象之间并没有直接的连接。实际上，是导航控制器的 [`decodeRestorableState(with:)`](https://developer.apple.com/documentation/uikit/uistaterestoring/1616854-decoderestorablestate) 方法重新建立了与其子视图控制器的关系。

在状态恢复完成后，UIKit 调用应用程序委托的 [`application(_:didFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application) 方法。您可以使用该方法对界面进行最后一刻的更改或添加。例如，您可以将登录屏幕添加到视图控制器层级中。

## 重新创建您的视图控制器

在恢复过程中，UIKit 会尝试从保留的界面创建或定位视图控制器对象。UIKit 首先要求您提供视图控制器对象。如果您没有提供视图控制器，UIKit 将隐式地寻找它。以下是 UIKit 用于重新创建视图控制器的步骤顺序：

1. **询问视图控制器的恢复类。** 恢复类知道如何创建特定的视图控制器。通过给视图控制器的 [`restorationClass`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621472-restorationclass) 属性赋值，您可以指定一个恢复类。在恢复过程中，UIKit 调用恢复类的 [`viewController(withRestorationIdentifierPath:coder:)`](https://developer.apple.com/documentation/uikit/uiviewcontrollerrestoration/1616859-viewcontroller) 方法来请求视图控制器的新实例，然后您的方法返回该实例。如果您返回 `nil`，UIKit 将停止尝试创建该视图控制器，并排除它在恢复过程之外。
  
2. **询问应用程序委托。** 如果视图控制器没有恢复类，UIKit 将调用应用程序委托的 [`application(_:viewControllerWithRestorationIdentifierPath:coder:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623062-application) 方法。如果您从该方法返回 `nil`，UIKit 将继续搜索。
  
3. **检查现有对象。** UIKit 寻找已经创建的具有完全相同恢复路径的视图控制器。
  
4. **从故事板实例化视图控制器。** 如果仍然没有找到视图控制器，UIKit 将自动从您的应用的故事板中实例化它。
  

在状态恢复之前，UIKit会从您的故事板加载应用程序的默认视图控制器。由于UIKit自动加载这些视图控制器，最好不要使用恢复类或应用程序委托来创建它们。对于其他所有视图控制器，只有在故事板中未定义视图控制器时才分配恢复类。您还可以分配恢复类以防止在特定情况下创建视图控制器。例如，如果相关的恢复存档引用陈旧或丢失的数据，您可能希望避免显示该视图控制器。

在代码中重新创建视图控制器时，除了其他初始化操作之外，需要始终重新为视图控制器的 [`restorationIdentifier`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621499-restorationidentifier) 属性赋值。根据需要，还要为 [`restorationClass`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621472-restorationclass) 属性赋值。在创建时分配这些值可以确保视图控制器在下一个周期中得到保留。

```swift
func viewController(withRestorationIdentifierPath 
                    identifierComponents: [Any], 
                    coder: NSCoder) -> UIViewController? {
   let vc = MyViewController()

   vc.restorationIdentifier = identifierComponents.last as? String
   vc.restorationClass = MyViewController.self

   return vc
}
```

> 您的恢复类应始终返回UIKit所期望的类。恢复存档包括每个保留视图控制器的类。如果您的恢复类返回一个不同类的实例，UIKit 将不会调用该视图控制器的 [`decodeRestorableState(with:)`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621429-decoderestorablestate) 方法。
{: .prompt-info }
---
title: UIKit文档翻译（6）——App and environment
author: wizard
date: 2023-05-25 14:26:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/app_and_environment)

# App and environment（应用程序和环境）

> 管理应用程序的生命周期事件和UI场景（scenes），并获取有关特征和应用程序运行环境的信息。

## 概览

在iOS 13及以上版本中，用户可以同时创建和管理多个应用程序用户界面实例，并使用应用程序切换器在它们之间进行切换。在iPad上，用户还可以并排显示多个应用程序实例。每个UI实例显示不同的内容，或者以不同的方式显示相同的内容。例如，用户可以显示一个展示特定日期的日历应用程序实例，另一个则显示整个月份。

UIKit使用特征集合（`trait collections`）来传达有关当前环境的详细信息，这些特征集合反映了设备设置、界面设置和用户偏好的组合。例如，可以使用特征集合来检测当前视图或视图控制器是否启用了深色模式（Dark Mode）。当您想要基于当前环境自定义[`UIView`](https://developer.apple.com/documentation/uikit/uiview)或[`UIViewController`](https://developer.apple.com/documentation/uikit/uiviewcontroller)对象的内容时，请查阅其当前的特征集合。当您想要使其他对象接收特征通知更改时，请在这些对象中采用[`UITraitEnvironment`](https://developer.apple.com/documentation/uikit/uitraitenvironment)协议。



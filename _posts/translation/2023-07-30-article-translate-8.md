---
title: UIKit文档翻译（8）——Responding to memory warnings
author: wizard
date: 2023-07-30 11:56:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle/responding_to_memory_warnings)

# Responding to memory warnings（响应内存警告）

> 在系统要求释放内存时，及时释放内存。

## 概览

如果系统的可用内存不足，并且无法通过终止挂起的应用程序来回收内存，UIKit会向正在运行的应用程序发送低内存警告。UIKit通过以下方式传递低内存警告：

* 调用应用程序委托的 [`applicationDidReceiveMemoryWarning(_:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623063-applicationdidreceivememorywarni) 方法。

* 调用任何活动的 [`UIViewController`](https://developer.apple.com/documentation/uikit/uiviewcontroller) 类的 [`didReceiveMemoryWarning()`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621409-didreceivememorywarning) 方法。

* 向任何已注册观察者发布一个 [`didReceiveMemoryWarningNotification`](https://developer.apple.com/documentation/uikit/uiapplication/1622920-didreceivememorywarningnotificat) 对象。

* 向类型为 [`DISPATCH_SOURCE_TYPE_MEMORYPRESSURE`](https://developer.apple.com/documentation/dispatch/dispatch_source_type_memorypressure) 的调度队列发送警告。

当应用程序收到低内存警告时，尽可能快速地释放尽可能多的内存。删除对图片、媒体文件或任何已经有磁盘表示并且可以稍后重新加载的大型数据文件的引用。删除对任何不再需要的临时对象的引用。如果活动任务可能消耗大量内存，请暂停调度队列或限制应用程序执行的同时操作数量。

> **重要信息：**
> 
> 如果未能减少应用程序的内存使用可能会导致应用程序被终止。因此，考虑将任何未保存的数据写入磁盘作为清理工作的一部分。
{: .prompt-warning }

为了测试应用程序对低内存警告的响应，请在iOS模拟器中使用“模拟内存警告”命令。
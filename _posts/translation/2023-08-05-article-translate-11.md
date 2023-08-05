---
title: UIKit文档翻译（11）——Performing one-time setup for your app
author: wizard
date: 2023-08-05 20:45:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app/performing_one-time_setup_for_your_app)

# Performing one-time setup for your app（为您的应用程序执行一次性设置）

> 确保您的应用程序环境配置正确

## 概览

当用户首次启动您的应用程序时，您可能希望通过执行一些一次性任务来准备应用程序环境。例如，您可能希望：

* 从服务器下载所需的数据。
  
* 将文档模板或可修改数据文件从应用程序包（app bundle）复制到可写目录。
  
* 为用户配置默认偏好设置。
  
* 设置用户帐户或收集其他所需数据。
  

在您的应用程序委托的 [`application(_:willFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623032-application) 或 [`application(_:didFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application) 方法中执行任何一次性任务。不要阻塞应用程序的主线程用于不需要用户输入的任务。相反地，使用调度队列异步地启动任务，并在应用程序完成启动时让它们在后台运行。对于需要用户输入的任务，请在 `application(_:didFinishLaunchingWithOptions:)` 方法中对用户界面进行所有更改。

## 将文件安装在适当的位置

您的应用程序有自己的用于存储文件的容器目录，您应该始终将应用程序特定的文件放置在 `~/Library` 子目录中。具体来说，将文件存储在以下 `~/Library` 子目录中：

* `~/Library/Application Support/` - 存储您希望与用户的其他内容一起备份的特定应用程序的文件。 (如有需要，您可以在此处创建自定义子目录。) 使用此目录存储数据文件、配置文件、文档模板等等。
  
* `~/Library/Caches/` - 存储可以轻松重新生成或下载的临时数据文件。
  

要获取应用程序容器中一个目录的URL，请使用 [`FileManager`](https://developer.apple.com/documentation/foundation/filemanager) 的 [`urls(for:in:)`](https://developer.apple.com/documentation/foundation/filemanager/1407726-urls) 方法。

**代码清单1** 获取特定应用程序目录的位置

```swift
let appSupportURL = FileManager.default.urls(for: 
      .applicationSupportDirectory, in: .userDomainMask)


let cachesURL = FileManager.default.urls(for: 
      .cachesDirectory, in: .userDomainMask)
```

将任何临时文件放置在您的应用程序的 `tmp/` 目录中。 临时文件可能包括您打算在其内容被提取和安装到其他地方后删除的压缩文件。 使用 [`FileManager`](https://developer.apple.com/documentation/foundation/filemanager) 的 [`temporaryDirectory`](https://developer.apple.com/documentation/foundation/filemanager/1642996-temporarydirectory) 方法检索应用程序临时目录的URL。

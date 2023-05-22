---
title: UIKit文档翻译（5）——Encrypting Your App’s Files
author: wizard
date: 2023-05-22 18:25:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/encrypting_your_app_s_files)

# Encrypting Your App’s Files (加密应用程序文件)

> 通过磁盘加密来保护iOS中的用户数据。

## 概览

数据保护（Data protection）是iOS的一项功能，您可以使用它来保护您应用程序的文件并防止未经授权的访问。当用户为设备设置了有效的密码时，数据保护会自动启用。您可以正常读写文件，但系统会在幕后加密和解密您的内容。加密和解密过程自动进行且使用硬件加速。

您可以指定应用于每个文件的数据保护级别。有四个级别可用，每个级别确定何时可以访问该文件。如果您在创建文件时未指定保护级别，则iOS会自动应用默认保护级别。

* **无保护**。文件始终可访问。

* **第一次用户身份验证前完全保护**（默认）。文件在用户首次解锁设备之前无法访问。在设备首次解锁后，文件会保持可访问状态，直到设备关闭（shuts down）或重新启动（reboots）。

* **除非打开否则完全保护**。只有在设备解锁时才能打开现有文件。如果您已经打开了文件，则即使用户锁定设备，您仍然可以继续访问该文件。您还可以创建新文件并在设备锁定或解锁时访问它们。

* **完全保护**。文件只有在设备解锁时才能访问。

要在一个步骤中创建和加密新文件，请使用文件内容构造数据对象并调用[`write(to:options:)`](https://developer.apple.com/documentation/foundation/data/1779858-write)方法。调用该方法时，请指定要应用于文件的数据保护选项。[清单1](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/encrypting_your_app_s_files#2928956)显示了如何将[`Data`](https://developer.apple.com/documentation/foundation/data)实例的内容写入文件并使用**完全保护**级别加密的示例。

**清单1：第一次写入加密文件**

```swift
do {
   try data.write(to: fileURL, options: .completeFileProtection)
} catch {
   // 处理错误。
}
```

要更改现有文件的数据保护级别，请使用[`NSURL`](https://developer.apple.com/documentation/foundation/nsurl)的[`setResourceValue(_:forKey:)`](https://developer.apple.com/documentation/foundation/nsurl/1413819-setresourcevalue)方法。调用该方法时，请将新的数据保护级别赋值给[`fileProtectionKey`](https://developer.apple.com/documentation/foundation/urlresourcekey/1616246-fileprotectionkey)资源键。清单2显示了将此键添加到现有文件的示例。

**清单2：加密现有磁盘上的文件**

```swift
do {
   try (fileURL as NSURL).setResourceValue( 
                  URLFileProtection.complete,
                  forKey: .fileProtectionKey)
}
catch {
   // Handle errors.
}
```

## 管理加密文件的访问

根据文件的保护级别，在用户随后锁定设备时尝试读取或写入其内容可能会失败。为确保您的应用程序能够访问文件，请执行以下操作：

* 根据您的需要选择正确的数据保护级别。

* 使用应用程序委托(app delegate)的[`applicationProtectedDataWillBecomeUnavailable(_:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623019-applicationprotecteddatawillbeco)和[`applicationProtectedDataDidBecomeAvailable(_:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623044-applicationprotecteddatadidbecom)方法，使用[`completeFileProtection`](https://developer.apple.com/documentation/foundation/nsdata/writingoptions/1617198-completefileprotection)级别关闭和重新打开文件。

将完全保护级别分配给您的应用程序仅在前台访问的文件。如果您的应用程序支持后台功能（例如处理位置更新），则为您可能在后台访问的文件分配不同的保护级别。例如，健身应用程序可能会在使用后台记录位置事件的文件上使用**除非打开否则完全保护（complete unless open）** 级别。

包含有关用户的个人信息或由用户直接创建的文件始终需要最强的保护级别。为用户数据文件分配完全保护级别，并使用应用程序委托方法（app delegate methods）管理对这些文件的访问。应用程序委托方法让您有时间在文件变得无法访问您的应用程序之前关闭它们。

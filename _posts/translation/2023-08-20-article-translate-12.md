---
title: UIKit文档翻译（12）——Preserving your app’s UI across launches
author: wizard
date: 2023-08-20 17:12:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches)

# Preserving your app’s UI across launches（在跨多次启动时保留您的应用程序用户界面的状态）

> 在系统终止您的应用程序后，将应用程序恢复到之前的状态

## 概览

保留您的应用程序用户界面有助于维持您的应用程序始终在运行的错觉。在 iOS 设备上，中断可能经常发生，长时间的中断可能会导致系统终止您的应用程序以释放资源。但是，用户不知道您的应用程序已经终止，并且不希望您的应用程序的状态发生改变。相反，他们希望您的应用程序处于他们离开时的相同状态。状态保留和恢复能确保您的应用程序在下次启动时返回到先前的状态。

在适当的时间，UIKit 将您的应用程序视图和视图控制器的状态保留到加密的磁盘文件中。当您的应用程序被终止并在以后重新启动时，UIKit 从保留的数据重建您的视图和视图控制器。保留和恢复过程是自动启动的，但您还必须进行一些特定的工作以支持这些过程：

* 启用状态保留和恢复支持。
  
* 为要保留的视图控制器分配恢复标识符。
  
* 在恢复时根据需要重新创建视图控制器。
  
* 对恢复视图控制器到先前状态所需要的自定义数据进行编码和解码。
  

如果您完全在故事板（storyboard）中定义界面，则 UIKit 知道如何重新创建您的视图控制器，并自动执行此操作。如果您不使用故事板，或者如果您想更好地控制视图控制器的创建和初始化，则可以自己创建它们。

有关状态保留和恢复的示例，请参见 [保留应用程序状态](https://developer.apple.com/documentation/uikit/uiscenedelegate/restoring_your_app_s_state)。

## 启用应用程序的状态保留和恢复

通过实现应用程序委托（app delegate）的 [`application(_:shouldSaveSecureApplicationState:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/3395862-application) 和 [`application(_:shouldRestoreSecureApplicationState:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/3395861-application) 方法，您可以选择启用状态保留和恢复。这两个方法都返回一个布尔值，表示是否应发生相关的的过程，在大多数情况下，您只需返回 `true`。但是，在不合适去恢复应用程序界面的情况下，您可以返回 `false`。

当 UIKit 调用您的 `application(_:shouldSaveSecureApplicationState:)` 方法时，您除了返回 `true` 之外还可以保存数据。您可能会希望保存在恢复过程中需要使用到的数据。例如，以下代码显示了一个示例，该示例保存应用程序的当前版本号。在恢复过程中，`application(_:shouldRestoreSecureApplicationState:)` 方法检查归档中的版本号，并在不匹配预期版本的情况下防止恢复发生。

```swift
func application(_ application: UIApplication, 
            shouldSaveSecureApplicationState coder: NSCoder) -> Bool {
   // 将当前应用程序版本保存到归档中。
   coder.encode(11.0, forKey: "MyAppVersion")

   // 总是保存状态信息。
   return true
}

func application(_ application: UIApplication, 
            shouldRestoreSecureApplicationState coder: NSCoder) -> Bool {
   // 仅在应用程序版本匹配时恢复状态。
   let version = coder.decodeFloat(forKey: "MyAppVersion")
   if version == 11.0 {
      return true
   }

   // 不要从旧数据恢复。
   return false
}
```

如果您阻止了恢复操作，您仍然可以在应用程序委托的[`application(_:didFinishLaunchingWithOptions:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application)方法中手动配置您的应用程序界面。

## 为您的视图控制器分配恢复标识符

通过为视图控制器分配恢复标识符，您可以明确告诉 UIKit 要保留哪些视图控制器。恢复标识符是您通过编程或在 Interface Builder 中分配给视图控制器的唯一字符串。视图控制器（VC）类的名称通常是适合的恢复标识符，但您也可以使用任何字符串。将该字符串添加到您的故事板文件中的视图控制器中，或在运行时将其分配给视图控制器的 [`restorationIdentifier`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621499-restorationidentifier) 属性。

![](https://docs-assets.developer.apple.com/published/fadf4e4180/850766c1-2d64-4f20-95bd-fd5f098f37a8.png)

在保存状态时，UIKit 会尝试保留您应用程序窗口的根视图控制器（root view controllers）。对于具有恢复标识符的每个根视图控制器，UIKit 都会要求该视图控制器将其自定义数据编码到存档中。容器视图控制器（container view controller）可以将其子视图控制器的引用作为其自定义数据的一部分进行编码。如果这样做，并且这些视图控制器也具有恢复标识符，则 UIKit 将尝试保留子视图控制器及其内容。这个过程会递归地继续下去，跟随一个视图控制器到下一个视图控制器的连接，直到所有视图控制器都被保存或忽略。

您不需要为每个视图控制器分配恢复标识符。实际上，有时您可能不希望保留所有视图控制器。例如，如果您的应用程序显示一个临时登录屏幕，则可能不希望保留该屏幕。相反，您会在恢复时确定是否显示它。在这种情况下，您不会为登录屏幕的视图控制器分配恢复标识符。

有关被保留的详细信息，请参见 [关于 UI 保留过程](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches/about_the_ui_preservation_process)。

## 编码和解码自定义内容

在保存状态过程中，UIKit 会调用每个被保存的视图和视图控制器的 [`encodeRestorableState(with:)`](https://developer.apple.com/documentation/uikit/uistaterestoring/1616866-encoderestorablestate) 方法。使用此方法来保留您需要返回视图或视图控制器到其当前状态所需的信息。

* **要** 保存有关视图和控件视觉状态的详细信息。
  
* **要** 保存您也想要保留的子视图控制器的引用。
  
* **要** 保存可以丢弃而不影响用户数据的信息。
  
* **不要** 包含已经存在于您应用程序的持久存储中的数据。相反，包括一个您可以稍后用于定位该数据的标识符。
  

状态保留不能替代将您的应用程序数据保存到磁盘。UIKit 可以自行决定丢弃状态保留数据，使您的应用程序返回到其默认状态。使用保留过程来存储有关您应用程序的用户界面状态的信息，例如表格的当前选定行。不要使用它来存储该表格中包含的数据。

以下代码显示了一个视图控制器示例，其中包含用于收集名字和姓氏的文本字段。如果其中一个文本字段包含未保存的值，则该方法会保存未保存的值和包含该值的text field的标识符。在这种情况下，未保存的值不是应用程序持久数据的一部分；它是一个临时值，如果需要，可以丢弃。

```swift
override func encodeRestorableState(with coder: NSCoder) {
   super.encodeRestorableState(with: coder)

   // Save the user ID so that we can load that user later.
   coder.encode(userID, forKey: "UserID")


   // Write out any temporary data if editing is in progress.
   if firstNameField!.isFirstResponder {
      coder.encode(firstNameField?.text, forKey: "EditedText")
      coder.encode(Int32(1), forKey: "EditField")
   }
   else if lastNameField!.isFirstResponder {
      coder.encode(lastNameField?.text, forKey: "EditedText")
      coder.encode(Int32(2), forKey: "EditField")
   }
   else {
      // No editing was in progress.
      coder.encode(Int32(0), forKey: "EditField")
   }
}
```

有关 UIKit 如何保留您应用程序的视图、视图控制器和状态信息的更多信息，请参见 [关于 UI 保留过程](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches/about_the_ui_preservation_process)。

## 响应并创建视图控制器

如果在启动应用程序时保留的状态信息是可用的，则系统会尝试使用保留的数据恢复应用程序的界面。

1. UIKit 调用您的应用程序委托的 [`application(_:shouldRestoreSecureApplicationState:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/3395861-application) 方法来确定是否应该进行恢复。
  
2. UIKit 使用您的应用程序的 storyboard 来重新创建您的视图控制器。
  
3. UIKit 调用每个视图控制器的 [`decodeRestorableState(with:)`](https://developer.apple.com/documentation/uikit/uistaterestoring/1616854-decoderestorablestate) 方法来恢复其状态信息。
  

UIKit 最初从 storyboard 中加载视图控制器及其视图（views）。在这些对象被加载和初始化后，UIKit 开始恢复它们的状态信息。使用 `decodeRestorableState(with:)` 方法将您的视图控制器返回到其先前的状态。

以下代码展示了解码先前示例中被编码的状态信息的过程。该方法从保留的用户 ID 中恢复视图控制器的数据。如果正在编辑文本字段，则该方法还将正在进行的值恢复并使相应的文本字段成为第一响应者（FirstResponder），从而为该文本字段显示键盘。

```swift
override func decodeRestorableState(with coder: NSCoder) {
   super.decodeRestorableState(with: coder)

   // Restore the first name and last name from the user ID.
   let identifier = coder.decodeObject(forKey: "UserID") as! String
   setUserID(identifier: identifier)

   // Restore an in-progress values that was not saved.
   let activeField = coder.decodeInteger(forKey: "EditField")
   let editedText = coder.decodeObject(forKey: "EditedText") as! String?

   switch activeField {
      case 1:
         firstNameField?.text = editedText
         firstNameField?.becomeFirstResponder()
         break

      case 2:
         lastNameField?.text = editedText
         lastNameField?.becomeFirstResponder()
         break

     default:
         break  // Do nothing.
  }
}
```

在故事板（storyboard）中定义您的视图控制器是管理状态恢复的最简单方法，但并不是唯一的方法。有关其他重新创建视图控制器的方法的更多信息，请参见[关于 UI 恢复过程](https://developer.apple.com/documentation/uikit/view_controllers/preserving_your_app_s_ui_across_launches/about_the_ui_restoration_process)。


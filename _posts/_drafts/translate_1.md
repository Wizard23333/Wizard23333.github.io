# UIKit

> 用于构建和管理基于图形化、事件驱动的用户界面，用于您的iOS、iPadOS或tvOS应用程序。

## 概览

UIKit提供了各种功能来构建应用程序，包括用于构建iOS、iPadOS或tvOS应用程序核心基础设施的组件。该框架提供了窗口和视图架构，用于实现您的UI，提供了处理事件的基础设施，用于向您的应用程序提供多点触控和其他类型的输入，以及用于管理用户、系统和您的应用程序之间交互的主运行循环。

<img src="https://docs-assets.developer.apple.com/published/e9dc54c3f1/renderedDark2x-1661916508.png" title="" alt="" data-align="center">

图片中展示了运行着应用程序的iPhone和iPad，展示了UIKit用户界面元素。iPhone正在运行一个示例应用程序，显示了一个颜色选择器和导航栏。iPad正在运行一个示例应用程序，显示了一个分割视图界面，包括一个概要视图、一个食谱列表和一个详细视图，其中包含有关特定食谱的信息。

UIKit还包括支持动画、文档、绘图和打印、文本管理和显示、搜索、应用程序扩展、资源管理以及获取有关当前设备的信息等。您还可以自定义无障碍支持，并为不同的语言、国家或文化地区本地化您的应用程序的界面。

UIKit与SwiftUI框架无缝配合，因此您可以在SwiftUI中实现UIKit应用程序的部分内容，或在两个框架之间混合界面元素。例如，您可以在SwiftUI视图中放置UIKit视图和视图控制器，反之亦然。

要构建macOS应用程序，您可以使用SwiftUI创建适用于所有苹果平台的应用程序，或使用AppKit创建仅适用于Mac的应用程序。或者，您可以使用Mac Catalyst将UIKit iPad应用程序带到Mac上。

> 重要信息：除非文档中另有说明，否则仅在应用程序的主线程或主调度队列中使用UIKit类。此限制特别适用于从UIResponder派生的类或以任何方式操纵应用程序用户界面的类。

# Protecting the User’s Privacy (保护用户隐私)

> 保护个人数据，并尊重用户对数据使用方式的偏好。

## 概览

设计用户隐私是非常重要的。大多数苹果设备都包含用户不希望向应用程序或外部实体公开的个人数据。如果您的应用程序不当地访问或使用数据，用户可能会停止使用您的应用程序，甚至从设备中删除它。

仅在符合适用法律的情况下获得用户知情同意后，才能访问用户或设备数据。此外，应采取适当的措施保护用户和设备数据，并透明地说明您如何使用它。

## 审查来自政府和行业来源的准则

请参考以下文件：

* [Mobile Privacy Disclosures: Building Trust Through Transparency](https://www.ftc.gov/sites/default/files/documents/reports/mobile-privacy-disclosures-building-trust-through-transparency-federal-trade-commission-staff-report/130201mobileprivacyreport.pdf).美国联邦贸易委员会关于移动隐私的报告。

* [Opinion 02/2013 on Apps on Smart Devices](https://ec.europa.eu/justice/article-29/documentation/opinion-recommendation/files/2013/wp202_en.pdf).欧盟数据保护专员对移动应用程序数据保护的意见。

* [Privacy on the Go: Recommendations for the Mobile Ecosystem](https://oag.ca.gov/sites/all/files/agweb/pdfs/privacy/privacy_on_the_go.pdf).加利福尼亚州检察长针对移动隐私的建议。

* Smartphone Privacy Initiative（2012）[英文版](http://www.soumu.go.jp/main_sosiki/joho_tsusin/eng/presentation/pdf/Initiative.pdf)或[日文版](http://www.soumu.go.jp/main_content/000171225.pdf)，以及Smartphone Privacy Initiative II（2013）[英文版](http://www.soumu.go.jp/main_sosiki/joho_tsusin/eng/presentation/pdf/Summary_II.pdf)或[日文版](http://www.soumu.go.jp/main_content/000247654.pdf)。日本内阁官房内的智能手机隐私倡议。

## 只在应用程序需要才请求访问数据权限

在您的应用程序需要敏感的用户或设备数据（如位置、联系人和照片）时请求访问权限。在您的应用程序的`Info.plist`文件中提供一个目的字符串（有时称为使用说明字符串），系统可以向用户解释您的应用程序为什么需要访问权限。在用户不授权访问所请求的数据的情况下，提供合理的后备行为。有关详细信息，请参见[请求访问受保护资源](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/requesting_access_to_protected_resources)。

## 透明公开地说明用户数据的用途

例如，当您将应用程序提交到App Store时，作为您的App Store Connect元数据的一部分，指定一个URL作为您的隐私政策或声明。您也可以在应用程序描述中总结该政策或声明。

## 给予用户控制和保护被收集数据的权利

尊重用户的偏好，并采取合理的措施保护您在应用程序中收集的数据：

* 提供设置，允许用户禁用对敏感信息的访问。操作系统通过设置应用程序的“隐私”菜单自动完成对受保护系统资源（如位置、联系人和健康数据）的访问控制。将此行为扩展到您从这些来源缓存或直接收集的任何数据。例如，如果您的用户创建包含个人信息的社交媒体资料，请为他们提供删除数据的方法（包括您拥有的所有服务器副本）。

* 在iOS中存储文件时，请使用适合您的应用程序的最强数据保护级别，如[加密您的应用程序文件](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/encrypting_your_app_s_files)中所述。在通过网络发送用户或设备数据时，请使用App Transport Security，如[NSAppTransportSecurity](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/plist/info/NSAppTransportSecurity)中所述。

* 如果您的应用程序使用[`ASIdentifierManager`](https://developer.apple.com/documentation/adsupport/asidentifiermanager)类，请尊重其[`isAdvertisingTrackingEnabled`](https://developer.apple.com/documentation/adsupport/asidentifiermanager/1614148-isadvertisingtrackingenabled)属性的值。如果用户将该属性设置为false，则仅将[`ASIdentifierManager`](https://developer.apple.com/documentation/adsupport/asidentifiermanager)类用于有限的广告目的，例如频率限制、归因、转化事件、估计独特用户数、广告欺诈检测和调试。请参阅[AdSupport](https://developer.apple.com/documentation/adsupport)框架以获取更多信息。

* 如果您必须持久标识用户，请使用[`UIDevice`](https://developer.apple.com/documentation/uikit/uidevice)类的[`identifierForVendor`](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)属性或[`ASIdentifierManager`](https://developer.apple.com/documentation/adsupport/asidentifiermanager)类的[`advertisingIdentifier`](https://developer.apple.com/documentation/adsupport/asidentifiermanager/1614151-advertisingidentifier)属性。

## 使用所需的最小数据量

仅请求和使用完成特定任务所需的最小用户或设备数据。不要为不必要或不明显的原因或因为您认为它可能以后有用而寻求访问或收集数据。

如果您的应用程序支持音频输入，请仅在实际准备开始录制时配置音频会话以进行录制。如果您不打算立即录制，请勿在启动时配置音频会话以进行录制。当应用程序配置其音频会话进行录制时，系统会向用户发出警报，并为用户提供禁用应用程序录制的选项。

# 加密应用程序文件

> 通过磁盘加密来保护iOS中的用户数据。

## 概览

数据保护（Data protection）是iOS的一项功能，您可以使用它来保护您应用程序的文件并防止未经授权的访问。当用户为设备设置了有效的密码时，数据保护会自动启用。您可以正常读写文件，但系统会在幕后加密和解密您的内容。加密和解密过程自动进行且硬件加速。

您可以指定要应用于每个文件的数据保护级别。有四个级别可用，每个级别确定何时可以访问该文件。如果您在创建文件时未指定保护级别，则iOS会自动应用默认保护级别。

* **无保护**。文件始终可访问。

* **第一次用户身份验证前完全保护**（默认）。文件在用户首次解锁设备之前无法访问。在设备首次解锁后，文件会保持可访问状态，直到设备关闭或重新启动。

* **除非打开否则完全保护**。只有在设备解锁时才能打开现有文件。如果您已经打开了文件，则即使用户锁定设备，您仍然可以继续访问该文件。您还可以创建新文件并在设备锁定或解锁时访问它们。

* **完全保护**。文件只有在设备解锁时才能访问。

要在一步中创建和加密新文件，请使用文件内容构造数据对象并调用[`write(to:options:)`](https://developer.apple.com/documentation/foundation/data/1779858-write)方法。调用该方法时，请指定要应用于文件的数据保护选项。[清单1](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/encrypting_your_app_s_files#2928956)显示了如何将[`Data`](https://developer.apple.com/documentation/foundation/data)实例的内容写入文件并使用完全保护级别加密的示例。

**清单1：第一次写入加密文件**

```swift
do {
   try data.write(to: fileURL, options: .completeFileProtection)
} catch {
   // 处理错误。
}
```

要更改现有文件的数据保护级别，请使用[`NSURL`](https://developer.apple.com/documentation/foundation/nsurl)的[`setResourceValue(_:forKey:)`](https://developer.apple.com/documentation/foundation/nsurl/1413819-setresourcevalue)方法。调用该方法时，请将新的数据保护选项分配给[`fileProtectionKey`](https://developer.apple.com/documentation/foundation/urlresourcekey/1616246-fileprotectionkey)资源键。清单2显示了将此键添加到现有文件的示例。

**清单2：加密现有磁盘上的文件**

```swift
do {
   try data.write(to: fileURL, options: .completeFileProtection)
} catch {
   // 处理错误。
}
```

## 管理加密文件的访问

根据文件的保护级别，在用户随后锁定设备时尝试读取或写入其内容可能会失败。为确保您的应用程序能够访问文件，请执行以下操作：

* 根据您的需要选择正确的数据保护级别。

* 使用应用程序委托的[`applicationProtectedDataWillBecomeUnavailable(_:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623019-applicationprotecteddatawillbeco)和[`applicationProtectedDataDidBecomeAvailable(_:)`](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623044-applicationprotecteddatadidbecom)方法，使用[`completeFileProtection`](https://developer.apple.com/documentation/foundation/nsdata/writingoptions/1617198-completefileprotection)级别关闭和重新打开文件。

将完全保护级别分配给您的应用程序仅在前台访问的文件。如果您的应用程序支持后台功能（例如处理位置更新），则为您可能在后台访问的文件分配不同的保护级别。例如，健身应用程序可能会在使用后台记录位置事件的文件上使用除非打开否则完全保护级别。

包含有关用户的个人信息或由用户直接创建的文件始终需要最强的保护级别。为用户数据文件分配完全保护级别，并使用应用程序委托方法管理对这些文件的访问。应用程序委托方法为您提供在文件变得对您的应用程序不可访问之前关闭文件的时间。

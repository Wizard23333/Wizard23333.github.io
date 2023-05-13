# UIKit

> 用于构建和管理基于图形化、事件驱动的用户界面，用于您的iOS、iPadOS或tvOS应用程序。

## 概览

UIKit提供了各种功能来构建应用程序，包括用于构建iOS、iPadOS或tvOS应用程序核心基础设施的组件。该框架提供了窗口和视图架构，用于实现您的UI，提供了处理事件的基础设施，用于向您的应用程序提供多点触控和其他类型的输入，以及用于管理用户、系统和您的应用程序之间交互的主运行循环。

<img src="https://docs-assets.developer.apple.com/published/e9dc54c3f1/renderedDark2x-1661916508.png" title="" alt="" data-align="center">

图片中展示了运行着应用程序的iPhone和iPad，展示了UIKit用户界面元素。iPhone正在运行一个示例应用程序，显示了一个颜色选择器和导航栏。iPad正在运行一个示例应用程序，显示了一个分割视图界面，包括一个概要视图、一个食谱列表和一个详细视图，其中包含有关特定食谱的信息。

UIKit还包括支持动画、文档、绘图和打印、文本管理和显示、搜索、应用程序扩展、资源管理以及获取有关当前设备的信息等。您还可以自定义无障碍支持，并为不同的语言、国家或文化地区本地化您的应用程序的界面。

UIKit与SwiftUI框架无缝配合，因此您可以在SwiftUI中实现UIKit应用程序的部分内容，或在两个框架之间混合界面元素。例如，您可以在SwiftUI视图中放置UIKit视图和视图控制器，反之亦然。

要构建macOS应用程序，您可以使用SwiftUI创建适用于所有苹果平台的应用程序，或使用AppKit创建仅适用于Mac的应用程序。或者，您可以使用Mac Catalyst将UIKit iPad应用程序带到Mac上。

> **重要信息：**
> 
> 除非文档中另有说明，否则仅在应用程序的主线程或主调度队列中使用UIKit类。此限制特别适用于从UIResponder派生的类或以任何方式操纵应用程序用户界面的类。

# About App Development with UIKit (使用UIKit进行应用程序开发)

> 了解UIKit和Xcode为您的iOS和tvOS应用程序提供的基本支持。

## 概览

UIKit框架提供了构建iOS和tvOS应用程序所需的核心对象。您可以使用这些对象在屏幕上显示内容、与该内容交互以及管理与系统的交互。应用程序依赖于UIKit进行基本行为，而UIKit提供了许多方法，让您可以自定义这些行为以满足您的特定需求。

> **重要信息：**
> 
> 您始终可以通过在苹果的集成开发环境Xcode中创建一个项目来开始iOS或tvOS应用程序的开发。如果您没有安装Xcode，可以从App Store下载它。您也可以从[developer.apple.com](https://developer.apple.com/)下载最新版本。

Xcode为您创建的每个应用程序提供模板项目作为起点。例如，[图1](https://developer.apple.com/documentation/uikit/about_app_development_with_uikit#3004316)展示了使用Xcode中的Single View App模板创建的应用程序的结构。这些模板项目提供了最小的用户界面，因此您可以立即构建和运行您的项目，并在设备或模拟器上查看结果。

**图1 单视图iOS应用程序的结构**

<img title="" src="https://docs-assets.developer.apple.com/published/c922d75ec8/8783d1ba-8cc8-4966-afa9-4780a24cc430.png" alt="" data-align="center" width="280">

## 必要资源

每个UIKit应用程序都需要以下资源：

* 应用程序图标（App icons）

* 启动屏幕故事板（Launch screen storyboard）

系统会在主屏幕、设置和需要区分您的应用程序与其他应用程序的任何地方显示您的应用程序图标。由于它在多个位置和多个设备上使用，因此您需要在Xcode项目的AppIcon图像资源中提供多个版本的应用程序图标。您的应用程序图标应该是独特的，以帮助用户在主屏幕上快速识别您的应用程序。但是，您可以根据必须提供的不同图像大小来调整图标的细节。

**图2 为iOS应用程序提供图标**

![](https://docs-assets.developer.apple.com/published/06fabca2b6/3004317~dark@2x.png)

LaunchScreen.storyboard文件包含您的应用程序的初始用户界面，可以是闪屏或您实际界面的简化版本。当用户点击您的应用程序图标时，系统会立即显示启动屏幕，让用户知道您的应用程序正在启动。启动屏幕还为您的应用程序提供了覆盖层，因为它正在初始化自身。当您的应用程序准备好时，系统会隐藏启动屏幕并显示您的应用程序的实际界面。

## 必要的应用元数据

系统从应用程序包中的信息属性列表（Info.plist）文件中获取有关您的应用程序配置和功能的信息。Xcode为每个新项目模板提供了一个预配置的版本，但您可能需要在某个时候修改此文件。例如，如果您的应用程序依赖于特定的硬件或使用特定的系统框架，您可能需要将与这些功能相关的信息添加到此文件中。

您可以对Info.plist文件进行的一个常见修改是声明应用程序的硬件和软件要求。这些要求是您向系统传达您的应用程序运行所需的信息的方式。例如，导航应用程序可能需要GPS硬件才能提供逐步转向指示。应用商店会阻止在不符合您的应用程序要求的设备上安装应用程序。

**图3 声明应用程序的硬件和软件要求**

![](https://docs-assets.developer.apple.com/published/afd6ca8355/3004318~dark@2x.png)

有关您可以在`Info.plist`文件中包含的键的信息，请参阅[信息属性列表键参考](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009247)。

## UIKit应用程序的代码结构

UIKit提供了许多应用程序的核心对象，包括那些与系统交互、运行应用程序的主事件循环以及在屏幕上显示内容的对象。您可以直接使用大多数这些对象，或仅进行轻微修改。知道哪些对象需要修改，以及何时修改它们，对于实现您的应用程序至关重要。

UIKit应用程序的结构基于模型-视图-控制器（MVC）设计模式，其中对象根据其目的进行划分。模型对象管理应用程序的数据和业务逻辑。视图对象提供数据的可视化表示。控制器对象充当模型和视图对象之间的桥梁，在适当的时间移动数据。

[图4](https://developer.apple.com/documentation/uikit/about_app_development_with_uikit#3004320)代表了UIKit应用程序的一个相当典型的结构。您提供代表应用程序数据结构的模型对象。UIKit提供大多数视图对象，虽然您可以根据需要为数据定义自定义视图。协调数据对象和UIKit视图之间的数据交换的是视图控制器和应用程序委托对象。

**图4 应用程序核心对象**

<img title="" src="https://docs-assets.developer.apple.com/published/4e7c26b6ad/ff7aa08f-4857-44ce-88d5-7dacbef84509.png" alt="" data-align="center" width="445">

UIKit和Foundation框架提供了许多基本类型，用于定义应用程序的模型对象。UIKit提供了一个[`UIDocument`](https://developer.apple.com/documentation/uikit/uidocument)对象，用于组织属于基于磁盘的文件中的数据结构。Foundation框架定义了表示字符串、数字、数组和其他数据类型的基本对象。[Swift标准库](https://developer.apple.com/documentation/swift/swift-standard-library)提供了许多与Foundation框架中可用的相同类型。

UIKit提供了大多数控制器和视图层中的对象。具体来说，UIKit定义了[`UIView`](https://developer.apple.com/documentation/uikit/uiview)类，通常负责在屏幕上显示您的内容。(您也可以使用Metal和其他系统框架直接将内容呈现到屏幕上。) [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)对象运行您的应用程序的主事件循环并管理您的应用程序的整个生命周期。

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

# Requesting access to protected resources (请求访问受保护的资源)

> 请提供一个目的字符串，以向用户解释为什么您需要访问他们设备上的受保护资源。

## 概览

现代设备收集并存储了关于使用它们的人的大量敏感信息。许多应用程序依赖于此类数据及生成它的设备硬件来执行有用的工作。例如，导航应用程序需要一个人的GPS坐标来在地图上定位该人。但是，并不是所有应用程序都需要访问所有数据。同样的导航应用程序不需要访问一个人的健康历史记录、相机接口或蓝牙外设。

确保您的应用程序只访问它需要执行其工作的内容。为了支持这一原则，苹果的操作系统默认限制对受保护数据和资源的访问。应用程序可以逐个案例地请求访问，同时提供为何需要访问的说明。使用应用程序的人决定是否授予或拒绝请求。

> **提示信息：**
> 
> 除了向用户请求访问资源的权限外，在某些情况下，您还需要通过向应用程序添加授权来单独声明您的意图，如[授权](https://developer.apple.com/documentation/bundleresources/entitlements)所述。

## 提供目的字符串

当您的应用程序首次尝试访问受保护资源时，系统会提示应用程序使用者授予权限。以下是一个示例，一个名为FoodDeliveryApp的iOS应用程序，提供食品送货服务，生成了一个提示请求访问用户的位置：

<img title="" src="https://docs-assets.developer.apple.com/published/463f770ecd/renderedDark2x-1667327329.png" alt="" data-align="center" width="265">

如果授权被允许，系统会记住使用者的选择，并且不会再次提示。如果使用者拒绝了授权，访问尝试将以特定于资源的方式失败。对于访问位置数据的特定情况，用户可以通过点击“仅允许一次”来允许仅在一次会话中使用位置数据。

系统会自动生成提示的标题，其中包括您的应用程序的名称。您需要提供一个称为“目的字符串”或“用途说明”的消息 - 在本例中是“您的位置允许您查看送餐范围内的餐厅。” - 以指示您的应用程序需要访问的原因。准确而简洁地向用户解释为什么您的应用程序需要访问敏感数据通常需要一句完整的句子，这有助于用户做出知情决策并提高他们授权的几率。

您可以通过为添加到应用程序的[`Information Property List`](https://developer.apple.com/documentation/bundleresources/information_property_list)文件的特定于资源的键设置字符串值来提供目的字符串。例如，上面图像中的消息是与[`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/documentation/bundleresources/information_property_list/nslocationwheninuseusagedescription)键相关联的字符串。使用Xcode中的属性列表编辑器修改您的`Info.plist`文件。

<img src="https://docs-assets.developer.apple.com/published/4ec12be08e/renderedDark2x-1667327328.png" title="" alt="" data-align="center">

如果您的应用程序使用受保护资源，请始终在Info.plist文件中提供有效的目的字符串。如果您没有提供有效的目的字符串，访问资源的尝试将失败，甚至可能导致您的应用程序崩溃。

如果您的应用程序支持多个语言环境，请在Info.plist文件中提供目的字符串的本地化版本，并将本地化字符串放置在每个语言环境的InfoPlist.strings文件中以进行支持。

## 遵守目的字符串的要求

为了向用户提供有用、简洁的关于为什么请求访问受保护资源的信息，请确保您提供的每个目的字符串都是有效的，检查以下内容：

* 目的字符串不为空，并且不仅由空格字符组成。

* 目的字符串长度小于4,000字节。典型的目的字符串是一句完整的句子，但您可以提供附加信息以帮助用户做出正确的有关共享个人信息的决定。

* 目的字符串具有相应键所需的正确类型，通常是字符串。

* 目的字符串提供的描述准确、有意义且具体，说明应用程序为何需要访问受保护资源。

请遵守这些要求，对`Info.plist`文件和特定于语言环境的`InfoPlist.strings`文件中的每个目的字符串都要进行检查。

App Review会检查是否使用了受保护资源，并拒绝包含未经目的字符串授权的访问这些资源的代码的应用程序。例如，访问位置的应用程序可能会从App Review接收到有关需要存在[`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/documentation/bundleresources/information_property_list/nslocationwheninuseusagedescription)键的要求的以下信息：

```textile
ITMS-90683: Missing purpose string in Info.plist. 
Your app’s code references one or more APIs that access sensitive user 
data, or the app has one or more entitlements that permit such access. 
The Info.plist file for the "{app-bundle-path}" bundle should contain a 
NSLocationWhenInUseUsageDescription key with a user-facing purpose string 
explaining clearly and completely why your app needs the data.
If you’re using external libraries or SDKs, they may reference APIs that 
require a purpose string. While your app might not use these APIs, a 
purpose string is still required. For details, visit: 
https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/requesting_access_to_protected_resources.

ITMS-90683: Info.plist文件中缺少目的字符串。
您的应用程序代码引用了一个或多个访问敏感用户数据的API，或应用程序具有一个或多个允许此类访问的授权。
"{app-bundle-path}"包的Info.plist文件应包含一个NSLocationWhenInUseUsageDescription键，
其中包含一个面向用户的目的字符串，清楚而完整地解释您的应用程序为何需要这些数据。
如果您正在使用外部库或SDK，它们可能引用需要目的字符串的API。
尽管您的应用程序可能不使用这些API，但仍需要提供目的字符串。有关详细信息，请访问：
https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy/requesting_access_to_protected_resources。
```

要解决此问题，请提供一个目的字符串，说明应用程序为何需要访问这些敏感信息，或删除访问该资源的代码。

> **提示：**
> 
> 如果您正在使用外部库或SDK，则它们可能引用需要目的字符串的API。尽管您的应用程序可能不使用这些API，但仍需要提供目的字符串以供App Review审核。您可以联系库或SDK的开发人员，请求有关开发人员使用的受保护资源及其目的的信息，或请求开发人员发布不包含这些API的代码版本。您负责所有受保护资源的访问，包括外部SDK和库的访问。

## 检查授权

许多提供访问受保护资源的系统框架都有专用的API，用于检查和请求授权以使用这些资源。该模型允许您根据当前访问权限调整应用程序的行为。例如，如果用户拒绝了您的应用程序的权限，您可以从用户界面中删除相关元素。

由于用户可以随时使用“设置”更改授权状态，请在访问受保护资源之前始终检查其特征的授权状态。在没有专用API的情况下，准备您的应用程序以优雅地处理访问失败。

## 重置授权访问

当您的应用程序在第一次尝试后尝试访问受保护资源时，系统将记住用户的权限选择并不会再次提示。要再次提示用户，您需要在设备或系统上重置对这些资源的访问。

要在iOS应用程序中重置对受保护资源的权限访问，请在设备上点击“设置”>“通用”>“还原”>“重置位置与隐私”。

> **重要信息：**
> 
> 使用“重置位置与隐私”将重置您设备上所有服务的位置和隐私设置。使用“重置位置与隐私”将重置您设备上所有服务的位置和隐私设置。

要在 macOS 应用程序中重置特定服务的权限，请在终端中运行`tccutil reset <服务名称>`命令。例如，要重置 AppleEvents 的所有权限，请输入：

```shellsession
$ tccutil reset AppleEvents
```

这个命令会重置所有使用受保护资源的应用程序的授权访问。您也可以单独指定摄像头、日历、提醒事项或其他服务来重置它们。

# Encrypting Your App’s Files (加密应用程序文件)

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

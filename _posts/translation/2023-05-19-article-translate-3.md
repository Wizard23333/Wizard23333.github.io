---
title: 文档翻译（3）——保护用户隐私
author: wizard
date: 2023-05-19 13:05:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/protecting_the_user_s_privacy)

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

* 如果您必须持久地标识用户，请使用[`UIDevice`](https://developer.apple.com/documentation/uikit/uidevice)类的[`identifierForVendor`](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor)属性或[`ASIdentifierManager`](https://developer.apple.com/documentation/adsupport/asidentifiermanager)类的[`advertisingIdentifier`](https://developer.apple.com/documentation/adsupport/asidentifiermanager/1614151-advertisingidentifier)属性。

## 使用所需的最小数据量

仅请求和使用完成特定任务所需的最小用户或设备数据。不要为不必要或不明显的原因或因为您认为它可能以后有用而寻求访问或收集数据。

如果您的应用程序支持音频输入，请仅在实际准备开始录制时配置音频会话以进行录制。如果您不打算立即录制，请勿在启动时配置音频会话以进行录制。当应用程序配置其音频会话进行录制时，系统会向用户发出提示，并为用户提供禁用应用程序录制的选项。

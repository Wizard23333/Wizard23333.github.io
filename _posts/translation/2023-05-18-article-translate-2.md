---
title: 文档翻译（2）——使用UIKit进行应用程序开发
author: wizard
date: 2023-05-18 14:03:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
> 
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/about_app_development_with_uikit)

# About App Development with UIKit (使用UIKit进行应用程序开发)

> 了解UIKit和Xcode为您的iOS和tvOS应用程序提供的基本支持。

## 概览

UIKit框架提供了构建iOS和tvOS应用程序所需的核心对象。您可以使用这些对象在屏幕上显示内容、与该内容交互以及管理与系统的交互。应用程序依赖于UIKit进行基本行为，而UIKit提供了许多方法，让您可以自定义这些行为以满足您的特定需求。

> **重要信息：**
> 
> 您始终可以通过在苹果的集成开发环境Xcode中创建一个项目来开始iOS或tvOS应用程序的开发。如果您没有安装Xcode，可以从App Store下载它。您也可以从[developer.apple.com](https://developer.apple.com/)下载最新版本。
{: .prompt-warning }

Xcode为您创建的每个应用程序提供模板项目作为起点。例如，[图1](https://developer.apple.com/documentation/uikit/about_app_development_with_uikit#3004316)展示了使用Xcode中的Single View App模板创建的应用程序的结构。这些模板项目提供了最小的用户界面，因此您可以立即构建和运行您的项目，并在设备或模拟器上查看结果。

**图1 单视图iOS应用程序的结构**

<img title="" src="https://docs-assets.developer.apple.com/published/c922d75ec8/8783d1ba-8cc8-4966-afa9-4780a24cc430.png" alt="" width="276" data-align="center">

当你构建应用程序时，Xcode会编译你的源代码文件并为你的项目创建一个应用程序包。应用程序包是一个结构化的目录，包含与应用程序相关的代码和资源。资源包括图像资产（image assets）、故事板文件（storyboard files）、字符串文件（strings files）和支持你的代码的应用程序元数据（app metadata）。应用程序包的结构很重要，但是Xcode知道你的资源需要放在哪里，所以现在不用担心它。

## 必要资源

每个UIKit应用程序都需要以下资源：

* 应用程序图标（App icons）

* 启动屏幕故事板（Launch screen storyboard）

系统会在主屏幕、设置（Settings）和需要区分您的应用程序与其他应用程序的任何地方显示您的应用程序图标。由于它在多个位置和多个设备上使用，因此您需要在Xcode项目的AppIcon图像资源中提供多个版本的应用程序图标。您的应用程序图标应该是独特的，以帮助用户在主屏幕上快速识别您的应用程序。但是，您可以根据必须提供的不同的图像大小来调整图标的细节。

**图2 为iOS应用程序提供图标**

![](https://docs-assets.developer.apple.com/published/06fabca2b6/3004317~dark@2x.png)

LaunchScreen.storyboard文件包含您的应用程序的初始用户界面，可以是启动画面或您实际界面的简化版本。当用户点击您的应用程序图标时，系统会立即显示启动屏幕，让用户知道您的应用程序正在启动。启动屏幕还为您的应用程序提供了覆盖层，因为它正在初始化自身。当您的应用程序准备好时，系统会隐藏启动屏幕并显示您的应用程序的实际界面。

## 必要的应用元数据

系统从应用程序包中的信息属性列表（Info.plist）文件中获取有关您的应用程序配置和功能的信息。Xcode为每个新项目模板提供了一个预配置的版本，但您可能需要在某个时候修改此文件。例如，如果您的应用程序依赖于特定的硬件或使用特定的系统框架，您可能需要将与这些功能相关的信息添加到此文件中。

您可以对Info.plist文件进行的一个常见修改是声明应用程序的硬件和软件要求。这些要求是您向系统传达您的应用程序运行所需的信息的方式。例如，导航应用程序可能需要GPS硬件才能提供逐步转向指示。应用商店会阻止在不符合您的应用程序要求的设备上安装应用程序。

**图3 声明应用程序的硬件和软件要求**

![](https://docs-assets.developer.apple.com/published/afd6ca8355/3004318~dark@2x.png)

有关您可以在`Info.plist`文件中包含的键的信息，请参阅[信息属性列表键参考](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009247)。

## UIKit应用程序的代码结构

UIKit提供了许多应用程序的核心对象，包括那些与系统交互、运行应用程序的主事件循环以及在屏幕上显示内容的对象。您可以直接使用大多数这些对象，或仅进行轻微修改。知道哪些对象需要修改，以及何时修改它们，对于实现您的应用程序至关重要。

UIKit应用程序的结构基于模型-视图-控制器（MVC）设计模式，其中对象根据其目的进行划分。模型对象管理应用程序的数据和业务逻辑。视图对象提供数据的可视化表示。控制器对象充当模型和视图对象之间的桥梁，在适当的时间在他们之间传递数据。

[图4](https://developer.apple.com/documentation/uikit/about_app_development_with_uikit#3004320)代表了UIKit应用程序的一个相当典型的结构。您提供代表应用程序数据结构的模型对象。UIKit提供大多数视图对象，虽然您可以根据需要为数据定义自定义视图。协调数据对象和UIKit视图之间的数据交换的是您的视图控制器和应用程序委托对象。

**图4 应用程序核心对象**

![](https://docs-assets.developer.apple.com/published/4e7c26b6ad/ff7aa08f-4857-44ce-88d5-7dacbef84509.png)

UIKit和Foundation框架提供了许多基本类型，用于定义应用程序的模型对象。UIKit提供了一个[`UIDocument`](https://developer.apple.com/documentation/uikit/uidocument)对象，用于组织属于基于磁盘的文件中的数据结构。Foundation框架定义了表示字符串、数字、数组和其他数据类型的基本对象。[Swift标准库](https://developer.apple.com/documentation/swift/swift-standard-library)提供了许多与Foundation框架中可用的相同类型。

UIKit提供了大多数控制器和视图层中的对象。具体来说，UIKit定义了[`UIView`](https://developer.apple.com/documentation/uikit/uiview)类，通常负责在屏幕上显示您的内容。(您也可以使用Metal和其他系统框架直接将内容呈现到屏幕上。) [`UIApplication`](https://developer.apple.com/documentation/uikit/uiapplication)对象运行您的应用程序的主事件循环并管理您的应用程序的整个生命周期。
---
title: 文档翻译（1）——UIKit
author: wizard
date: 2023-05-18 12:49:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
{: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit)

# UIKit

> 用于构建和管理基于图形化、事件驱动的用户界面，用于您的iOS、iPadOS或tvOS应用程序。

## 概览

UIKit提供了各种功能来构建应用程序，包括用于构建iOS、iPadOS或tvOS应用程序核心基础设施的组件。该框架提供了窗口（window）和视图（view）架构，用于实现您的UI，提供了处理事件的基础设施，用于向您的应用程序提供多点触控（Multi-Touch）和其他类型的输入，以及用于管理用户、系统和您的应用程序之间交互的主运行循环（main run loop）。

![](https://docs-assets.developer.apple.com/published/e9dc54c3f1/renderedDark2x-1661916508.png)

图片中展示了运行着应用程序的iPhone和iPad，展示了UIKit用户界面元素。iPhone正在运行一个示例应用程序，显示了一个颜色选择器和导航栏。iPad正在运行一个示例应用程序，显示了一个分割视图界面，包括一个概要视图、一个食谱列表和一个详细视图，其中包含有关特定食谱的信息。

UIKit还包括动画、文档、绘图和打印、文本管理和显示、搜索、应用程序扩展、资源管理以及获取有关当前设备的信息等的支持。您还可以自定义无障碍支持，并为不同的语言、国家或文化地区本地化您的应用程序的界面。

UIKit与SwiftUI框架无缝配合，因此您可以在SwiftUI中实现UIKit应用程序的部分内容，或在两个框架之间混合界面元素。例如，您可以在SwiftUI视图中放置UIKit视图和视图控制器，反之亦然。

要构建macOS应用程序，您可以使用SwiftUI创建适用于所有苹果平台的应用程序，或使用AppKit创建仅适用于Mac的应用程序。或者，您可以使用Mac Catalyst将UIKit iPad应用程序带到Mac上。

> **重要信息：**
> 
> 除非文档中另有说明，否则仅在应用程序的主线程或主调度队列中使用UIKit类。此限制特别适用于从UIResponder派生的类或以任何方式操纵应用程序用户界面的类。
> 
{: .prompt-warning }
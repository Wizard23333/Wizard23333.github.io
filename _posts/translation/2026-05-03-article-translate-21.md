---
title: UIKit文档翻译（16）——Responding to the launch of your app
author: wizard
date: 2026-05-03 20:40:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]
---

> 本文基于 Apple 官方文档进行中文翻译与整理。
{: .prompt-info }

原文地址：[Apple 官方文档](https://developer.apple.com/documentation/uikit/responding-to-the-launch-of-your-app)

# Responding to the launch of your app

> 初始化 App 的数据结构，准备 App 运行，并响应系统在启动时交给 App 的请求。

## 概览

当用户在主屏幕上点击 App 图标时，系统会启动你的 App。如果你的 App 请求过特定事件，系统也可能在后台启动 App 来处理这些事件。

所有 App 都有关联的进程，并且至少有一个 scene。`UIApplication` 和 `UIScene` 对象分别表示这些概念。App 还会有一个 app delegate 对象，它遵循 `UIApplicationDelegate` 协议，用于响应进程内发生的重要事件。

即使 App 采用 scene life cycle，也仍然会使用 app delegate 来管理启动、终止等基础事件。启动时，UIKit 会自动创建 `UIApplication` 对象和你的 app delegate，然后在连接一个或多个 scene 之前启动 App 的主事件循环。

## 提供 launch storyboard

当用户第一次在设备上启动 App 时，系统会显示 launch storyboard，直到 App 准备好显示自己的 UI。显示 launch storyboard 可以让用户知道 App 已经启动，并且正在执行工作。如果 App 初始化和 UI 准备速度很快，用户可能只会短暂看到 launch storyboard。

Xcode 项目会自动包含一个默认的 launch storyboard，你可以对其进行自定义，也可以根据需要添加更多 launch storyboard。

添加新的 launch storyboard：

1. 在 Xcode 中打开项目。
2. 选择 **File > New > File**。
3. 向项目添加 **Launch Screen** 资源。

你可以向 launch storyboard 添加视图，并使用 Auto Layout 约束来设置尺寸和位置，使其能够适应不同环境。UIKit 会准确显示你提供的内容，并依据约束将视图放入可用空间。设计指导可参考 Human Interface Guidelines。

> **重要：**
>
> 不要为 launch screen 使用静态图片。在 iOS 14 及之后版本中，launch screen 限制为 25 MB。
{: .prompt-warning }

## 初始化 App 的数据结构

把启动阶段的初始化代码放在以下一个或两个方法中：

- `application(_:willFinishLaunchingWithOptions:)`
- `application(_:didFinishLaunchingWithOptions:)`

UIKit 会在 App 启动周期的开始阶段调用这些方法。你可以在这些方法中执行以下工作：

- 初始化 App 在多个 scene 之间共享的数据结构。
- 确认 App 具备运行所需的资源。
- 在 App 第一次启动时执行一次性设置。例如，把模板文件或用户可修改文件安装到可写目录中。
- 连接 App 使用的关键服务。例如，如果 App 支持远程通知，可以连接 Apple Push Notification service。
- 检查 launch options 字典，了解 App 为什么被启动。

对于尚未采用 scene life cycle 的 App，UIKit 会在启动时自动加载默认用户界面。你可以在 `application(_:didFinishLaunchingWithOptions:)` 中，在界面显示到屏幕之前进行额外调整。例如，可以根据用户上次使用 App 时的状态安装不同的 view controller。

## 将耗时任务移出主线程

用户启动 App 时，快速响应会形成更好的第一印象。如果在 `application(_:didFinishLaunchingWithOptions:)`、`application(_:willFinishLaunchingWithOptions:)` 或 `scene(_:willConnectTo:options:)` 中执行耗时任务，用户可能会觉得 App 启动迟缓。

快速返回对后台启动同样重要，因为系统会限制 App 在后台的执行时间。

应将对启动不关键的任务移出启动序列。例如：

- 延迟初始化 App 并不立即需要的功能。
- 将重要但耗时的任务移出主线程，例如放到全局 dispatch queue 或 `Task` 中异步执行。

## 判断 scene 为什么连接

当 UIKit 连接 App 中的 scene 时，会传入一个 `UIScene.ConnectionOptions` 对象。这个对象包含 UIKit 为什么连接该 scene 的信息。

例如，用户可能请求 App 打开一个 `URL`。你可以从连接选项中读取该 `URL`，再根据其中的信息显示相应页面。

处理思路通常是：

1. 在 `scene(_:willConnectTo:options:)` 中确认当前 scene 是窗口 scene。
2. 检查 `connectionOptions.urlContexts` 中是否包含 URL。
3. 使用 `URLComponents` 解析 URL。
4. 如果 URL 中有 query items，就根据这些信息配置界面或执行对应逻辑。

## 判断 App 为什么启动

当 UIKit 启动 App 时，会调用 `application(_:willFinishLaunchingWithOptions:)` 和 `application(_:didFinishLaunchingWithOptions:)`，表示启动流程到达了对应阶段。

如果 App 没有采用 scene life cycle，UIKit 会把 launch options 字典传给这些方法。这个字典中的键表示 App 需要立即处理的重要任务。例如，它们可能表示用户在别处发起、并希望在你的 App 中继续完成的操作。

你应该始终检查 launch options 字典中是否包含你预期的键，并在存在时做出相应处理。

> **重要：**
>
> 采用 scene life cycle 后，UIKit 仍然会调用 `application(_:willFinishLaunchingWithOptions:)` 和 `application(_:didFinishLaunchingWithOptions:)`，但不再通过这些方法提供 launch options 字典。你应实现 scene connection 相关方法，以接收 App 启动和 scene 连接的详细信息。更多内容可参考 Scenes。
{: .prompt-warning }

如果 App 需要处理传入 URL，可以在 app delegate 的 `application(_:didFinishLaunchingWithOptions:)` 中检查 launch options，确认其中是否有 URL，以及 URL 是否包含可用于配置 App 的 query items。若 App 无法处理这些 launch options，可以返回 `false`；可以处理则返回 `true`。

## 相关主题

### 启动阶段

- **About the app launch sequence**：了解系统在 App 启动时执行代码的顺序。
- **Performing one-time setup for your app**：确保 App 环境得到正确配置。
- **Preserving your app’s UI across launches**：在系统终止 App 后，将界面恢复到之前状态。

## 另请参阅

- **Managing your app’s life cycle**：当前台或后台状态变化时响应系统通知，并处理其他重要系统事件。
- `UIApplication`：iOS App 的集中控制与协调点。
- `UIApplicationDelegate`：管理 App 共享行为的一组方法。
- **Scenes**：同时管理 App UI 的多个实例，并把资源分配给合适的 UI 实例。

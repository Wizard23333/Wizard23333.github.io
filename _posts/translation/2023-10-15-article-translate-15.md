---
title: UIKit文档翻译（15）——UIStackView
author: wizard
date: 2023-10-15 16:40:00 +0800
categories: [文档翻译]
tags: [文档翻译, UIKit]

---

> 本文使用chatGPT（gpt-3.5-turbo）翻译，加以部分人工润色排版，如有问题请在评论区指出
> {: .prompt-info }

原文地址：[链接](https://developer.apple.com/documentation/uikit/uistackview)

# UIStackView

> 用于在列或行中布置视图集合的简化界面

```swift
@MainActor
class UIStackView : UIView
```

## 概览

堆栈视图（Stack View）可以利用自动布局的强大功能，创建能动态适应设备方向、屏幕尺寸以及可用空间变化的用户界面。堆栈视图能管理其[`arrangedSubviews`](https://developer.apple.com/documentation/uikit/uistackview/1616232-arrangedsubviews)属性中所有视图的布局。这些视图根据它们在[`arrangedSubviews`](https://developer.apple.com/documentation/uikit/uistackview/1616232-arrangedsubviews)数组中的顺序，在堆栈视图的轴线上进行排列。具体的布局方式取决于堆栈视图的[`axis`](https://developer.apple.com/documentation/uikit/uistackview/1616223-axis)、[`distribution`](https://developer.apple.com/documentation/uikit/uistackview/1616233-distribution)、[`alignment`](https://developer.apple.com/documentation/uikit/uistackview/1616243-alignment)、[`spacing`](https://developer.apple.com/documentation/uikit/uistackview/1616225-spacing)和其他属性。

<img title="" src="https://docs-assets.developer.apple.com/published/82128953f6/uistack_hero_2x_04e50947-5aa0-4403-825b-26ba4c1662bd.png" alt="" data-align="center" width="537">

要使用堆栈视图，可以在 Storyboard 中打开它。从对象库中拖动水平堆栈视图（Horizontal Stack View）或垂直堆栈视图（Vertical Stack View），并将堆栈视图放置在需要的位置上。接下来，拖动需要放入堆栈视图的内容，可以将其他视图或控件放入堆栈视图中。您可以根据需要继续向堆栈中添加视图和控件。Interface Builder 会根据内容调整堆栈的大小。您还可以通过在属性检查器（Attributes inspector）中修改堆栈视图的属性来调整堆栈内容的外观。

> 您负责定义堆栈视图的位置以及大小（可选）。然后，堆栈视图会管理其内容的布局和大小。
{: .prompt-info }

## 堆栈视图和自动布局

堆栈视图使用自动布局来定位和调整其内部排列视图的大小。堆栈视图会将第一个和最后一个视图的边缘与堆栈的轴线对齐。在水平堆栈中，这意味着第一个排列视图的前缘与堆栈的前缘对齐，最后一个排列视图的后缘与堆栈的后缘对齐。在垂直堆栈中，顶部和底部边缘分别与堆栈的顶部和底部边缘对齐。如果将堆栈视图的[`isLayoutMarginsRelativeArrangement`](https://developer.apple.com/documentation/uikit/uistackview/1616220-islayoutmarginsrelativearrangeme)属性设置为`true`，堆栈视图将其内容固定到相关边距而不是边缘。

除了[`UIStackView.Distribution.fillEqually`](https://developer.apple.com/documentation/uikit/uistackview/distribution/fillequally)分布之外，堆栈视图在沿着堆栈的轴线计算大小时使用每个排列视图的[`intrinsicContentSize`](https://developer.apple.com/documentation/uikit/uiview/1622600-intrinsiccontentsize)属性。[`UIStackView.Distribution.fillEqually`](https://developer.apple.com/documentation/uikit/uistackview/distribution/fillequally)会调整所有排列视图的大小，使它们具有相同的大小，并填充堆栈视图沿着其轴线。如果可能，堆栈视图会拉伸所有排列视图以匹配沿着堆栈轴线具有最长固有尺寸的视图。

除了[`UIStackView.Alignment.fill`](https://developer.apple.com/documentation/uikit/uistackview/alignment/fill)对齐方式之外，堆栈视图在计算垂直于堆栈轴线的大小时使用每个排列视图的`intrinsicContentSize`属性。[`UIStackView.Alignment.fill`](https://developer.apple.com/documentation/uikit/uistackview/alignment/fill)会调整所有排列视图的大小，以使它们在垂直于堆栈轴线的方向上填充堆栈视图。如果可能，堆栈视图会拉伸所有排列视图以匹配垂直于堆栈轴线的方向上具有最大固有尺寸的视图。

### 堆栈视图的位置和大小

堆栈视图允许您在不直接使用自动布局的情况下布局其内容，但您仍然需要使用自动布局来定位堆栈视图本身。通常，这意味着将堆栈视图的至少两个相邻边缘固定来确定其位置。如果没有其他约束条件，系统会根据堆栈视图的内容计算其大小。

- 沿着堆栈视图的轴线，其适合大小（fitting size）等于所有排列视图的大小之和加上视图之间的间距。
- 垂直于堆栈视图的轴线，其适合大小等于最大排列视图的大小。
- 如果将堆栈视图的[`isLayoutMarginsRelativeArrangement`](https://developer.apple.com/documentation/uikit/uistackview/1616220-islayoutmarginsrelativearrangeme)属性设置为`true`，堆栈视图的适合大小将增加边距的空间。

您可以提供额外的约束条件来指定堆栈视图的高度、宽度或两者。在这些情况下，堆栈视图会调整其排列视图的布局和大小，以填充指定的区域。具体的布局方式取决于堆栈视图的属性。有关堆栈视图如何处理额外空间或不足空间的完整描述，请参阅[`UIStackView.Distribution`](https://developer.apple.com/documentation/uikit/uistackview/distribution)和[`UIStackView.Alignment`](https://developer.apple.com/documentation/uikit/uistackview/alignment)枚举。

您还可以基于堆栈视图的第一个或最后一个基线来定位堆栈视图，而不是使用顶部、底部或中心的Y位置。与堆栈视图的适合大小一样，这些基线是根据堆栈视图的内容计算的。

- 对于[`水平堆栈视图forFirstBaselineLayout`](https://developer.apple.com/documentation/uikit/uiview/1622452-forfirstbaselinelayout)和[`forLastBaselineLayout`](https://developer.apple.com/documentation/uikit/uiview/1622633-forlastbaselinelayout)方法都返回高度最高的视图。如果最高的视图也是一个堆栈视图，则返回在嵌套堆栈视图上调用`forFirstBaselineLayout`或`forLastBaselineLayout`的结果。
- 垂直堆栈视图对于`forFirstBaselineLayout`返回其第一个排列视图，对于`forLastBaselineLayout`返回其最后一个排列视图。如果其中任何一个视图也是堆栈视图，则返回在嵌套堆栈视图上调用`forFirstBaselineLayout`或`forLastBaselineLayout`的结果。

> 基线对齐仅适用于高度与其内在内容大小的高度相匹配的视图。如果视图被拉伸或压缩，基线会出现在错误的位置。
{: .prompt-info }

### 定义常见的堆栈视图布局

使用堆栈视图进行内容布局的常见方法：

**仅定义位置**。您可以通过将堆栈视图的两个相邻边固定到其父视图来定义堆栈视图的位置。在这种情况下，堆栈视图的大小根据其排列的视图自由地在两个维度上增长。当您希望堆栈视图的内容以其内在内容大小显示，并且希望按相对于堆栈视图排列其他用户界面元素时，这种方法特别有用。

下图显示了一个堆栈视图，其前导和顶部边缘固定在其父视图上。标签首先以基线对齐，它们之间有8个点的间隔，在其父视图中左对齐堆栈视图的内容。

<img src="https://docs-assets.developer.apple.com/published/f8e8e88004/786347a6-3c16-480d-949e-8319ebcc68f7.png" title="" alt="A horizontal stack view with its leading and top edges pinned to its superview. The stack view contains two labels, a date and a location name. The labels have baseline alignment with an 8-point space between them. With these constraints, the stack view’s content is left-aligned in its superview." data-align="center">

**定义堆栈视图沿其轴的大小**。在这种情况下，将堆栈视图沿其轴的两个边固定到其父视图，定义堆栈视图在该维度上的大小。您还需要固定另一个边，以定义堆栈视图的位置。堆栈视图根据其轴上的大小调整其内容的大小和位置，以填充定义的空间；但是，未固定的边根据最大排列视图的大小自由移动。

下图显示了一个堆栈视图，其前导、顶部和尾部边缘固定在其父视图上。使用填充分配将导致内容调整大小以填充视图的宽度，因为文本字段的内容抱紧优先级低于标签，所以会根据需要进行拉伸。

<img src="https://docs-assets.developer.apple.com/published/19ba11ed1c/0b922694-dbfc-4123-b3d6-02f70ac60a6c.png" title="" alt="A horizontal stack view with a label and a text field. The label is only as wide as the string it contains, but the text field fills the stack view’s width." data-align="center">

**定义堆栈视图垂直于其轴的大小**。这种方法与前面的示例类似，但是您将垂直于堆栈视图的轴固定两个边缘，并且仅沿轴固定一个边缘。这样，当您添加和删除排列视图时，堆栈视图可以沿其轴增长和收缩。除非使用[`UIStackView.Distribution.fillEqually`](https://developer.apple.com/documentation/uikit/uistackview/distribution/fillequally)分配，否则排列的视图根据其内在内容大小调整大小。在轴的垂直方向上，视图在定义的空间中根据堆栈视图的对齐方式进行布局。

下图显示了一个垂直堆栈，其中包含四个标签和一个按钮。堆栈使用8个点的间距和居中对齐。随着向堆栈中添加或删除项目，堆栈视图的高度会增长和收缩。

<img src="https://docs-assets.developer.apple.com/published/cc6e21faab/651cc05e-4df4-493a-8113-41fed7da7d3f.png" title="" alt="A vertical stack view with four labels "Item 1" through "Item 4," and a button to add more items below. The stack view has a flexible height to account for adding more items." data-align="center">

**定义堆栈视图的大小和位置**。在这种情况下，您固定堆栈视图的四个边缘，使堆栈视图在提供的空间中布局其内容。

下图显示了一个垂直堆栈视图，其四个边缘固定在其父视图上。通过使用居中对齐和填充分配，堆栈视图确保其内容在水平方向上居中，并垂直填充屏幕。然而，使用这种方法来获得所需的布局需要一些额外的步骤。默认情况下，堆栈视图会垂直拉伸标签而不是图像视图。要调整图像视图的大小，将其内容抱紧优先级设置为低于标签的内容抱紧优先级。此外，为了在调整大小时保持图像视图的纵横比，将其模式设置为Aspect Fit。在图像视图和堆栈视图之间添加等宽约束有助于确保图像的大小适应可用空间。

<img src="https://docs-assets.developer.apple.com/published/e77213019f/1ccce13c-b8b5-4653-8866-f5cc53331641.png" title="" alt="A vertical stack view with all four edges pinned to its superview. The stack view contains an image and uses the center alignment and fill distribution." data-align="center">

## 管理堆栈视图的外观

一个堆栈视图（`UIStackView`）管理其排列视图的位置和大小。有许多属性定义了堆栈视图如何布局其内容。

* [`axis`](https://developer.apple.com/documentation/uikit/uistackview/1616223-axis) 属性确定堆栈的方向，可以是垂直或水平的。

* [`distribution`](https://developer.apple.com/documentation/uikit/uistackview/1616233-distribution) 属性确定沿堆栈轴线布局排列视图的方式。

* [`alignment`](https://developer.apple.com/documentation/uikit/uistackview/1616243-alignment) 属性确定垂直于堆栈轴线的排列视图的布局方式。

* [`spacing`](https://developer.apple.com/documentation/uikit/uistackview/1616225-spacing) 属性确定排列视图之间的最小间距。

* [`isBaselineRelativeArrangement`](https://developer.apple.com/documentation/uikit/uistackview/1616224-isbaselinerelativearrangement) 属性确定是否从基线测量视图之间的垂直间距。

* [`isLayoutMarginsRelativeArrangement`](https://developer.apple.com/documentation/uikit/uistackview/1616220-islayoutmarginsrelativearrangeme) 属性确定堆栈视图是否相对于其布局边距布局排列视图。

通常情况下，您可以使用单个堆栈视图来布局少量项目。您可以通过将堆栈视图嵌套在其他堆栈视图中来构建更复杂的视图层次结构。例如，下面的图片显示了一个垂直堆栈视图，其中包含两个水平堆栈视图。每个水平堆栈视图包含一个带有“First Name”文本的标签和一个用于输入相应文本的空白文本字段。

<img src="https://docs-assets.developer.apple.com/published/82128953f6/NestedStacks_2x_551c6bce-06bc-4bcb-98ea-ab8776378d11.png" title="" alt="A vertical stack view containing two horizontal stack views. The top horizontal stack view contains a label with the text First Name and a blank text field for entering the corresponding text. The bottom horizontal stack view contains a label with the text Last Name and a blank text field." data-align="center">

您还可以通过向排列视图添加额外的约束来微调其外观。例如，您可以使用约束为视图设置最小或最大高度或宽度。或者，您可以为视图定义一个纵横比。堆栈视图在布局其内容时使用这些约束。

> 在为堆栈视图内部的视图添加约束时，要注意避免引入冲突。一般来说，如果某个视图的尺寸在给定维度上默认回到其内在内容尺寸，那么您可以安全地为该维度添加约束。
{: .prompt-info }

### 保持排列视图和子视图之间的一致性

堆栈视图确保其`arrangedSubviews`属性始终是其`subviews`属性的子集。具体而言，堆栈视图遵循以下规则：

* 当堆栈视图将一个视图添加到其`arrangedSubviews`数组时，如果该视图尚未是其子视图，堆栈视图将其同时添加为子视图。

* 当一个子视图从堆栈视图中移除时，堆栈视图也会从`arrangedSubviews`数组中将其移除。

* 从`arrangedSubviews`数组中移除一个视图不会将其作为子视图移除。堆栈视图不再管理该视图的大小和位置，但该视图仍然是视图层次结构的一部分，如果可见，则在屏幕上呈现。

尽管`arrangedSubviews`数组始终包含`subviews`数组的一个子集，但这两个数组的顺序是独立的。

* `arrangedSubviews`数组的顺序定义了视图在堆栈中出现的顺序。对于水平堆栈来说，视图按照阅读顺序布局，较低索引的视图出现在较高索引的视图之前。例如，在英语中，视图按照从左到右的顺序排列。对于垂直堆栈来说，视图从上到下布局，较低索引的视图位于较高索引的视图之上。

* subviews数组的顺序定义了子视图的Z轴顺序。如果视图重叠，具有较低索引的子视图位于具有较高索引的子视图的后面。

### 动态改变堆栈视图的内容

堆栈视图在视图添加、移除或插入到`arrangedSubviews`数组中时，以及在任何一个被排列的子视图的`isHidden`属性发生变化时，会自动更新其布局。

```
// 似乎从堆栈中移除第一个排列的视图。
// 该视图仍然在堆栈中，只是不可见了，并且不再参与布局。
let firstView = stackView.arrangedSubviews[0]
firstView.isHidden = true
```

堆栈视图还会自动响应其任何属性的变化。例如，通过更新堆栈视图的`axis`属性，可以动态更改堆栈的方向。

```
// 在垂直和水平堆栈之间切换。
if stackView.axis == .horizontal {
    stackView.axis = .vertical
} else {
    stackView.axis = .horizontal
}
```

您可以通过将这些更改放置在动画块中，来对排列的子视图的`isHidden`属性以及堆栈视图的属性进行动画处理。

```
// 动画删除堆栈中的第一个项目。
UIView.animate(withDuration: 0.25) { () -> Void in
    let firstView = stackView.arrangedSubviews[0]
    firstView.isHidden = true
}
```

最后，您可以直接在Interface Builder中为许多堆栈视图的属性定义特定于大小类的值。系统在堆栈视图的大小类发生变化时自动进行这些更改的动画处理。

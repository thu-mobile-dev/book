# 实际案例

这一节课我们通过三个稍微炫酷一些的案例来综合前面讲过的所有知识，包括 Flutter 中 常用 Widgets、状态管理、布局与约束、动画这些核心知识点。

## 拖拽点餐

案例代码 [GitHub | drag_to_order](https://github.com/thu-flutter-dev/drag_to_order)

> 此案例源自 [Flutter Cookbook | Effects: Drag a UI element](https://docs.flutter.dev/cookbook/effects/drag-a-widget)。
>
> 案例中使用的图片来自 [原神 BWIKI](https://wiki.biligame.com/ys/)
>
> 代码中使用 separate widgets 而不是官方代码中的 helper methods 组织用户界面代码。关于这两种代码组织结构，可以参考 [Stack Overflow | What is the difference between functions and classes to create reusable widgets?](https://stackoverflow.com/a/53234826/14298786) 和 [YouTube | Widgets vs helper methods | Decoding Flutter](https://www.youtube.com/watch?v=IOyq-eTRhvo)。

### 简介

长按拖拽（drag-and-drop）在手机的手势操作中算是非常常见的操作：比如发微信朋友圈的时候长按拖拽调整图片顺序、在文件管理器中将文件拖入文件夹...我们来通过一个拖拽点餐的完整案例来学习在 Flutter 中实现该功能的方法。

我们希望做出的拖拽点餐的效果大概是这样的：

![](images-drag-to-order/official.gif)

可以看到，应用界面主要分为上方的菜单和下方的顾客栏，菜单中每个食物显示一行，包含图片、名称和价格，顾客栏则包含顾客头像、名字、价格、已点的菜品数量：

![](images-drag-to-order/screenshot-0.png)

当用户长按上方的某个食物时，界面上会多出一个半透明的食物图片跟着用户手指移动：

![](images-drag-to-order/screenshot-1.png)

当食物被拖拽到下方顾客处，顾客的名片会高亮，此时松手即算作对应的顾客点一份视频：

![](images-drag-to-order/screenshot-2.png)

同时下方每位顾客的食物总金额也会被计算得到：

![](images-drag-to-order/screenshot-3.png)

此示例的核心知识点有：

- 使用 Row、Column、SizedBox 等基础元素进行界面布局
- 长按拖拽功能

### 整体讲解

代码总共包含两个源文件：`lib/main.dart`（数据部分）和 `lib/model.dart`（界面部分）。

#### 数据部分

在 `lib/main.dart` 中，定义了类 `Item` 和 `Customer` 分别表示食物和顾客。同时定义了两个全局变量`test_items` 和 `test_customers` 放置示例（使用全局变量并不是一个好习惯，当你的数据需要与用户界面产生关联时（数据需要渲染在屏幕上时），最好使用 `StatefulWidget` 或者 `Provider` 之类的框架定义状态，确保 Flutter 在数据变化时能够正确渲染）。

#### 界面部分

应用整体是一个普通的单页应用，`MaterialApp` 套了单个 `Scaffold`。

在 `FoodStallOrderMenu` 中使用 `Column` 组织起上方的食品菜单 `ItemsView` 和下方的顾客栏 `CustomersView`。

接下来从较小的 Widget 看起：`ItemView` 和 `CustomerView` 分别代表单个食品和顾客的 Widget；将 `ItemView` 放入一个 `ListView` 即组成了 `ItemsView`，将 `CustomerView` 放入一个 `Row` 即组成了 `CustomersView`。

我们实现的拖拽功能集于上面所说的用户界面实现。Flutter 中，长按拖拽需要：

- 添加三个 Widget
    - 给被拖拽的 Widget 包裹 `LongPressDraggable`（`DraggableItemView`）
    - 拖拽提示的 Widget（`DraggingItemView`）
    - 给拖拽目标的 Widget 包裹 `DragTarget`（`DroppableCustomerView`）
- 使用范型添加拖拽传递的数据类型，如 `LongPressDraggable<Item>` 和 `DragTarget<Item>`

### 拖拽功能详解

#### DraggableItemView

`LongPressDraggable<Item>` 的参数比较简洁明了：

- `data` 拖拽过程中传输的数据。
- `dragAnchorStrategy` 拖拽使用的方法，`pointerDragAnchorStrategy` 表示 `DraggingItemView` 随着手指的移动而移动。
- `feedback` 为拖拽过程中显示的图片/图标。

#### DraggingItemView

`DraggingItemView` 用一个普通的 `ClipRRect` 来显示图片。

关于 `FractionalTranslation`：代码中使用的参数是 `translation: const Offset(-0.5, -0.5)`，同学们可以将这个值调整为 `Offset(0.0, 0.0)`，这时半透明的食物图片会在手的右下方。`FractionalTranslation` 相当于是给 `ClipRRect` 加了一个向左上角一半宽一般长的偏移。

#### DroppableCustomerView

`candidateItems` 为拖拽至区域中的数据。当用户松开手指，`onAccept` 被调用，食物被添加到顾客的 `items` 中。

## 多态下载按钮

案例代码 [GitHub | multi_state_download_button](https://github.com/thu-flutter-dev/multi_state_download_button)

> 此案例源自 [Flutter Cookbook | Create a download button](https://docs.flutter.dev/cookbook/effects/download-button)。

在 App Store 中下载应用基本上有下面这样的动画效果：

![](images-multi-state-download-button/official.gif)

这个案例我们就用 Flutter 构建这个下载按钮。

首先可以看到，这个按钮有几个阶段：

```dart
enum DownloadStatus {
  notDownloaded, // 尚未下载、按钮显示「下载」
  fetchingDownload, // 正在连接服务器获取下载内容、按钮转圈（isFetching）
  downloading, // 正在下载、按钮以蓝色圆圈呈现下载进度（isDownloading）
  downloaded, // 已下载、按钮显示「打开」（isDownloaded）
}
```

在这个案例中，我们首先要分别做出这四个状态对应的样式，然后考虑添加它们之间的动画衔接。

### 整体结构

应用整体是一个单页应用，居中显示 `MultiStateDownloadButton`。这是一个 `StatefulWidget`，状态为 `_downloadController` 下载的各个阶段，使用 `SimulatedDownloadController` 封装。

在 `ButtonShapeWidget` 中，`opacity: isDownloading || isFetching ? 0.0 : 1.0`（0.0 表示透明） 以及 `Text(isDownloaded ? "打开" : "下载",` 决定了「下载」和「打开」只在对应的阶段显示，正在获取和正在下载时隐藏。

在 `ProgressIndicatorWidget` 中，使用了 Material Design 的组件 `CircularProgressIndicator`，可以看到其一个参数为 `value: isFetching ? null : progress`，在获取信息的时候， `value` 为 `null`，`CircularProgressIndicator` 不断转圈；在下载时则显示进度。

在 `MultiStateDownloadButton` 中，则使用 `Stack` 将二者放在一起，通过透明度决定谁要被显示，可以说是比较简单但不失精巧的设计了。

注：关于下载。该案例并没有下载任何东西，而是使用 Dart 的一些异步 API 模拟了下载过程，在 `_doSimulatedDownload()` 可以看到完整的代码。

### 动画讲解

TODO

### 动态界面大小

此示例为了方便展示，需要一个比较大的按钮，通过常数来调整按钮大小不够简洁，我们希望能够适配不同的设备屏幕大小，以比例的形式呈现按钮，这样在电脑端展示的时候，可以通过放大窗口来让按钮变大。

这一个功能主要在 `lib/extension.dart` 中实现，核心是 `MediaQuery.of(context).size.width/height` 获取屏幕 / 窗口的长款，取短边然后乘以对应的倍数呈现界面中的元素，如：

```dart
Icon(
  Icons.stop,
  size: ScreenSize.shorterSide(context, percentage: 1 / 2 * 1 / 4),
  color: Colors.blue,
),
```

注：对于普通文字的大小调整，使用 `Theme` 进行管理是更好的选择。

## 简易照片滤镜

案例代码 [GitHub | photo_fliter](https://github.com/thu-flutter-dev/photo_fliter)

> 此案例源自 [Flutter Cookbook | Create a photo filter carousel](https://docs.flutter.dev/cookbook/effects/photo-filter-carousel)。

![](images-photo-fliter/official.gif)

可以看到界面主要是两层，背景是一张图片，上面在下方有一个颜色选择栏，选择对应的颜色会给图片添加滤镜。选择颜色时可以点击选择，或者左右拨动选择（电脑端不支持左右拨动）。在颜色选择的过程中，越靠近中间的颜色大小越大。

此案例的主要知识点：

- 手势信息的获取
- 动画的构建

### 整体结构

TODO

### 手势处理与动画详解

TODO

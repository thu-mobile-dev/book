# Widget

这节课我们会先了解 Flutter 应用的简单结构，然后介绍一些常用的用户界面组件以及一些常用的控制组件。下节课我们会说明 Flutter 中的两种 Widget 的区别与联系，重点讲述状态管理。

## 最简单的 Flutter 应用

Flutter 一个很重要的思想就是组合。在应用开发的设计过程中，我们也会先设计小的组件，然后将小的组件成组，逐渐堆叠组合形成完整的用户界面；可以说，组合这种思想是很适合现代应用开发的。具体来说，Flutter 中的所有组件都是 Flutter，包括 UI、手势控制、布局...这些都是 Widget，一个 Flutter 应用就由多个 Widgets 组成。

下面是一个最简单的 Flutter 应用：

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    const Center(
      child: Text(
        "Hello, world!",
        style: TextStyle(fontSize: 48, color: Colors.black),
        textDirection: TextDirection.ltr,
      ),
    ),
  );
}
```

- Flutter 中的所有元素基本上都是 Widget，可以使用 `import 'package:flutter/widgets.dart';` 来导入常用的 Widgets，或者使用 `import 'package:flutter/material.dart';` 来导入包括 Material Design 的更多常用 Widgets。
    - `Text` 是一个 Widget，用于显示文字。这里 `Text(...)` 的语法是在初始化 `Text` 这个类的实例。第一个参数是要显示的文字，后面的参数不添加则使用默认值。可以看到这里制定了文字的样式和方向，如果不指定方向会报错，因为没有默认的文字方向；不指定样式为黑色可能默认为白色看不到。
    - `Center` 也是一个 Widget，但它的作用不是显示什么东西，而是将它的 child 居中。可以看到 Widget 也可以用于布局（指定相对位置）。
    - 这样的层级关系体现出了声明式的特点，开发者可以比较容易的看到界面的层级关系。
- `runApp()` 接收一个 Widget 作为参数，将这个 Widget 铺满屏幕。最终的效果是，`Center` 这个 Widget 铺满了屏幕，并且将它的子 Widget 放到了最中间。

编写 Flutter 代码，最基础的事情就是「组合」。你需要使用最基础的 Widget 来实现整个应用的界面和功能。

## StatelessWidget

编写一个 Flutter 应用，我们主要会使用 `StatelessWidget` 和 `StatefulWidget`。Widget 有着自己的状态时选择 `StatefulWidget`，否则选择 `StatelessWidget`。`StatelessWidget` 最大的作用是封装成组，例如下面的例子：

> 提示：在 VS Code 中敲 `stl` 即可自动补全 `StatelessWidget`。

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp(text: "Hello, world!"));
}

class MyApp extends StatelessWidget {
  final String text;

  const MyApp({super.key, required this.text});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        text,
        style: const TextStyle(fontSize: 48, color: Colors.black),
        textDirection: TextDirection.ltr,
      ),
    );
  }
}
```

我们进一步将刚刚居中的文字变成了一个 `StatelessWidget`，并且添加了一个参数 `text`，这样这个 Widget 有了可复用性，同时也让代码整体看起来更清晰。

一个 Widget 主要的功能体现在 `build()` 中，这个函数描述该组件如何组合更底层的组件从而构建自己。

context（类型 BuildContext） 在各个 Widgets 的层级结构中传递着一些参数，比如当前的显示状态、默认文字样式等等，有一点类似于全局变量。我们之后会用到。

## MaterialApp

Flutter 主要使用的设计语言是 Material Design，里面包含动画、主题、很多便捷的 UI/UX 组件，我们在第二节课已经介绍过。这一节课我们主要聚焦于如何在 Flutter 中使用 Material Design。

Material Design 在 Flutter 中最关键的两个 Widget 是 MaterialApp 和 Scaffold。

- MaterialApp 是一个最顶层的 Widget，如果你希望你的应用使用 Material Design，最好在 runApp() 中传入 MaterialApp。
    - 与 MaterialApp 类似，还有使用了 Human Interface Guidelines 设计的 CupertinoApp 用来向 iOS 用户提供与系统应用类似的体验。课程只关注 Material Design。
- Scaffold 可以直观理解为应用的一页，但更准确的理解是其可以为 Material 组件提供一个动画层，进一步呈现各种动画效果。

下面的这个例子使用了一些 Material Design 的基础组件：

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "Learn Flutter",
      home: MyScaffold(),
    );
  }
}

class MyScaffold extends StatelessWidget {
  const MyScaffold({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: MyWidget(),
      appBar: AppBar(title: Text("DEMO")),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          debugPrint("clicked");
          ScaffoldMessenger.of(context).showSnackBar(SnackBar(
            content: Text("clicked"),
          ));
        },
        child: Icon(Icons.ads_click),
      ),
    );
  }
}

class MyWidget extends StatelessWidget {
  const MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(child: Text("Hello, world!", style: TextStyle(fontSize: 48)));
  }
}
```

- 从应用结构上来说，使用 MaterialApp 包含一个 Scaffold，很容易看出这是一个单页的应用，结构更加清楚。
- 在 Scaffold 中添加了 AppBar 和 floatingActionButton，运行应用可以看到界面和较多现有应用保持一致，这有助于给用户带来熟悉感。
- 在动画效果上，floatingActionButton 和 SnackBar 的添加让界面看起来更流畅。
- 可以看到，在使用 Text 这个 Widget 时，我们不需要再指定文字方向和字体颜色了，这是因为 MaterialApp 中包含一些通过 context 传递的参数，相当于一个默认值。

注意，使用 Flutter 框架不意味着必须要使用 Material Design，因为 Flutter 本身已经提供了足够多的基础组件，但使用 Material Design 会省去很多麻烦，极大提高开发应用的效率。在后续的学习过程中，我们也不区分 Flutter 原生的组件与 Material Design 的组件。

### 设置项

MaterialApp 中也有一些可以配置的项，比如：设置主题（theme）、应用的地域（locale）；一些与调试相关的选项：`debugShowCheckedModeBanner` 是否显示右上角的调试按钮、`debugShowMaterialGrid` 显示网格、`showSemanticsDebugger` 调试。

## 常用展示组件

### 嵌入式

- [Text](https://api.flutter.dev/flutter/widgets/Text-class.html)
    - [DefaultTextStyle](https://api.flutter.dev/flutter/widgets/DefaultTextStyle-class.html)
    - [RichText](https://api.flutter.dev/flutter/widgets/RichText-class.html)
- [Icon](https://api.flutter.dev/flutter/widgets/Icon-class.html)
- [Image](https://api.flutter.dev/flutter/widgets/Image-class.html)
- [Placeholder](https://api.flutter.dev/flutter/widgets/Placeholder-class.html)

### 弹出式

- [SnackBar](https://api.flutter.dev/flutter/material/SnackBar-class.html)
    - 在屏幕下方呈现的简短信息条，可以添加一些辅助按钮
- [AlertDialog](https://api.flutter.dev/flutter/material/AlertDialog-class.html)
    - 在屏幕中间呈现的警告，让用户确认或取消其操作
- [BottomSheet](https://api.flutter.dev/flutter/material/BottomSheet-class.html)
    - 在屏幕下方呈现多个操作栏目
- [SimpleDialog](https://api.flutter.dev/flutter/material/SimpleDialog-class.html)
    - 用于解释或提供一个用户输入的界面

### 信息呈现

- [Card](https://api.flutter.dev/flutter/material/Card-class.html)
- [Chip](https://api.flutter.dev/flutter/material/Chip-class.html)
- [DataTable](https://api.flutter.dev/flutter/material/DataTable-class.html)

### 进度展示

- [CircularProgressIndicator](https://api.flutter.dev/flutter/material/CircularProgressIndicator-class.html)
- [LinearProgressIndicator](https://api.flutter.dev/flutter/material/LinearProgressIndicator-class.html)

### 几何形状

绘制形状可以参考 [Canvas](https://api.flutter.dev/flutter/dart-ui/Canvas-class.html) 和 [CustomPainter](https://api.flutter.dev/flutter/rendering/CustomPainter-class.html)，继承 CustomPainter 之后重载 `paint()` 并在其中调用 Canvas 的 `drawLine` `drawArc()` `drawCircle()` `drawPath()` 等方法在 Canvas 上绘制形状。

## 常用交互组件

### 文字输入

[TextField](https://api.flutter.dev/flutter/material/TextField-class.html)

### 选择

- [Switch](https://api.flutter.dev/flutter/material/Switch-class.html)
    - 开关
- [Radio](https://api.flutter.dev/flutter/material/Radio-class.html)
    - 单选
- [Checkbox](https://api.flutter.dev/flutter/material/Checkbox-class.html)
    - 多选
- [Slider](https://api.flutter.dev/flutter/material/Slider-class.html)
    - 滑动条
- [showDatePicker()](https://api.flutter.dev/flutter/material/showDatePicker.html)
    - 日期时间选择器

### 按钮

我们可以使用 [GestureDetector](https://api.flutter.dev/flutter/widgets/GestureDetector-class.html) 来对一个 Widget 添加手势检测，并添加对应的回调函数 `onTap()` 来实现用户点击后的逻辑：

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
    title: "Learn Flutter",
    home: Scaffold(
      body: MyWidget(),
    ),
  ));
}

class MyWidget extends StatelessWidget {
  const MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
        child: GestureDetector(
            onTap: () {
              debugPrint("pressed");
            },
            child: Text("Hello, world!", style: TextStyle(fontSize: 48))));
  }
}
```

### 带样式的按钮

- [TextButton](https://api.flutter.dev/flutter/material/TextButton-class.html)
    - 样式最少的  Material 按钮
    - child 只要是 Widget 就可以，但为 Text 时显示效果最好
- [OutlinedButton](https://api.flutter.dev/flutter/material/OutlinedButton-class.html) 
    - TextButton 添加一个外框
- [IconButton](https://api.flutter.dev/flutter/material/IconButton-class.html)
    - 样式较少的 Material 图标按钮，可配置项很多
- [ElevatedButton](https://api.flutter.dev/flutter/material/ElevatedButton-class.html)
    - 圆角矩形纯色动效丰富的 Material 按钮
- [FloatingActionButton](https://api.flutter.dev/flutter/material/FloatingActionButton-class.html)
    - 作为应用的一个主要操作入口的悬浮按钮

### 其他手势

[GestureDetector](https://api.flutter.dev/flutter/widgets/GestureDetector-class.html)

在 Flutter 中，几乎所有的手势都可以用 GestureDetector 这个 Widget 来检测并执行回调函数。一些比较顶层的 Widget 的手势控制内部也是 GestureDetector 来实现的。

查看其构造函数：

![](image-widgets/gesturedetector-construct.png)

我们可以归类出如下的手势检测：

- onTap 点击
- onDoubleTap 双击
- onLongPress 长按
- onVerticalDrag onHorizontalDrag 拖动
- onPan 缩放

使用方法基本上是在 child 中传入组件添加手势检测区域，使用回调函数结合状态来对界面进行更新，状态相关的知识我们下节课会进行介绍。

### 菜单

- [DropdownButton](https://api.flutter.dev/flutter/material/DropdownButton-class.html)
- [PopupMenuButton](https://api.flutter.dev/flutter/material/PopupMenuButton-class.html)

## 常用布局组件

### 容器

- [Row](https://api.flutter.dev/flutter/widgets/Row-class.html) & [Column](https://api.flutter.dev/flutter/widgets/Column-class.html)
    - Expanded
    - Divider
- Stack
    - Positioned

### 相对位置

- [Container](https://api.flutter.dev/flutter/widgets/Container-class.html)
    - [Padding](https://api.flutter.dev/flutter/widgets/Padding-class.html)
- [SizedBox](https://api.flutter.dev/flutter/widgets/SizedBox-class.html)
- [Align](https://api.flutter.dev/flutter/widgets/Align-class.html)
    - [Center](https://api.flutter.dev/flutter/widgets/Center-class.html)
- [AspectRatio](https://api.flutter.dev/flutter/widgets/AspectRatio-class.html)

### 滑动

- [ListView](https://api.flutter.dev/flutter/widgets/ListView-class.html)
- [GridView](https://api.flutter.dev/flutter/widgets/GridView-class.html)
- [PageView](https://api.flutter.dev/flutter/widgets/PageView-class.html)
- [SingleChildScrollView](https://api.flutter.dev/flutter/widgets/SingleChildScrollView-class.html)

### 平面滑动

[InteractiveViewer](https://api.flutter.dev/flutter/widgets/InteractiveViewer-class.html) 提供了平面滚动，且支持了缩放的手势。参考 <https://stackoverflow.com/questions/71976826/simultaneous-2d-scrolling-in-body-of-scaffold-design-apps-miro-or-figma> 得到的案例如下：

```dart
import 'package:flutter/material.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: const Text("InteractiveViewer")),
        body: const MyWidget(),
      ),
    );
  }
}

class MyWidget extends StatelessWidget {
  const MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: InteractiveViewer(
        constrained: false,
        minScale: 0.1,
        maxScale: 2,
        child: Column(
          children: List.generate(100, (i) {
            return Row(
              children: List.generate(100, (j) {
                return Container(
                  width: 64,
                  height: 64,
                  decoration: BoxDecoration(border: Border.all()),
                  child: Center(child: Text("($i, $j)")),
                );
              }),
            );
          }),
        ),
      ),
    );
  }
}
```

### 获取布局信息

https://api.flutter.dev/flutter/widgets/MediaQuery-class.html  主要是长和宽什么的

## 常用的导航组件

- [AppBar](https://api.flutter.dev/flutter/material/AppBar-class.html)
- [BottomNavigationBar](https://api.flutter.dev/flutter/material/BottomNavigationBar-class.html)
- [Drawer](https://api.flutter.dev/flutter/material/Drawer-class.html)
- [TabBarView](https://api.flutter.dev/flutter/material/TabBarView-class.html)

### Navigator

要使用 Navigator，应用需要有 MaterialApp 或 CupertinoApp 包裹，它们都使用了 WidgetsApp 来提供导航。

https://docs.flutter.dev/development/ui/navigation 讲 Navigator 的用法（什么 deep linking 不讲）

## References

- https://docs.flutter.dev/development/ui/widgets-intro
- https://docs.flutter.dev/development/ui/assets-and-images
- https://docs.flutter.dev/development/ui/interactive
- https://docs.flutter.dev/development/ui/advanced/gestures

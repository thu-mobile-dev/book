# Widgets

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

编写一个 Flutter 应用，我们主要会使用 `StatelessWidget` 和 `StatefulWidget`。Widget 有着自己的状态时选择 `StatefulWidget`（下节课介绍），否则选择 `StatelessWidget`。`StatelessWidget` 最大的作用是封装成组，例如下面的例子：

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

`context`（类型 `BuildContext`） 在各个 Widgets 的层级结构中传递着一些参数，比如当前的显示状态、默认文字样式等等，有一点类似于全局变量。我们之后会用到。

## MaterialApp

Flutter 主要使用的设计语言是 Material Design，里面包含动画、主题、很多便捷的 UI/UX 组件，我们在第二节课已经介绍过。这一节课我们主要聚焦于如何在 Flutter 中使用 Material Design。

Material Design 在 Flutter 中最关键的两个 Widget 是 `MaterialApp` 和 `Scaffold`。

- `MaterialApp` 是一个最顶层的 Widget，如果你希望你的应用使用 Material Design，最好在 `runApp()` 中传入 `MaterialApp`。
    - 与 `MaterialApp` 类似，还有使用了 Human Interface Guidelines 设计的 `CupertinoApp` 用来向 iOS 用户提供与系统应用类似的体验。课程只关注 Material Design。
- `Scaffold` 可以直观理解为应用的一页，但更准确的理解是其可以为 Material 组件提供一个动画层，进一步呈现各种动画效果。

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

- 从应用结构上来说，使用 `MaterialApp` 包含一个 `Scaffold`，很容易看出这是一个单页的应用，结构更加清楚。
- 在 `Scaffold` 中添加了 `AppBar` 和 `floatingActionButton`，运行应用可以看到界面和较多现有应用保持一致，这有助于给用户带来熟悉感。
- 在动画效果上，`floatingActionButton` 和 `SnackBar` 的动画效果添加让界面看起来更流畅。
- 可以看到，在使用 `Text` 这个 Widget 时，我们不需要再指定文字方向和字体颜色了，这是因为 `MaterialApp` 中包含一些通过 `context` 传递的参数，相当于一个默认值。

注意，使用 Flutter 框架不意味着必须要使用 Material Design，因为 Flutter 本身已经提供了足够多的基础组件，但使用 Material Design 会省去很多麻烦，极大提高开发应用的效率。在后续的学习过程中，我们也不区分 Flutter 原生的组件与 Material Design 的组件。

### 设置项

`MaterialApp` 中也有一些可以配置的项，比如：设置主题（`theme`）、应用的地域（`locale`）；一些与调试相关的选项：`debugShowCheckedModeBanner` 是否显示右上角的调试按钮、`debugShowMaterialGrid` 显示网格、`showSemanticsDebugger` 调试。

## 常用展示组件

### 嵌入式

- [Icon](https://api.flutter.dev/flutter/widgets/Icon-class.html)
    - 显示图标
- [Text](https://api.flutter.dev/flutter/widgets/Text-class.html)
    - 显示纯文本
    - [DefaultTextStyle](https://api.flutter.dev/flutter/widgets/DefaultTextStyle-class.html)
        - 设置默认的文字样式。在使用 `MaterialApp` 时不推荐使用，因为 `MaterialApp` 有默认样式，通过主题来修改是更好的选择。
    - [RichText](https://api.flutter.dev/flutter/widgets/RichText-class.html)
        - 显示富文本。
- [Image](https://api.flutter.dev/flutter/widgets/Image-class.html)
    - 显示图片
- [Placeholder](https://api.flutter.dev/flutter/widgets/Placeholder-class.html)
    - 在开发中临时占位代表未添加的组件。

#### Icon

一种图标是 Flutter 框架自带的 Material Design 图标，在 [Icons 文档](https://api.flutter.dev/flutter/material/Icons-class.html) 中可以查看全部的图标。使用方法：

```dart
Icon(Icons.close)
```

另一种比较推荐的是 Human Interface Guidelines 中的图标，在 [CupertinoIcons 文档](https://api.flutter.dev/flutter/cupertino/CupertinoIcons-class.html) 中可以查看全部的图标。在 `pubspec.yaml` 的 `dependecies` 中添加 `cupertino_icons`。用法如下：

```dart
import 'package:cupertino_icons/cupertino_icons.dart';

Icon(CupertinoIcons.multiply)
```

图标有着非常简洁却又有着很高的表现力，非常推荐大家在寻找组件时优先在上面两个官方支持的图标库中挑选。

#### Text

TODO （H4小标题）添加 Text 的各种参数的意义，（H5小标题）解释size的计算

#### Image

TODO （H4小标题）介绍 Image 显示图片的多种方式（网络、本地路径、AssetBundle（引到[添加 Assets](./assets.md)））

### 弹出式

- [SnackBar](https://api.flutter.dev/flutter/material/SnackBar-class.html)
    - 在屏幕下方呈现的简短信息条，可以添加一些辅助按钮
- [BottomSheet](https://api.flutter.dev/flutter/material/BottomSheet-class.html)
    - 在屏幕下方呈现多个操作栏目
- [AlertDialog](https://api.flutter.dev/flutter/material/AlertDialog-class.html)
    - 在屏幕中间呈现的警告，让用户确认或取消其操作
- [SimpleDialog](https://api.flutter.dev/flutter/material/SimpleDialog-class.html)
    - 用于解释或提供一个用户输入的界面

#### SnackBar

![SnackBar](image-widgets/material-SnackBar.png)

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
      home: Scaffold(
    body: BodyWidget(),
  )));
}

class BodyWidget extends StatelessWidget {
  BodyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        ScaffoldMessenger.of(context)
            .showSnackBar(SnackBar(content: Text("Hello, world!")));
      },
    );
  }
}
```

#### BottomSheet

![BottomSheet](image-widgets/material-BottomSheet.png)

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
      home: Scaffold(
    body: BodyWidget(),
  )));
}

class BodyWidget extends StatelessWidget {
  const BodyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: ElevatedButton(
        child: const Text('showBottomSheet'),
        onPressed: () {
          Scaffold.of(context).showBottomSheet<void>(
            (BuildContext context) {
              return Container(
                height: 200,
                color: Colors.amber,
                child: Center(
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    mainAxisSize: MainAxisSize.min,
                    children: <Widget>[
                      const Text('BottomSheet'),
                      ElevatedButton(
                        child: const Text('Close BottomSheet'),
                        onPressed: () {
                          Navigator.pop(context);
                        },
                      ),
                    ],
                  ),
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```


#### AlertDialog

![AlertDialog](image-widgets/material-AlertDialog.png)

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
      home: Scaffold(
    body: BodyWidget(),
  )));
}

class BodyWidget extends StatelessWidget {
  BodyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        showDialog<void>(
          context: context,
          barrierDismissible: false, // user must tap button!
          builder: (BuildContext context) {
            return AlertDialog(
              title: const Text('AlertDialog Title'),
              content: SingleChildScrollView(
                child: Text('AlertDialog Demo'),
              ),
              actions: <Widget>[
                TextButton(
                  child: const Text('Cancle'),
                  onPressed: () {
                    Navigator.of(context).pop();
                  },
                ),
                TextButton(
                  child: const Text('OK'),
                  onPressed: () {
                    Navigator.of(context).pop();
                  },
                )
              ],
            );
          },
        );
      },
    );
  }
}
```

#### SimpleDialog

![SimpleDialog](image-widgets/material-SimpleDialog.png)

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
      home: Scaffold(
    body: BodyWidget(),
  )));
}

class BodyWidget extends StatelessWidget {
  const BodyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: OutlinedButton(
        onPressed: () => _dialogBuilder(context),
        child: const Text('Open Dialog'),
      ),
    );
  }

  Future<void> _dialogBuilder(BuildContext context) {
    return showDialog<void>(
      context: context,
      builder: (BuildContext context) {
        return SimpleDialog(
          title: const Text('Select your choice'),
          children: <Widget>[
            SimpleDialogOption(
              onPressed: () {
                Navigator.pop(context);
                debugPrint("chose A");
              },
              child: const Text('A'),
            ),
            SimpleDialogOption(
              onPressed: () {
                Navigator.pop(context);
                debugPrint("chose B");
              },
              child: const Text('B'),
            ),
          ],
        );
      },
    );
  }
}
```

### 信息呈现

- [Card](https://api.flutter.dev/flutter/material/Card-class.html)
- [Chip](https://api.flutter.dev/flutter/material/Chip-class.html)
- [DataTable](https://api.flutter.dev/flutter/material/DataTable-class.html)

#### Card

![Card](image-widgets/material-Card.png)

TODO

#### Chip

![Chip](image-widgets/material-Chip.png)

TODO

#### DataTable

![DataTable](image-widgets/material-DataTable.png)

TODO

### 进度展示

- [CircularProgressIndicator](https://api.flutter.dev/flutter/material/CircularProgressIndicator-class.html)
- [LinearProgressIndicator](https://api.flutter.dev/flutter/material/LinearProgressIndicator-class.html)

#### CircularProgressIndicator

![CircularProgressIndicator](image-widgets/material-CircularProgressIndicator.png)

TODO

#### LinearProgressIndicator

![LinearProgressIndicator](image-widgets/material-LinearProgressIndicator.png)

TODO

### 几何形状

绘制形状可以参考 [Canvas](https://api.flutter.dev/flutter/dart-ui/Canvas-class.html) 和 [CustomPainter](https://api.flutter.dev/flutter/rendering/CustomPainter-class.html)，继承 CustomPainter 之后重载 `paint()` 并在其中调用 Canvas 的 `drawLine` `drawArc()` `drawCircle()` `drawPath()` 等方法在 Canvas 上绘制形状。

## 常用交互组件

### 文字输入

[TextField](https://api.flutter.dev/flutter/material/TextField-class.html)

![TextField](image-widgets/material-TextField.png)

TODO 添加用法 如何获取文字内容

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

#### Switch

![Switch](image-widgets/material-Switch.png)

TODO

#### Radio

![Radio](image-widgets/material-Radio.png)

TODO

#### Checkbox

![Checkbox](image-widgets/material-Checkbox.png)

TODO

#### Slider

![Slider](image-widgets/material-Slider.png)

TODO

#### DatePicker

![DatePicker](image-widgets/material-DatePicker.png)

TODO

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

![TextButton](image-widgets/material-TextButton.png)
![OutlinedButton](image-widgets/material-OutlinedButton.png)
![IconButton](image-widgets/material-IconButton.png)
![ElevatedButton](image-widgets/material-ElevatedButton.png)
![FloatingActionButton](image-widgets/material-FloatingActionButton.png)

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

#### DropdownButton

![DropdownButton](image-widgets/material-DropdownButton.png)

TODO 简单说一下其中的参数和使用方法

#### PopupMenuButton

![PopupMenuButton](image-widgets/material-PopupMenuButton.png)

TODO 简单说一下其中的参数和使用方法

## 常用布局组件

### 容器

- [Row](https://api.flutter.dev/flutter/widgets/Row-class.html) & [Column](https://api.flutter.dev/flutter/widgets/Column-class.html)
    - 使用 children 横向和竖向组织 Widgets。
    - [Expanded](https://api.flutter.dev/flutter/widgets/Expanded-class.html)
        - 让 Row 和 Column 中的 Widget 占尽可能大的地方
    - [Divider](https://api.flutter.dev/flutter/material/Divider-class.html)
        - 水平分割线，可以自定义粗细、颜色、padding。
- [Stack](https://api.flutter.dev/flutter/widgets/Stack-class.html)
    - 使用 children 纵向组织 Widgets。
    - [Positioned](https://api.flutter.dev/flutter/widgets/Positioned-class.html)
        - 添加 Stack 中的 Widget 的位置信息。

#### Row & Column

![Row](image-widgets/Row.png)
![Column](image-widgets/Column.png)
![Divider](image-widgets/material-Divider.png)

TODO 结合 Icon 简单说一下用法（开启下面这个查看效果）
import 'package:flutter/rendering.dart';
debugPaintSizeEnabled = true;

#### Stack

![Stack](image-widgets/Stack.png)

TODO

### 相对位置

- [Container](https://api.flutter.dev/flutter/widgets/Container-class.html)
    - [Padding](https://api.flutter.dev/flutter/widgets/Padding-class.html)
- [SizedBox](https://api.flutter.dev/flutter/widgets/SizedBox-class.html)
- [Align](https://api.flutter.dev/flutter/widgets/Align-class.html)
    - [Center](https://api.flutter.dev/flutter/widgets/Center-class.html)
- [AspectRatio](https://api.flutter.dev/flutter/widgets/AspectRatio-class.html)

#### Container

![Container](image-widgets/Container.png)
![Padding](image-widgets/Padding.png)

TODO

#### SizedBox

![SizedBox](image-widgets/SizedBox.png)

TODO

#### Align

![Align](image-widgets/Align.png)
![Center](image-widgets/Center.png)

#### AspectRatio

![AspectRatio](image-widgets/AspectRatio.png)

TODO

### 滑动

- [ListView](https://api.flutter.dev/flutter/widgets/ListView-class.html)
- [GridView](https://api.flutter.dev/flutter/widgets/GridView-class.html)
- [PageView](https://api.flutter.dev/flutter/widgets/PageView-class.html)
- [SingleChildScrollView](https://api.flutter.dev/flutter/widgets/SingleChildScrollView-class.html)
    - 当单一元素过大无法一次性在屏幕上显示时，可以使用 SingleChildScrollView，用户可以通过横向或纵向移动的方法看到界面的全貌。

#### ListView

![ListView](image-widgets/ListView.png)

TODO

#### GridView

![GridView](image-widgets/GridView.png)

TODO

#### PageView

TODO

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

主要使用 [MediaQuery](https://api.flutter.dev/flutter/widgets/MediaQuery-class.html)。例如，从当前的 BuildContext 中获取屏幕宽度和高度（`double`）：

```dart
MediaQuery.of(context).size.width;
MediaQuery.of(context).size.height;
```

更多可以获取的值可以查看 [MediaQueryData 的文档](https://api.flutter.dev/flutter/widgets/MediaQueryData-class.html)。

## 常用的导航组件

首先要明确导航所做的事情是在不同的界面中进行跳转。

### Navigator

简单的导航方式使用一个栈来记录历史的页面，栈顶的页面始终为当前的界面。跳转到新的界面则需要在栈中 push 这个页面，返回之前的页面即弹出栈顶的当前页面。这种导航在 Flutter 中使用 `Navigator` 实现，一个页面为一个 `Route`。要使用 `Navigator`，应用需要有 `MaterialApp` 或 `CupertinoApp` 包裹，它们都使用了 `WidgetsApp` 来提供导航功能。

下面的案例来自 [Flutter Cookbook | Navigate to a new screen and back](https://docs.flutter.dev/cookbook/navigation/navigation-basics)：

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MaterialApp(
    title: 'Navigation Basics',
    home: FirstRoute(),
  ));
}

class FirstRoute extends StatelessWidget {
  const FirstRoute({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('First Route'),
      ),
      body: Center(
        child: ElevatedButton(
          child: const Text('Open route'),
          onPressed: () {
            Navigator.push(
              context,
              MaterialPageRoute(builder: (context) => const SecondRoute()),
            );
          },
        ),
      ),
    );
  }
}

class SecondRoute extends StatelessWidget {
  const SecondRoute({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Second Route'),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.pop(context);
          },
          child: const Text('Go back!'),
        ),
      ),
    );
  }
}
```

![](image-widgets/navigator-1.png)

![](image-widgets/navigator-2.png)

- 使用 `Navigator.push(context, MaterialPageRoute(builder: (context) => const SecondRoute()))` 来创建 Route 并跳转到新的界面。
- 使用 `Navigator.pop(context);` 返回上级界面。
- 可以看到，在第二个页面的 AppBar 中自动出现了一个返回的箭头，可以说这是 MaterialApp 的功劳，我们不需要额外配置；当然不使用 MaterialApp 也可以手动添加，但是相对麻烦。

### Material

Material Design 也提供了很多其他的导航方式：

- [AppBar](https://api.flutter.dev/flutter/material/AppBar-class.html)
    - 在 `AppBar` 中可以放置返回按钮等辅助导航。
- [BottomNavigationBar](https://api.flutter.dev/flutter/material/BottomNavigationBar-class.html)
    - 现代应用比较常见的一种导航方式，通过点击下方的按钮选择不同的界面进行呈现。
- [TabBarView](https://api.flutter.dev/flutter/material/TabBarView-class.html)
    - 与 `BottomNavigationBar` 类似，可以在上方提供 `Tab`s 进行导航。
- [Drawer](https://api.flutter.dev/flutter/material/Drawer-class.html)
    - 屏幕左侧滑出可以用来选择页面的组件。

#### AppBar

![AppBar](image-widgets/material-AppBar.png)

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
      home: Scaffold(
    appBar: AppBar(title: Text("AppBar")),
    body: BodyWidget(),
  )));
}

class BodyWidget extends StatelessWidget {
  const BodyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text("Hello, world!"),
    );
  }
}
```

#### BottomNavigationBar

![BottomNavigationBar](image-widgets/material-BottomNavigationBar.png)

```dart
import 'package:flutter/material.dart';

void main() => runApp(MaterialApp(
      home: MyStatefulWidget(),
    ));

class MyStatefulWidget extends StatefulWidget {
  const MyStatefulWidget({super.key});

  @override
  State<MyStatefulWidget> createState() => _MyStatefulWidgetState();
}

class _MyStatefulWidgetState extends State<MyStatefulWidget> {
  int _selectedIndex = 0;
  static const TextStyle optionStyle =
      TextStyle(fontSize: 30, fontWeight: FontWeight.bold);
  static const List<Widget> _widgetOptions = <Widget>[
    Text(
      'Index 0: Home',
      style: optionStyle,
    ),
    Text(
      'Index 1: Business',
      style: optionStyle,
    ),
    Text(
      'Index 2: School',
      style: optionStyle,
    ),
  ];

  void _onItemTapped(int index) {
    setState(() {
      _selectedIndex = index;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: _widgetOptions.elementAt(_selectedIndex),
      ),
      bottomNavigationBar: BottomNavigationBar(
        items: const <BottomNavigationBarItem>[
          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: 'Home',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.business),
            label: 'Business',
          ),
          BottomNavigationBarItem(
            icon: Icon(Icons.school),
            label: 'School',
          ),
        ],
        currentIndex: _selectedIndex,
        selectedItemColor: Colors.amber[800],
        onTap: _onItemTapped,
      ),
    );
  }
}
```

#### TabBarView

![TabBarView](image-widgets/material-TabBarView.png)

```dart
import 'package:flutter/material.dart';

void main() => runApp(const MaterialApp(
      home: MyStatelessWidget(),
    ));

const List<Tab> tabs = <Tab>[
  Tab(text: 'Zeroth'),
  Tab(text: 'First'),
  Tab(text: 'Second'),
];

class MyStatelessWidget extends StatelessWidget {
  const MyStatelessWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return DefaultTabController(
      length: tabs.length,
      // The Builder widget is used to have a different BuildContext to access
      // closest DefaultTabController.
      child: Builder(builder: (BuildContext context) {
        final TabController tabController = DefaultTabController.of(context);
        tabController.addListener(() {
          if (!tabController.indexIsChanging) {
            // Your code goes here.
            // To get index of current tab use tabController.index
          }
        });
        return Scaffold(
          appBar: AppBar(
            bottom: const TabBar(
              tabs: tabs,
            ),
          ),
          body: TabBarView(
            children: tabs.map((Tab tab) {
              return Center(
                child: Text(
                  '${tab.text!} Tab',
                  style: Theme.of(context).textTheme.headlineSmall,
                ),
              );
            }).toList(),
          ),
        );
      }),
    );
  }
}
```

#### Drawer

![Drawer](image-widgets/material-Drawer.png)

TODO 案例在电脑上无法验证

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MaterialApp(
      home: Scaffold(
    drawer: Drawer(
        child: ListView(
      padding: EdgeInsets.zero,
      children: const <Widget>[
        DrawerHeader(
          decoration: BoxDecoration(
            color: Colors.blue,
          ),
          child: Text(
            'Drawer Header',
            style: TextStyle(
              color: Colors.white,
              fontSize: 24,
            ),
          ),
        ),
        ListTile(
          leading: Icon(Icons.message),
          title: Text('Messages'),
        ),
        ListTile(
          leading: Icon(Icons.account_circle),
          title: Text('Profile'),
        ),
        ListTile(
          leading: Icon(Icons.settings),
          title: Text('Settings'),
        ),
      ],
    )),
    body: BodyWidget(),
  )));
}

class BodyWidget extends StatelessWidget {
  const BodyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text("Hello, world!"),
    );
  }
}
```

## References

- https://docs.flutter.dev/development/ui/widgets-intro
- https://docs.flutter.dev/development/ui/assets-and-images
- https://docs.flutter.dev/development/ui/interactive
- https://docs.flutter.dev/development/ui/advanced/gestures
- https://docs.flutter.dev/development/ui/navigation
- https://docs.flutter.dev/cookbook#navigation

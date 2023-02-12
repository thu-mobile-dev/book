# 作业

## 状态分类

### 知识点

- 状态

### 内容

下面列举了一些实际中可能遇到的状态，请对其进行分类，并在括号中写出理由。（分类并不绝对，言之有理即可。）

Widget 状态：`_____`，应用状态：`_____`，持久存储的数据：`_____`。

- A 时钟应用的闹钟列表
- B 相机应用中由摄像头实时捕捉的当前帧
- C 搜索引擎输入框中的文字
- D 相册中的图片
- E 用户的登录状态

### 提示

- 可以先判断需要持久存储的状态，这类状态在应用关闭、甚至重启之后都仍然保留。
- 接下来根据一个状态是否全局用到或是否短暂来决定是 Widget 状态还是应用状态。

## 秒表

### 知识点

- `Animation<double>` 中 `[0.0, 1.0]` 的映射关系
- `AnimationController` 的熟练使用

### 内容

只使用一个 `Animation<double>` 或 `AnimationController` 创建一个 60 秒转一圈的秒表。

- 秒表在界面呈现出来后就开始旋转。
- 当秒表达到 59 秒时，再过 1 秒后秒表应该继续渲染，而不应该像上课一样用户点击一下才转一下。

### 提示

- 考虑 `[0.0, 1.0]` 的映射关系，课堂上所讲的案例都是将 `[0.0, 1.0]` 映射到一秒，对应了 `2 * pi / 60` 的角度，在这项作业中，`[0.0, 1.0]` 应该映射为什么呢？想一想怎样的变化是一个完整的变化周期。
- 课堂上的案例我们使用 `GestureDetector` 让用户的点击事件来使得动画开始，在这项作业中，你可以使用 `AnimationController` 的 `repeat()` 方法来使得秒表一直旋转。
- 为了方便测试，你可以将秒表的初值设置为靠近为 `60` 的数字，如 `55`。

### 样例代码

```dart
import 'package:flutter/material.dart';
import 'dart:math';
// import 'package:flutter/rendering.dart' show debugPaintSizeEnabled;

void main() {
  // debugPaintSizeEnabled = true;

  runApp(const MaterialApp(
      home: Scaffold(
          body: Center(
    child: ClockView(seconds: 55),
  ))));
}

class ClockView extends StatelessWidget {
  final double seconds;

  const ClockView({super.key, required this.seconds});

  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(builder: (context, constraints) {
      final size = constraints.maxHeight * 0.5;

      return Stack(alignment: Alignment.topCenter, children: [
        ClockBorderView(
          size: size,
          ringWidth: size * 0.06,
        ),
        ClockHandView(
          size: size,
          handWidth: size * 0.03,
          seconds: seconds,
        ),
      ]);
    });
  }
}

class ClockHandView extends StatelessWidget {
  final double size;
  final double handWidth;
  final double seconds;

  const ClockHandView(
      {super.key,
      required this.size,
      required this.seconds,
      required this.handWidth});

  @override
  Widget build(BuildContext context) {
    return Transform.rotate(
      alignment: Alignment.bottomCenter,
      angle: seconds / 60 * 2 * pi,
      child: SizedBox(
        height: size * 0.5,
        width: handWidth,
        child: VerticalDivider(color: Colors.black, thickness: handWidth),
      ),
    );
  }
}

class ClockBorderView extends StatelessWidget {
  final double size;
  final double ringWidth;

  const ClockBorderView(
      {super.key, required this.size, required this.ringWidth});

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      height: size,
      width: size,
      child: DecoratedBox(
        decoration: BoxDecoration(
          shape: BoxShape.circle,
          border: Border.fromBorderSide(
            BorderSide(width: ringWidth, color: Colors.black),
          ),
        ),
      ),
    );
  }
}
```

## 发帖按钮

### 知识点

- Flow
- 动画

### 内容

使用 [Flow](https://api.flutter.dev/flutter/widgets/Flow-class.html) 和 [FlowDelegate](https://api.flutter.dev/flutter/rendering/FlowDelegate-class.html) 创建一个发帖按钮：

- 在用户未点击时，呈现一个「加号」
- 在用户点击「加号」时，加号变为「叉号」，在加号的左上方、正上方、右上方弹出三个图标按钮，分别表示「发布文字」、「发布图片」、「发布视频」。

### 提示

- 可以参考 [YouTube | Flow (Flutter Widget of the Week)](https://www.youtube.com/watch?v=NG6pvXpnIso) 中展示的案例。
- 图标
    - 加号 `Icon(Icons.add)`
    - 叉号 `Icon(Icons.close)`
    - 文字 `Icon(Icons.article_outlined)`
    - 图片 `Icon(Icons.mms_outlined)`
    - 视频 `Icon(Icons.video_camera_back_outlined)`

## 可选择图片的 photo_filter

### 知识点

- 第三方包的寻找和使用

### 内容

在案例 photo_filter 中，我们在 `assets/photo.jpg` 中加入了一张图片，并在代码中使用这种图片作为背景。现在希望加入插件，使得用户可以通过一个摄像头按钮调用系统相机拍照或通过一个相册按钮选择系统图库中的一张图片来更换背景图片。

### 提示

- 你可能需要一些 Android 或 iOS 的原生开发知识以修改一些工程文件来取得调用摄像头或访问系统图库的权限。
- 可选用的第三方库 [Flutter | Cookbook: Take a picture using the camera](https://docs.flutter.dev/cookbook/plugins/picture-using-camera)

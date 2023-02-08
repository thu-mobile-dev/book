# 作业

- 分类题：哪些数据属于 widget 状态、app 状态、持久存储的数据
- 找几个比较炫酷的 Animation 的例子，让同学实现一下
- 试着用 https://docs.flutter.dev/cookbook/plugins/picture-using-camera 更新上课讲的图片滤镜案例，而不是使用一张固定的图片

## 秒表

### 知识点

- `Animation<double>` 中 `[0.0, 1.0]` 的映射关系
- `AnimationController` 的熟练使用

### 要求

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

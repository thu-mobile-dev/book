# 动画

## 什么是动画

什么是动画？这个问题非常重要。但是答案却很简单，动起来就是动画。那么什么是动起来呢？

- 屏幕刷新的新一帧界面和上一帧界面不一样，画面就动了起来，可以说这是广义的动画。
- 现在的设备普遍支持 60 帧每秒的刷新率，由于人眼的视觉暂留效果，当屏幕以 60 帧每秒的刷新率刷新，人看起来感觉很流畅。这可以说是狭义的动画，重要是流畅。

## StatefulWidget 刷新

在第四课入门中，我们其实已经讲过，Flutter 为声明式的 UI 框架，声明式的核心是下面的这个公式：

```
UI = f(states)
```

对于动画来说，这个公式中有用的点为：`states` 改变时，`UI` 改变。简单来说，我们想要让画面动起来，只需要让 `states` 动起来即可。所以，在 Flutter 中做动画，使用 `StatefulWidget` 是完全足够的。

### 钟表秒针案例

我们来制作一个钟表来感受通过改变数值来改变对动画的刷新。案例中的钟表只显示秒针，每当用户点击，秒数加一，秒针往前走一格。

注：本节课的所有钟表秒针案例代码可以在 [GitHub | thu-flutter-dev/running_clock](https://github.com/thu-flutter-dev/running_clock) 的 `codes/` 文件夹中获取，复制粘贴至 `lib/main.dart` 中即可运行查看效果。

#### 无状态

我们先构建出来 `ClockView`，其接受一个参数 `seconds`，根据秒数来决定屏幕上秒针的位置，如 `ClockView(seconds: 45)`。

```dart
import 'package:flutter/material.dart';
import 'dart:math';
// import 'package:flutter/rendering.dart' show debugPaintSizeEnabled;

void main() {
  // debugPaintSizeEnabled = true;

  runApp(const MaterialApp(
      home: Scaffold(
          body: Center(
    child: ClockView(seconds: 45),
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

- `ClockBorderView` 使用 `DecoratedBox` 的边框绘制一个圆形
- `ClockHandView` 使用 `VerticalDivider` 绘制秒针
- `ClockView` 中使用 `Stack(alignment: Alignment.topCenter, ...)` 来将秒针指向正上方
- `ClockHandView` 使用 `Transform.rotate` 来确定秒针位置
- 查看布局情况，可以将 `main()` 中的 `debugPaintSizeEnabled = true;` 取消注释。

![](images-animation/clock.png){width="300"}

#### 有状态

要想让界面动起来，我们需要有一个状态来存储秒数，还需要一种方式来改变秒数的值。我们需要一个 `StatefulWidget`。

注：下面的代码省去 `ClockView` `ClockHandView` `ClockBorderView`。

```dart
import 'package:flutter/material.dart';
import 'dart:math';
// import 'package:flutter/rendering.dart' show debugPaintSizeEnabled;

void main() {
  // debugPaintSizeEnabled = true;

  runApp(const MaterialApp(
      home: Scaffold(
          body: Center(
    child: ClickableClickView(),
  ))));
}

class ClickableClickView extends StatefulWidget {
  const ClickableClickView({super.key});

  @override
  State<ClickableClickView> createState() => _ClickableClickViewState();
}

class _ClickableClickViewState extends State<ClickableClickView> {
  int seconds = 0;

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
        onTap: () {
          setState(() {
            seconds += 1;
          });
        },
        child: ClockView(seconds: seconds.toDouble()));
  }
}
```

- `ClickableClickView` 有一个状态 `seconds`，在 `GestureDetector.onTap()` 中我们在 `setState()` 中对其进行修改，这样 `ClockView` 会得到刷新。

这样我们就实现了一个最简单的动画，用户点击，界面变化。

但是现在的界面有一个问题，相邻的秒之间是跳变的，也就是说，每次用户点击秒针旋转的时候，我们都觉得不流畅。要想让这种变化流畅起来，我们需要每秒 60 帧的动画，下面我们就会提到。

## 流畅刷新（Explicit Animation）

注：这里我们在标题中引入 Explicit Animation，但是后面讲到 Implicit Animation 是才会对比说明意思。这里暂时先只考虑「流畅刷新」。

这一部分的三个重点是 `Animation<double>` `AnimationController` `AnimatedWidget`。可以先查看它们的继承关系：

- `Object` > `Listenable` > `Animation<double>` > `AnimationController`
- `Object` > `Widget` > `StatefulWidget` > `AnimatedWidget`

### Animation

可以看到，`Animation<double>` 继承自 `Listenable`。

- `Listenable`（`abstract class Listenable`）是一个抽象类，存储着「接收者（`listeners`）」列表，方法有 `addListener()` 和 `removeListener()`。
- `ValueListenable<T>`（`abstract class ValueListenable<T> extends Listenable`）是 `Listenable` 的一个子类，添加了一个值为 `T value`，当 `value` 发生改变的时候，`listeners` 得到通知。
- `Animation<T>`（`abstract class Animation<T> extends Listenable implements ValueListenable<T>`）结合两者，添加了一个属性 `AnimationStatus status`，其取值有 `dismissed`（动画未开始） `forward`（动画正在前进） `reverse`（动画正在后退） `completed`（动画已完成）。当 `T value` 或者 `AnimationStatus status` 发生改变时，订阅的 `listeners` 会收到通知。

可以说，`Animation<T>` 类似一个状态，当这个状态（`value` 和 `status`）的值发生改变时，使用这个状态的类（如 `Widget`）会收到改变的通知，从而可以重新渲染刷新界面。

一般动画随着时间变化，所以最常用的动画是 `Animation<double>`，其中 `value` 的取值为 `[0.0, 1.0]`。这相当于一个时间轴，`0.0` 表示动画还没开始，`1.0` 表示动画已经结束。

### AnimationController

在实际使用动画时，我们会使用 `Animation<T>` 的子类 `AnimationController`，因为其中包含了驱动 `Animation<T>`。如果说 `Animation<double>` 是时间轴，那么 `AnimationController` 就有着让时间前进或者后退的控制器。我们之后会重点关注 `AnimationController`，其重要的属性和方法如下：

- 属性
    - `double value` 动画的值
    - `AnimationStatus status` 动画的状态
    - `double lowerBound` `value` 的最小值，默认 0.0
    - `double upperBound` `value` 的最大值，默认 1.0
    - `Duration? duration` 动画的持续时间
- 方法
    - `forward({double? from}) → TickerFuture` 开始正向的动画
    - `animateTo(double target, {Duration? duration, Curve curve = Curves.linear}) → TickerFuture` 开始正向的动画
    - `reverse({double? from}) → TickerFuture` 开始反向的动画
    - `animateBack(double target, {Duration? duration, Curve curve = Curves.linear}) → TickerFuture` 开始正向的动画
    - `repeat({double? min, double? max, bool reverse = false, Duration? period}) → TickerFuture` 循环播放动画
    - `stop({bool canceled = true}) → void`
    - `reset() → void`

注：这里所说的「开始动画」，不是我们一般说的屏幕动起来，这里还没有提到任何屏幕刷新的事情。「动画」指的是 `Animation<T>` 也就是从 0.0 到 1.0 变化的一个值。「开始动画」指的是这个值开始变化（增加或减少）。

`AnimationController` 的构造函数如下：

```dart
AnimationController({double? value, Duration? duration, Duration? reverseDuration, String? debugLabel, double lowerBound = 0.0, double upperBound = 1.0, AnimationBehavior animationBehavior = AnimationBehavior.normal, required TickerProvider vsync})
```

可以看到必须要传入的参数有 `TickerProvider vsync`。对于这个参数的解释如下：对于连续刷新的动画，每秒要做到刷新 60 帧、或者 120 帧，这取决于设备屏幕的情况。我们需要有一个触发器来在每帧实际呈现在屏幕上之前、还在渲染的时候，告诉 `AnimationController` 让它根据当前的 `value` 和 设置的 `duration` 来计算出下一帧的 `value`。这个触发器就是 `vsync`。（`Ticker` 的作用是每一帧都会进行一次触发。）

关于 `vsync` 的获取，官方提供的方法是：

> If you are creating an `AnimationController` from a State, then you can use the `TickerProviderStateMixin` and `SingleTickerProviderStateMixin` classes to obtain a suitable `TickerProvider`. 

也就是说，我们可以在 `StatefulWidget` 中直接拿到这个值，下面的案例会进行演示。

### AnimatedWidget

刚刚我们也提到，`AnimationController` 控制一个值增大或减小，但是我们怎么将这个值呈现在屏幕上呢？当然是需要使用一个 `StatefulWidget`，不过我们这里可以方便的使用 `AnimatedWidget`。构造函数如下：

```dart
AnimatedWidget({Key? key, required Listenable listenable})
```

注：通过继承关系 `Object` > `Widget` > `StatefulWidget` > `AnimatedWidget` 和 `Object` > `Widget` > `StatefulWidget` > `AnimatedBuilder` 可以看到，`AnimatedWidget` 和 `AnimatedBuilder` 的地位相近，作用是相似的，只是写法不同。

可以看到我们需要传入一个 `Lisenable`，当 `Lisenable` 改变时，`AnimatedWidget` 就可以接收到，从而刷新界面。

结合上面所说的继承关系：`Object` > `Listenable` > `Animation<double>` > `AnimationController`，我们可以将 `AnimationController` 传入 `AnimatedWidget`。

### 流畅的钟表秒针案例

刚刚的案例中，相邻两秒的秒针渲染很生硬。我们接下来会在相邻两秒的秒针旋转之间添加一秒的动画，来模拟实际旋转的情况。

注：下面的代码省去 `ClockView` `ClockHandView` `ClockBorderView`。

```dart
import 'package:flutter/material.dart';
import 'dart:math';
// import 'package:flutter/rendering.dart' show debugPaintSizeEnabled;

void main() {
  // debugPaintSizeEnabled = true;

  runApp(const MaterialApp(
      home: Scaffold(
          body: Center(
    child: ClickableAnimatedClockView(),
  ))));
}

class ClickableAnimatedClockView extends StatefulWidget {
  const ClickableAnimatedClockView({super.key});

  @override
  State<ClickableAnimatedClockView> createState() =>
      _ClickableAnimatedClockViewState();
}

class _ClickableAnimatedClockViewState extends State<ClickableAnimatedClockView>
    with TickerProviderStateMixin {
  late int seconds;
  late final AnimationController controller;

  @override
  void initState() {
    super.initState();
    seconds = 0;
    controller = AnimationController(
      duration: const Duration(seconds: 1),
      vsync: this,
    );
  }

  @override
  void dispose() {
    controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        debugPrint("user tapped");
        debugPrint("current animation status: ${controller.status}");
        switch (controller.status) {
          case AnimationStatus.dismissed:
            debugPrint("start animation for 1 second");
            controller.forward().whenComplete(() {
              debugPrint("completed");
              setState(() {
                seconds += 1;
                if (seconds == 60) {
                  seconds = 0;
                }
              });
              controller.reset();
            });
            break;
          default:
        }
      },
      child: AnimatedClockView(
        controller: controller,
        intSeconds: seconds,
      ),
    );
  }
}

class AnimatedClockView extends AnimatedWidget {
  final int intSeconds;

  const AnimatedClockView({
    super.key,
    required AnimationController controller,
    required this.intSeconds,
  }) : super(listenable: controller);

  double get seconds => intSeconds + (listenable as Animation<double>).value;

  @override
  Widget build(BuildContext context) {
    return ClockView(seconds: seconds);
  }
}
```

我们从上往下看：

- `AnimatedClockView` 继承了 `AnimatedWidget`。
    - 接受参数 `AnimationController controller` 用于初始化 `AnimatedWidget.listenable`，接受参数 `intSeconds` 用于初始化自身的 `intSeconds`。每当 `lisenable` 改变时（`controller` 发出通知），`AnimatedWidget` 重新绘制界面。
    - 在 `build()` 中，向 `ClockView` 传递的 `seconds` 是由 `intSeconds` 和 `controller.value` 相加得到的值。
- `ClickableAnimatedClockView` 是一个 `StatefulWidget`，状态有 `int seconds` 和 `AnimationController controller`。
    - `class _ClickableAnimatedClockViewState extends State<ClickableAnimatedClockView> with TickerProviderStateMixin` 使得在 `AnimationController` 初始化时可以直接将 `this` 传入 `vsync`，这是官方推荐的获取 `TickerProvider` 的做法。使用 `switch-case` 语句对 `controller.status` 进行判断，只有当状态为 `AnimationStatus.dismissed` 时用户的点击才有效：使用 `controller.forward()` 让动画开始，当动画结束时，对 `seconds` 加一，将动画 `reset()`。

这样我们再进行点击，可以看到相邻两秒内的动画非常流畅，时间间隔也确实是一秒。

### FooTransition

查看继承关系：

`Object` > `DiagnosticableTree` > `Widget` > `StatefulWidget` > `AnimatedWidget` > `FooTransition`

可以看到，`FooTransition` 与 `AnimatedWidget` 的作用基本一致，但使用起来更方便，代码的层级关系和功能也更容易看出。

你可以在 [`AnimatedWidget` 的文档](https://api.flutter.dev/flutter/widgets/AnimatedWidget-class.html) 中找到 Flutter 内置的所有 `FooTransition`。

我们完全可以用 [`RotationTransition`](https://api.flutter.dev/flutter/widgets/RotationTransition-class.html)（`RotationTransition({Key? key, required Animation<double> turns, Alignment alignment = Alignment.center, FilterQuality? filterQuality, Widget? child})`）改写上面的案例，将 `ClockHandView` 用 `RotationTransition` 包裹，将 `AnimationController` 转为 `Animation` 传入 `turns` 参数。

这里不提供具体的代码实现，在当前的案例中 `RotationTransition` 的意义不大，需要逐层传递 `animation` 或 `controller` 与直接传递 `seconds` 的思路是一样的。

## Implicit Animation

上面讲的所有内容，其实只是 Flutter 中众多动画的一种：Explicit Animation。那么它明确（explicit）在哪里呢？我们使用 `AnimationController` 来使动画前进或停止，这是由开发者指定的。另一种动画，Implicit Animation，则通过 Flutter 框架本身来操作动画的进展，也就是说，并不再需要开发者手动创建管理 `AnimationController` 了。

### 自动的钟表秒针案例

注：下面的代码省去 `ClockView` `ClockHandView` `ClockBorderView`。

```dart
import 'package:flutter/material.dart';
import 'dart:math';
// import 'package:flutter/rendering.dart' show debugPaintSizeEnabled;

void main() {
  // debugPaintSizeEnabled = true;

  runApp(const MaterialApp(
      home: Scaffold(
          body: Center(
    child: ClickableClickView(),
  ))));
}

class ClickableClickView extends StatefulWidget {
  const ClickableClickView({super.key});

  @override
  State<ClickableClickView> createState() => _ClickableClickViewState();
}

class _ClickableClickViewState extends State<ClickableClickView> {
  double seconds = 0.0;

  @override
  Widget build(BuildContext context) {
    return TweenAnimationBuilder<double>(
      tween: Tween<double>(begin: 0, end: seconds),
      duration: const Duration(seconds: 1),
      builder: (BuildContext context, double tweenSeconds, Widget? child) {
        return GestureDetector(
            onTap: () {
              setState(() {
                seconds += 1;
              });
            },
            child: ClockView(seconds: tweenSeconds));
      },
    );
  }
}
```

- 使用 `TweenAnimationBuilder`，其中 `Tween` 可以理解开头和结尾的两个值，`TweenAnimationBuilder` 负责将这两个值插值，将得到的值传递给 `builder` 的 `double`，案例中命名为 `tweenSeconds`，将这个插值得到的值直接传给 `ClockView`。

运行看到效果和上面的 Explicit Animation 一模一样，但是代码量急剧减少。事实上，当需要对动画精确控制时，我们才会需要考虑 Explicit Animation，大多数的动画，只是给出一个初值、一个结束值、动画的持续时间，然后框架自动触发这个动画就好了。Implicit Animation 对平凡的开发者来说是应用动画的一大福音。

### AnimatedFoo

基于下面的继承关系：

`Object` > `Widget` > `StatefulWidget` > `ImplicitlyAnimatedWidget` > `AnimatedFoo`

完全可以用 [`AnimatedRotation`](https://api.flutter.dev/flutter/widgets/AnimatedRotation-class.html) 来对上面的案例进行改写，课程不呈现这部分代码。在大多数的时间里中，使用 Flutter 已有的 `AnimatedFoo` 能够使代码更加简洁清晰。

你可以在 [`ImplicitlyAnimatedWidget` 的文档](https://api.flutter.dev/flutter/widgets/ImplicitlyAnimatedWidget-class.html) 中找到 Flutter 内置的所有 `AnimatedFoo`。

## Curve

我们之前提到 `Animation<double>` 从 0.0 到 1.0，使用 `AnimationController` 让其前进，通过 `animation.value` 取出其值。在这个过程中，从 0.0 到 1.0 的过程是均匀的，也就是说，变化的速度为 `(1.0 - 0.0) / controller.duration`。这被成为线性动画。

现实中的动画从 0.0 到 1.0 则有着多种变化方式，在 Flutter 中，[`Curve`](https://api.flutter.dev/flutter/animation/Curve-class.html) 表述这种变化，你可以在 [`Curves`](https://api.flutter.dev/flutter/animation/Curves-class.html) 中看到多种变化的曲线，在代码中可以进行添加。

你也可以自定义 `Curve`，比如下面的代码（来自 [YouTube | Animation Basics with Implicit Animations](https://youtu.be/IVTjpW3W33s)）：

```dart
class SineCurve extends Curve {
  final double count;
  SineCurve({this.count = 1});
  @override
  double transformInternal(double t) {
    return sin(count * 2 * pi * t) * 0.5 + 0.5;
  }
}
```

## 其他动画

也有一些比较常用的动画，Flutter 框架做了适当的封装。比如 Hero animations 和 Staggered Animations，同学可以查看 [Flutter Animations | Common animation patterns](https://docs.flutter.dev/development/ui/animations#common-animation-patterns) 获取更多信息。

### Hero Animation

Hero Animation 从效果上来说，就是 macOS Keynote（如果有同学用过） 的 Magic Move。当两个场景有相同元素的时候，使用 Hero Animation 可以在两个场景之间流畅转换。

一个简单的使用场景是，用户点击图库中的一张图片，然后图片放大铺满全屏幕。在这个场景中，图库中的图片需要用 `Hero` 包裹起来，全屏幕的图片也用 `Hero` 包裹起来，给它们添加相同的 `tag`，在页面切换时 Flutter 会帮你完成转换的动画。

### Staggered Animation

直观来说，Staggered Animation 类似分段函数，在 `Animation<double>` 0.0 到 1.0 的时间段中使用不同的曲线对不同的属性进行修改。感兴趣的同学可以查看 [Flutter Animations | Staggered animations](https://docs.flutter.dev/development/ui/animations/staggered-animations)

## 绘制类动画

对于 Explicit Animation 和 Implicit Animation，它们都是针对 Widget 的一些属性进行插值得到流畅的动画，但是如果一些动画很难通过 Widget 进行表达，那么在它就脱离了 Flutter 动画的框架。不过 Flutter 也提供了底层的动画接口，开发者可以通过使用一些第三方包来得到绘制类动画。课程不对此类动画做要求。

## 学习资源

这一节课讲动画比较抽象，同学可以查看 [Flutter 官方推出的系列视频教程](https://www.youtube.com/playlist?list=PLjxrf2q8roU2v6UqYlt_KPaXlnjbYySua) 来复习课程中提到的内容。里面还提到了动画选取的方法。

[官方文档的动画部分](https://docs.flutter.dev/development/ui/animations)，同学们也可以阅读，加深对动画的理解。

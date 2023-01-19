# Flutter 插件

## 什么是插件

TODO

- 一些常用的库函数（如数学库）
- 一些常用的 widgets
- 为了让跨平台的 Flutter 能够调用各种操作系统的功能

## 使用方法

TODO 或者用 flutter pub add 或者在 .yml 中添加

## 常用插件

### Flutter Favorite packages

TODO 说明 https://docs.flutter.dev/development/packages-and-plugins/favorites

[Flutter Favorite packages](https://pub.dev/packages?q=is%3Aflutter-favorite) 现在（2022.1.19）有 67 个

### 个人推荐

TODO 可以简单分个类

#### cupertino_icons

<https://pub.dev/packages/cupertino_icons>

```dart
import 'package:flutter/cupertino.dart';
```

```dart
Icon(
  CupertinoIcons.heart_fill,
)
```

可以在 <https://api.flutter.dev/flutter/cupertino/CupertinoIcons-class.html#constants> 查看所有的图标。

#### google_fonts

TODO

## 创建插件

TODO https://docs.flutter.dev/development/packages-and-plugins/developing-packages

### 实践

TODO 创建一个插件，可以获取当前设备的电量，至少支持 Android 和 iOS，进一步支持 Windows 和 macOS，查看网页端是否支持该功能。并发布到 pub.dev 上。

## Flutter 嵌入原生应用（optional）

TODO https://docs.flutter.dev/development/add-to-app

必要性不是那么大，对于规模非常大的互联网企业来说，直接使用原生开发就可以了。对于小规模的应用开发团队而言，以 Flutter 为主体，嵌入原生的一些组件应该是更好的选择。这里所说的嵌入原生组件，其实就是上面所说的「插件」。

# 插件

## 什么是插件

TODO

- 一些常用的库函数（如数学库）
- 一些常用的 widgets
- 为了让跨平台的 Flutter 能够调用各种操作系统的功能
- 为了让 Flutter 调用原生平台的一些组件，减少重复造轮子的必要

## 使用方法

TODO 或者用 flutter pub add 或者在 .yml 中添加

### 版本

TODO semetic version

## 常用包

### Flutter Favorite packages

TODO 说明 https://docs.flutter.dev/development/packages-and-plugins/favorites

[Flutter Favorite packages](https://pub.dev/packages?q=is%3Aflutter-favorite) 现在（2022.3.7）有 67 个

### Flutter Package of the Week

Flutter 官方在 YouTube 开设了 [Flutter Package of the Week](https://www.youtube.com/playlist?list=PLjxrf2q8roU1quF6ny8oFHJ2gBdrYN_AK) 这一合集，其中介绍了很多常用的第三方包，能够极大提高开发效率；课程中用到的很多包也都包含在其中。每一集时间不长，感兴趣的同学应该可以很快看完。

## 创建插件

TODO https://docs.flutter.dev/development/packages-and-plugins/developing-packages

### 实践

TODO 创建一个插件，可以获取当前设备的电量、充电情况，至少支持 Android 和 iOS，进一步支持 Windows 和 macOS，查看网页端是否支持该功能。并发布到 pub.dev 上。

## 原生中添加 Flutter 部分（optional）

如果你想将现有的应用使用 Flutter 改写，但是并不希望改写全部，那么可以将一些组件替换为 Flutter。具体可以查看 [Flutter | Add Flutter to existing app](https://docs.flutter.dev/development/add-to-app)。

但个人感觉必要性不是那么大，对于规模非常大的互联网企业来说，如果需要使用原生开发，直接全部使用原生开发就可以了。对于小规模的应用开发团队而言，以 Flutter 为主体开发更合适。

如果确实有一些功能 Flutter 无法实现或者重复造轮子，嵌入原生的一些组件应该是更好的选择。这里所说的嵌入原生组件，其实就是上面所说的「插件」，对应官网文档 [Flutter | Platform integration](https://docs.flutter.dev/development/platform-integration) 的部分。

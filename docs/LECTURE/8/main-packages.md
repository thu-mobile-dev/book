# 插件

Flutter 与原生应用的交互主要有两种：

1. 原生应用为主体，其中一些组件使用 Flutter Widget 构建（原生应用中的一些代码调用 Dart 代码）
2. Flutter 应用为主体，其中的一些组件使用原生组件构建（Flutter 应用中的一些代码调用原生平台的代码）

第一种交互主要是由于 Flutter 是一款新的框架，现在市场上很多应用还是基于原生构建的，如果希望改为 Flutter，可能很难整个应用从零开始改写，可以将一些通用的组件替换为 Flutter。具体可以查看官方教程 [Flutter | Add Flutter to existing app](https://docs.flutter.dev/development/add-to-app)。

但个人感觉必要性不是那么大，对于规模非常大的互联网企业来说，如果需要使用原生开发，直接全部使用原生开发就可以了，没有必要在其中插入 Flutter 组件折腾。

第二种交互也是由于 Flutter 是一款新的框架，底层轮子还不够多。对于小规模的应用开发团队而言，以 Flutter 为主体开发更合适。不过你可能找不到一些能够直接生产使用的组件，这时嵌入原生平台的一些组件是很好的选择。官网文档 [Flutter | Platform integration](https://docs.flutter.dev/development/platform-integration) 给出了对应的指导。

我们的课程主要聚焦在「嵌入原生」，也就是「Platform integration」，后面我们简称为插件、（第三方）包。

## 什么是插件

TODO

- 一些常用的库函数（如数学库）
- 一些常用的 widgets
- 为了让跨平台的 Flutter 能够调用各种操作系统的功能
- 为了让 Flutter 调用原生平台的一些组件，减少重复造轮子的必要

### 使用方法

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

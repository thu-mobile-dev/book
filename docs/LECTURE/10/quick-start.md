# 快速入门 Flutter

!!! warning
    这一份入门指南主要针对两种同学：

    - 有移动应用开发基础，了解至少一种其他的开发框架，希望快速了解 `Flutter`。
    - 没有移动应用开发基础，但靠文档自学能力较强，希望能快速入门 `Flutter`。

    推荐按照从上到下的顺序阅读。

## Flutter 是声明式 UI 框架

- [声明式和命令式的区别](https://docs.flutter.dev/get-started/flutter-for/declarative)
    - 声明式是 Flutter 框架的一大特色。如果你以前使用的是命令式开发框架，最好看一下这篇文章。

## 在 Windows 或 macOS 上安装 Flutter SDK

- [安装命令行 flutter](https://docs.flutter.dev/get-started/install)
    - [使用国内镜像站](https://docs.flutter.dev/community/china)
    - 安装好后打开命令行执行命令 `flutter --version` 的输出应该包含 `Flutter 3.x.x`
- [安装编辑器 VS Code](https://code.visualstudio.com)
    - `VS Code` 是一款非常经典好用的编辑器，这也是有 `Flutter` 官方支持的一款编辑器。
    - 使用方法可以参考 <https://docs.flutter.dev/development/tools/vs-code>
- [安装调试工具 Chrome](https://www.google.com/chrome/)
    - `Flutter` 主要的任务是在一块 `height*width` 的屏幕上渲染你构建的应用，UI 部分和具体的平台没太大的关系。`Flutter` 是跨平台的框架，不仅支持移动端、桌面端，还支持网页端，因此我们选择大家电脑上可能已经有的 `Chrome` 进行调试即可。
    - 虽然课程聚焦移动应用开发，但是如果要使用 `Android` 或者 `iOS` 的模拟器运行应用，需要进行的安装非常麻烦。可以入门之后再安装对应的 `IDE` 和模拟器。
- 打开 `VS Code` 安装插件
    - `Dart`
        - 官方 Dart 插件
    - `Flutter`
        - 官方 Flutter 插件
    - `Awesome Flutter Snippets`
        - 第三方快速补全插件
        - 如输入 `statel` 回车就可以快速创建一个 `StatelessWeight`
- 安装完毕后检查本机环境
    - 命令 `flutter doctor` 的输出应该至少包括
        - `[✓] Flutter`
        - `[✓] Chrome - develop for the web`
        - `[✓] VS Code`
        - `[✓] HTTP Host Availability`
    - 命令 `flutter devices` 的输出应该至少包括
        - `Chrome (web)    • chrome • web-javascript • Google Chrome`

## 第一个 Flutter demo

- 注意
    - 用 `flutter create learnflutter` 来新建一个名为 `learnflutter` 的项目
    - 用 `code learnflutter` 在 `VS Code` 中打开新建的项目
    - 删除 `test` 文件夹下面的文件
    - 将官方提供的 `lib/main.dart` 复制粘贴到项目的 `lib/main.dart` 中保存
    - 用 `flutter run -d chrome --web-renderer html` 即可运行应用
    - 应用运行时  在终端敲 `R`（注意是大写）即可重新编译启动应用
- Write your first Flutter app
    - [part 1](https://docs.flutter.dev/get-started/codelab)
    - [part 2](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2)
- 发布网页应用
    - `flutter build web --web-renderer html`
    - `cd build/web && python3 -m http.server 8000`
    - 浏览器打开 `localhost:8000` 即可看到应用
- 关于网页应用两种构建方式的区别  见<https://docs.flutter.dev/development/platform-integration/web/renderers>。使用 `--web-renderer html` 这种渲染方式是因为另一种默认的渲染方式似乎无法显示文字。

## 有应用开发经验

如果你之前有移动端原生开发的经验，可以看官方提供的教程：

- 熟悉 Android 原生开发 <https://docs.flutter.dev/get-started/flutter-for/android-devs>
- 熟悉 iOS UIKit 开发 <https://docs.flutter.dev/get-started/flutter-for/ios-devs>
- 熟悉 iOS SwiftUI 开发 <https://docs.flutter.dev/get-started/flutter-for/swiftui-devs>
- 熟悉 Web 开发 <https://docs.flutter.dev/get-started/flutter-for/web-devs>

## Dart 是 Flutter 使用的编程语言

- 有 `C`/`C++`/`Python` 编程的基础只需要看 [A tour of the Dart language](https://dart.dev/guides/language/language-tour) 这一篇就行了

## 看 Flutter 官方文档

[英文文档链接](https://docs.flutter.dev)（[中文文档链接](https://flutter.cn/docs) 更推荐阅读英文文档），左侧边栏中：

- `Development / User interface` 下需要都看一下
    - 包含 `Introductoins to widgets` `Building layouts` `Adding interactivity` `Assets and images` `Navigatoin & routing` `Animations` `Advanced UI`
- `Development / Data & backend / State management` 状态管理是很重要的一个点
- 其他部分的文档可以挑感兴趣的看

## 常用 Widgets

`Flutter` 的所有界面元素都是 `Widget`，你需要熟悉一些常用的 `Widget`s 才能构建出你希望的 UI。

[Widget catalog](https://docs.flutter.dev/development/ui/widgets) 中包含了 `Flutter` 中常用的 `Widget`s 列表。

犹豫 `Flutter` 中使用 `Material Design` 比较普遍，也推荐了解 [Material Design](https://m3.material.io/components) 中的常见 UI 组件。[Material 3 updates](https://docs.flutter.dev/development/ui/material3-updates) 这一篇也可以看一下。

## 一些常见的实际案例

[Cookbook](https://docs.flutter.dev/cookbook) 中包含了很多实际使用 `Flutter` 的真实案例，都比较小，聚焦解决某一个经典的场景。

## FAQ

[FAQ](https://docs.flutter.dev/resources/faq) 包含各种关于 `Flutter` 的问答。

## 尾声

恭喜，如果你认真看完了上面的所有链接文档，也自己实际动手跑了上面的一些例程，那么基本上已经算是入门 `Flutter` 开发了。

接下来你可以查看 `Flutter` 官方文档中的其他部分，你也可以实际动手构建你想构建的应用来练手。

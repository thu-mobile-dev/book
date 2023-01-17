# Flutter 简介

> 本篇内容大部分摘自 <https://docs.flutter.dev/resources/faq>

## Flutter 理念

> [FAQ | What are Flutter’s guiding principles?](https://docs.flutter.dev/resources/faq#what-are-flutters-guiding-principles)

Flutter 团队相信：

- 为了尽可能多得接触到潜在用户，开发者需要开发多个平台的应用。
- HTML 技术相关的自动行为和历史遗留问题让达到高帧率和精准的用户体验很有难度。
- 开发多平台应用需要不同的团队、不同的代码库、不同的工作流、不同的工具，成本非常高。
- 开发者希望有一种简单优雅的方式使用单一代码库来完成高质量、可控、高性能的多平台应用的开发。

因此 Flutter 团队聚焦于：

- 「控制」：开发者能够掌握控制系统的所有层级。
- 「性能」：用户希望流畅的、及时响应的、不会卡顿的应用。
- 「精准」：每个人都希望有精确的、华丽的、开心的应用体验。

基于上述理念，Flutter 被创造出来。

## Flutter

> [FAQ | What is Flutter?](https://docs.flutter.dev/resources/faq#what-is-flutter)
> 
> Flutter is Google’s portable UI toolkit for crafting beautiful, natively compiled applications for mobile, web, and desktop from a single codebase. Flutter works with existing code, is used by developers and organizations around the world, and is free and open source.

从上面的介绍中可以看到 Flutter 有着以下的特点：

- UI 框架：Flutter 是 Google 制作的一款 UI 框架 / 应用开发框架。
- 全平台部署：Flutter 应用会被编译为平台原生机器码，支持移动端、网页端、桌面端。
- 可扩展性强：可以与现有的平台代码一起使用，Flutter 嵌入已有框架或已有框架嵌入 Flutter。
- 开源免费：企业可以基于 Flutter 定制框架；开源社区共同的努力降低使用门槛。

## 声明式

TODO https://docs.flutter.dev/get-started/flutter-for/declarative

## Flutter 架构

> Flutter’s framework is designed to be layered and customizable (and optional). Developers can choose to use only parts of the framework, or even replace upper layers of the framework entirely. -- [FAQ | Does Flutter come with a framework?](https://docs.flutter.dev/resources/faq#does-flutter-come-with-a-framework)

可以看到 Flutter 框架本身有着比较好的分层特性。

TODO https://docs.flutter.dev/resources/architectural-overview

## Flutter SDK

SDK - Software Development Kit - 软件开发套件

> [FAQ | What is inside the Flutter SDK?](https://docs.flutter.dev/resources/faq#what-is-inside-the-flutter-sdk)
> 
> Flutter includes:
> 
> - Heavily optimized, mobile-first 2D rendering engine with excellent support for text
> - Modern react-style framework
> - Rich set of widgets implementing Material Design and iOS-style
> - APIs for unit and integration tests
> - Interop and plugin APIs to connect to the system and 3rd-party SDKs
> - Headless test runner for running tests on Windows, Linux, and Mac
> - Dart DevTools for testing, debugging, and profiling your app
> - Command-line tools for creating, building, testing, and compiling your apps

- Flutter 底层使用高性能的 2D 渲染引擎来实现界面的渲染
    - Flutter 底层使用了 [Skia](https://skia.org) 这一款 2D 渲染引擎。-- [FAQ | What technology is Flutter built with?](https://docs.flutter.dev/resources/faq#what-technology-is-flutter-built-with)。
    - 在 [FAQ | What kind of app performance can I expect?](https://docs.flutter.dev/resources/faq#what-kind-of-app-performance-can-i-expect) 中提到，Flutter 可以支持 60 Hz 的界面刷新。事实上在支持更高帧率的机型上，Flutter开源社区也有相应的适配，比如在 Apple 推出的 ProMotion 机型上，Flutter 使用底层的 Metal 图形框架渲染让应用能够达到 120 Hz 的刷新率。
    - Flutter 目前还不支持3D的渲染，这是因为 Flutter 聚焦应用开发，一般都是2D的。-- [FAQ | Can I build 3D (OpenGL) apps with Flutter?](https://docs.flutter.dev/resources/faq#can-i-build-3d-opengl-apps-with-flutter)
    - 对 Flutter 框架的内部实现感兴趣的同学可以查看 [Flutter | Architectural overview](https://docs.flutter.dev/resources/architectural-overview)。对三维渲染感兴趣的同学可以学习 [GAMES101 - 现代计算机图形学入门](https://www.bilibili.com/video/BV1X7411F744/) 了解理论知识。对 3D 游戏引擎实现感兴趣的同学可以学习 [GAMES104 - 现代游戏引擎：从入门到实践](https://www.bilibili.com/video/BV1oU4y1R7Km/)。
- Flutter 借鉴了 [React](https://reactjs.org) 框架的思想。
    - 这是一种组件化的思想，每个 UI 组件有着自己的状态，当状态改变时，UI 组件重新渲染。这一点我们在第四节课会展开讲。
- Flutter 官方库中实现了很多常用的有目标平台风格的组件。
    - Flutter 并不使用目标平台的原生组件，而是调用目标平台的底层图形 API 从零渲染。-- [FAQ | Does Flutter use my operating system’s built-in platform widgets?](https://docs.flutter.dev/resources/faq#does-flutter-use-my-operating-systems-built-in-platform-widgets)
- Flutter 包含单元测试和整体测试的 API。
    - 现代软件开发有个比较重要的思想：面向测试开发。测试在很大程度上能够保证软件的质量。Flutter 中可以很方便的对逻辑、UI 进行测试，这一点我们在第七节课会提到。
- Flutter 支持通过平台原生代码调用系统底层 SDK，也有着插件系统支持第三方插件。
- Flutter 提供了易用的 Windows Linux Mac 平台的测试器。
    - 关于 headless 可以查看 [Blog | Getting Started with Headless Chrome](https://developer.chrome.com/blog/headless-chrome/)，简单来说是指一个不含 GUI 的完整命令行环境。
- Flutter 包含 Dart 语言开发工具以支持测试、调试、监视应用。
- Flutter 提供用来创建、构建、测试、编译应用的命令行工具，主要是 `fluter` 和 `dart`。

## Flutter 编程范式

> [FAQ | What programming paradigm does Flutter’s framework use?](https://docs.flutter.dev/resources/faq#what-programming-paradigm-does-flutters-framework-use)

TODO

## Dart 语言

> [FAQ | Why did Flutter choose to use Dart?](https://docs.flutter.dev/resources/faq#why-did-flutter-choose-to-use-dart)

Flutter 的开发者们在语言的选择上综合考虑了 Flutter 开发者、应用开发者、用户的体验。Dart 被创建成为 Flutter 使用的语言，有这下面几点原因：

- 开发效率高（Developer productivity）
- 面向对象（Object-orientation）
- 可预测的高性能（Predictable, high performance）
- 高速内存申请（Fast allocation）

这里并未展开说，具体内容可以点击上面的链接查看原文。我们在下一节课会带大家了解 Dart 的基础知识和语法。

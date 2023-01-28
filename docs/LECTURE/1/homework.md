# 作业

- 安装 Flutter SDK
    - 按照 [安装 SDK](./flutter-installation.md) 的步骤安装命令行 `flutter`
    - 安装调试工具 [Chrome](https://www.google.com/chrome/)
    - 安装编辑器 [VS Code](https://code.visualstudio.com)
        - 注意课程之后主要使用 VS Code 作为带有扩展的编辑器，而非 IDE，主要命令在集成终端中运行。如果你希望使用 VS Code 集成的功能，请自行查看 [Flutter tools | VS Code](https://docs.flutter.dev/development/tools/vs-code)。
    - 打开 VS Code 安装插件
        - Dart - 官方 Dart 插件
        - Flutter - 官方 Flutter 插件
- 安装模拟器
    - Android Studio
    - 按照 [在 Windows 上创建打开 Android 模拟器进行调试](./simulator-windows-android.md) 或 [在 macOS 上创建打开 Android 模拟器进行调试](./simulator-macos-android.md) 创建一个 Android 模拟器并打开。
- 使用 `flutter create newproject` 创建一个新的项目
    - 用刚创建的 Android 模拟器运行这个项目。用 `flutter devices` 查看连接的设备，用 `flutter run -d xxx` 运行。
    - 查看 `lib/main.dart` 中的内容，自己探索了解案例中的内容

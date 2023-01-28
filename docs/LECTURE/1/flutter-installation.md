# 安装 SDK

目前 Flutter 支持安装在 Windows、macOS、Linux、Chrome OS。课程组支持的两个平台为 Windows 和 macOS。

Flutter 支持全平台的应用开发，包含移动端、桌面端、网页端。课程组主要聚焦移动端，即 Android 和 iOS。网页端只在未配置好移动端模拟器时或是希望减少开发过程中的系统资源占用时使用。

> 如果访问速度慢或是无法访问到相应的资源，可以尝试更换国内源，官方指引为 [Flutter | Using Flutter in China](https://docs.flutter.dev/community/china)。

## Windows 安装

请参考文档 [Flutter | Windows install](https://docs.flutter.dev/get-started/install/windows) 进行安装。

安装的过程基本上为下载 Flutter SDK 安装包，解压并设置环境变量。

安装完毕后使用 `flutter --version` 命令检查环境变量配置无误，输出当前版本即可。

Windows 还需要安装 Android Studio，具体请查看 Android setup 部分自行安装配置。

配置完毕后，使用 `flutter doctor` 命令，应该至少出现：

- `[✓] Flutter (Channel stable ...`
- `[✓] Android toolchain - develop for Android devices (Android SDK version ...`
- `[✓] Android Studio (version ...`

## macOS 安装

请参考文档 [Flutter | macOS install](https://docs.flutter.dev/get-started/install/macos) 进行安装。

安装的过程基本上为下载 Flutter SDK 安装包（注意选择 Intel 芯片或 ARM 芯片的版本），解压并设置环境变量。

安装完毕后使用 `flutter --version` 命令检查环境变量配置无误，输出当前版本即可。

macOS 还需要安装 Android Studio，具体请查看 Android setup 部分自行安装配置。

（可选）如果你希望在 iOS 模拟器上运行应用，请查看 iOS setup 部分安装 Xcode。

配置完毕后，使用 `flutter doctor` 命令，应该至少出现：

- `[✓] Flutter (Channel stable ...`
- `[✓] Android toolchain - develop for Android devices (Android SDK version ...`
- `[✓] Android Studio (version ...`
- （可选）`[✓] Xcode - develop for iOS and macOS (Xcode ...`

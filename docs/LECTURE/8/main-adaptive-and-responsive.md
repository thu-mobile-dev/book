# 多平台适配

在谈到多平台多端适配这一点时，我们关注的准则主要有两个：

- responsive（有应变能力）
    - 当界面发生改变（设备朝向旋转或用户将窗口大小改变）时，应用能够对界面进行重新布局。
    - 可以说代码只有一份，这一份代码的应变能力很强，在什么尺寸的屏幕都可以跑起来，而且有好的 UI 效果。
- adaptive（有适配性）
    - 针对不同平台，适合平台原生用户的习惯，提供特定的功能支持。
    - 代码需要判断当前的平台，并选用合适的交互方式（比如移动端用手势操作、桌面端用键鼠）。

## responsive

针对界面的处理，我们之前课程所提到的例子基本上都可以多端运行，这主要是因为使用了下面的两种方法获取界面尺寸：

- `LayoutBuilder({Key? key, required Widget builder(BuildContext, BoxConstraints)})`
    - 通过 `LayoutBuilder` 的 `constraints: BoxConstraints` 拿到 `maxWidth` 和 `maxHeight`
- `MediaQuery.of()`
    - 拿到界面的尺寸、旋转情况等，根据这些属性构建界面

Flutter 官方提供了很多组件和教学资源，感兴趣的同学可以查看 [Creating a responsive Flutter app](https://docs.flutter.dev/development/ui/layout/adaptive-responsive#creating-a-responsive-flutter-app)

## adaptive

adaptive 要求针对不同平台做适配，因此我们需要获取当前的平台：

```dart
bool get isMobileDevice => !kIsWeb && (Platform.isIOS || Platform.isAndroid);
bool get isDesktopDevice =>
    !kIsWeb && (Platform.isMacOS || Platform.isWindows || Platform.isLinux);
bool get isMobileDeviceOrWeb => kIsWeb || isMobileDevice;
bool get isDesktopDeviceOrWeb => kIsWeb || isDesktopDevice;
```

[Flutter | Building adaptive apps](https://docs.flutter.dev/development/ui/layout/building-adaptive-apps) 中提到，构建 adaptive 的应用，主要需要考虑：

- 界面布局
    - 移动端使用手势触控不像鼠标光标那么精确，布局可以相对松一些。
    - 多平台的测试在桌面端进行可以随意调整窗口大小，但也要时不时使用移动端查看手势交互的效果。
    - ...
- 输入方式
    - 手势与键鼠的操作非常不同。
    - 鼠标滚轮与触控板的滚动方式也很不同。
    - ...
- 平台特性
    - 注意与平台的原生应用保持相似。
    - 注意保留应用的特性。
    - ...

### Android 与 iOS 系统交互的差异

Android 与 iOS 的交互方式有很多差异。一些是系统层面的，一些是组件行为层面上的。这些差异主要体验在设计语言上。在第二讲我们提到，Android 官方使用 [Material Design](https://m3.material.io) 作为设计语言，iOS 使用 [Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/guidelines/overview/) 作为设计语言。Flutter 作为跨平台的框架，在考虑多平台适配时也注意到了这些差异，一些通用的组件在不同平台上的实现也考虑了不同平台的设计语言和组件的默认行为。感兴趣的同学可以阅读 [Platform-specific behaviors and adaptations](https://docs.flutter.dev/resources/platform-adaptations)。

## References

- https://docs.flutter.dev/development/ui/layout/adaptive-responsive
- https://docs.flutter.dev/development/ui/layout/building-adaptive-apps
- https://docs.flutter.dev/resources/platform-adaptations
- https://docs.flutter.dev/development/ui/advanced/gestures#pointers

# 调试与测试

TODO 测试与调试是什么 重要性

## 调试

### debugPrint

TODO 

### MaterialApp

TODO 使用方法

- MaterialApp.debugShowCheckedModeBanner
- MaterialApp.debugShowMaterialGrid
- MaterialApp.showSemanticsDebugger

### 断点调试

TODO VSCode 断点 F5

### Layout 调试

> https://docs.flutter.dev/development/tools/devtools/inspector#show-guidelines

```dart
import 'package:flutter/rendering.dart' show debugPaintSizeEnabled;

void showLayoutGuidelines() {
  debugPaintSizeEnabled = true;
}
```

### 官方调试工具

TODO https://docs.flutter.dev/testing/debugging https://docs.flutter.dev/testing/code-debugging

## 测试

TODO https://docs.flutter.dev/cookbook#testing 主要用实例教学 https://docs.flutter.dev/testing#continuous-integration-services

### 单元测试

TODO

### 界面测试

TODO

### 整体测试

TODO

## 性能

> https://docs.flutter.dev/perf
>
> - What is performance?
> - Why is performance important?
> - How do I improve performance?

关于前两个问题，详见 [Flutter | Appendix: More thoughts about performance](https://docs.flutter.dev/perf/appendix)。简单来说，「性能（performance）」是「性能测试器（performer）」根据「性能标准（metric）」得到的一系列属性；「性能」提供给开发者一个相对客观准确的评价应用某些方面的方式。

关于第三个问题，首先需要测试出现有的性能，然后再考虑提升。Flutter 应用的性能主要有 [Flutter | Performance metrics](https://docs.flutter.dev/perf/metrics) 所说的几项。简单来说有：

- 帧率（speed）
    - Flutter 相当于一个渲染框架，类似游戏引擎，达到 60 帧或者更高的帧率是一个很重要的目标。
- 内存使用（memory）
- 应用包体大小（app size）
- 应用耗电量（energy）

测试的方法主要是依托官方推出的一些性能测试工具，在 [Flutter | Performance profiling](https://docs.flutter.dev/perf/ui-performance) 中有详细的介绍。

对于提高应用性能，官网中给出了常见的一些问题和解答，以及提高应用性能的最佳实践，同时针对帧率和应用包体大小提出了具体的优化方法。

### 常见问题

TODO [Flutter | Performance FAQ](https://docs.flutter.dev/perf/faq)

### 最佳实践

TODO [Flutter | Performance best practices](https://docs.flutter.dev/perf/best-practices)

### 优化帧率

TODO [Flutter | Improving rendering performance](https://docs.flutter.dev/perf/rendering-performance)

### 优化包体大小

TODO [Flutter | Measuring your app's size](https://docs.flutter.dev/perf/app-size)

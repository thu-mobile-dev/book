# 打包与发布

TODO

## 国际化

TODO [Flutter | Internationalizing Flutter apps](https://docs.flutter.dev/development/accessibility-and-localization/internationalization)

## 打包构建

### Android

TODO https://docs.flutter.dev/deployment/android

### iOS

TODO https://docs.flutter.dev/deployment/ios

### Web

TODO https://docs.flutter.dev/deployment/web

### 应用安全性

我们所写的 Flutter 应用使用的语言是高级语言 Dart，在应用以 release 模式发布时，编译器会将高级代码编译为机器码。俗话说，“防人之心不可无”，在一些高安全性要求的应用中（如银行客户端、支付系统）以及一些商业应用中（如会员解锁、游戏反作弊），需要对应用的机器码做一定的处理，防止别有用心之人“开挂”。这是一个非常大的话题，这里也只是提出来供同学们思考。

官方提到了[混淆 Dart 代码](https://docs.flutter.dev/deployment/obfuscate)，感兴趣的同学可以查看。

## 发布

TODO 遵守各个应用商店的规矩

App Store （必须遵守 因为没得选择 有传闻说之后苹果也会有侧载 但是这谁都说不好）
Google Play
华为应用商店
小米应用商店

## 迭代优化

TODO 一般发布的软件都会有需要提升的地方、或者会出现一些 bug。应用需要持续更新。

### 收集用户反馈

TODO

### 更新应用

#### 发布新版本

TODO

#### 热更新

TODO 热更新的优缺点，什么能热更新、什么不能，热更新的一般思路，热更新的基础设施（下载器、执行器），热更新的技术（web显示、游戏资源，提高应用的通用性）

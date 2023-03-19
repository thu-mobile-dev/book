# 打包与发布

应用开发结束之后，我们需要将其打包发布。

## 国际化

如果你的应用有国际化的需求，也就是说，你需要对应用中呈现的文本做翻译，那么你需要有一个工具来根据国家或地区对「原始文本」做映射得到翻译后的文本。翻译是一个比较长且持续的工作流，市面上已经有很多工具来支持开发过程中的翻译工作。

有时国际化也需要对界面的布局做本地化适配，如果只有两三种地区的适配且不同的地区的操作习惯相似，那么用 if-else 进行判断是一种解决的方法。但如果这种情况很普遍，最好使用一些通用的第三方库来处理，不重复造轮子。

官方给出的国际化工具很好的解决了上面所说的两点，甚至还有更多的内容。应用有国际化需求的同学可以查看 [Flutter | Internationalizing Flutter apps](https://docs.flutter.dev/development/accessibility-and-localization/internationalization)。

## 打包构建

### Web

网页的发布相对简单，你可以使用 `flutter build web` 或 `flutter build web --web-renderer html` 构建出网页端应用。构建结束的网页应用就在 `build/web/` 下。

本地查看可以用 `cd build/web/ && python3 -m http.server 8000` 将应用部署到本机的 `8000` 端口。打开浏览器输入 `localhost:8000` 就可以访问应用了。部署到服务器的话也只需要将这个文件夹打包拷贝到服务器再做 Nginx 的设置即可。

关于更多 Web 平台发布的事项，查看 [Flutter | Build and release a web app](https://docs.flutter.dev/deployment/web)。

### Android

如果你只是想打包得到 apk 安装包，使用 `flutter build apk` 等待即可（这个过程可能会很久）。构建结束得到的 APK 文件在 `build/app/outputs/apk/release/app-release.apk`。我们可以直接将这个文件拷贝出来发送到 Android 手机，然后安装即可。

关于更多 Android 平台发布的准备，查看 [Flutter | Build and release an Android app](https://docs.flutter.dev/deployment/android)

### iOS

由于 iOS 系统的封闭性，你没有办法直接打包出一个安装包发给用户进行安装，你需要有 Apple 的开发者账号，并且需要通过 [Xcode](https://developer.apple.com/xcode/) 和 [App Store Connect](https://developer.apple.com/app-store-connect/) 进行一些操作。在测试阶段，你可以使用 [TestFlight](https://developer.apple.com/testflight/) 发布你的应用，在小范围内邀请用户测试收集反馈。需要正式上线 [App Store](https://developer.apple.com/app-store/) 时，你需要通过 Apple 比较严格的审核，填写应用相关信息，准备相关资料。

关于更多 iOS 平台发布的准备，查看 [Flutter | Build and release an iOS app](https://docs.flutter.dev/deployment/ios)。

### 应用安全性

我们所写的 Flutter 应用使用的语言是高级语言 Dart，在应用以 release 模式发布时，编译器会将高级代码编译为机器码。俗话说，“防人之心不可无”，在一些高安全性要求的应用中（如银行客户端、支付系统）以及一些商业应用中（如会员解锁、游戏反作弊），需要对应用的机器码做一定的处理，防止别有用心之人“开挂”。这是一个非常大的话题，这里也只是提出来供同学们思考。

官方提到了[混淆 Dart 代码](https://docs.flutter.dev/deployment/obfuscate)，感兴趣的同学可以查看。

## 发布

应用的发布渠道多种多样，除了在官网发布，在遵守各地法律法规和市场规范的前提下，你也需要遵守各个应用商店的规矩：

- App Store
    - [App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/)
    - 开发者必须遵守  因为 iOS 平台除了 App Store 没有别的发布选择
    - 有传闻说之后苹果也会有侧载  但是这种事情谁都说不好
- Google Play
    - [Developer Policy Center](https://play.google.com/about/developer-content-policy/)
    - [Google Play Developer Distribution Agreement](https://play.google.com/intl/en_us/about/developer-distribution-agreement.html)
- 华为应用商店
    - [华为应用市场审核指南](https://developer.huawei.com/consumer/cn/doc/50104)
- 小米应用商店
    - [小米开发者生态政策](https://dev.mi.com/distribute/doc/details?pId=1321)

## 迭代优化

一般发布的软件都会有需要提升的地方、或者会出现一些 bug。应用需要持续更新。在这个过程中也需要持续收集用户反馈。

### 发布新版本

一般应用都会持续迭代优化，当有 bug 修复或者新功能添加的时候，我们需要发布新版本推送给用户让用户更新。如果不使用热更新技术，发布新版本的方式和发布第一个版本的方式几乎相同。

### 热更新

现在越来越多的应用采取热更新的方式对应用进行更新。热更新，也就是用户不需要在应用商店点击更新、或是在官网下载安装包；当用户打开应用时，新的资源和程序自动下载，在应用内完成更新，提供更便利快捷的更新体验。

对于 Web 平台来说，可以来说平台本身做到了即时的热更新，因为每次可执行的内容都是从服务器拉取，服务器更新后用户可以随时获取最新的版本。不过这一点在移动应用中有些难实现，比如 App Store 就在一定程度上禁止应用内内置解释器、编译器等。

同学在课下感兴趣的话可以查看相关的论文或博客深入了解热更新技术。

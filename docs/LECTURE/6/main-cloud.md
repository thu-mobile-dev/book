# 云端数据存储

## HTTP

大多数数据都可以通过 HTTP 协议传输，HTTP 协议是相当偏应用层的协议，省去了开发者对底层网络事件的监听和处理。基于 HTTP 传输的数据主要使用 JSON，这一节课我们通过简单的案例来学习在 Flutter 中发送网络请求和对数据进行渲染的方法。

在 Flutter 中，我们主要使用 [http](https://pub.dev/packages/http) 这个库对数据进行请求。

### 天气 API

我们可以使用 Python 的 `http.server` 库在本地搭建一个 HTTP server 用于测试。这里我们提供一个返回近七天最高最低温度的天气 API：

```py
from http.server import HTTPServer, BaseHTTPRequestHandler
import json
import traceback
import threading
import time


temperature_dict = {
    "max": [6, 1, 1, 7, 3, 8, 5],
    "min": [-2, -6, -6, -1, -4, -5, -6],
}
port = 12345
process_duration = 0.2


class ResquestHandler(BaseHTTPRequestHandler):
    def _ResponseJsonDict(self, status_code: int, json_dict: dict) -> None:
        self.send_response(code=status_code)
        self.send_header("Content-type", "application/json")
        self.end_headers()
        response_json = json.dumps(json_dict)
        self.wfile.write(response_json.encode())
        return

    def do_GET(self):
        try:
            time.sleep(process_duration)
            self._ResponseJsonDict(status_code=200, json_dict=temperature_dict)
        except Exception as err:
            error_message = f"\n{err} {type(err)}\n{traceback.format_exc()}\n"
            print("error", error_message)
            response_dict = {
                "message": f"{error_message}\nContact the developer team...\n"
            }
            self._ResponseJsonDict(status_code=503, json_dict=response_dict)


def ThreadHttpServer():
    """提供 HTTP 服务的线程"""
    print("log", f"开始在{port}端口提供HTTP服务")
    # `0.0.0.0` means that this server is exposed to all network interfaces
    host = (
        "0.0.0.0",
        port,
    )
    server = HTTPServer(server_address=host, RequestHandlerClass=ResquestHandler)
    server.serve_forever()


if __name__ == "__main__":
    th_HttpServer = threading.Thread(target=ThreadHttpServer, name=None, args=[])
    th_HttpServer.daemon = True
    th_HttpServer.start()
    while True:
        userInput = input()
        if userInput == "q":
            exit(0)
```

将上面的内容保存到 `main.py`，使用 `python3 main.py` 在本地运行。浏览器打开 `localhost:12345` 或者在命令行使用 `curl localhost:12345` 就可以拿到结果 `{"max": [6, 1, 1, 7, 3, 8, 5], "min": [-2, -6, -6, -1, -4, -5, -6]}`。

### 请求与渲染

在 `pubspec.yaml` 的 `dependencies` 中添加 `http`。

> 如果你在 Android 模拟器或真机上运行，需要在 `AndroidManifest.xml` 中添加：
> 
> ```xml
> <!-- Required to fetch data from the internet. -->
> <uses-permission android:name="android.permission.INTERNET" />
> ```
> 
> 如果你在 macOS 桌面端运行，需要在 `macos/Runner/DebugProfile.entitlements` 和 `macos/Runner/Release.entitlements` 中添加：
> 
> ```xml
> <key>com.apple.security.network.client</key>
> <true/>
> ```

示例代码如下：

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(MaterialApp(
      home: Scaffold(
          body: Center(
              child: DefaultTextStyle(
                  style: TextStyle(fontSize: 48, color: Colors.black),
                  child: ContentWidget())))));
}

class ContentWidget extends StatefulWidget {
  const ContentWidget({super.key});

  @override
  State<ContentWidget> createState() => _ContentWidgetState();
}

class _ContentWidgetState extends State<ContentWidget> {
  Future<http.Response>? _response;

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        debugPrint("[0]");
        setState(() {
          _response = http.get(Uri.http("127.0.0.1:12345"));
        });
      },
      child: FutureBuilder<http.Response>(
          future: _response,
          builder:
              (BuildContext context, AsyncSnapshot<http.Response> snapshot) {
            if (_response == null) {
              return Text("点击获取近七天温度");
            } else if (snapshot.hasData) {
              return Text(
                  snapshot.data != null ? snapshot.data!.body : "获取到的数据为 null");
            } else if (snapshot.hasError) {
              return Text("出现错误");
            } else {
              return Text("正在加载");
            }
          }),
    );
  }
}
```

- 主要使用 `FutureBuilder` 来进行异步处理。
    - 在 `ContentWidget` 中有一个状态 `_response` 初值为 `null`，这时界面呈现「点击获取近七天温度」
    - 用户点击之后，`http.get` 立刻返回 `Future<http.Response>`，`FutureBuilder` 根据 `snapshot` 的值进行判断。如果获取到数据，则在屏幕上呈现。

在实际应用场景中，只需要把 `Text(snapshot.data != null ? snapshot.data!.body : "获取到的数据为 null")` 替换为 Widget，将 `snapshot.data!.body` 的 JSON 数据取出呈现即可。

## Firebase

> Firebase is a Backend-as-a-Service (BaaS) app development platform that provides hosted backend services such as a realtime database, cloud storage, authentication, crash reporting, machine learning, remote configuration, and hosting for your static files. -- [Flutter Data & backend | Firebase](https://docs.flutter.dev/development/data-and-backend/firebase)

结合 Firebase 和 Flutter，你可以很快完成一款需要后端的应用，比如官方推荐的案例 [Building chat app with Flutter and Firebase
](https://medium.com/flutter-community/building-a-chat-app-with-flutter-and-firebase-from-scratch-9eaa7f41782e)。

## References

- https://docs.flutter.dev/development/data-and-backend/networking
- https://docs.flutter.dev/development/data-and-backend/json
- https://github.com/dart-lang/http/issues/808

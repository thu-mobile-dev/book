# TodoApp 设计开发全流程

这一节课我们将通过开发一个完整的应用 TodoApp 来快速了解 Flutter 构建应用的全流程。

TodoApp 大家可能也用过，类似备忘录的软件。一般可以把一些要做的事情记录进去，随时可以查看。在应用开发中，这也算是最简单的一种应用了。

## 数据结构设计

在实际动手开发之前，我们需要先想清楚应用中都有哪些数据。

对于一条 `Todo` 而言，用户在输入框中输入待办事项可以创建一条。那么这一条 `Todo` 至少要包含一个 `content`，类型是字符串。在展示多条 `Todo` 的时候，我们需要对它们进行排序，这里比较方便的是使用创建时间进行排序，因此我们还需要添加一个 `createdAt` 的时间戳。因为每一条 `Todo` 都是唯一的，所以我们再添加一个 `UUID`，以后查找某一条 `Todo` 就可以通过 `UUID` 来找。

<!-- TODO 这里最好添加一下 UUID 的定义 -->

但是我们第一节课演示不需要这么复杂，我们只需要 `content` 和 `number` 这两个值就可以了。

在使用 Flutter 构建应用程序的过程中，我们使用的语言是 Dart。这是 Google 创造的一门新语言，说的是对应用开发专门优化过。Dart 是一门面向对象的语言，因为面向对象是很适合 UI 构建的；Dart 也和诸多现代语言一样支持函数编程、协议扩展等多种便捷的操作。

使用 Dart 定义一个 `Todo` 类如下：

```dart
class Todo {
  int number = 0;
  String content = "";

  Todo(int number, String content) {
    this.number = number;
    this.content = content;
  }
}
```

或者使用 initializing formal parameters 简写为：

```dart
class Todo {
  int number = 0;
  String content = "";

  Todo(this.number, this.content);
}

void main(List<String> arguments) {
  Todo a = Todo(0, "完成课堂作业");
  Todo b = Todo(1, "看课件预习");
  print("${a.number} ${a.content}");
  print("${b.number} ${b.content}");
}
// $ dart run
// Building package executable... 
// Built dartlearn:dartlearn.
// 0 完成课堂作业
// 1 看课件预习
```

继续考虑数据结构，要展示多条 `Todo`，还需要一个数组来存放现有的 `Todo`。

```dart
class Todo {
  int number = 0;
  String content = "";

  Todo(this.number, this.content);
}

class TodoList {
  List<Todo> data = [];
  int count = 0;
}
```

- `List<Todo>` 的 `List` 是一种类型，也就是我们常说的数组。这里的 `<Todo>` 是范型，指这一种 `List` 中数据的类型为 `Todo`，也就是我们创建了一个名为 `data` 的 `Todo` 数组。

到这里 TodoApp 的数据已经定义结束了。但是我们还需要定义一些用户操作，比如用户添加一个 `Todo`；一件事情做完了，用户需要完成某件 `Todo`，这时我们考虑直接删除掉这一条 `Todo`。

```dart
class Todo {
  int number = 0;
  String content = "";

  Todo(this.number, this.content);
}

class TodoList {
  List<Todo> data = [];
  int count = 0;

  void insert(String content) {
    data.add(Todo(count, content));
    count += 1;
  }

  void delete(int number) {
    data.removeWhere((todo) {
      return todo.number == number;
    });
  }

  void display() {
    for (final todo in data) {
      print("${todo.number} ${todo.content}");
    }
  }
}

void main(List<String> arguments) {
  TodoList todoList = TodoList();
  todoList.insert("完成课堂作业");
  todoList.insert("看课件预习");
  todoList.display();
  todoList.delete(0);
  todoList.display();
}
// $ dart run
// Building package executable... 
// Built dartlearn:dartlearn.
// 0 完成课堂作业
// 1 看课件预习
// 1 看课件预习
```

- `data.add(...)`: `add()` 方法可以给数组末尾添加一个新值。
- `data.removeWhere((todo) { return todo.number == number; });` 这里 `removeWhere()` 函数的参数是一个函数，这个函数的输入为一个 `Todo`，输出为 `bool` 值；如果 `bool` 值为 `True`，那么这个值就会被移除掉。这里算是函数式编程的思想，往函数中传递一个函数。
- `for (final todo in data) { ... }` 一个标记为 `final` 的变量只能被赋值一次。这里只读取出数组中的数据进行显示。

## UI/UX 设计

数据结构设计完之后我们要考虑如何将这些数据显示到屏幕上，当然最终我们是需要 Flutter 这个框架将其实现，但是在早期阶段，我们需要使用 UI/UX 设计工具先简单画一下我们脑中的想法，这样子之后照着设计稿去写代码效率是更高的。

这里我十分推荐 [Figma](https://www.figma.com/) 这款 UI 设计工具，Figma 简单易上手，网页端的优化做的很好，团队协作功能超强。可以说是从入门到进阶都可以使用使用的一款 UI 设计工具。

打开网站之后注册或者登陆账号，新建一个 Draft 就可以开始画 UI 了。左上角点击长方形的框，我们先把手机的框画出来。随便画一个长方形就行了。

![](images-main/figma-0.png)

画好之后点击这个矩形，在右侧可以调整宽高、圆角度数、颜色等信息。

![](images-main/figma-1.png)

接下来我们要考虑怎么把界面元素放上去：

- TodoApp 的数据 `todoList.data` 可以用一个上下滚动的视图显示出来
- 用户需要添加一条 `Todo` 的时候，也就是需要调用 `todoList.insert()` 时，应该有一个文本输入框接收用户输入，然后有一个确认键添加这一条 `Todo`。类似微信下方发送消息的输入框和右侧的发送键。
- 用户完成一条 `Todo` 的时候，也就是需要调用 `todoList.delete()` 时，需要在一条 `Todo` 上进行操作。比如长按出现一个菜单其中包含「完成」按钮。这里我们直接将「完成」按钮放在 `todo.content` 的左侧，用户点击左侧的「完成」按钮之后，这一条 `Todo` 就会消失。

然后继续在 Figma 中画 UI 吧。利用矩形、文字这些基础的组件，调整大小、圆角度数，可以画出一个下方的输入区：

![](images-main/figma-2.png)

- 用 `cmd / ctrl G` 来让有关系的组件成组，方便后续查看和修改。
- 注意层级遮挡关系，在左侧可以看到当前界面所有的元素，在上方的组件会遮挡下方的组件。

同理，我们画出一个 Todo，然后成组，然后复制出另一个 Todo。这里我们希望有更多的 Todo，于是选中两个现有的 Todo，再次进行复制移动。可是这里间距不一致，我们可以使用 Figma 提供的排版工具，`Tidy up` 一下就整齐了。

![](images-main/figma-3.png)

完成效果图如下：

![](images-main/figma-4.png)

## 安装 Flutter 准备开发环境

画好 UI、想好 UX 之后，我们开始用 Flutter 来做 TodoApp。

Flutter 是 Google 推出的一款是声明式 UI 构建框架，其借鉴了前端 React 框架的思想，将用户界面视为 App 中所有状态的映射，也即，用这些状态渲染出当前的界面 UI。下一节课我们会详细讲讲 Flutter 背后的一些原理，这节课我们只要知道 Flutter 一个大致的用法就行了。

安装请查看 [安装 Flutter](./flutter-installation.md) 这篇文章。

安装成功后，你还需要一款编辑器，这里推荐使用 [VS Code](https://code.visualstudio.com)，打开后还需要安装 Dart 和 Flutter 这两款官方的插件。

同时你需要安装模拟器方便在开发的过程中查看应用的效果。请查看：

- [在 Windows 上创建打开 Android 模拟器进行调试](./simulator-windows-android.md)
- [在 macOS 上创建打开 Android 模拟器进行调试](./simulator-macos-android.md)
- [在 macOS 上创建打开 iOS 模拟器进行调试](./simulator-macos-ios.md)

开模拟器对电脑性能有着不小的开销……我这里使用 Chrome 进行调试，查看 [使用 Chrome 进行调试](./simulator-chrome.md)。

有同学会问，我们不是移动应用开发课程吗，为什么会用到 Chrome 呢？这就要说到 Flutter 的跨平台特性了。Flutter 现在支持的发布渠道有 Android、iOS、Windows、macOS、Linux、网页端，能支持全平台的原因是，Flutter 框架做的事情只是在一块 `height*width` 的画布上渲染出 UI。因此我们只要把 Chrome 拉成竖屏的分辨率，其实在开发阶段是会方便一些的。当然最终也需要在模拟器和真机上运行，这样才能确保应用实际可用（比如在电脑端输入就不会从下方弹出键盘占界面）。当然你也可以直接发布成网页版在手机浏览器中打开。

## 创建项目




### Hello world

讲一下 scaffold和materialapp

## StatelessWidget 呈现整个 UI

## StatefullWidget 呈现 todoList

## 添加按钮

## 完成按钮

## Provider 状态管理

## SQLite 持久存储

发现问题，退出app之后再打开记录的东西都没了，引出持久存储

## 发布应用

### 安卓 APK

### 网页版

关于网页应用两种构建方式的区别  见 https://docs.flutter.dev/development/platform-integration/web/renderers

## 可改进的地方

- 给 `Todo` 加上 `createdAt` 的时间戳还有唯一标志 `UUID`。
- 用户添加一条 `Todo` 之后无法修改，这是个比较糟糕的事情。
- 添加动画
    - 添加一条 `Todo` 之后，滑至最下方
    - 删除一条 `Todo` 之后，让 `Todo` 消失的自然一些，比如从上方滑出，从左侧滑出
- 更好的 UI 设计？
    - 用浮动的一个加号表示添加，而不是时刻都在最下方显示输入框。

# Dart

## 学习资料

Dart 在有 C / C++ / Python 基础时非常容易入手，最好的方式是自己阅读官方文档。

- 结合之前已经学习掌握的 C / C++ / Python，阅读 [Dart ｜ A tour of the Dart language](https://dart.dev/guides/language/language-tour) 快速上手熟悉 Dart，了解 Dart 和 C / C++ / Python 的异同。
    - 如果你熟悉 JavaScript，可以阅读 [Dart | Learning Dart as a JavaScript developer](https://dart.dev/guides/language/coming-from/js-to-dart)。
    - 如果你熟悉 Swift，可以阅读 [Dart | Learning Dart as a Swift developer](https://dart.dev/guides/language/coming-from/swift-to-dart)。
- 通过 [Dart | Language samples](https://dart.dev/samples) 中的一些案例熟悉 Dart。
- 阅读 [Dart | Asynchronous programming: futures, async, await](https://dart.dev/codelabs/async-await) 熟悉 Dart 的异步编程方式。
- 阅读 [Dart | A tour of the core libraries](https://dart.dev/guides/libraries/library-tour) 了解 Dart 的核心库。

你可以使用 [Dart 的在线编辑器](https://dart.dev/#try-dart) 尝试 Dart 的语法，你也可以在安装好 Flutter 之后，在本地使用下面的命令来创建项目并编译运行：

```sh
$ dart create learn_dart
$ code learn_dart
# 删掉 test/, 修改 bin/ 和 lib/ 文件下的源代码
$ dart run
```

---

以下内容主要提示一些 Dart 比较关键的语法。

## 类型

### 内建类型

- Numbers (`int`, `double`)
- Strings (`String`)
- Booleans (`bool`)
- Lists (`List`,)
- Sets (`Set`)
- Maps (`Map`)
- `null`

### 声明变量和常量

声明变量有以下几种方式：

- 类型在前（推荐）：`int n = 10;` `String text = "Hello, world!";`
- 使用类型推断：`var n = 10;` `var text = "Hello, world!";`
- 如果不希望在声明变量时立即赋值，也可以将变量声明为「可空」类型：
    ```dart
    void main(List<String> arguments) {
      int? a; // a 的类型为「可空的整形」，默认赋值为 null
      print(a); // null
      a = 16;
      print(a); // 16
    }
    ```
- 在类中声明变量不能立即初始化，需要使用 `late` 关键字：
    ```dart
    class A {
      late int a;
      A(this.a);
    }
    ```

常量声明时有两种情况：

- 某个量需要在编译器被确定且之后不会改变：使用 `const` 关键字。eg. `const double pi = 3.14;`
- 某个量只会在运行时被赋值一次：使用 `final` 关键字。
    ```dart
    void main(List<String> arguments) {
      final List<String> listA = List.generate(8, (i) => "$i");
      print(listA); // [0, 1, 2, 3, 4, 5, 6, 7]
      final List<String> listB;
      listB = List.generate(8, (i) => "$i");
      print(listB); // [0, 1, 2, 3, 4, 5, 6, 7]
      listB = []; // error: The final variable 'listB' can only be set once. Try making 'listB' non-final.
    }
    ```

### 范型

以 List 为例，列表中的数据类型不一定是特定一种，因此使用范型 `List<T>` 来表示。当需要具体某一个类型的数组时，需要指明数据类型，如 `List<int>` 表示整型数组、`List<String>` 表示字符串数组。

### 函数类型

在 Dart 中，函数也是一种类型，如：

```dart
void sayHello() {
  print("Hello");
}

void main(List<String> arguments) {
  Function welcome = sayHello;
  welcome();
}
```

## 函数

### 参数传递

一种传参方式是与 C 一致的，在调用时不需要添加参数名称：

```dart
int add(int a, int b) {
  return a + b;
}

void main(List<String> arguments) {
  print(add(3, 4));
}
```

另一种方式调用更明确，需要写出参数名称：

```dart
int add({required int a, required int b}) {
  return a + b;
}

void main(List<String> arguments) {
  print(add(a: 3, b: 4));
}
```

两者也可以混用：

```dart
int add(int a, int b, {int c = 0}) {
  return a + b + c;
}

void main(List<String> arguments) {
  print(add(3, 4));
}
```

### 匿名函数

因为函数被理解为一种类型，你可将匿名函数理解为函数这个类型的值：

```dart
int calculate(int a, int b, int Function(int, int) operate) {
  return operate(a, b);
}

void main(List<String> arguments) {
  print(calculate(3, 4, (i0, i1) {
    return i0 + i1;
  }));
  print(calculate(3, 4, (i0, i1) => i0 * i0 + i1 * i1));
}
```

可以看到，这里 `calculate()` 的第三个参数输入的是一个函数，这个函数接收两个 `int` 参数，返回值为 `int`。

在下方，通过 `() { ... }` 和 `() => ...` 分别创建了两个匿名函数传入 `calculate()` 的第三个参数。

## 异步与错误处理

### 同步

要知道在 Dart 中如何用异步，首先要明白异步是什么；要明白异步是什么，首先要明白同步是什么。从代码执行的逻辑上来说，程序、函数的执行都是同步执行的

```dart
void main() {
  funcA();
  funcB();
  funcC();
}
```

比如上面的例子中，`funcA()` 执行结束 `funcB()` 开始执行，`funcB()` 执行结束 `funcC()` 开始执行，`funcC()` 执行结束 `main()` 返回 `void`。

这个过程我们也可以理解为 `funcA()` 返回 `void` 之后，`funcB()` 才开始执行……

更好的同步的例子是：

```dart
void main() {
  int a = add(1, 2);
  int b = add(a, 3);
  print("$a, $b");
}
```

`b` 的值必须等待 `a`，`print` 必须等待 `b`。

### 异步

现在开始对于异步的讲解，如果一个函数执行的时间特别长，比如应用中需要通过网络获取一张图片，这个过程可能花费 500ms 甚至更长的时间。我们希望在这个过程中持续显示一个进度条：

```dart
void main() {
  Image a = fetchImage();
  // showIndicator();
  displayImage(a);
}
```

如果这个逻辑为上面的方式，那么你会发现，只有等待 `fetchImage()` 的结果返回，`a` 才会有值，在这个等待的过程中，没有办法执行其他的代码。比如用户界面的旋转指示也会被阻塞停住，一些后台处理的算法也会被阻塞。

要将这个代码改为异步，逻辑是：在用到结果的时候再等待：

```dart
void main() {
  Future<Image> a = fetchImage();
  showIndicator();
  doOtherPreparations();
  displayImage(await a);
}
```

- 有一点需要注意，`fetchImage()` 在定义的时候需要加上 `async` 关键字，`main()` 也是。这表示这些函数都是异步执行的函数。
- 这里的 `fetchImage()` 已经变成了一个异步函数，被调用时其会立即返回，返回一个未完成的 `Future`。返回之后，这个函数会开始在后台发出网络请求开始获取图片。
- 因为 `a` 的值已经被赋了一个未完成的 `Future`，所以代码继续同步执行。
- 直到执行到需要展示图片的代码，这里必须要等待 `a` 里面是一张拿到的图才能显示，所以要用 `await` 确保图片已经下载完成。也就是说，`await` 确保 `a` 已经是一个已完成的 `Future`，然后将其中的 `Image` 取出送入到 `displayImage()`。
- 可以看到，这里要显示图片，所以必须要等待 `fetchImage()` 的执行结束，但是其他的代码不需要，那就可以用异步的逻辑，使得实际的程序执行顺序和代码顺序不一致，从而提高程序运行的效率。也就是说，从调用一个 `async` 函数开始，到第一个 `await` 出现要获取这个 `async` 函数的返回值，这中间的函数都可以无需等待直接执行。

### 错误处理

使用 `try-catch` 处理错误即可：

```dart
try {
  Image b = await fetchImage();
} catch (err) {
  print('error: $err');
}
```

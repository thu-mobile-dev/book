# Dart 语言

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

以 List 为例，列表中的数据类型不一定是特定一种，因此使用范型 `List<T> ` 来表示。当需要具体某一个类型的数组时，需要指明数据类型，如 `List<int>` 表示整型数组、`List<String>` 表示字符串数组。

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

## 异步



# 本地数据存储

之前我们的所有数据都是存储在内存中的，应用一旦关闭，内存被回收，数据也就会消失。这节课我们希望将数据持久存储。持久存储的方式本质上来说只有一种——就是把数据存到磁盘上，需要时就可以从磁盘读取。具体来说，可以存到安装应用的用户的本机上，也可以存到提供应用的企业的服务器上，也就是本地存储和云端存储。

这节课我们先讲本地的数据存储。

## 访问文件系统

最简单的方式就是使用目标平台的文件系统来存储数据，包括序列化文本、图片等等。

在 Flutter 中我们使用的是 [path_provider](https://pub.dev/packages/path_provider) 这个包。这里我们首先需要明白的是，在移动端获取一个绝对的文件路径是比较困难的，这是为了防止不同的应用随意读取数据。一般移动端都会提供一个应用专属的文件夹路径，应用可以将所有的数据存到这里。此外，一些临时的缓存文件，你可以存到刚刚所说的文件路径中，你也可以放到操作系统提供的缓存文件夹中。

path_provider 这个包在 Android 和 iOS 端（包括桌面端，不包括网页端）提供了获取这两种文件夹路径的方式：

```dart
Directory tempDir = await getTemporaryDirectory();
String tempPath = tempDir.path;

Directory appDocDir = await getApplicationDocumentsDirectory();
String appDocPath = appDocDir.path;
```

要使用这个包，我们需要先在 `pubspec.yaml` 中的 `dependencies:` 下添加 `path_provider: ^2.0.11`。

然后在对应的文件中导入：

```dart
import 'package:path_provider/path_provider.dart';
```

读写文件需要用到 File 类，我们还需要引入 dart.io：

```dart
import 'dart:io';
```

接下来我们创建一个文本文件 a.txt 并写内容读取：

```dart
Future<File> getFile(String name) async {
  final directory = await getApplicationDocumentsDirectory();
  final path = directory.path;
  return File("$path/$name");
}

Future<File> writeFile(String name, String contents) async {
  final file = await getFile(name);
  return file.writeAsString(contents);
}

Future<String> readFile(String name) async {
  try {
    final file = await getFile(name);
    final contents = await file.readAsString();
    return contents;
  } catch (e) {
    // If encountering an error, return ""
    return "";
  }
}

void main() async {
  // Avoid errors caused by flutter upgrade.
  // Importing 'package:flutter/widgets.dart' is required.
  WidgetsFlutterBinding.ensureInitialized();
  await writeFile("a.txt", "Hello, world!");
  debugPrint(await readFile("a.txt"));
  runApp(const MyApp());
}
```

## 键值数据存储

应用中往往会出现一类键值数据，比如用户的设置、应用的临时状态，这类数据往往比较小。Android 和 iOS 端都提供了读写这种数据的 API。在 Flutter 中，我们使用 [shared_preferences](https://pub.dev/packages/shared_preferences) 这个包来读写键值数据。

首先需要在 `pubspec.yaml` 中的 `dependencies:` 中添加 `shared_preferences: ^2.0.16`。

然后在需要使用的文件中导入即可：

```dart
import 'package:shared_preferences/shared_preferences.dart';
```

以下使用方法来自 [pub.dev | shared_preferences](https://pub.dev/packages/shared_preferences)：

### 写

```dart
// Obtain shared preferences.
final prefs = await SharedPreferences.getInstance();

// Save an integer value to 'counter' key.
await prefs.setInt('counter', 10);
// Save an boolean value to 'repeat' key.
await prefs.setBool('repeat', true);
// Save an double value to 'decimal' key.
await prefs.setDouble('decimal', 1.5);
// Save an String value to 'action' key.
await prefs.setString('action', 'Start');
// Save an list of strings to 'items' key.
await prefs.setStringList('items', <String>['Earth', 'Moon', 'Sun']);
```

### 读

```dart
// Try reading data from the 'counter' key. If it doesn't exist, returns null.
final int? counter = prefs.getInt('counter');
// Try reading data from the 'repeat' key. If it doesn't exist, returns null.
final bool? repeat = prefs.getBool('repeat');
// Try reading data from the 'decimal' key. If it doesn't exist, returns null.
final double? decimal = prefs.getDouble('decimal');
// Try reading data from the 'action' key. If it doesn't exist, returns null.
final String? action = prefs.getString('action');
// Try reading data from the 'items' key. If it doesn't exist, returns null.
final List<String>? items = prefs.getStringList('items');
```

### 删除

```dart
// Remove data for the 'counter' key.
final success = await prefs.remove('counter');
```

## 数据库

### SQLite

可以说，对于本地的数据，使用 SQLite 数据库是最好的选择了。SQLite 有着简单易用、轻量、快速、全平台、零配置等诸多优点，这让它成为了本地存储和嵌入式存储的最佳选择。

资源链接：

- [SQLite 官网](https://www.sqlite.org/)
- [在 Python 中使用 SQLite](https://docs.python.org/3/library/sqlite3.html)

### 复习 SQL

相信大多数人已经学过数据库相关的知识了，对 SQL 也很熟悉了，这里课堂不提及。需要学习或者复习 SQL 的同学可以戳下面的链接：

<https://www.w3schools.com/sql/>

### sqflite

在 Flutter 我们使用 SQLite 主要是通过 [sqflite](https://pub.dev/packages/sqflite)，其支持的平台有 Android、iOS 和 macOS。

首先需要在 `pubspec.yaml` 中的 `dependencies:` 中添加 `sqflite: ^2.2.3`。

然后在需要使用的文件中导入即可：

```dart
import 'package:sqflite/sqflite.dart';
```

### 给 TodoApp 添加持久存储

首先我们需要明确任务：

- 只在 sqflite 支持的平台使用持久存储
- 在应用第一次被打开时创建一个 SQLite 数据库并建表
- 每次修改 todoList 也要对应修改数据库
- 在应用每次打开前连接数据库读取数据存入 todoList

#### 项目代码

##### pubspec.yaml

```dart
dependencies:
  flutter:
    sdk: flutter
  provider: ^6.0.5
  sqflite: ^2.2.3
  path: 1.8.2
```

- 修改 pubspec.yaml 之后保存 flutter 和 dart 命令行工具就会自动下载这些依赖。

##### model.dart

```dart
import 'dart:math';
import 'package:flutter/material.dart';
import 'package:sqflite/sqflite.dart';

const String databaseName = "todo_db";
const String tableName = "todos";
final List<Todo> debugTodos = List.generate(16, (i) => Todo(i, "Todo $i"));

class Todo {
  int id = 0;
  String content = "";

  Todo(this.id, this.content);

  // Convert a Todo into a Map. The keys must correspond to the names of the
  // columns in the database.
  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'content': content,
    };
  }
}

class TodoListModel extends ChangeNotifier {
  // source of states
  List<Todo> todos;
  int count = 0;

  Future<Database>? database;

  TodoListModel({required this.todos, required this.database}) {
    if (todos.isNotEmpty) {
      count = todos.map((e) => e.id).toList().reduce(max) + 1;
    }
  }

  Future<void> insert(String content) async {
    final todo = Todo(count, content);
    todos.add(todo);
    count += 1;
    notifyListeners(); // re-build widgets

    if (database != null) {
      final db = await database!;

      await db.insert(
        tableName,
        todo.toMap(),
        conflictAlgorithm: ConflictAlgorithm.replace,
      );
    }
  }

  Future<void> delete(int id) async {
    todos.removeWhere((todo) {
      return todo.id == id;
    });
    notifyListeners(); // re-build widgets

    if (database != null) {
      final db = await database!;

      // Remove the Todo from the database.
      await db.delete(
        tableName,
        // Use a `where` clause to delete a specific todo.
        where: 'id = ?',
        // Pass the Todo's id as a whereArg to prevent SQL injection.
        whereArgs: [id],
      );
    }
  }
}
```

- 可以看到 `TodoListModel` 多了一个属性 `Future<Database>? database;`，其为 `null` 的时候说明不使用数据库，否则我们使用 `await` 可以取出其中的数据库进行数据操作。
- `insert()` 中添加了判断，使用替换的方式将 `id` 相同的 `Todo` 在数据库中替换。

##### main.dart

```dart
import 'dart:io';
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:flutter/foundation.dart' show kDebugMode;
import 'package:sqflite/sqflite.dart';
import 'package:path/path.dart';

import 'model.dart';

void main() async {
  if (kDebugMode) {
    runApp(MyApp(defaultTodos: debugTodos, database: null));
  } else {
    if (Platform.isAndroid || Platform.isIOS || Platform.isMacOS) {
      // Avoid errors caused by flutter upgrade.
      // Importing 'package:flutter/widgets.dart' is required.
      WidgetsFlutterBinding.ensureInitialized();

      // 创建并连接数据库
      // Open the database and store the reference.
      final database = openDatabase(
        // Set the path to the database. Note: Using the `join` function from the
        // `path` package is best practice to ensure the path is correctly
        // constructed for each platform.
        join(await getDatabasesPath(), databaseName),
        // When the database is first created, create a table to store todos.
        onCreate: (db, version) {
          // Run the CREATE TABLE statement on the database.
          return db.execute(
            "CREATE TABLE $tableName(id INTEGER PRIMARY KEY, content TEXT)",
          );
        },
        // Set the version. This executes the onCreate function and provides a
        // path to perform database upgrades and downgrades.
        version: 1,
      );

      // 读取用户上次使用存储的数据
      // Get a reference to the database.
      final db = await database;
      // Query the table for all The Todos.
      final List<Map<String, dynamic>> maps = await db.query(tableName);
      // Convert the List<Map<String, dynamic> into a List<Todo>.
      var defaultTodos = List.generate(maps.length, (i) {
        return Todo(maps[i]['id'], maps[i]['content']);
      });
      runApp(MyApp(defaultTodos: defaultTodos, database: database));
    } else {
      runApp(MyApp(defaultTodos: [], database: null));
    }
  }
}

class MyApp extends StatelessWidget {
  final List<Todo> defaultTodos;
  final Future<Database>? database;
  MyApp({super.key, required this.defaultTodos, required this.database});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "TodoApp",
      home: Scaffold(
          body: ChangeNotifierProvider(
        create: (context) =>
            TodoListModel(todos: defaultTodos, database: database),
        child: ContentWidget(),
      )),
      debugShowCheckedModeBanner: false,
    );
  }
}

class ContentWidget extends StatelessWidget {
  ContentWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Expanded(
          child: Consumer<TodoListModel>(builder: (context, model, child) {
            if (model.todos.isNotEmpty) {
              return ListView(
                children:
                    model.todos.map((todo) => TodoWidget(todo: todo)).toList(),
              );
            } else {
              return Center(
                  child: Text(
                "欢迎使用 TodoApp\n你可以在下方输入新的 Todo",
                style: TextStyle(fontSize: 36),
                textAlign: TextAlign.center,
              ));
            }
          }),
        ),
        AddTodoWidget()
      ],
    );
  }
}

class TodoWidget extends StatelessWidget {
  final Todo todo;

  TodoWidget({super.key, required this.todo});

  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        Consumer<TodoListModel>(builder: (context, model, child) {
          return TextButton(
              onPressed: () {
                model.delete(todo.id);
              },
              child: Icon(
                Icons.circle_outlined,
                size: 36,
              ));
        }),
        Text(
          todo.content,
          style: TextStyle(fontSize: 36),
        ),
      ],
    );
  }
}

class AddTodoWidget extends StatefulWidget {
  AddTodoWidget({super.key});

  @override
  State<AddTodoWidget> createState() => _AddTodoWidgetState();
}

class _AddTodoWidgetState extends State<AddTodoWidget> {
  final textFieldController = TextEditingController();

  @override
  void dispose() {
    // Clean up the controller when the widget is disposed.
    textFieldController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        Expanded(
          child: TextField(
            controller: textFieldController,
            decoration: InputDecoration(
              border: OutlineInputBorder(),
              labelText: '新 Todo 的内容',
            ),
          ),
        ),
        Consumer<TodoListModel>(builder: (context, model, child) {
          return TextButton(
              onPressed: () {
                debugPrint("添加按钮按下 内容为${textFieldController.text}");
                model.insert(textFieldController.text);
                textFieldController.text = "";
              },
              child: Text(
                "添加",
                style: TextStyle(fontSize: 24),
              ));
        })
      ],
    );
  }
}
```

- 由于 sqflite 这个库只支持 Android iOS macOS 这三个平台，我们需要进行判断。此外 debug 的时候我们只希望检查 UI/UX 的正确性，因此也不使用数据库。
- 这里的数据源有两个，一个是 `TodoListModel.todos`，一个是数据库中实际存储的数。每次打开应用的时候，我们先从数据库读取数据拿到内存中（`TodoListModel.todos` 中）。每次内存中的数据有修改也对应修改数据库。
- 其余 UI/UX 部分未变。

### 安装测试

构建上面的应用，在 Android 手机安装，打开之后输入 Todo 再关闭应用打开看到之前的数据，这说明数据已经持久存储了。

## References

- https://docs.flutter.dev/cookbook/persistence/reading-writing-files
- https://docs.flutter.dev/cookbook/persistence/key-value
- https://docs.flutter.dev/cookbook/persistence/sqlite

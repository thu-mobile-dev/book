# 有状态的 Widgets

## 状态

### 声明式框架的渲染方式

```
UI = f(states)
```

状态决定 UI，这是声明式的框架所特有的（在传统的命令式编程中，UI 的改变由函数调用控制）。具体来说：

- 当前时刻的用户界面由当前时刻的状态通过函数 `f()` 渲染得到。
    - 在 Flutter 中，Widget 的 `build()` 函数组成这个 `f()`。
    - 也就是说，除了修改状态，不会有其他的方式对用户界面造成修改。唯一的一条路径能够减少应用中的逻辑链，相对简洁。
- 状态是自变量，当状态改变时，用户界面随之改变。
    - 结合屏幕刷新率考虑，60 Hz 的屏幕至少要在 1/60 s 内判断 states 是否发生改变，如果发生改变要重新调用 `f()` 来重新渲染新的用户界面。
    - 注意，不是根据 states 的变化修改旧的用户界面，而是将旧的用户界面直接抛弃，重新渲染新的用户界面。这样做确实会降低应用的渲染效率，但由于 Flutter 对 `build()` 函数做过相当多的优化，开发者绝大多数情况并不用担心性能。
    - 在 Flutter 中，在 `StatefulWidget` 中改变状态需要调用 `setState()`。
- 一个 Widget 不能直接更新其他 Widget，而是通过更新状态来更新另一个 Widget
    - 这样做保证 Widget 之间的强耦合较少，Widget 能够比较好的进行复用。

### 什么是状态

那么什么是状态呢？简单来说就是一些变量值：

- 黑夜模式的开关（`bool isDarkModeOn`）
    - 为 `true` 时开关呈现开启状态，界面呈现黑夜模式
    - 为 `false` 时开关呈现关闭状态，界面
- 用户购物车中的商品（`List<Item> itemsInCart`）
    - 用户添加商品到购物车时给列表中添加项，购物车按照新的列表重新刷新界面
- 游戏中角色的血量（`double hp`）
    - 角色受到攻击时血量减少，界面的血条减少，游戏角色的速度可能会变慢
    - 用户点击回血道具，血量增加，界面的血条增加
- ...

你甚至可以认为应用内的所有数据都是状态。

### 短暂状态与应用状态

状态分为短暂状态（ephemeral state）和应用状态（app state），但是这并没有明确的界限。如果一个状态在整个应用的多个 Widget 中都要使用（比如黑夜模式的开关），那么它相对靠近应用状态一些；如果一个状态只是在一个 Widget （动画的计时器）或者相邻的两个 Widget（多选框的选择结果要在当前界面呈现）中使用，那么它相对靠近短暂状态。

短暂的状态我们使用 `StatefulWidget` 进行处理，应用状态我们主要使用第三方状态管理包 `Provider` 进行处理。

## StatefulWidget

### 原理

TODO

### 案例

> 注：在 VS Code 中敲 `stf` 即可自动补全 `StatefulWidget`。

TODO 用 switch 或者 多选框做例子感觉比较好。就用黑夜模式吧，单个页面的黑夜模式。

## Provider

> Declarative UI frameworks eliminate whole classes of bugs. But they can also be a puzzle until you figure out how to structure your app logic around them. -- [YouTube | Pragmatic State Management in Flutter (Google I/O'19)](https://www.youtube.com/watch?v=d_m5csmrf7I)

状态管理是声明式框架非常核心也是比较复杂的一块内容，在 [List of state management approaches](https://docs.flutter.dev/development/data-and-backend/state-mgmt/options) 中官方介绍了很多进行状态管理的第三方包，可以看到第三方包的数量非常多，可以说是各有优劣。

本课程选取 [Provider](https://pub.dev/packages/provider) 进行教学，这是官方推荐的一个状态管理包，对于初学者来说也足够简单易用。

### 原理

TODO

### 案例

TODO https://docs.flutter.dev/development/data-and-backend/state-mgmt/simple 也用黑夜模式，只不过用多个页面的黑夜模式。主要原理要讲清楚各个xxx的用法。

### 进阶使用

TODO 讲一下 Multiple 和 Stream，主要内容来自 provider 的 readme。

## References

- https://docs.flutter.dev/development/data-and-backend/state-mgmt/declarative
- https://docs.flutter.dev/development/data-and-backend/state-mgmt/ephemeral-vs-app
- https://docs.flutter.dev/development/data-and-backend/state-mgmt/simple
- https://docs.flutter.dev/development/data-and-backend/state-mgmt/options
- https://www.youtube.com/watch?v=d_m5csmrf7I

# StatelessWidget与StatefulWidget



## StatelessWidget

Flutter中的`StatelessWidget`是一个不需要状态更改的widget - 它没有要管理的内部状态。

**示例代码：**

```dart
class Test extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```

## StatefulWidget

statefulWidget是可变状态的widget。 使用`setState`方法管理StatefulWidget的状态的改变。调用`setState`告诉Flutter框架，某个状态发生了变化，Flutter会重新运行build方法，以便应用程序可以应用最新状态。

**示例代码：**

```dart
class Test extends StatefulWidget {
  @override
  _TestState createState() => _TestState();
}

class _TestState extends State<Test> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```


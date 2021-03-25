# Scaffold

Scaffold 有下面几个主要属性：

- appBar：显示在界面顶部的一个 AppBar，也就是 Android 中的 ActionBar 、Toolbar
- body：当前界面所显示的主要内容 Widget
- floatingActionButton：纸墨设计中所定义的 FAB，界面的主要功能按钮
- persistentFooterButtons：固定在下方显示的按钮，比如对话框下方的确定、取消按钮
- drawer：侧边栏控件
- backgroundColor： 内容的背景颜色，默认使用的是 ThemeData.scaffoldBackgroundColor 的值
- bottomNavigationBar： 显示在页面底部的导航栏



## bottomNavigationBar

scaffold的属性，用于定义底部的tab栏，主要有下列属性：

|               属性名                |                            说明                             |
| :---------------------------------: | :---------------------------------------------------------: |
| items List<BottomNavigationBarItem> |                     底部导航 条按钮集合                     |
|            currentIndex             |                         选中第几个                          |
|                type                 | 底部导航栏的类型，有fixed和shifting两个类型，显示效果不一样 |
|             fixedColor              |                         选中的颜色                          |
|                onTap                |               选中时的回调函数，带有index参数               |

### BottomNavigationBarItem

|      属性       |                     说明                      |
| :-------------: | :-------------------------------------------: |
|      icon       |        要显示的图标控件，一般都是Iocn         |
|      title      |        要显示的标题控件，一般都是Text         |
|   activeIcon    |       选中时要显示的icon，一般也是Icon        |
| backgroundColor | BottomNavigationBarType为shifting时的背景颜色 |

## pageView

**PageView** 是一个滑动视图列表，它也是继承至 CustomScrollView 的。

|     属性      |                      说明                      |
| :-----------: | :--------------------------------------------: |
|   children    |                   存放子组件                   |
|  controller   | 配置，配合PageController可以实现非常酷炫的效果 |
| onPageChanged |      页面更换时的回调函数，带有index参数       |
|    physics    |    NeverScrollableScrollPhysics()：取消滑动    |

## Text

|      属性       |                       说明                        |    值类型    |
| :-------------: | :-----------------------------------------------: | :----------: |
|      data       |             Text显示的文本，必填参数              |    String    |
|    textAlign    | 文本的对齐方式,可以选择左对齐、右对齐还是居中对齐 |  TextAlign   |
|    maxLines     |                文本显示的最大行数                 |     int      |
|    overflow     |                文本显示的截断方式                 | TextOverflow |
| textScaleFactor |                  文本的缩放比例                   |    double    |
|      style      | 用于指定文本显示的样式如颜色、字体、粗细、背景等  |  TextStyle   |

### TextStyle

|     属性      |      说明      |   值类型   |
| :-----------: | :------------: | :--------: |
|     color     | 设置文本的颜色 |   Color    |
|   fontSize    |  设置字体大小  |   double   |
|  fontWeight   | 字体的加粗权重 | FontWeight |
|   fontStyle   |  文本显示样式  | FontStyle  |
| letterSpacing | 单词之间的间距 |   double   |
|  wordSpacing  | 字母之间的间距 |   double   |
|    height     | 行高（比例值） |   double   |

## MediaQuery.removePadding

通过`MediaQuery.removePadding`可以移除元素的pandding，需要注意要指定移除哪个方向的padding。

|            属性             |          说明           |      取值       |
| :-------------------------: | :---------------------: | :-------------: |
|           context           |                         | context（必传） |
| removeLeft/Right/Top/Bottom | 移除哪个方向上的padding |   true/false    |
|            child            |         子组件          |      必传       |

## RefreshIndicator

下拉刷新组件

|   属性    |                    说明                    |
| :-------: | :----------------------------------------: |
|   child   |             监听子组件（必传）             |
| onRefresh | 下拉回调函数(方法需要有async和await关键字) |

## NotificationListener

在widget树中，子widget滚动时会向上发送notification，通过NotificationListener可以监控到该notification。NotificationListener也是一个widget，可以将被监控的widget放入其child内。

|      属性      |                             说明                             |
| :------------: | :----------------------------------------------------------: |
|     child      |                      被监控的子widget树                      |
| onNotification | 监控到notification后的回调方法，onNotification(scrollNotification{} |

## Container

  官方给出的简介，是一个结合了绘制（painting）、定位（positioning）以及尺寸（sizing）widget的widget。

它是一个组合的widget，内部有绘制widget、定位widget、尺寸widget。

## TextField

文档：https://www.cnblogs.com/joe235/p/11711653.html

```dart
const TextField({
    Key key,
    this.controller,                    // 控制正在编辑文本
    this.focusNode,                     // 获取键盘焦点
    this.decoration = const InputDecoration(),              // 边框装饰
    TextInputType keyboardType,         // 键盘类型
    this.textInputAction,               // 键盘的操作按钮类型
    this.textCapitalization = TextCapitalization.none,      // 配置大小写键盘
    this.style,                         // 输入文本样式
    this.textAlign = TextAlign.start,   // 对齐方式
    this.textDirection,                 // 文本方向
    this.autofocus = false,             // 是否自动对焦
    this.obscureText = false,           // 是否隐藏内容，例如密码格式
    this.autocorrect = true,            // 是否自动校正
    this.maxLines = 1,                  // 最大行数
    this.maxLength,                     // 允许输入的最大长度
    this.maxLengthEnforced = true,      // 是否允许超过输入最大长度
    this.onChanged,                     // 文本内容变更时回调
    this.onEditingComplete,             // 提交内容时回调
    this.onSubmitted,                   // 用户提示完成时回调
    this.inputFormatters,               // 验证及格式
    this.enabled,                       // 是否不可点击
    this.cursorWidth = 2.0,             // 光标宽度
    this.cursorRadius,                  // 光标圆角弧度
    this.cursorColor,                   // 光标颜色
    this.keyboardAppearance,            // 键盘亮度
    this.scrollPadding = const EdgeInsets.all(20.0),        // 滚动到视图中时，填充边距
    this.enableInteractiveSelection,    // 长按是否展示【剪切/复制/粘贴菜单LengthLimitingTextInputFormatter】
    this.onTap,                         // 点击时回调
})
```


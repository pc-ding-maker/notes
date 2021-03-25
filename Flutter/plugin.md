# Plugin

开发过程中常用的插件。

## flutter-swiper

常用属性：

|            属性             |                       说明                       |                默认值                |
| :-------------------------: | :----------------------------------------------: | :----------------------------------: |
|          itemCount          |                    轮播图个数                    |               （必传）               |
|         itemBuilder         |             生成轮播item，返回Widget             | (BuildContext context, int index) {} |
|       scrollDirection       |  滚动方向，设置为Axis.vertical如果需要垂直滚动   |           Axis.horizontal            |
|            loop             |                 无限轮播模式开关                 |                 true                 |
|            index            |                初始的时候下标位置                |                  0                   |
|          autoplay           |                   自动播放开关                   |                false                 |
|        autoplayDely         |                自动播放延迟毫秒数                |                 3000                 |
| autoplayDiableOnInteraction |       当用户拖拽的时候，是否停止自动播放.        |                 true                 |
|       onIndexChanged        | 当用户手动拖拽或者自动播放引起下标改变的时候调用 |      onIndexChanged(int index)       |
|            onTap            |           当用户点击某个轮播的时候调用           |           onTap(int index)           |
|          duration           |               动画时间，单位是毫秒               |                300.0                 |
|         pagination          | 设置 `new SwiperPagination()` 展示默认分页指示器 |                 null                 |
|           control           |   设置 `new SwiperControl()` 展示默认分页按钮    |                 null                 |

#### pagination：分页指示器

`new SwiperPagination()` 展示默认分页.



|   属性    |                             说明                             |         默认值         |
| :-------: | :----------------------------------------------------------: | :--------------------: |
| alignment |     如果要将分页指示器放到其他位置，那么可以修改这个参数     | Alignment.bottomCenter |
|  margin   |                  分页指示器与容器边框的距离                  |  EdgeInsets.all(10.0)  |
|  builder  | 目前已经定义了两个默认的分页指示器样式： `SwiperPagination.dots` 、 `SwiperPagination.fraction`，都可以做进一步的自定义. | SwiperPagination.dots  |

**实例代码：**

```dart
Widget get _banner {
    return Container(
      height: 160,
      child: Swiper(
        itemCount: bannerList.length,
        autoplay: true,
        itemBuilder: (BuildContext context, int index) {
          return Image.network(
              bannerList[index].icon,
              fit: BoxFit.fill,
            );
        },
        pagination: SwiperPagination(),
      ),
    );
  }
```


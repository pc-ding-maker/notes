# SVG
------
>SVG英文全称为Scalable Vector Graphics，意思为可缩放的矢量图
+ 默认的宽度是300px, 默认的高度是150px
##### 位图和矢量图：

+ 在计算机中有两种图形, 一种是位图, 一种是矢量图
+ 位图:
	
		 传统的 jpg / png / gif图都是位图
		 位图是一个个很小的颜色小方块组合在一起的图片。一个小方块代表1px
		 优点: 色彩丰富逼真
		 缺点: 放大后会失真, 体积大

+ 矢量图:

	    矢量图是用XML格式定义, 通过各种「路径」和「填充颜色」来描述渲染的的图片
	    优点: 放大后不会失真, 体积小
	    缺点: 不易制作色彩变化太多的图象

## SVG使用方式:
#### svg常用有四种使用方式：
1. 内嵌到HTML中(直接在HTML中绘制)
2. 通过浏览器直接打开SVG文件
**注意点:**
如果需要将svg保存到一个单独的文件中, 并且需要通过浏览器直接打开, 那么就必须给svg添加一个属性
`xmlns="http://www.w3.org/2000/svg"`
3. 在HTML的img标签中引用
4. 作为CSS背景使用

## SVG常用属性：

 	fill: 修改填充颜色
    fill-opacity: 0~1 设置填充颜色的透明度
    stroke: 修改描边颜色
    stroke-width: 修改描边宽度
    stroke-opacity: 0~1 设置描边透明度
    stroke-linecap: butt/square/round  设置线段两端帽子
    stroke-dasharray: 设置虚线
    stroke-dashoffset: 设置虚线偏移位
    stroke-linejoin: miter/bevel/round 设置折线转角样式
##### 注意点：
+ 在SVG中这些所有的常用属性都是可以直接在CSS中使用的

## SVG绘制：
### 绘制矩形：
+ 通过rect标签来绘制

##### 属性：

    x/y: 指定绘制的位置
    width/height: 指定绘制的大小
    fill: 修改填充的颜色
    stroke: 修改描边的颜色
    stroke-width: 修改描边的宽度
    rx/ry: 设置圆角的半径

###### 代码示例：
```html
<body>
<svg width="500" height="500">
    <rect x="100" y="100" width="100" height="100" fill="blue"  rx="10" ></rect>
</svg>
</body>
```

### 绘制圆形：
+ 通过circle标签来绘制

###### 代码示例
```html
<body>
<svg width="500" height="500">
    <!--
    cx/cy: 圆绘制的位置(圆心的位置)
    r: 圆的半径
    -->
    <circle cx="100" cy="100" r="50"></circle>
</svg>
</body>
```

### 绘制椭圆：
+ 通过ellipse标签来绘制

###### 代码示例：
```html
<body>
<svg width="500" height="500">
    <!--
    cx/cy: 椭圆绘制的位置(圆心的位置)
    rx: 水平方向的半径
    ry: 垂直方向的半径
    -->
    <ellipse cx="100" cy="100" rx="100" ry="50"></ellipse>
</svg>
</body>
```

### 绘制直线
+ 通过line标签来绘制

###### 代码示例:
```html
<body>
<svg width="500" height="500">
    <!--
    x1/y1: 设置起点
    x2/y2: 设置终点
    -->
    <line x1="100" y1="100" x2="300" y2="100" stroke="#000"></line>
</svg>
</body>
```

### 绘制折线：
+ 通过polyline来绘制

###### 代码示例:
```html
<body>
<svg width="500" height="500">
    <!--
    points: 设置所有的点, 两两一对
    -->
    <polyline points="100 100 300 100 300 300" stroke="#000" fill="none"></polyline>
</svg>
</body>
```

### 绘制多边形：
+ 通过polygon来绘制
+ polygon和polyline差不多, 只不过会自动连接第一个点和最后一个点

###### 代码示例:
```html
<svg width="500" height="500">
    <!--
   points: 设置所有的点, 两两一对
    -->
    <polygon points="100 100 300 100 300 300" stroke="#000" fill="none"></polygon>
</svg>
```

## SVG路径
+ SVG路径可以绘制任意图形
+ 通过path标签来绘制路径

##### 常用取值：
+ 都是写在path的d属性中

		M = moveto  起点
	    L = lineto  其它点
	    H = horizontal lineto 和上一个点Y相等
	    V = vertical lineto   和上一个点X相等
	    Z = closepath  关闭当前路径

##### 代码示例：
```html
<svg width="500" height="500">
    <path d="M 100 100 L 300 100" stroke="red"></path>
    <path d="M 100 100 L 300 100 L 300 300" stroke="red" fill="none" stroke-width="10"></path>
    <path d="M 100 100 L 300 100 L 300 300 L 100 100" stroke="red" fill="none" stroke-width="10"></path>
</svg>
```

###### 注意点：
	路径的属性区分大小写
	大写字母是绝对定位, 小写字母是相对定位
    绝对定位: 写什么位置就是什么位置
    相对定位: 相对上一次的位置, 在上一次位置基础上做调整

### 绘制圆弧：
+ 通过path标签中d属性的A来绘制

	 	A = elliptical Arc
	    A(rx, ry, xr, laf, sf, x, y) 从当前位置绘制弧线到指定位置
	    rx (radiux-x): 弧线X半径
	    ry (radiux-y): 弧线Y半径
	    xr (xAxis-rotation): 弧线所在椭圆旋转角度
	    laf(large-arc-flag): 是否选择弧长较长的那一段  取值:0 较短的，1:较长的
	    sf (sweep-flag): 是否顺时针绘制               取值:0 逆时针。 1:顺时针
	    x,y: 弧的终点位置

##### 代码示例:
```html
<svg width="500" height="500">
    <path d="M 100 400 A 100 50 90 1 1 200 450" stroke="red" fill="none"></path>
</svg>
```

### 绘制文本：
+ 通过text标签来绘制
+ 以左下角作为参考
+ 文字的基线和指定的位置对齐

##### 代码示例：
```html
 <!--
    x/y: 指定绘制位置
    style: 设置文字样式 (大小/字体等)
    text-anchor: 指定文字水平方向对齐方式
    dominant-baseline: 指定文字垂直方向对齐方式
    dx/dy: 相对于前一个文字位置, 未设置位置的文字会继承前一个文字
    -->

//绘制一行文字
 <text x="250" y="250" style="font-size: 40px;" fill="none" stroke="red" text-anchor="middle" dominant-baseline="middle">SVG绘制文字</text>

//绘制多行文字
<text fill="yellow">
        <tspan x="100" y="100">SSSSS</tspan>
        <tspan x="100" y="150">VVVVV</tspan>
        <tspan x="100" y="200">GGGGG</tspan>
    </text>

```

### 绘制路径文本
+ 文字基于给定的路径来绘制

##### 步骤：
1. 定义一个路径
2. 告诉文本需要按照哪个路径来绘制
	+ 通过`text`标签下的`textPath`标签的`xlink.href`属性来指定

##### 注意点：
+ 如果是绘制路径文本, 那么超出路径范围的内容不会被绘制出来

##### 代码示例：
```html
<svg width="500" height="500">
    <path id="myPath" d="M 100 100 Q 150 50 200 100" stroke="red" fill="none"></path>
    <text>
        <textPath xlink:href="#myPath">SVG666</textPath>
    </text>
</svg>
```

### 绘制超链接
+ 可以给任意内容添加超链接, 只需要用超链接包裹起来即可

##### 代码示例:
```html
<svg width="500" height="500">
    <!--
    xlink:href: 指定链接地址
    xlink:title: 指定链接提示
    target: 指定打开方式
    -->
    <a xlink:href="http://www.baidu.com" xlink:title="百度" target="_blank">
        <circle cx="100" cy="100" r="100" fill="red"></circle>
    </a>
</svg>
```

### 绘制图片
+ 通过image标签来绘制
+ 默认情况下我们的指定的图片多大就绘制多大
+ 当设置的尺寸和图片实际尺寸不一样时，高度填满, 宽度等比拉伸

###### 代码示例:
```html
<svg width="500" height="500">
	//通过xlink:href来指定图片地址
    <image xlink:href="images/image.jpg" x="100" y="100"></image>
</svg>
```
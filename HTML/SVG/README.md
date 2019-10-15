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

## 结构元素
### g
+ g是group的缩写, 可以将多个元素放到一个g标记中, 这样就组成了一个组,以便统一操作
+ 对g标记设置的所有样式都会应用到这一组所有的元素中
+ g封装的元素默认是可见的

##### 代码示例：
```html
 <g id="myGroup">
            <circle cx="100" cy="100" r="100"></circle>
            <circle cx="100" cy="200" r="50"></circle>
            <circle cx="100" cy="300" r="30"></circle>
        </g>
<use xlink:href="#myGroup" x="300" fill="blue"></use>
```

### defs
+ defs默认不可见
+ 如果仅仅是需要定义一组模板, 将来需要用到时候才显示, 那么就可以使用defs包裹起来

##### 代码示例：
```html
   <defs>
        <g id="myGroup">
            <circle cx="100" cy="100" r="100"></circle>
            <circle cx="100" cy="200" r="50"></circle>
            <circle cx="100" cy="300" r="30"></circle>
        </g>
    </defs>
    <use xlink:href="#myGroup" x="0" fill="blue"></use>
    <use xlink:href="#myGroup" x="300" fill="red"></use>
```

### symbol
+ symbol兼具<g>的分组功能和<defs>初始不可见的特性
+  symbol能够创建自己的视窗，所以能够应用viewBox和preserveAspectRatio属性。

##### 代码示例：
```html
<symbol>
        <g id="myGroup">
            <circle cx="100" cy="100" r="100"></circle>
            <circle cx="100" cy="200" r="50"></circle>
            <circle cx="100" cy="300" r="30"></circle>
        </g>
    </symbol>
    <use xlink:href="#myGroup" x="0" fill="blue"></use>
    <use xlink:href="#myGroup" x="300" fill="red"></use>
```

### use
+ g结构元素封装的图形还可以通过<use>元素进行复制使用
+ `<use  xlink:href="#id"/>`

## 裁剪和蒙版
### 裁剪
+ 通过clipPath标签实现
+ 只有路径范围内的内容会被显示, 路径范围外的内容不会被显示
+ 裁切路径是可见与不可见的突变

##### 代码示例:
```html
<clipPath id="myClip">
        <circle cx="200" cy="200" r="100" fill="red"></circle>
    </clipPath>
    <rect x="100" y="100" width="300" height="200" fill="blue" clip-path="url(#myClip)"></rect>
```

### 蒙版
+ 通过mask标签实现
+ 蒙版则是可见与不可见的渐变

##### 代码示例：
```html
<mask id="myMask">
        <circle cx="200" cy="200" r="100" fill="rgba(255, 0, 0, 0.5)"></circle>
    </mask>
    <rect x="100" y="100" width="300" height="200" fill="blue" mask="url(#myMask)"></rect>
```

## 渐变
+ 线性渐变通过linearGradient标签来实现
+ 径向渐变通过radialGradient标签来实现

### 渐变属性：
	 x1/y1: 渐变范围开始位置
     x2/y2: 渐变范围结束位置
     默认情况下x1/y1/x2/y2是当前元素的百分比
     可以通过gradientUnits修改
     gradientUnits="objectBoundingBox"
     gradientUnits="userSpaceOnUse"

###### 注意点：
+ 使用渐变颜色需要通过url(#id)格式来使用

#### 代码示例:
```html
<linearGradient id="myColor" x1="100" y1="100" x2="400" y2="100" gradientUnits="userSpaceOnUse">
            <stop offset="0" stop-color="red"></stop>
            <stop offset="1" stop-color="blue"></stop>
</linearGradient>
<rect x="100" y="100" width="300" height="200" fill="url(#myColor)"></rect>
```

## 画笔：
+ 在SVG中除了可以使用纯色和渐变色作为填充色以外, 还可以使用自定义图形作为填充
+ 通过pattern来定义

#### 属性：
	width/height默认情况下也是百分比
    可以通过gradientUnits修改
    patternUnits="objectBoundingBox"  
    patternUnits="userSpaceOnUse"	

#### 代码示例:
```html
<pattern id="myPattern" width="20" height="20" patternUnits="userSpaceOnUse">
            <circle cx="10" cy="10" r="10" fill="red"></circle>
</pattern>
<rect x="100" y="100" width="300" height="200" fill="url(#myPattern)"></rect>
```	

## 形变
+ 和Canvas一样, 改变的是坐标系

## ViewBox
+ ViewBox就是可视区域, 用户能看到的区域
+ 默认情况下，可视区域的大小和内容区域大小是一致的

#### 属性
	viewBox="x y width height"
    x:修改可视区域x方向位置
    y:修改可视区域y方向位置
    width/height: 修改可视区域尺寸, 近大远小

#### 代码示例:
```html
<svg width="200" height="200" viewBox="0 0 200 200">
    <circle cx="100" cy="100" r="50" fill="red"></circle>
</svg>
```

##### 注意点:
	默认情况下如果viewBox的尺寸是等比缩放的, 那么调整后viewBox区域的xy和内容区域的xy对齐,
	但是如果viewBox的尺寸不是等比缩放的, 那么系统就会调整viewBox的位置, 我们设置的x/y会失效
	此时如果需要viewBox的xy和内容区域(viewProt)的xy继续保持从何, 那么就需要使用preserveAspectRatio属性来设置对齐方式

#### preserveAspectRatio属性:
	preserveAspectRatio 第一个参数
	xMin	viewport和viewBox左边对齐
	xMid	viewport和viewBox x轴中心对齐
	xMax	viewport和viewBox右边对齐
	YMin	viewport和viewBox上边缘对齐。注意Y是大写。
	YMid	viewport和viewBox y轴中心点对齐。注意Y是大写。
	YMax	viewport和viewBox下边缘对齐。注意Y是大写。

	preserveAspectRatio 第二个参数
	meet	保持纵横比缩放viewBox适应viewport
	slice	保持纵横比同时比例小的方向放大填满viewport
	none	扭曲纵横比以充分适应viewport

#### 代码示例：
```html
<svg width="200" height="200" viewBox="0 0 50 150" preserveAspectRatio="xMinYMin">
    <circle cx="50" cy="50" r="50" fill="red"></circle>
</svg>
```

## 动画
### 动画使用方式
1. 直接在需要执行动画的标签下定义动画

2. 将定义好的动画运用到指定标签上

### 动画属性：
	attributeType: CSS/XML 规定的属性值的名称空间
    attributeName: 规定元素的哪个属性会产生动画效果
    from/to: 从哪到哪
    dur: 动画时长
    fill: 动画结束之后的状态 保持freeze结束状态/remove恢复初始状态
	repeatCount: 规定动画重复的次数。
    repeatDur: 规定动画重复总时长
    begin: 规定动画开始的时间
        begin="1s"
        begin="click"
        begin="click + 1s"
    restart: 规定元素开始动画之后，是否可以被重新开始执行
        always：动画可以在任何时候被重置。这是默认值。
        whenNotActive：只有在动画没有被激活的时候才能被重置，例如在动画结束之后。
        never：在整个SVG执行的过程中，元素动画不能被重置。
    calcMode: 规定每一个动画片段的动画表现
        linear：默认属性值, 匀速动画
        discrete: 非连续动画, 没有动画效果瞬间完成
        paced: 规定整个动画效果始终以相同的速度进行，设置keyTimes属性无效
        spline: 配合keySplines属性来定义各个动画过渡效, 自定义动画
    keyTimes:
        划分动画时间片段, 取值0-1
    values:
        划分对应取值片段的值
#### 基础动画
+ 通过animate标签定义
##### 代码示例
```html
<circle cx="100" cy="100" r="50" fill="blue">
            <animate
                    attributeName="r"
                    from="50"
                    to="100"
                    dur="5s"
                    fill="freeze"
            ></animate>
        </circle>
```

#### 形变动画
+ 通过animateTransform标签定义

##### 代码示例:
```html
<rect x="100" y="100" width="300" height="200" fill="blue">
        <animateTransform
                attributeName="transform"
                type="scale"
                from="1 1"
                to="0.5 1"
                dur="2s"
                begin="click"
                fill="freeze"
        ></animateTransform>
    </rect>
```

#### 路径动画:
+ 通过animateMotion标签定义

##### 代码示例
```html
<path d="M0 0 C0 300 300 300 300 0" stroke="red" stroke-width="2" fill="none"></path>
    <rect x="0" y="0" width="40" height="40" fill="rgba(255,0,0,0.5)">
        <animateMotion
            path="M0 0 C0 300 300 300 300 0"
            dur="5s"
            begin="click"
            fill="freeze"
            rotate="auto"
        ></animateMotion>
    </rect>
```

## SVG脚本编程
#### 脚本编程注意点：
1. 创建SVG时必须指定命名空间
```javascript
	const SVG_NS = "http://www.w3.org/2000/svg"
    // let oSvg = document.createElement("svg"); //错误
    let oSvg = document.createElementNS(SVG_NS,"svg");//正确
```

2. 使用xlink属性也必须指定命名空间
```javascript
	const XLINK_NS = "http://www.w3.org/1999/xlink";
    // oImage.setAttribute("xlink:href", "images/image.jpg");//错误
    oImage.setAttributeNS(XLINK_NS,"xlink:href", "images/lnj.jpg");//正确
```

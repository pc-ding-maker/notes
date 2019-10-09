# canvas
----
+ Canvas是H5新增的一个标签, 我们可以通过JS在这个标签上绘制各种图案
+ [文档链接](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API)

##### 操作canvas步骤：
1. 通过js代码拿到canvas标签
	`let oCanvas = document.querySelector("canvas");`
2. 从canvas中拿到绘图工具
	`let oCtx = oCanvas.getContext("2d");`
3. 进行操作

###### canvas注意点：
1. canvas有默认的宽度和高度，默认宽`300px`, 高`150px`
2. 不能通过CSS设置画布的宽高（ 通过CSS设置画布宽高会在默认宽高的基础上拉伸）,需要通过行内属性`width`和`height`来设置
3. 通过canvas绘制的线条默认宽度是1px, 颜色是纯黑色，但是由于默认情况下canvas会将线条的中心点和像素的底部对齐,
    所以会导致显示效果是2px和非纯黑色问题	
4. `</canvas>` 标签不可省（如果结束标签不存在，则文档的其余部分会被认为是替代内容，将不会显示出来）

## 绘制线条：
```JavaScript
//拿到canvas标签
 let oCanvas = document.querySelector("canvas");
//拿到绘图工具 
let oCtx = oCanvas.getContext("2d");
//设置起点坐标
 oCtx.moveTo(50, 50);
//设置点坐标
 oCtx.lineTo(200, 50);
//设置线条的宽度
 oCtx.lineWidth = 20;
//设置线条的样式
 oCtx.strokeStyle = "blue";
//将两个点链接起来
 oCtx.stroke();

//生成一条新的路径
oCtx.moveTo(50, 100);
oCtx.lineTo(200, 100);
// 重新设置当前路径样式
oCtx.lineWidth = 10; 
oCtx.strokeStyle = "red";
oCtx.stroke();
```

##### 注意点：
1. 如果是同一个路径, 那么路径样式会被重用(第二次绘制会复用第一次的样式)
2. 如果是同一个路径, 那么后设置的路径样式会覆盖先设置的路径样式
3. 需要绘制多条线条并单独设置样式时，绘制另一条线条是需要先利用`beginPath()`来重启一个路径

##### 常用方法：
1. `closePath()`：自动创建从当前点回到起始点的路径
2. `lineJoin`(属性): 设置相交线的拐点样式 miter(默认)、round、bevel
3. `setLineDash()`: 在填充线时使用虚线模式。 它使用一组值来指定描述模式的线和间隙的交替长度。如果要切换回至实线模式，将 dash list 设置为一个空数组即可。
4. `getLineDash()`: 获取虚线的排列方式 获取的是不重复的那一段的排列方式
5. `lineDashOffset()`: 设置虚线的偏移位
## 绘制矩形：
```javascript
//1. 绘制一个填充的矩形
	fillRect(x, y, width, height)
//2. 绘制一个矩形的边框
	strokeRect(x, y, width, height)
//3. 清除指定矩形区域，让清除部分完全透明。
	clearRect(x, y, width, height)
```

## 绘制圆弧
```javascript
/*
1.基本概念
    角度: 一个圆360度, 一个半圆是180度
    弧度: 一个圆2π, 一个半圆π
2.角度转换弧度公式:
    ∵ 180角度 = π弧度
    ∴ 1角度 = π/180;
    ∴ 弧度 = 角度 * π/180;
       90角度 * π/180 = π/2
3.弧度转换角度公式:
    ∵ π弧度 = 180角度
    ∴ 1弧度 = 180/π
    ∴ 角度 = 弧度 * 180/π
       π/2 * 180/π = 180/2 = 90度
*/
arc(x, y, radius, startAngle, endAngle, Boolean)；
/*	x, y: 确定圆心
    radius: 确定半径
    startAngle: 确定开始的弧度
    endAngle: 确定结束的弧度
    Boolean: 默认是false, false就是顺时针绘制, true就是逆时针绘制
*/
```
## 绘制文字
```javascript
//在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的
 fillText(text, x, y [, maxWidth])
// 在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.
strokeText(text, x, y [, maxWidth])
```
##### 注意点：在绘制文字的时候, 是以文字的左下角作为参考点进行绘制

## 绘制图片
```javascript
// 1.拿到canvas
    let oCanvas = document.querySelector("canvas");
    // 2.从canvas中拿到绘图工具
    let oCtx = oCanvas.getContext("2d");
    // 3.加载图片
    let oImg = new Image();
    oImg.onload = function () {
        // 如果只有三个参数, 那么第一个参数就是需要绘制的图片
        // 后面的两个参数是指定图片从什么位置开始绘制
        // oCtx.drawImage(oImg, 100, 100);

        // 如果只有五个参数, 那么第一个参数就是需要绘制的图片
        // 后面的两个参数是指定图片从什么位置开始绘制
        // 最后的两个参数是指定图片需要拉伸到多大
        // oCtx.drawImage(oImg, 100, 100, 100, 100);

        // 如果有九个参数, 那么第一个参数就是需要绘制的图片
        // 最后的两个参数是指定图片需要拉伸到多大
        // 第6~7个参数指定图片从什么位置开始绘制
        // 第2~3个参数指定图片上定位的位置
        // 第4~5个参数指定从定位的位置开始截取多大的图片
        oCtx.drawImage(oImg, 50, 50, 100, 100, 100, 100, 100, 100);
    }
    oImg.src = "images/image.jpg";
```
## 渐变色
```javascript
/*设置图形渐变背景颜色步骤
    1、通过绘图工具创建渐变背景颜色
    2、指定渐变的范围
    3、将渐变背景颜色设置给对应的图形
*/
// 1.拿到canvas
    let oCanvas = document.querySelector("canvas");
    // 2.从canvas中拿到绘图工具
    let oCtx = oCanvas.getContext("2d");

    // 1.创建一个渐变的方案
    /*
    可以通过x0,y0 / x1,y1确定渐变的方向和渐变的范围
    * */
    let linearGradient = oCtx.createLinearGradient(100, 100, 300, 300);
    /*
    第一个参数是一个百分比 0~1
    第二个参数是一个颜色
    * */
    linearGradient.addColorStop(0, "green");
    linearGradient.addColorStop(0.5, "yellow");
    linearGradient.addColorStop(1, "blue");

    oCtx.fillStyle = linearGradient;
    oCtx.fillRect(100, 100, 200, 200);
```
## 形变
```javascript
//移动
translate(x, y)
//translate 方法接受两个参数。x 是左右偏移量，y 是上下偏移量
//旋转
rotate(angle)
// 这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。
//缩放
scale(x, y)
/*
scale  方法可以缩放画布的水平和垂直的单位。两个参数都是实数，可以为负数，x 为水平缩放因子，y 为垂直缩放因子，如果比1小，会比缩放图形， 如果比1大会放大图形。默认值为1， 为实际大小。
*/
```
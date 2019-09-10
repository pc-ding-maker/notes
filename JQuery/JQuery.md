# jQuery
------
### 内容过滤选择器
+ :empty
	+ 作用：找到既没有文本内容也没有子元素的指定元素

	```JavaScript
	let $div = $("div:empty");
	//查找既没有文本内容也没有子元素的div
	```
+ :parent
	+ 作用：找到有文本内容或有子元素的指定元素

	```javascript
	let $div = $("div:parent")
	//查找有文本内容或者有子元素的div元素
	```

+ :contains(text)
	+ 作用: 找到包含指定文本内容的指定元素

	```javascript
	let $div = $("div:contains('我是div')");
	//查找包含‘我是div’文本内容的div元素
	```

+ :has(selector)
	+ 作用: 找到包含指定子元素的指定元素

	```javascript
	let $div = $("div:has('span')")
	//查找有子元素为span标签的div元素
	```

### 操作类相关方法
+ addClass(class|fn)
		
		作用：添加一个类
		如果想添加多个，多个类名之间用空格隔开即可
		$("div").addClass("class1 class2");//给div添加class1和class2

+ removeClass(class|fn)
		
		作用：删除一个类
		如果想删除多个，多个类名之间用空格隔开即可
		$("div").removeClass("class2 class1");//给div删除class1和class2

+ toggleClass(class|fn)
		
		作用：切换类
		如果有就删除，如果没有就添加
		$("div").toggleClass("class2 class1");

### 文本值相关方法
+ html()
	
		作用：和原生JS的innerHtml()一样
		用于获取或设置元素的文本内容

+ text()

		作用：和原生JS的innerText()一样
		用于获取或设置元素的文本内容

+ val()
		
		作用：返回或设置被选元素的值。
		元素的值是通过 value 属性设置的。该方法大多用于 input 元素。
		如果该方法未设置参数，则返回被选元素的当前值。

### 操作样式相关方法
+ .css()
		
		作用：设置或获取相关样式

	```javascript
	//1.逐个设置
            $("div").css("width", "100px");
            $("div").css("height", "100px");
            $("div").css("background", "red");
	// 2.链式设置
            // 注意点: 链式操作如果大于3步, 建议分开
            $("div").css("width", "100px").css("height", "100px").css("background", "blue");
	//3.批量设置
            $("div").css({
                width: "100px",
                height: "100px",
                background: "red"
            });
	// 4.获取CSS样式值
		$("div").css("width")
	```

### 位置尺寸相关方法
+ width()
		
		作用：获取元素的宽度
+ offset([coordinates])

		作用: 获取元素距离窗口的偏移位
+ position()

		作用: 获取元素距离定位元素的偏移位
		注意点: position方法只能获取不能设置
+ scrollTop()

		作用：获取或设置滚动条的偏移位
		// 注意点: 为了保证浏览器的兼容, 获取网页滚动的偏移位需要按照如下写法
           $("body").scrollTop()+$("html").scrollTop();
		为了保证浏览器的兼容, 设置网页滚动偏移位的时候必须按照如下写法
          $("html,body").scrollTop(300);


## 事件相关

##### 事件绑定（可以添加多个相同或者不同类型的事件,不会覆盖）
+ 绑定方式一：

		eventName(fn)
		编码效率略高/ 部分事件jQuery没有实现,所以不能添加
		 $("button").click(function () {
              alert("hello button");
         });
+ 绑定方式二：

		on(eventName, fn)
		编码效率略低/ 所有js事件都可以添加
		$("button").on("click", function () {
                alert("hello click1");
            });


##### 事件移除
+ off

	 	off方法如果不传递参数, 会移除所有的事件
        $("button").off();
        off方法如果传递一个参数, 会移除所有指定类型的事件
        $("button").off("click");
        off方法如果传递两个参数, 会移除所有指定类型的指定事件(移除指定执行的操作)
		$("button").off("click", test1);

##### 阻止事件冒泡
+ 什么是事件冒泡?
	+ 事件冒泡是从目标元素逐级向上传播到根节点的过程
	+ 小明告诉爸爸他有一个女票,爸爸告诉爷爷孙子有一个女票,一级级向上传递就是事件冒泡
<br>

+ 如何阻止事件冒泡?
	+ 多数情况下，我们希望在触发一个元素的事件处理程序时，不影响它的父元素, 此时便可以使用停止事件冒泡
	+ `stopPropagation()`
	```javascript
	$(function () {
            $(".son").click(function (event) {
                console.log(".son");
                // 在子元素中停止事件冒泡,时间不会继续向上传播,所以父元素click方法不会被触发
                event.stopPropagation();
            });
            $(".father").click(function () {
                console.log(".father");
            });
        });
	```

##### 阻止事件默认行为
+ 什么是默认行为?
	+ 网页中的元素有自己的默认行为,例如单击超链接后会跳转,点击提交表单按钮会提交
<br>

+ 方式一：**event.preventDefault()**
+ 方式二: **return false;**

```javascript
$("a").click(function (event) {
                alert("阻止默认行为");
                // return false;  			方式一 
                event.preventDefault();              //  方式二
            });
```

##### 自动触发事件
+ 通过代码控制事件, 不用人为点击/移入/移除等事件就能被触发

+ 方式一：
	+ trigger("eventName")
	+ 触发事件的同时会触发事件冒泡
	+ 触发事件的同时会触发事件默认行为


+ 方式二：
	+ triggerHandler("eventName");
	+ 触发事件的同时不会触发事件冒泡
	+ 触发事件的同时不会触发事件默认行为

```javascript
$(".son").trigger("click");
$(".son").triggerHandler("click");
```

##### 自定义事件
+ 自定义事件就是自己可以随便起一个不存在的事件名称来注册事件, 然后通过这个名称还能触发对应的方法执行, 这就是传说中的自定义事件
+ 注意点：
	+ 事件必须是通过on绑定的
	+ 事件必须通过trigger来触发
	+ 因为trigger方法可以自动触发对应名称的事件,所以只要事件的名称和传递给trigger的名称一致就能执行对应的事件方法

```javascript
$(function () {
           $(".father").on("dpcClick", function () {
               alert("dpcClick");
           });
           $(".father").trigger("dpcClick");
});
```

##### 事件命名空间
+ 事件命名空间主要用于区分相同类型的事件,区分不同前提条件下到底应该触发哪个人编写的事件
+ 格式：**"eventName.命名空间"**
+ 注意点：
	+ 事件是通过on来绑定的
	+ 通过trigger触发事件
<br>

+ 面试常考题：

		利用trigger触发子元素带命名空间的事件, 那么父元素带相同命名空间的事件也会被触发. 而父元素没有命名空间的事件不会被触发
            利用trigger触发子元素不带命名空间的事件,那么子元素所有相同类型的事件和父元素所有相同类型的事件都会被触发
		不带命名空间事件被trigger调用,会触发带命名空间事件
		带命名空间事件被trigger调用,只会触发带命名空间事件
		下级不带命名空间事件被trigger调用,会冒泡触发上级不带命名空间和带命名空间事件
		下级带命名空间事件被trigger调用,不会触发上级不带命名空间事件
	        下级带命名空间事件被trigger调用,会触发上级带命名空间事件
##### 事件委托
+ 请别人帮忙做事情, 然后将做完的结果反馈给我们
+ 好处
	+ 减少监听数量
	+ 新增元素自动有事件响应处理

+ 格式:`$(parentSelector).delegate(childrenSelector, eventName, callback)`

```javascript
 $(function () {
            // 1.委托ul监听li的点击事件
            $("ul").delegate("li","click",function () {
                // 前面我们说过事件委托就是让别人帮忙做事,但最终的结果还是会返回到我们手里,所以这里的this是触发事件的li
                // 这里的this之所以是触发事件的li,本质是因为"事件冒泡", 触发事件的li向上传递到ul,触发了click事件.
                // 弹出当前点击行内容
                alert($(this).html());
            });

            // 2.监听新增按钮点击
            var count = 0;
            $("button").eq(0).click(function () {
                count++;
                // 新增一行内容
                $("ul").append("<li>我是新增内容"+count+"</li>")
            });
        });
```

##### 移入移出事件
+ mouseover/mouseout事件, 子元素被移入移出也会触发父元素的事件
+ mouseenter/mouseleave事件, 子元素被移入移出不会触发父元素的事件（推荐使用）


## 动画相关
##### 显示隐藏动画：
+ 显示动画：`show([s,[e],[fn]])`
+ 内部实现原理根据当前操作的元素是块级还是行内决定, 块级内部调用`display:block;`,行内内部调用`display:inline;`
```javascript
	//第一个参数：执行事件
	//第二个参数：用来指定切换效果，默认是"swing"，可用参数"linear"
	//第三个参数：执行完动画后调用的函数
    // 注意: 这里的时间是毫秒
    $("div").show(1000, function () {
        // 作用: 动画执行完毕之后调用
        alert("显示动画执行完毕");
    });

```

+ 隐藏动画：`hide([s,[e],[fn]])`
```javascript
	//第一个参数：执行事件
	//第二个参数：用来指定切换效果，默认是"swing"，可用参数"linear"
	//第三个参数：执行完动画后调用的函数
    // 注意: 这里的时间是毫秒
     $("div").hide(1000, function () {
        alert("隐藏动画执行完毕");
    });

```
+ 切换动画:`toggle([spe],[eas],[fn])`
	+ 切换动画(显示变隐藏,隐藏变显示)

```javascript
	//第一个参数：执行事件
	//第二个参数：用来指定切换效果，默认是"swing"，可用参数"linear"
	//第三个参数：执行完动画后调用的函数
 $("div").toggle(1000, function () {
        alert("切换动画执行完毕");
    });
```
##### 展开、收起动画
+ 参数、注意事项和显示隐藏动画一模一样, 只不过动画效果不一样而已
+ 展开动画：`slideDown([s],[e],[fn])`
+ 收起动画：`slideUp([s,[e],[fn]])`
+ 切换动画(展开变收起,收起变展开):`slideToggle([s],[e],[fn])`

##### 淡入、淡出动画
+ 参数、注意事项和显示隐藏动画一模一样, 只不过动画效果不一样而已
+ 淡入动画：`fadeIn([s],[e],[fn])`
+ 淡出动画：`fadeOut([s],[e],[fn])`
+ 切换动画(显示变淡出,不显示变淡入)：`fadeToggle([s,[e],[fn]])`
+ 淡入到指定透明度动画:`fadeTo([[s],o,[e],[fn]])`
	+ 可以通过第二个参数,淡入到指定的透明度(取值范围0~1)


##### 自定义动画
+ 有时候jQuery中提供的集中简单的固定动画无法满足我们的需求, 所以jQuery还提供了一个自定义动画方法来满足我们复杂多变的需求
+ `animate(p,[s],[e],[fn])`
```javascript
/*
第一个参数: 接收一个对象, 可以在对象中修改属性
第二个参数: 指定动画时长
第三个参数: 指定动画节奏, 默认就是swing
第四个参数: 动画执行完毕之后的回调函数
*/
$(".two").animate({
    marginLeft: 500
}, 5000, "linear", function () {
    // alert("自定义动画执行完毕");
});
//每次开始运动都必须是初始位置或者初始状态,如果想在上一次位置或者状态下再次进行动画可以使用累加动画
$("button").eq(1).click(function () {
    $(".one").animate({
        width: "+=100"
    }, 1000, function () {
        alert("自定义动画执行完毕");
    });
});
//同时操作多个属性,自定义动画会执行同步动画,多个被操作的属性一起执行动画
$(".one").animate({
    width: 500,
    height: 500
}, 1000, function () {
    alert("自定义动画执行完毕");
});
```
##### 动画队列
+ 多个动画方法链式编程,会等到前面的动画执行完毕再依次执行后续动画
	`$("div").slideDown(1000).slideUp(1000).show(1000);`
+ 如果后面紧跟一个非动画方法则会被立即执行
	```javascript
	// 立刻变为黄色,然后再执行动画
	$(".one").slideDown(1000).slideUp(1000).show(1000).css("background", "yellow");	
	```
+ 如果想颜色再动画执行完毕之后设置
	 1.使用回调
	 2.使用动画队列
	```javascript
	$(".one").slideDown(1000).slideUp(1000).show(1000).queue(function () {
    	$(".one").css("background", "yellow")
	});
	```
	+ 注意点:
		+ 动画队列方法queue()后面不能继续直接添加queue()
		+ 如果想继续添加必须在上一个queue()方法中next()方法
	```javascript
	$(".one").slideDown(1000).slideUp(1000).show(1000).queue(function (next) {
    	$(".one").css("background", "yellow");
   			 next(); // 关键点
		}).queue(function () {
    		$(".one").css("width", "500px")
				});
	```

##### 动画相关方法
+ 设置动画延迟时长 `delay(d,[q])`
+ 停止指定元素上正在执行的动画:`stop([c],[j])`
	```javascript
	 // 立即停止当前动画, 继续执行后续的动画
      $("div").stop();
      $("div").stop(false);
      $("div").stop(false, false);

      立即停止当前和后续所有的动画
      $("div").stop(true);
      $("div").stop(true, false);

      立即完成当前的, 继续执行后续动画
      $("div").stop(false, true);

      立即完成当前的, 并且停止后续所有的
      $("div").stop(true, true);
	```

## 节点相关操作
##### 添加节点
	
	内部插入
            append(content|fn)
            appendTo(content)
            会将元素添加到指定元素内部的最后

            prepend(content|fn)
            prependTo(content)
            会将元素添加到指定元素内部的最前面


     外部插入
            after(content|fn)
            会将元素添加到指定元素外部的后面

            before(content|fn)
            会将元素添加到指定元素外部的前面

            insertAfter(content)
            insertBefore(content)
##### 删除节点
+ `empty()`:删除指定元素的内容和子元素, 指定元素自身不会被删除
+ `remove([expr])`:删除指定元素
+ `detach([expr])`:删除指定元素
+ remove和detach的区别：
	+ remove删除元素后,元素上的事件会被移出
	+ detach删除元素后,元素上的事件会被保留
##### 替换节点
+ `replaceWith(content|fn)`: 将所有匹配的元素替换成指定的HTML或DOM元素
+ `replaceAll(selector)`:用匹配的元素替换掉所有 selector匹配到的元素
+ 区别：调用者不同

		$("h1").replaceWith($item);//将h1替换为$item
	    $item.replaceAll("h1");//用$item来替换h1
##### 克隆节点
+ `clone([Even[,deepEven]])`
+ 复制一个节点,浅复制不会复制节点的事件,深复制会复制节点的事件
+ 传**true**为深复制
+ 传**false**为浅复制

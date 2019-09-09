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
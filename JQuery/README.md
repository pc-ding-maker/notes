# jQuery
-----
### 简介：
+ jQuery是一款优秀的JavaScript库，从命名可以看出jQuery最主要的用途是用来做查询（jQuery=js+Query）.

### 特点：
+ 强大选择器: 方便快速查找DOM元素
+ 链式调用: 可以通过.不断调用jQuery对象的方法
+ 隐式遍历(迭代): 一次操作多个元素
+ 事件处理
+ DOM操作
+ 样式操作
+ 动画
+ 丰富的插件支持
+ 浏览器兼容

### jQuery版本
+ 1.x：兼容ie678，但相对其它版本文件较大，官方只做BUG维护，功能不再新增，最终版本：1.12.4 (2016年5月20日).
+ 2.x：不兼容ie678，相对1.x文件较小，官方只做BUG维护，功能不再新增，最终版本：2.2.4 (2016年5月20日)
+ 3.x：不兼容ie678，只支持最新的浏览器，很多老的jQuery插件不支持这个版本，相对1.x文件较小，提供不包含Ajax/动画API版本。

### jQuery入口函数
+ 注意点：
        原生JS和jQuery入口函数的加载模式不同
        原生JS会等到DOM元素加载完毕,并且图片也加载完毕才会执行
        jQuery会等到DOM元素加载完毕,但不会等到图片也加载完毕就会执行
		原生的JS如果编写了多个入口函数,后面编写的会覆盖前面编写的
		jQuery中编写多个入口函数,后面的不会覆盖前面的
<br>

+ 写法：

	+ 第一种写法：
	``` JavaScript
	$(document).ready(function () {
    });
	```

	+ 第二种写法：
	```JavaScript
	jQuery(document).ready(function () {
       });
	```

	+ 第三种写法：(推荐方式)
	```JavaScript
		 $(function () {
        });
	```
	+ 第二种写法：
	```JavaScript
	jQuery(document).ready(function () {
     });
	```
		
	+ 第三种写法：(推荐方式)
	```JavaScript
		 $(function () {
        });
	```
		
	+ 第四种写法:
	``` JavaScript
		jQuery(function(){
		});
	```
		
<br>

+ 解决$（符号）冲突
	+ 方式一：释放$使用权(释放后$就不能用了只能使用jQuery代替$,释放操作必须在其他jquery操作前编写)
	```javaScript
	jQuery.noConflict();
	``` 
	+ 方式二：使用别的符号代替
	```javaScript
	let dpc = jQuery.noConflict();
	//用dpc符号代替$
	```
 
### $()传入不同参数：
	1.传入 '' null undefined NaN  0  false, 返回空的jQuery对象
    2.字符串:
       代码片段:会将创建好的DOM元素存储到jQuery对象中返回
       选择器: 会将找到的所有元素存储到jQuery对象中返回
    3.数组:
       会将数组中存储的元素依次存储到jQuery对象中立返回
    4.除上述类型以外的:
       会将传入的数据存储到jQuery对象中返回
### jQuery静态方法：
+ each

	```JavaScript
	/*
        第一个参数: 遍历到的元素
        第二个参数: 当前遍历到的索引
        注意点:
        原生的forEach方法只能遍历数组, 不能遍历伪数组
        */
        // arr.forEach(function (value, index) {
        //     console.log(index, value);
        // });

        // 1.利用jQuery的each静态方法遍历数组
        /*
        第一个参数: 当前遍历到的索引
        第二个参数: 遍历到的元素
        注意点:
        jQuery的each方法是可以遍历伪数组的
        */
        // $.each(arr, function (index, value) {
        //     console.log(index, value);
        // });
	```

+ map
	```javaScript
	 // 1.利用原生JS的map方法遍历
        /*
        第一个参数: 当前遍历到的元素
        第二个参数: 当前遍历到的索引
        第三个参数: 当前被遍历的数组
        注意点:
        和原生的forEach一样,不能遍历的伪数组
        */
        // arr.map(function (value, index, array) {
        //     console.log(index, value, array);
        // });
        /*
        第一个参数: 要遍历的数组
        第二个参数: 每遍历一个元素之后执行的回调函数
        回调函数的参数:
        第一个参数: 遍历到的元素
        第二个参数: 遍历到的索引
        注意点:
        和jQuery中的each静态方法一样, map静态方法也可以遍历伪数组
        */
        // $.map(arr, function (value, index) {
        //     console.log(index, value);
        // });
	```

#### jquery中each和map的区别
        each静态方法默认的返回值就是, 遍历谁就返回谁
        map静态方法默认的返回值是一个空数组

        each静态方法不支持在回调函数中对遍历的数组进行处理
        map静态方法可以在回调函数中通过return对遍历的数组进行处理, 然后生成一个新的数组返回

+ $.trim() 

		作用: 去除字符串两端的空格
        参数: 需要去除空格的字符串
        返回值: 去除空格之后的字符串

+ $.isWindow()

		作用: 判断传入的对象是否是window对象
        返回值: true/false

+ $.isArray()

		作用: 判断传入的对象是否是真数组
        返回值: true/false

+ $.isFunction()

		作用: 判断传入的对象是否是一个函数
        返回值: true/false
		 注意点:
        jQuery框架本质上是一个函数
        (function( window, undefined ) {
         })( window );
        let res = $.isFunction(jQuery);
        console.log(res); //true

+ $.holdReady(true/false)：

		参数：true or false
		作用：传true代表暂停入口函数的执行，传false代表恢复入口函数的执行
		

### 属性相关函数
+ attr
		attr(name|pro|key,val|fn)
        作用: 获取或者设置属性节点的值
         可以传递一个参数, 也可以传递两个参数
         如果传递一个参数, 代表获取属性节点的值
         如果传递两个参数, 代表设置属性节点的值

         注意点:
         如果是获取:无论找到多少个元素, 都只会返回第一个元素指定的属性节点的值
         如果是设置:找到多少个元素就会设置多少个元素
         如果是设置: 如果设置的属性节点不存在, 那么系统会自动新增

		$("span").attr("class", "box"); //给找到的span添加类名box	

+ removeAttr

		删除属性节点
        注意点:
        会删除所有找到元素指定的属性节点
		$("span").removeAttr("class"); //删除span的类

+ prop

		特点和attr方法一致

+ removeProp

		特点和removeAttr方法一致

#### 注意点：

	prop方法不仅能够操作属性, 他还能操作属性节点
    官方推荐在操作属性节点时,具有 true 和 false 两个属性的属性节点，如 checked, selected 或者 disabled 使用prop()，其他的使用 attr()

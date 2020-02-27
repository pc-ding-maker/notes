# Vue
------
+ Vue是一套构建用户界面的框架。
+ Vue是一款MVVM设计模式的框架。

### 优势：
1. 通过数据驱动页面更新，无需操作DOM来更新页面。
2. 组件化开发，可以将网页拆分成独立组件来编写。


### 基本模板：
``` html
/*
	1.引入vue.js文件
	2.创建一个div并给div添加一个id
	3.创建Vue对象(new Vue({}))
	3.指定Vue对象控制的区域 (el:'#app')
*/
/*
 	MVVM设计模式由三部分组成：
	1. M ：Model 数据模型
	2. V ：View  视图
	3. VM ： ViewModel （数据和视图的桥梁）
*/
<script scr="vue.js"></script>
//MVVM中的View
<div id = "app"></div>
//MVVM中的ViewModel
<script>
	let vue = new Vue({
		el:'#app',
		//MVVM中的Model
		data:{
			
			},
		methods:{
		
		}
	})
</script>
```


### 数据绑定
+ Vue数据可以单向绑定，也可以双向绑定

##### 单向绑定：
```

<p>{{message}}</p>

data:{
	message:'单向绑定'
}
```
##### 双向绑定：
+ Vue双向绑定是利用`V-model`标签实现的。
+ Vue中`input`、`textarea`、`select`元素可以双向绑定
```
<input v-model="value">
data:{
	value:'双向绑定'
}
```
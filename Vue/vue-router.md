# Vue Router
-----
+ 官方提供的路由管理器
+ 用于组件切换

#### 基本使用
1. 引入`vue router`（引入之前需要先引入`vue`）
2. 创建路由规则(路由数组)
3. 根据路由数组创建路由对象
4. 将路由对象挂载到`Vue`实例上
5. 通过`<router-view></router-view>`渲染匹配组件

```html
<div id="app">
	<router-view></router-view>
<script>
//创建路由规则
let routes = [
	{
		path:'/path',//路由路径
		component:'',//匹配的组件
		name:'name',//指定的路由名称
	}
]
//根据路由规则创建路由对象
let router = new VueRouter({
	routes:routes
})
let vue = new Vue({
	el:'#app',
	//将创建好的路由对象挂载到vue实例上
	router:router
})
</script>
<div>
```
#### 路由跳转
**方式一：通过`a`标签进行跳转** 
`<a href = '#/path'></a>`
+ 注意点:
	+ `vueRouter`默认是通过哈希来进行切换的,所以需要在路径前加上`#`号

**方式二:通过`<router-link></router-link>`进行跳转**
`<router-link to ="/path"></router-link>`
+ 注意点:
	+ 通过`router-link`来进行跳转不用在路径前加`#`号
	+ `Vue`在渲染`router-link`的时候, 是通过`a`标签来渲染的
	+ 如果不想使用`a`标签渲染，可以通过`tag`属性来指定标签
		+ `<router-link to ="/path" tag = "button"></router-link>`,通过按钮来渲染


**方式三:通过函数的方式进行跳转:**
+ `this.$router.push()`函数
	- 重复使用这个函数跳转同一个页面会报错
	- 利用这个函数可以传递参数
		- `this.$router.push({path:'/path',query:{message:'message'}})`
		- `this.$router.push({name:'name',params:{message:'message'}})`
	- **注意点:**
		- `params`传参（类似post请求）：路径不能使用 `path` 只能使用` name`，否则` params` 将无效。
		- `query`传参（类似get请求）：路径可以使用`path` 或者 `name`。
+ `this.$router.replace()`函数
+ `this.$router.go()`函数

#### 路由传值：
**方式一：通过URL参数的方式传递**
```html
<!--通过URL参数的方式传递-->
 <router-link to="/one?name=dpc&age=23" tag="button">路由跳转</router-link>
<script>
//通过this.$route.query获取
let name = this.$route.query.name;
let age = this.$route.query.age;
</script>
```
**方式二:通过路由规则中的占位符传递**
```html
<!--在指定路由规则的时候通过/:key/:key的方式来指定占位符
    在指定HASH的时候, 通过/value/value的方式来传递值
    在传递的组件的生命周期方法中通过 this.$route.params的方式来获取-->
 <router-link to="/path/dpc/23" tag="button">路由跳转</router-link>
<script>
//路由规则
 const routes = [
 { path: '/path/:name/:age', component: two }
    ];
let name = this.$route.params.name;
let age = this.$route.params.age;
</script>
```

#### 嵌套路由
+ 嵌套路由也称之为子路由, 就是在被切换的组件中又切换其它子组件


**嵌套路由路由规则：**
```javascript
 const routes = [
        {
            path: '/one',
            component: one,
            //使用children来嵌套路由
            children:[
                {
                    // 注意点: 如果是嵌套路由(子路由), 那么不用写一级路径的地址, 并且也不用写/
                    path: "onesub1",
                    component: onesub1
                },
                {
                    // 注意点: 如果是嵌套路由(子路由), 那么不用写一级路径的地址, 并且也不用写/
                    path: "onesub2",
                    component: onesub2
                }
            ]
        }
    ];
```

#### 命名视图
+ 命名视图和具名插槽很像, 都是让不同的出口显示不同的内容
+ 命名视图就是当路由地址被匹配的时候同时指定多个出口, 并且每个出口中显示的内容不同
+ 通过`<router-view name="name"></router-view>`实现

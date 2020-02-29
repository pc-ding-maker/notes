# Vue-CLI
-----
+ `Vue-CLI`是vue官方提供的脚手架工具
+ 默认已经帮我们搭建好了一套利用`webpack`管理`vue`的项目结构
+ 现在最新版本是4.x,生成的文件结构和2.x生成的目录结构不一样

**使用步骤:**
1. 安装 `npm install -g @vue/cli`
2. 创建 `vue create project-name`
	+ 可以默认创建也可以按需创建

**vue-cli3.0以前：**

	生成的项目结构中我们能够看到build文件夹和config文件夹
	这两个文件夹中存储了webpack相关的配置, 我们可以针对项目需求修改webpack配置

**vue-cli3.0以后:**

	生成的项目结构中已经没有了build文件夹和config文件夹
	使用者不用关心webpack, 只用关心如何使用Vue

**目录结构：**
	
	node_modules文件夹: 存储了依赖的相关的包
	public文件夹: 任何放置在 public 文件夹的静态资源都会被简单的复制，
	              而不经过 webpack。你需要通过绝对路径来引用它们
	              一般用于存储一些永远不会改变的静态资源或者webpack不支持的第三方库
	src文件夹: 代码文件夹
	 |----assets文件夹: 存储项目中自己的一些静态文件(图片/字体等)
	 |----components文件夹: 存储项目中的自定义组件(小组件,公共组件)
	 |----views文件夹: 存储项目中的自定义组件(大组件,页面级组件,路由级别组件)
	 |----router文件夹: 存储VueRouter相关文件
	 |----store文件夹: 存储Vuex相关文件
	 |----App.vue:根组件
	 |----main.js:入口js文件

## 使用Vue-CLI创建和以前的区别：
#### vue实例：
+ 以前直接在`index.html`中引入`vue.js`文件，创建一个`div`，然后编写代码创建`vue实例对象`并绑定
+ 使用`vue-cli`之后，仅需要在`index.html`中创建一个`div`，然后在`App.vue`文件中编写根组件,在`main.js`中创建vue实例，并绑定导入的`App.vue`,更改页面的时候不再需要更改index.html文件

**index.html**
```html
<div id="app"></div>
```
**App.vue**
```html
<template>
  <div id="app">
   
  </div>
</template>

<script>
export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

<style>

</style>
```
**main.js**
```javascript
import Vue from 'vue'
import App from './App.vue'
new Vue({
  render: h => h(App)//代替components:{App:App}
}).$mount('#app') //手动挂载，代替el:'#app'
/*
render函数是用于渲染组件的，和直接用components渲染不同，通过render方法来渲染, 会覆盖Vue实例控制区域而不是插入到控制区域内
render:function(createElement){
    let html = createElement("one");
    return html;
}
*/
```
### vuex使用:
**store/index.js**
```javascript
//引入vue和vuex
import Vue from 'vue'
import Vuex from 'vuex'

//全局使用vuex
Vue.use(Vuex)

//暴露一个vuex实例对象
export default new Vuex.Store({
  state: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```
**main.js**
```javascript
import Vue from 'vue'
import App from './App.vue'
import store from './store'
new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```
### vue-router使用：
**router/index.js**
```javascript
//引入vue、vue-router和组件
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue'
//全局使用vueRouter
Vue.use(VueRouter)
//创建路由规则
const routes = [
  {
    path: '/',
    name: 'home',
    component: Home
  },
  {
    path: '/about',
    name: 'about',
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
];
//创建路由示例对象
const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
});
//将创建的对象暴露出去
export default router
```
**main.js**
+ 见vuex使用中的main.js

### 配置webpack
+ 项目初始化的时候没有`webpack`配置文件，如果想修改部分配置需要手动创建一个`vue.config.js`文件
+ 在创建的文件中的`moudle.exports{//配置信息}`中添加即可

**注意点:**
+ `vue-cli`将常见的配置属性给我们封装好了，直接使用即可，没有封装的需要写到`configureWebpack`属性里

**例：vue.config.js**
```javascript
const path = require('path');
const webpack = require('webpack');
module.exports = {
  // output: {
  //   path: path.resolve(__dirname, 'bundle')
  // }
  //vue-cli封装好的output
  outputDir: 'bundle',
  //在这里面编写没有封装的配置
  configureWebpack: {
    // 就可以在这个对象中编写原生的webpack配置
    plugins: [
      new webpack.BannerPlugin({
        banner: '版权信息'
      })
    ]
  }
}
```

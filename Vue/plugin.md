# Vue插件
-----
**自定义加载框插件：**
+ `plugin/loading/loading.vue`

```html
<!--plugin/loading/loading.vue

-->
<template>
    <div>
        <div class="container" v-show="isShow">
            <div class="loading"></div>
            <p class="title">{{title}}</p>
        </div>
    </div>
</template>

<script>
export default {
  name: 'loading',
  data () {
    return {
      title: '努力加载中...',
      isShow: true
    }
  }
}
</script>

<style scoped>
.container{
  width: 200px;
  height: 200px;
  border-radius: 10px;
  background-color: rgba(33, 33, 33, 0.5);
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
.container .loading{
  width: 80px;
  height: 80px;
  border: 2px solid white;
  border-right-color: #007aff;
  border-radius: 50%;
  position: absolute;
  left: 50%;
  top: 50%;
  animation: loading 2s linear infinite;
}
  .container .title{
    text-align: center;
    margin-top: 140px;
    color: ghostwhite;
  }
@keyframes loading {
  from{
    transform: translate(-50%,-70%) rotate(0deg);
  }
  to{
    transform: translate(-50%,-70%) rotate(360deg);
  }
}
</style>

```

+ `plugin/loading/index.js`

```javascript
//plugin/loading/index.js
import Loading from './loading'
/*
* 如果要将一个组件封装成一个插件，必须提供一个install方法
* */
export default {
  install: function (Vue, options) {
    // 1.根据组件生成一个构造函数
    const LoadingConstructor = Vue.extend(Loading)
    // 2.根据构造函数创建实例对象
    const LoadingInstance = new LoadingConstructor()
    // 3.随便创建一个标签
    const oDiv = document.createElement('div')
    // 4.将创建好的标签添加到界面上
    document.body.appendChild(oDiv)
    // 5.将创建好的实例对象挂载到创建好的元素上
    LoadingInstance.$mount(oDiv)

    Vue.prototype.$showLoading = function (title = '努力加载中...') {
      LoadingInstance.title = title
      LoadingInstance.isShow = true
    }
    Vue.prototype.$hiddenLoading = function () {
      LoadingInstance.isShow = false
    }
  }
}

```
+ 使用：在`main.js`中引入插件的js文件，然后通过use函数使用
```javascript
import Loading from './plugin/loading/index'
Vue.use(Loading)
```
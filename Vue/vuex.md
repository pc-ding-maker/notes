# vuex
-----
+ `Vuex`是一个专为`Vue.js`应用程序开发的状态管理模式

### vuex核心：
#### State
+ 用于保存全局共享的数据
#### Getter
+ 专门用于保存获取全局共享的数据的方法
#### Mutation
+ 用于保存修改全局共享的数据的方法
+ 修改`state`中的数据不能直接修改，要通过`mutation`修改
+ `mutation`中只能编写`同步`代码
#### Action
+ 用于保存触发`mutations`中保存的方法的方法
+ `action`中可编写**异步**代码
#### Module
+ `vuex`模块

##### 例子：(起步)
```html
<div id="app">
        <father></father>
    </div>
    <template id="father">
        <div>
            <p>{{this.$store.state.count}}</p>
            <p>{{this.$store.getters.formatCount}}</p>
            <one></one>
            <two></two>
        </div>
    </template>
    <template id="one">
        <div>
            <button @click="add">增加</button>
            <button @click="sub">减少</button>
        </div>
    </template>
    <template id="two">
        <div>
            <button @click="add">增加</button>
            <button @click="sub">减少</button>
        </div>
    </template>

    <script>
        const store = new Vuex.Store({
           state:{
               count:0
           } ,
            mutations:{
               add(state){
                   state.count++;
               },
               sub(state){
                   state.count--;
               }
            },
            getters:{
               formatCount(state){
                   return '我是getters返回的：'+state.count
               }
            }
        });
        let vue = new Vue({
            el:'#app',
            store:store,
            components:{
                'father':{
                    template:'#father',
                    components:{
                        'one':{
                            template:'#one',
                            methods:{
                                add(){
                                    this.$store.commit('add');
                                },
                                sub(){
                                    this.$store.commit('sub');
                                }
                            }
                        },
                        'two':{
                            template:'#one',
                            methods:{
                                add(){
                                    this.$store.commit('add');
                                },
                                sub(){
                                    this.$store.commit('sub');
                                }
                            }
                        }
                    }
                }
            },

        })
    </script>	
```
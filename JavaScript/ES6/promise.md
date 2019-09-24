# Promise
------
#### 概念：
+ promise是ES6中新增的异步编程解决方案, 在代码中的表现是一个对象


#### 作用:
+ 通过Promise就可以实现 用同步的流程来表示异步的操作
+ 通过Promise就可以 避免回调函数层层嵌套(回调地狱)问题

## 基本使用：
#### 创建：
+ `new Promise(function(resolve, reject){});`
+ promise对象不是异步的, 只要创建promise对象就会立即执行存放的代码
+ 创建时必须有一个函数形参，要不会报错。

#### 状态：
+ promise对象是通过状态的改变来实现的, 只要状态发生改变就会自动触发对应的函数
+ 三种状态：
	+ pending: 默认状态，只要没有告诉promise任务是成功还是失败就是pending状态
    + fulfilled(resolved): 只要调用resolve函数, 状态就会变为fulfilled, 表示操作成功
    + rejected:  只要调用rejected函数, 状态就会变为rejected, 表示操作失败

+ 注意点：状态一旦改变既不可逆, 既从pending变为fulfilled, 那么永远都是fulfilled，既从pending变为rejected, 那么永远都是rejected

+ resolved --> then()
    rejected --> catch()

## then方法：

    0、then方法接收两个参数,
    第一个参数是状态切换为成功时的回调,
    第二个参数是状态切换为失败时的回调
	
    1、在修改promise状态时, 可以传递参数给then方法中的回到函数
	
    2、同一个promise对象可以多次调用then方法,
    当该promise对象的状态时所有then方法都会被执行
	
    3、then方法每次执行完毕后会返回一个新的promise对象(可以链式编程)
	
    4、可以通过上一个promise对象的then方法给下一个promise对象的then方法传递参数
	    注意点:
	    无论是在上一个promise对象成功的回调还是失败的回调传递的参数,
	    都会传递给下一个promise对象成功的回调
	
    5、如果then方法返回的是一个Promise对象, 那么会将返回的Promise对象的
    执行结果中的值传递给下一个then方法
	
	6、Promise.resolve('foo')
	// 等价于
	new Promise(resolve => resolve('foo'))

## catch方法：
	
    0、catch 其实是 then(undefined, () => {}) 的语法糖
	1.catch方法和then一样, 在修改promise状态时, 可以传递参数给catch方法中的回到函数
	
	3、和then一样, 同一个promise对象可以多次调用catch方法,
    当该promise对象的状态时所有catch方法都会被执行
	4、和then一样, catch方法每次执行完毕后会返回一个新的promise对象
	5、和then方法一样, 上一个promise对象也可以给下一个promise成功的传递参数
	6、Promise.reject('foo')
	// 等价于
	new Promise(reject => reject('foo'))
    注意点:
    无论是在上一个promise对象成功的回调还是失败的回调传递的参数,
    都会传递给下一个promise对象成功的回调
##### 注意点：
+ 如果需要分开监听, 也就是通过then监听成功通过catch监听失败
  那么必须使用链式编程, 否则会报错
+ 原因：
    1. 如果promise的状态是失败, 但是没有对应失败的监听就会报错
    2. then方法会返回一个新的promise, 新的promise会继承原有promise的状态
    3. 如果新的promise状态是失败, 但是没有对应失败的监听也会报错


## 静态方法：all
1. all方法接收一个数组,
2. 如果数组中有多个Promise对象,只有都成功才会执行then方法,
并且会按照添加的顺序, 将所有成功的结果重新打包到一个数组中返回给我们
3. 如果数组中不是Promise对象, 那么会直接执行then方法

+ 应用场景：
	+ 批量加载, 要么一起成功, 要么一起失败

## 静态方法：race
1. all方法接收一个数组,
2. 如果数组中有多个Promise对象, 谁先返回状态就听谁的, 后返回的会被抛弃
3. 如果数组中不是Promise对象, 那么会直接执行then方法 

+ 应用场景：
	+ 接口调试, 超时处理


## 手撕promise:
```javascript
    // 定义常量保存对象的状态
    const PENDING = "pending";
    const FULFILLED = "fulfilled";
    const REJECTED = "rejected";

    class MyPromise{
        constructor(handle){
            // 0.初始化默认的状态
            this.status = PENDING;
            // 定义变量保存传入的参数
            this.value = undefined;
            this.reason = undefined;
            // 定义变量保存监听的函数
            // this.onResolvedCallback = null;
            // this.onRejectedCallback = null;
            this.onResolvedCallbacks = [];
            this.onRejectedCallbacks = [];
            // 1.判断是否传入了一个函数, 如果没有传入就抛出一个异常
            if(!this._isFunction(handle)){
                throw new Error("请传入一个函数");
            }
            // 2.给传入的函数传递形参(传递两个函数)
            handle(this._resolve.bind(this), this._reject.bind(this));

        }
        then(onResolved, onRejected){
            return new MyPromise((nextResolve, nextReject) => {
                // 1.判断有没有传入成功的回调
                if(this._isFunction(onResolved)){
                    // 2.判断当前的状态是否是成功状态
                    if(this.status === FULFILLED){
                        try {
                            // 拿到上一个promise成功回调执行的结果
                            let result = onResolved(this.value);
                            // console.log("result", result);
                            // 判断执行的结果是否是一个promise对象
                            if(result instanceof MyPromise){
                                result.then(nextResolve, nextReject);
                            }else{
                                // 将上一个promise成功回调执行的结果传递给下一个promise成功的回调
                                nextResolve(result);
                            }
                        }catch (e) {
                            nextReject(e);
                        }
                    }
                }
                // 1.判断有没有传入失败的回调
                // if(this._isFunction(onRejected)){
                try {
                    // 2.判断当前的状态是否是失败状态
                    if(this.status === REJECTED){
                        let result = onRejected(this.reason);
                        if(result instanceof MyPromise){
                            result.then(nextResolve, nextReject);
                        }else if(result !== undefined){
                            nextResolve(result);
                        }else{
                            nextReject();
                        }
                    }
                }catch (e) {
                    nextReject(e);
                }
                // }
                // 2.判断当前的状态是否是默认状态
                if(this.status === PENDING){
                    if(this._isFunction(onResolved)){
                        // this.onResolvedCallback = onResolved;
                        this.onResolvedCallbacks.push(() => {
                            try {
                                let result = onResolved(this.value);
                                if(result instanceof MyPromise){
                                    result.then(nextResolve, nextReject);
                                }else{
                                    nextResolve(result);
                                }
                            }catch (e) {
                                nextReject(e);
                            }
                        });
                    }
                    // if(this._isFunction(onRejected)){
                    // this.onRejectedCallback = onRejected;
                    this.onRejectedCallbacks.push(() => {
                        try {
                            let result = onRejected(this.reason);
                            if(result instanceof MyPromise){
                                result.then(nextResolve, nextReject);
                            }else if(result !== undefined){
                                nextResolve(result);
                            }else{
                                nextReject();
                            }
                        }catch (e) {
                            nextReject(e);
                        }
                    });
                    // }
                }
            });
        }
        catch(onRejected){
            return this.then(undefined, onRejected);
        }
        _resolve(value){
            // 这里是为了防止重复修改
            if(this.status === PENDING){
                this.status = FULFILLED;
                this.value = value;
                // this.onResolvedCallback(this.value);
                this.onResolvedCallbacks.forEach(fn => fn(this.value));
            }
        }
        _reject(reason){
            if(this.status === PENDING) {
                this.status = REJECTED;
                this.reason = reason;
                // this.onRejectedCallback(this.reason);
                this.onRejectedCallbacks.forEach(fn => fn(this.reason));
            }
        }
        _isFunction(fn){
            return typeof fn === "function";
        }
        static all(list){
            return new MyPromise(function (resolve, reject) {
                let arr = [];
                let count = 0;
                for(let i = 0; i < list.length; i++){
                    let p = list[i];
                    p.then(function (value) {
                        arr.push(value);
                        count++;
                        if(list.length === count){
                            resolve(arr);
                        }
                    }).catch(function (e) {
                        reject(e);
                    });
                }
            });
        }
        static race(list){
            return new MyPromise(function (resolve, reject) {
                for(let p of list){
                    p.then(function (value) {
                        resolve(value);
                    }).catch(function (e) {
                        reject(e);
                    });
                }
            })
        }
    }
```
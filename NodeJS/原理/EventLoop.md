# 事件环（Event Loop）
------ 
> JS是单线程的
  JS中的代码都是串行的, 前面没有执行完毕后面不能执行
+ 执行顺序：

		1、程序运行会从上至下依次执行所有的同步代码
		2、在执行的过程中如果遇到异步代码会将异步代码放到事件循环中
		3、当所有同步代码都执行完毕后, JS会不断检测 事件循环中的异步代码是否满足条件
		4、一旦满足条件就执行满足条件的异步代码
## 宏任务和微任务
>在JS的异步代码中又区分"宏任务(MacroTask)"和"微任务(MicroTask)"
宏任务: 宏/大的意思, 可以理解为比较费时比较慢的任务
微任务: 微/小的意思, 可以理解为相对没那么费时没那么慢的任务
### 常见的宏任务和微任务
+ 宏任务: setTimeout, setInterval, setImmediate（IE独有）...
+ 微任务: Promise, MutationObserver(nodejs中没有) ,process.nextTick（node独有) ...

> MutationObserver是专门用于监听节点的变化

## 浏览器中的事件循环
+ 注意点: 
	+ 所有的宏任务和微任务都会放到自己的执行队列中, 也就是有一个宏任务队列和一个微任务队列
    + 所有放到队列中的任务都采用"先进先出原则", 也就是多个任务同时满足条件, 那么会先执行先放进去的
	+ 每执行完一个宏任务都会立刻检查微任务队列有没有被清空, 如果没有就立刻清空

+ 执行顺序：

		1.从上至下执行所有同步代码
		2.在执行过程中遇到宏任务就放到宏任务队列中,遇到微任务就放到微任务队列中
		3.当所有同步代码执行完毕之后, 就执行微任务队列中满足需求所有回调
		4.当微任务队列所有满足需求回调执行完毕之后, 就执行宏任务队列中满足需求所有回调

##### 例子：
```javascript
   // 1.定义一个宏任务
    setTimeout(function () {
        console.log("setTimeout1");
        // 2.定义一个微任务 p2
        Promise.resolve().then(function () {
            console.log("Promise2");
        });
        // 2.定义一个微任务 p3
        Promise.resolve().then(function () {
            console.log("Promise3");
        });
    }, 0);
    // 2.定义一个微任务 p3
    Promise.resolve().then(function () {
        console.log("Promise1");
        // s2
        setTimeout(function () {
            console.log("setTimeout2");
        });
        // s3
        setTimeout(function () {
            console.log("setTimeout3");
        });
    });
	// 输出结果：
	/*
	Promise1
	setTimeout1
	Promise2
	Promise3
	setTimeout2
	setTimeout3
	*/
```

## nodejs中的事件循环
#### NodeJS事件环和浏览器事件环区别：
	1、任务队列个数不同
	浏览器事件环有2个事件队列(宏任务队列和微任务队列)
	NodeJS事件环有6个事件队列
	2、微任务队列不同
	浏览器事件环中有专门存储微任务的队列
	NodeJS事件环中没有专门存储微任务的队列
	3、微任务执行时机不同
	浏览器事件环中每执行完一个宏任务都会去清空微任务队列
	NodeJS事件环中只有同步代码执行完毕和其它队列之间切换的时候回去清空微任务队列
	4、微任务优先级不同
	浏览器事件环中如果多个微任务同时满足执行条件, 采用先进先出
	NodeJS事件环中如果多个微任务同时满足执行条件, 会按照优先级执行
### NodeJS中的任务队列
	    ┌───────────────────────┐
	┌> │timers          │执行setTimeout() 和 setInterval()中到期的callback
	│  └──────────┬────────────┘
	│  ┌──────────┴────────────┐
	│  │pending callbacks│执行系统操作的回调, 如:tcp, udp通信的错误callback
	│  └──────────┬────────────┘
	│  ┌──────────┴────────────┐
	│  │idle, prepare   │只在内部使用
	│  └──────────┬────────────┘
	│  ┌──────────┴────────────┐
	│  │poll            │执行与I/O相关的回调
	    │                  (除了close回调、定时器回调和setImmediate()之外，几乎所有回调都执行);
	│  └──────────┬────────────┘
	│  ┌──────────┴────────────┐
	│  │check           │执行setImmediate的callback
	│  └──────────┬────────────┘
	│  ┌──────────┴────────────┐
	└─┤close callbacks │执行close事件的callback，例如socket.on("close",func)
    └───────────────────────┘

##### 注意点：
+ 和浏览器不同的是没有宏任务队列和微任务队列的概念
+ 宏任务被放到了不同的队列中, 但是没有队列是存放微任务的队列
+ 微任务会在执行完同步代码和队列切换的时候执行
+ 当队列为空(已经执行完毕或者没有满足条件回到)
或者执行的回调函数数量到达系统设定的阈值时任务队列就会切换
+ 在NodeJS中process.nextTick微任务的优先级高于Promise.resolve微任务
+ 执行完poll, 会查看check队列是否有内容, 有就切换到check
如果check队列没有内容, 就会查看timers是否有内容, 有就切换到timers
如果check队列和timers队列都没有内容, 为了避免资源浪费就会阻塞在poll

### 例子：
```javascript
setTimeout(function () {
    console.log("setTimeout1");
    // p1
    Promise.resolve().then(function () {
        console.log("Promise1");
    });
    // n1
    process.nextTick(function () {
        console.log("process.nextTick1");
    });
});
console.log("同步代码 Start");
setTimeout(function () {
    console.log("setTimeout2");
    // p2
    Promise.resolve().then(function () {
        console.log("Promise2");
    });
    // n2
    process.nextTick(function () {
        console.log("process.nextTick2");
    });
});
console.log("同步代码 End");
/*执行结果：
同步代码 Start
同步代码 End
setTimeout1
setTimeout2
process.nextTick1
process.nextTick2
Promise1
Promise2
*/
```

### 注意点：
如下代码输出的结果是随机的(两者单独出现时)
        在NodeJS中指定的延迟时间是有一定的误差的, 所以导致了输出结果随机的问题
```javascript
setTimeout(function () {
    console.log("setTimeout");
}, 0);
setImmediate(function () {
    console.log("setImmediate");
});
```

以下代码不会随机，会先执行setImmediate后执行setTimeout
```javascript
const path = require("path");
const fs = require("fs");

fs.readFile(path.join(__dirname, "04.js"), function () {
    setTimeout(function () {
        console.log("setTimeout");
    }, 0);
    setImmediate(function () {
        console.log("setImmediate");
    });
});
```
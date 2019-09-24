# ES6
------
+ ES6新增的知识

## 什么Symbol
+ Symbol是ES6中新增的一种数据类型, 被划分到了基本数据类型中

#### ES6后基本类型有六种：
+  字符串、数值、布尔、undefined、null、Symbol

#### 作用：
+ 用来表示一个独一无二的值

#### 使用：
+ `let xxx = Symbol();`
+ 注意：Symbol是基本类型使用时不是 `new Symbol()`

#### 注意点：
+ 通过Symbol生成独一无二值时需要在后面加上(), 但是前面不能加new, 因为它不是引用类型
+ 通过Symbol生成独一无二值时传入的字符串仅仅是一个标记, 方便我们阅读代码, 没有其它任何意义
+ 做类型转换的时候不能转换成数值
+ 不能做任何运算
+ Symbol生成的值作为属性或方法名称时, 一定更要保存下来, 否则后续无法使用
	```javascript
	 let name = Symbol("name");
     let obj = {
          [name]: "pc"
         [Symbol("name")]: "it666"
     }
     // console.log(obj[name]);        //pc
     console.log(obj[Symbol("name")]); //undefined
	```
+ for循环无法遍历出Symbol的属性和方法

## Iterator
+ Iterator又叫做迭代器, 是一种接口
+ 它规定了不同数据类型统一访问的机制, 这里的访问机制主要指数据的遍历
    在ES6中Iterator接口主要供for...of消费
+ 默认情况下以下数据类型都实现的Iterator接口
	+ Array/Map/Set/String/TypedArray/函数的 arguments 对象/NodeList 对象
<br>
	
	1.只要一个数据已经实现了Iterator接口, 那么这个数据就有一个叫做[Symbol.iterator]的属性
    2.[Symbol.iterator]的属性会返回一个函数
    3.[Symbol.iterator]返回的函数执行之后会返回一个对象
    4.[Symbol.iterator]函数返回的对象中又一个名称叫做next的方法
    5.next方法每次执行都会返回一个对象{value: 1, done: false}
		done为false表示没取完，为true表示取完
    6.这个对象中存储了当前取出的数据和是否取完了的标记

#### 应用场景：
+ 解构赋值
+ 扩展运算符

## Generator
+ Generator 函数是 ES6 提供的一种异步编程解决方案
    Generator 函数内部可以封装多个状态, 因此又可以理解为是一个状态机

#### 定义方式：
+ 只需要在普通函数的function后面加上*即可

#### Generator函数和普通函数区别
	调用Generator函数后, 无论函数有没有返回值, 都会返回一个迭代器对象,
    调用Generator函数后, 函数中封装的代码不会立即被执行

<br>

	真正让Generator具有价值的是yield关键字
    在Generator函数内部使用yield关键字定义状态
    并且yield关键字可以让 Generator内部的逻辑能够切割成多个部分。
    通过调用迭代器对象的next方法执行一个部分代码,
    执行哪个部分就会返回哪个部分定义的状态
	yield关键字只能在Generator函数中使用, 不能在普通函数中使用

```javascript
function* gen() {
        console.log("123");
        let res = yield "aaa";

        console.log(res);
        console.log("567");
        yield 1 + 1;

        console.log("789");
        yield true;
    }
    let it = gen();
     console.log(it);
    console.log(it.next());
    console.log(it.next("it666"));
    console.log(it.next());
    console.log(it.next());

	/*打印结果：
	gen {<suspended>}
	 123
	 {value: "aaa", done: false}
	 it666
	 567
	 {value: 2, done: false}
	 789
	 {value: true, done: false}
 	{value: undefined, done: true}
	*/
```

#### 利用 Generator 函数，可以在任意对象上快速部署 Iterator 接口
```javascript
 let obj = {
        name: "dpc",
        age: 22,
        gender: "man"
    }
    function* gen(){
        let keys = Object.keys(obj);
        for(let i = 0; i < keys.length; i++){
            yield obj[keys[i]];
        }
    }
    obj[Symbol.iterator] = gen;
    let it = obj[Symbol.iterator]();
    console.log(it.next());
    console.log(it.next());
    console.log(it.next());
    console.log(it.next());
	 for (let value of obj){
        console.log(value); //输出：dpc、22、man
    }
```

## async
+ async函数是ES8中新增的一个函数, 用于定义一个异步函数
    async函数函数中的代码会自动从上至下的执行代码

##### await操作符：
+ await操作符只能在异步函数 async function 中使用
    await表达式会暂停当前 async function 的执行，等待 Promise 处理完成。
    若 Promise 正常处理(fulfilled)，其回调的resolve函数参数作为 await 表达式的值，然后继续执行 async function。
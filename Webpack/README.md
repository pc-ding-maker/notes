# webpack
------
>webpack是一套基于NodeJS的"模块打包工具",
在webpack刚推出的时候就是一个单纯的JS模块打包工具,可以将多个模块的JS文件合并打包到一个文件中
但是随着时间的推移、众多开发者的追捧和众多开发者的贡献
现在webpack不仅仅能够打包JS模块, 还可以打包CSS/LESS/SCSS/图片等其它文件

+ [文档地址](https://www.webpackjs.com/)

##### webpack简单使用步骤：
```node
//1.初始化node
npm init -y
//2.安装webpack
npm install --save-dev webpack
npm install --save-dev webpack-cli
//3.打包
npx webpack 需要打包的js文件
//打包之后的文件会放到dist目录中, 名称叫做main.js
```

## 配置文件webpack.config.js
	我们在打包JS文件的时候需要输入:  npx webpack index.js
	这句指令的含义是: 利用webpack将index.js和它依赖的模块打包到一个文件中
	其实在webpack指令中除了可以通过命令行的方式告诉webpack需要打包哪个文件以外,

	还可以通过配置文件的方式告诉webpack需要打包哪个文件
	配置文件的名称必须叫做: webpack.config.js, 否则直接输入 npx webpack打包会出错
	如果要使用其它名称, 那么在输入打包命令时候必须通过 --config 指定配置文件名称
	npx webpack --config xxx

##### 常见配置：
```javascript
const path = require("path");

module.exports = {
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    }
};
```

## sourcemap
> webpack打包后的文件会自动添加很多代码, 在开发过程中非常不利于我们去调试
因为如果运行webpack打包后的代码,错误提示的内容也是打包后文件的内容
所以为了降低调试的难度, 提高错误代码的阅读性, 我们就需要知道打包后代码和打包之前代码的映射关系
只要有了这个映射关系我们就能很好的显示错误提示的内容, 存储这个映射关系的文件我们就称之为sourcemap

#### 开启方法：
+ 在webpack.config.js中添加`devtool: "配置模式"`, 

#### 配置项说明：
	eval:
	不会单独生成sourcemap文件, 会将映射关系存储到打包的文件中, 并且通过eval存储
	优势: 性能最好
	缺点: 业务逻辑比较复杂时候提示信息可能不全面不正确
	
	source-map:
	会单独生成sourcemap文件, 通过单独文件来存储映射关系
	优势: 提示信息全面,可以直接定位到错误代码的行和列
	缺点: 打包速度慢
	
	inline:
	不会单独生成sourcemap文件, 会将映射关系存储到打包的文件中, 并且通过base64字符串形式存储
	
	cheap:
	生成的映射信息只能定位到错误行不能定位到错误列
	
	module:
	不仅希望存储我们代码的映射关系, 还希望存储第三方模块映射关系, 以便于第三方模块出错时也能更好的排错

#### 企业中常用配置：
+ 开发模式：
`cheap-module-eval-source-map`
只需要行错误信息, 并且包含第三方模块错误信息, 并且不会生成单独sourcemap文件

+ 线上模式：
`cheap-module-source-map`
只需要行错误信息, 并且包含第三方模块错误信息, 并且会生成单独sourcemap文件

## watch
+ webpack 可以监听打包文件变化，当它们修改后会重新编译打包
那么如何监听打包文件变化呢?  使用 watch

#### watch相关配置watchOptions：
```javascript
/*
	poll: 1000 // 每隔多少时间检查一次变动
	aggregateTimeout:  // 防抖, 和函数防抖一样, 改变过程中不重新打包, 只有改变完成指定时间后才打包
	ignored: 排除一些巨大的文件夹, 不需要监控的文件夹, 例如 node_modules
*/
watch: true,
    watchOptions: {
        aggregateTimeout: 300,
        poll: 1000, 
        ignored: /node_modules/
    },
```

## ES6模块
>在 ES6 前， 实现模块化使用的是 RequireJS 或者 seaJS（分别是基于 AMD 规范的模块化库，  和基于 CMD 规范的模块化库）。
ES6 引入了模块化，其设计思想是在编译时就能确定模块的依赖关系，以及输入和输出的变量。
ES6 的模块化分为导出（export） @与导入（import）两个模块。

###### 特点：
+ ES6 的模块自动开启严格模式，不管你有没有在模块头部加上 use strict
+ 模块中可以导入和导出各种类型的变量，如函数，对象，字符串，数字，布尔值，类等。

+ 每个模块都有自己的上下文，每一个模块内声明的变量都是局部变量，不会污染全局作用域。

+ 每一个模块只加载一次（是单例的）， 若再去加载同目录下同文件，直接从内存中读取。


##### 导入导出：
```javascript
/*
分开导入导出
export xxx;
import {xxx} from "path";

一次性导入导出
export {xxx, yyy, zzz};
import {xxx, yyy, zzz} from "path";

默认导入导出
export default xxx;
import xxx from "path";
*/

```
##### 注意点：
+ 接收导入变量名必须和导出变量名一致
如果想修改接收变量名可以通过 xxx as newName方式
变量名被修改后原有变量名自动失效
+ 一个模块只能使用一次默认导出, 多次无效
默认导出时, 导入的名称可以和导出的名称不一致

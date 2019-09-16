# Node.js
------
+ Node.js 是一个基于"Chrome V8 引擎" 的JavaScript "运行环境"
+ V8引擎是一款专门解释和执行JS代码的虚拟机, 任何程序只要集成了V8引擎都可以执行JS代码
+ NodeJS不是一门编程语言, NodeJS是一个运行环境
+ NodeJs提供了操作"操作系统底层的API"

## Nodejs环境和浏览器环境的区别
1. **内置对象不同**
	- 浏览器环境中提供了window全局对象
	- NodeJS环境中的全局对象不叫window, 叫global

2. **this默认指向不同**
	- 浏览器环境中全局this默认指向window
	- NodeJS环境中全局this默认指向空对象{}

3. **API不同**
	- 浏览器环境中提供了操作节点的DOM相关API和操作浏览器的BOM相关API
	- NodeJS环境中没有HTML节点也没有浏览器, 所以NodeJS环境中没有DOM/BOM

## NodeJs全局对象上的属性(基础)
+ NodeJs中文文档地址：[中文文档地址](http://nodejs.cn/api/)

		__dirname: 当前文件所在文件夹的绝对路径
		__filename: 当前文件的绝对路径
		setInterval / clearInterval : 和浏览器中window对象上的定时器一样
		setTimeout /  clearTimeout : 和浏览器中window对象上的定时器一样
		console :  和浏览器中window对象上的打印函数一样

## Node模块
+ NodeJS采用CommonJS规范实现了模块系统
- 在CommonJS规范中一个文件就是一个模块
- 在CommonJS规范中每个文件中的变量函数都是私有的，对其他文件不可见的
- 在CommonJS规范中每个文件中的变量函数必须通过exports暴露(导出)之后其它文件才可以使用
- 在CommonJS规范中想要使用其它文件暴露的变量函数必须通过require()导入模块才可以使用

### Node模块导出的方式
	在NodeJS中想要导出模块中的变量函数有三种方式
	1.通过exports.xxx = xxx导出
	2.通过module.exports.xxx = xxx导出
	3.通过global.xxx = xxx导出
	注意点:
	无论通过哪种方式导出, 使用时都需要先导入(require)才能使用
	通过global.xxx方式导出不符合CommonJS规范, 不推荐使用

+ exports和module.exports区别
	+ exports只能通过 exports.xxx方式导出数据, 不能直接赋值
	+ module.exports既可以通过module.exports.xxx方式导出数据, 也可以直接赋值(**不要使用直接赋值的方式**)
<br>

+ require注意点
	+ require导入模块时可以不指定导入模块的类型（会依次查找.js .json .node文件）
	+ 导入自定义模块时必须指定路径
	+ 导入"系统模块"和"第三方模块"是不用添加路径
<br>

+ 导入"系统模块"和"第三方模块"是不用添加路径的原因
	+ 如果是"系统模块"直接到环境变量配置的路径中查找
	+ 如果是"第三方模块"会按照module.paths数组中的路径依次查找


## Node包管理工具(NPM)
	NPM包安装方式
    - 全局安装  (一般用于安装全局使用的工具, 存储在全局node_modules中)
    npm install -g 包名   (默认安装最新版本)
    npm uninstall -g 包名
    npm update -g 包名   (更新失败可以直接使用install)

    - 本地安装 (一般用于安装当前项目使用的包, 存储在当前项目node_modules中)
    npm install 包名
    npm uninstall 包名
    npm update 包名

    2.初始化本地包
    npm init   ->  初始化package.json文件
    npm init -y -> 初始化package.json文件

    npm install 包名 --save
    npm install 包名 --save-dev

    包描述文件 package.json, 定义了当前项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。
    npm install 命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境
    注意点:package.json文件中, 不能加入任何注释

    - dependencies：生产环境包的依赖，一个关联数组，由包的名称和版本号组成
    - devDependencies：开发环境包的依赖，一个关联数组，由包的名称和版本号组成

    1.将项目拷贝给其它人, 或者发布的时候, 我们不会将node_modules也给别人, 因为太大
    2.因为有的包可能只在开发阶段需要, 但是在上线阶段不需要, 所以需要分开指定

    npm i               所有的包都会被安装
    npm i --production  只会安装dependencies中的包
    npm i --development  只会安装devDependencies中的包

### NPM切换下载仓库地址的方式
+ npm默认回去国外下载资源, 所以对于国内开发者来说下载会比较慢

+ 方式：
	+ nrm
	+ cnpm

#### nrm:

	npm install -g nrm   安装NRM
	nrm --version 查看是否安装成功
	nrm ls    查看允许切换的资源地址
	nrm use taobao  将下载地址切换到淘宝

#### cnpm
+ npm install cnpm -g –registry=https://registry.npm.taobao.org  安装CNPM
+ cnpm 就是将下载源从国外切换到国内下载, 只不过是将所有的指令从npm变为cnpm而已
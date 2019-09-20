# Http模块
------
+ 通过Nodejs提供的http模块，我们可以快速的构建一个web服务器
+ [文档地址](http://nodejs.cn/api/http.html)

### 通过http模块创建服务器的步骤：
1. 导入HTTP模块
2. 创建服务器实例对象
3. 绑定请求事件
4. 监听指定端口请求

```javascript
//1.导入http模块
let http = require("http");
// 2.创建一个服务器实例对象
let server = http.createServer();
// 3.注册请求监听
server.on("request", function (req, res) {
    // end方法的作用: 结束本次请求并且返回数据
    // res.end("dpc");
    // writeHead方法的作用: 告诉浏览器返回的数据是什么类型的, 返回的数据需要用什么字符集来解析
    res.writeHead(200, {
        "Content-Type": "text/plain; charset=utf-8"
    });
    res.end("dpc");
});
// 4.指定监听的端口
server.listen(3000);
```
## 常用方法：
### 服务器相关：
1. 创建：creatsServer
	+ 返回创建的服务器实例
	`let server = http.creatsServer();`
2. 添加监听请求
	```javascript
	server.on("request",function(request,response){

	})
	//第一个参数（request）：事件名称
	//第二个参数（function）：回调函数
	/*
	回调函数中的参数说明：
	第一个参数（request）：请求相关
	第二个参数（response）：相应相关
	*/
	```
3. 监听指定端口：listen
	`server.listen(number)`
	+ number:指定的端口号

### 请求相关：（创建的服务器回调函数中的第一个参数request）
1. 判断请求路径
	`request.url.startsWith(URL)==="URL"`
2. 判断请求方式
	+ `request.methond`
3. 处理GET请求参数
	`let obj = url.parse(request.url, true);`
	+ 第二个参数不带true时获得的是字符串(?后面的内容)
	+ 加上true后可以直接通过`obj.query.key`来获取
4. 处理POST请求参数
	```javascript
	 // 1.定义变量保存传递过来的参数
    let params = "";
    // 注意点: 在NODEJS中 ,POST请求的参数我们不能一次性拿到, 必须分批获取
    req.on("data", function (chunk) {
        // 每次只能拿到一部分数据
        params += chunk;
    });
    req.on("end", function () {
        // 这里才能拿到完整的数据,不通过queryString.parse方法来处理得当的是字符串，key=value&key=value
        let obj = queryString.parse(params);
       //通过queryString.parse方法来处理后可以直接通过obj.key来获取
    });
	```
### 相应相关（创建的服务器回调函数中的第二个参数response）
1. 设置请求头:`response.writeHead`,
	例如：
	`response.writeHead(200, {
        "Content-Type": "text/plain; charset=utf-8"
    });`
2. 返回数据：
	1. `response.end(数据)`
		+ 通过end方法来返回数据, 那么只会返回一次
		+ end方法会结束当前请求
	2. `response.write(数据)`
		+ 通过write方法来返回数据, 那么可以返回多次
		+ write方法不具备结束本次请求的功能, 所以还需要手动的调用end方法来结束本次请求


## 返回静态资源
+ 返回对应的静态资源时需要设置请求头

### 封装的返回静态资源的模块
```javascript

let fs = require("fs");
//引入各个资源对应的响应头(百度即可)
let mime = require("./mime.json");

//rootPath:代表静态资源的根路径
function readFile(req, res, rootPath) {
    let filePath = path.join(rootPath, req.url);
    /*
    注意点:
    1.加载其它的资源不能写utf8
    2.如果服务器在响应数据的时候没有指定响应头, 那么在有的浏览器上, 响应的数据有可能无法显示
    * */
    let extName = path.extname(filePath);
    let type = mime[extName];
    if(type.startsWith("text")){
        type += "; charset=utf-8;";
    }
    res.writeHead(200, {
        "Content-Type": type
    });
    fs.readFile(filePath, function (err, content) {
        if(err){
            res.end("Server Error");
        }
        res.end(content);
    });
}

exports.StaticServer = readFile;
```
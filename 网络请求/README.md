# 关于网络请求的相关知识
------
## AJAX
+ AJAX 是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。

## json
> JavaScript 对象表示法（JavaScript Object Notation）。
> JSON 是存储和交换文本信息的语法。类似 XML。
> JSON 比 XML 更小、更快，更易解析。

### json字符串与js对象的区别

区别 | JSON | 	Javascript 
:-: | :-: | :-: 
含义 | 仅仅是一种数据格式 | 表示类的实例 
传输 | 可以跨平台数据传输，速度快| 不能传输 
表现 | 1.简直对方式，键必须加双引号<br>2.值不能是方法函数，不能是undefined/NaN| 1.键值对方式，键不加引号<br>2.值可以是函数、对象、字符串、数字、boolean 等 
相互转换 | Json转换Js对象1.JSON.parse(JsonStr);(不兼容IE7)<br>2.eval("("+jsonStr+")");(兼容所有浏览器，需要注意在值的两边需要加括号)| js对象转换JsonJSON.stringify(jsObj); 

##### 注意：json仅仅是一种格式，json对象其实是按照既定规则书写的js对象，json对象就是js对象
<br>

## 数据存储
+ cookie
+ LocalStorage
+ SessionStorage

## 同源策略
+ 同源策略（Same origin policy）是一种约定，它是浏览器最核心也最基本的安全功能
+ 同源是指: 协议，域名，端口都相同,就是同源, 否则就是跨域

#### 解决跨域问题：
+ jsonp
+ document.domain+iframe
+ location.hash + iframe
+ window.name + iframe
+ window.postMessage
+ flash等第三方插件
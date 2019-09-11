# AJAX
------
### 什么是AJAX？
> AJAX = 异步 JavaScript 和 XML。
> AJAX 是一种用于创建快速动态网页的技术。
> 通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

### 创建AJAX步骤：
1. 创建一个XMLHttpRequest对象
	+ `variable=new XMLHttpRequest();//现代浏览器`
	+ `variable=new ActiveXObject("Microsoft.XMLHTTP");//老版本浏览器`

2. 设置请求方式和请求地址:`open(method,url,async)`
	+ method:
		+ GET：GET请求
		+ POST：POST请求

	+ url：请求的地址
	+ async
		+ true：异步（ajax为异步，所以都设置为true）
		+ false：同步

3. 发送请求：`xmlhttp.send();`

4. 响应：
	1. **responseText**：获得字符串形式的响应数据。
	2. **responseXML**：获得 XML 形式的响应数据。

#### AJAX - onreadystatechange 事件
> 当请求被发送到服务器时，我们需要执行一些基于响应的任务。
> 每当 readyState 改变时，就会触发 onreadystatechange 事件。
>readyState 属性存有 XMLHttpRequest 的状态信息。

+ readyState
	+ 0: 请求未初始化
	+ 1: 服务器连接已建立
	+ 2: 请求已接收
	+ 3: 请求处理中
	+ 4: 请求已完成，且响应已就绪


<br>

### AJAX-GET请求
```javascript
 oBtn.onclick = function (ev1) {
     // 1.创建一个异步对象
     var xmlhttp=new XMLHttpRequest();
     // 2.设置请求方式和请求地址
     /*
     method：请求的类型；GET 或 POST
     url：文件在服务器上的位置
     async：true（异步）或 false（同步）
     */
     xmlhttp.open("GET", "URL", true);
     // 3.发送请求
     xmlhttp.send();
     // 4.监听状态的变化
     xmlhttp.onreadystatechange = function (ev2) {
         /*
         0: 请求未初始化
         1: 服务器连接已建立
         2: 请求已接收
         3: 请求处理中
         4: 请求已完成，且响应已就绪
         */
         if(xmlhttp.readyState === 4){
             // 判断是否请求成功
             if(xmlhttp.status >= 200 && xmlhttp.status < 300 ||
                xmlhttp.status === 304){
                 // 5.处理返回的结果
                 console.log("接收到服务器返回的数据");
             }else{
                 console.log("没有接收到服务器返回的数据");
             }

         }
     }
 }

```
### AJAX-POST请求
```javascript
	 var xhr = new XMLHttpRequest();
           xhr.open("POST","URL",true);
           // 注意点: 以下代码必须放到open和send之间
           xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
           xhr.send("userName=zs&userPwd=321");
           xhr.onreadystatechange = function (ev2) {
               if(xhr.readyState === 4){
                   if(xhr.status >= 200 && xhr.status < 300 ||
                   xhr.status === 304){
                       // alert("请求成功");
                       alert(xhr.responseText);
                   }else{
                       alert("请求失败");
                   }
               }
           }
```

### jQuery中使用AJAX

	`jQuery.ajax([settings])`
[参考链接](https://www.w3school.com.cn/jquery/ajax_ajax.asp)

### 封装AJAX
```javascript
function obj2str(data) {
    data = data || {}; // 如果没有传参, 为了添加随机因子,必须自己创建一个对象
    data.t = new Date().getTime();
    var res = [];
    for(var key in data){
        // 在URL中是不可以出现中文的, 如果出现了中文需要转码
        // 可以调用encodeURIComponent方法
        // URL中只可以出现字母/数字/下划线/ASCII码
        res.push(encodeURIComponent(key)+"="+encodeURIComponent(data[key])); // [userName=lnj, userPwd=123456];
    }
    return res.join("&"); // userName=lnj&userPwd=123456
}
function ajax(option) {
    // 0.将对象转换为字符串
    var str = obj2str(option.data); // key=value&key=value;
    // 1.创建一个异步对象
    var xmlhttp, timer;
    if (window.XMLHttpRequest)
    {// code for IE7+, Firefox, Chrome, Opera, Safari
        xmlhttp=new XMLHttpRequest();
    }
    else
    {// code for IE6, IE5
        xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
    }
    // 2.设置请求方式和请求地址
    /*
    method：请求的类型；GET 或 POST
    url：文件在服务器上的位置
    async：true（异步）或 false（同步）
    */
    if(option.type.toLowerCase() === "get"){
        xmlhttp.open(option.type, option.url+"?"+str, true);
        // 3.发送请求
        xmlhttp.send();
    }else{
        xmlhttp.open(option.type, option.url,true);
        // 注意点: 以下代码必须放到open和send之间
        xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
        xmlhttp.send(str);
    }

    // 4.监听状态的变化
    xmlhttp.onreadystatechange = function (ev2) {
        /*
        0: 请求未初始化
        1: 服务器连接已建立
        2: 请求已接收
        3: 请求处理中
        4: 请求已完成，且响应已就绪
        */
        if(xmlhttp.readyState === 4){
            clearInterval(timer);
            // 判断是否请求成功
            if(xmlhttp.status >= 200 && xmlhttp.status < 300 ||
                xmlhttp.status === 304){
                // 5.处理返回的结果
                // console.log("接收到服务器返回的数据");
                option.success(xmlhttp);
            }else{
                // console.log("没有接收到服务器返回的数据");
                option.error(xmlhttp);
            }
        }
    }
    // 判断外界是否传入了超时时间
    if(option.timeout){
        timer = setInterval(function () {
            console.log("中断请求");
            xmlhttp.abort();
            clearInterval(timer);
        }, option.timeout);
    }
}
```
### 调用自己封装的AJAX
```javascript
    ajax({
        type:"get",
        url:"URL",
        data:{"name":''},
        timeout: 3000,
        success: function (xhr) {
            alert(xhr.responseText);
        },
        error: function (xhr) {
            alert(xhr.status);
        }
    });

```
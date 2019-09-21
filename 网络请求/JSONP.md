# JSONSP
------
+ JSONP让网页从别的地址（跨域的地址）那获取资料，即跨域读取数据
+ JSONP只支持GET请求，不支持POST请求

### 原理：
	1、在同一界面中可以定义多个script标签
    2、同一个界面中多个script标签中的数据可以相互访问
    3、可以通过script的src属性导入其它资源
    4、通过src属性导入其它资源的本质就是将资源拷贝到script标签中
    5、script的src属性不仅能导入本地资源, 还能导入远程资源
    6、由于script的src属性没有同源限制, 所以可以通过script的src属性来请求跨域数据

### 实现流程：
1. 设定一个`script`标签。（通过js创建的script标签是异步的）
2. 服务端返回数据(可能直接返回数据，一般都返回一个函数调用)
3. 客户端接收数据（服务端返回的就是数据的时候直接接收，如果返回的是一个函数调用，则需要先定义一个函数，函数名与服务端返回的函数调用名称一样）

### jQuery中jsonp的使用：
```javascript
  $.ajax({
        url: "URL",
        data:{
           
        },
        dataType: "jsonp", // 告诉jQuery需要请求跨域的数据
        jsonp: "cb",  // 告诉jQuery服务器在获取回调函数名称的时候需要用什么key来获取
        jsonpCallback: "name", // 告诉jQuery服务器在获取回调函数名称的时候回调函数的名称是什么
        success: function (msg) {
            console.log(msg);
        }
    });
```

### JSONP封装：
```javascript
function obj2str(obj) {
    // 生成随机因子
    obj.t = (Math.random() + "").replace(".", "");
    let arr = [];
    for(let key in obj){
        arr.push(key + "=" + encodeURI(obj[key]));
    }
    let str = arr.join("&");
    // console.log(str);
    return str;
}
function myJSONP(options) {
    options = options || {};
    // 1.生成URL地址
    let url = options.url;
    if(options.jsonp){
        url += "?" + options.jsonp + "=";
    }else{
        url += "?callback=";
    }

    let callbackName = ("jQuery" + Math.random()).replace(".", "");
    if(options.jsonpCallback){
        callbackName = options.jsonpCallback;
        url += options.jsonpCallback;
    }else{
        // console.log(callbackName);
        url += callbackName;
    }
    if(options.data){
        let str = obj2str(options.data);
        url += "&" + str;
    }
    // console.log(url);

    // 2.获取跨域的数据
    let oScript = document.createElement("script");
    oScript.src = url;
    document.body.appendChild(oScript);

    // 3.定义回调函数
    window[callbackName] = function (data) {
        // 删除已经获取了数据的script标签
        document.body.removeChild(oScript);
        // 将获取到的数据返回给外界
        options.success(data);
    }
}
```
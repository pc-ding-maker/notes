# Html
------
>HTML 不是一门编程语言，而是一种用于定义内容结构的标记语言。HTML 由一系列的元素（elements）组成，这些元素可以用来包围不同部分的内容，使其以某种方式呈现或者工作。 


## HTML元素详解
![html标签解析](https://i.imgur.com/rK53MVq.png)

##### 开始标签:
+ 包含元素的名称，被大于号、小于号所包围。表示元素从这里开始或者开始起作用

##### 结束标签:
+ 与开始标签相似，只是其在元素名之前包含了一个斜杠。这表示着元素的结尾 

##### 内容:
+ 元素的内容

##### 元素:
+ 开始标签、结束标签与内容相结合，便是一个完整的元素。

## HTML文档详解:
```html
<!--最简单的HTML文档-->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>测试页面</title>
  </head>
  <body>
  </body>
</html>
```
##### `<!DOCTYPE html>`：
+ `!DOCTYPE`用于声名`html`文档类型
+ `!DOCTYPE`不是html标签
+ `!DOCTYPE`不区分大小写

##### `<html></html>`
+ 这个元素包含了整个页面的内容，有时也被称作根元素

#### `<head></head>`
+ 这个元素放置的内容不是展现给用户的，而是包含例如面向搜索引擎的搜索关键字、页面描述、CSS 样式表和字符编码声明等

#### `<meta charset="utf-8">`
+ 指定了当前文档使用 UTF-8 字符编码 

#### `<title></title>`
+ 用于设置网页标题

##### `<body></body>`
+ 这个元素包含期望让用户在访问页面时看到的内容

## HTML特殊字符：
原义字符 | 转义字符 
-|-
`<` | `&lt;` 
`>` | `&gt;`
`"` | `&quot;`
`'` | `&apos;` 
`&` | `&amp;`
`空格` | `&nbsp;`


## `<head></head>`中常用标签：
#### `<title></title>`
+ 用于设置网页标题

#### `<meta>`
+ 用于设置网页的元数据

1. `<meta charset="utf-8">`
	+ 设置网页的字符集

2. `<meta name="author" content="姓名">`
	+ 用于设置网页的作者

3. `<meta name="description" content="描述">`
	+ 用于设置网页的描述

4. `<meta name="keywords" content="关键字"/>`
	+ 用于设置网页关键字


#### `<link> `
+ 定义文档与外部资源的关系

1. `<link rel="shortcut icon" href="favicon.ico" type="image/x-icon">`
	+ 用于设置网页标题前面的图标

2. `<link rel="stylesheet" href="my-css-file.css">`
	+ 用于链接外部css样式

	
	
	
	
	
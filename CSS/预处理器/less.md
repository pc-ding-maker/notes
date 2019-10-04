# less
------ 
+ Less 是一门 CSS 预处理语言，为CSS赋予了动态语言的特征。
+ 它扩展了 CSS 语言，增加了变量、Mixin(混合)、嵌套、函数和运算等特性，使 CSS 更易维护和扩展

#### 后缀名：
+ 以.less结尾

## 注释：
+ 单行注释不会被编译(不会出现在编译后的文件中)
    多行注释会被编译  (会出现在编译后的文件中)

+ 单行注释：`//`
+ 多行注释：`/*  */`

## 变量：
+ 定义变量：`@变量名称: 值;`
+ 使用变量：`@变量名称;`
+ 特点：
	+ 后定义覆盖先定义
   + 可以把变量赋值给其它变量
   + 区分全局变量和局部变量(访问采用就近原则)
   + 可以先使用后定义

## 变量插值:
+ 在less中如果属性的取值可以直接使用变量, 但是如果是属性名称或者选择器名称并不能直接使用变量
如果属性名称或者选择器名称想使用变量中保存的值, 那么必须使用变量插值的格式
+ 格式：`@{变量名称}`

## 运算：
+ less中的运算和CSS3中新增的calc函数一样, 都支持+ - * / 运算

## 混合(Mix in)：
+ 什么是混合：
	+ 将需要重复使用的代码封装到一个类中, 在需要使用的地方调用封装好的类即可
在预处理的时候less会自动将用到的封装好的类中的代码拷贝过来
本质就是ctrl+c  --> ctrl + v
+ 注意点：
	+ 如果混合名称的后面没有(), 那么在预处理的时候, 会保留混合的代码
	+ 如果混合名称的后面加上(), 那么在预处理的时候, 不会保留混合的代码

+ 使用：
	```less
	
	.center(){
	  position: absolute;
	  left: 50%;
	  top: 50%;
	  transform: translate(-50%, -50%);
	}
	.father{
	  width: 300px;
	  height: 300px;
	  background: red;
	  .center();
	  .son{
	    width: 200px;
	    height: 200px;
	    background: blue;
	    .center();
	  }
	}
	```
#### 带参数的混合：
```less
  .whc(@w:100px, @h:100px, @c:pink){
  width: @w;
  height: @h;
  background: @c;
}
.box1{
  // 这里是给混合的指定形参传递数据
  .whc(@c:red);
}
.box2{
  .whc(300px, 300px, blue);
}
```

## 可变参数：
+ less中的@arguments和js中的arguments一样, 可以拿到传递进来的所有形参
+ less中的...表示可以接收0个或多个参数
如果形参列表中使用了..., 那么...必须写在形参列表最后
###### 使用：
```less
.animate(@name, @time, ...){
  transition: @arguments;
}
div{
  width: 200px;
  height: 200px;
  background: red;
  .animate(all, 4s, linear, 0s);
}

```
## 匹配模式：
+  就是通过混合的第一个字符串形参,来确定具体要执行哪一个同名混合
+  @_: 表示通用的匹配模式
+  无论同名的哪一个混合被匹配了, 都会先执行通用匹配模式中的代码

##### 使用：
```less
.triangle(@_, @width, @color){
  width: 0;
  height: 0;
  border-style: solid solid solid solid;
}
.triangle(Down, @width, @color){
  //width: 0;
  //height: 0;
  border-width: @width;
  //border-style: solid solid solid solid;
  border-color: @color transparent transparent transparent;
}

.triangle(Down, 80px, green);
```

## 导入其他less文件：
+ `@import "文件地址";`

## 内置函数：
+ 由于less的底层就是用JavaScript实现的,
    所以JavaScript中常用的一些函数在less中都支持
## 层级结构：
+ 如果在某一个选择器的{}中直接写上了其它的选择器, 会自动转换成后代选择器
+ 如果不想转换成后代选择器，例如伪元素选择器：前面加上&连接符

##### 使用：
```less
  .son{
    &:hover{
      background: skyblue;
    }
  }
//转换后：
.son:hover{
  background: skyblue;
}
```
## 继承：
+ 继承与混合的区别：
	+ 使用时的语法格式不同
	+ 转换之后的结果不同(混合是直接拷贝, 继承是并集选择器)

##### 使用：
```less
.center{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
//继承
.father:extend(.center){
  width: 300px;
  height: 300px;
  background: red;
	//混合
  //.center;
}
```
## 条件判断：
+ less中可以通过when给混合添加执行限定条件, 只有条件满足(为真)才会执行混合中的代码
when表达式中可以使用比较运算符(> < >= <= =)、逻辑运算符、或检查函数来进行条件判断
+ (),()相当于JS中的||
+ ()and()相当于JS中的&&

##### 使用：
```less
// (),()相当于JS中的||
.size(@width,@height) when (@width = 100px),(@height = 100px){
  width: @width;
  height: @height;
}

// ()and()相当于JS中的&&
.size(@width,@height) when (@width = 100px)and(@height = 100px){
  width: @width;
  height: @height;
}
```

##### 常见检查函数
	常见的类型检查函数：
	Iscolor：是否为颜色值
	Isnumber：是否为数值
	Isstring：是否为字符串
	Iskeyword：是否为关键字
	Isurl：是否为URL字符串

	以下是常见的单位检查函数
	Ispixel：是否为像素单位：
	ispercentage：是否为百分比
	isem：是否为em单位。
	isunit：是否为单位
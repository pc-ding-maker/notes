# Sass
-----
+ SASS是一套利用Ruby实现的, 最早最成熟的CSS预处理器, 诞生于2007年.
+ 它扩展了 CSS 语言，增加了变量、Mixin(混合)、嵌套、函数和运算等特性，使 CSS 更易维护和扩展

#### 后缀名：
+ SASS以.sass或者.scss结尾
+ 两种后缀名的区别：
	+ .sass结尾以缩进替代{}表示层级结构, 语句后面不用编写分号
	+ .scss以{}表示层级结构, 语句后面需要写分号
	+ 企业开发中推荐使用.scss结尾

## 注释：
+ 单行注释不会被编译(不会出现在编译后的文件中)
    多行注释会被编译  (会出现在编译后的文件中)

+ 单行注释：`//`
+ 多行注释：`/*  */`

## 变量：
+ 定义变量：`$变量名称: 值;`
+ 特点：
	+ 后定义覆盖先定义
   + 可以把变量赋值给其它变量
   + 区分全局变量和局部变量(访问采用就近原则)
   + 不可以先使用后定义

## 变量插值：
+ 如果是属性的取值可以直接使用变量,
    但是如果是属性名称或者选择器名称并不能直接使用变量, 必须使用变量插值的格式
+ 插值格式: `#{$变量名称}`

## 运算：
+ SASS中的运算支持 + - * /  运算
+ 运算都需要加上()

## 混合：
+ SASS中的混合和LESS中也一样, 只是定义格式和调用的格式不同
+ LESS中混合定义: .混合名称{} 或者 .混合名称(){}
    LESS中混合调用: .混合名称; 或者 .混合名称();
+ SASS中混合定义: @mixin 混合名称{}; 或者 @mixin 混合名称(){};
    SASS中混合调用: @include 混合名称; 或者 @include 混合名称();

## 可变参数：
+ SASS不是使用JS实现的, 所以不能直接在混合中使用arguments
    必须通过 $args...的格式来定义可变参数, 然后通过$args来使用
+ 和LESS一样可变参数必须写在形参列表的最后

##### 使用：
```scss
@mixin animate($name, $time, $args...){
  transition: $name $time $args;
}
div{
  width: 200px;
  height: 200px;
  background: red;
  //transition: all 4s linear 0s;
  @include animate(all, 4s, linear, 0s);
}
```
## 导入其他sass文件：
`@import '文件地址'`

## 内置函数：
+ 和LESS一样, SASS中也提供了很多内置函数方便我们使用

## 层级结构：
+  和LESS一样支持嵌套, 默认情况下嵌套的结构会转换成后代选择器
    和LESS一样也支持通过&符号不转换成后代选择器

## 继承：
+ SASS中的继承和LESS中的继承一样, 都是通过并集选择器来实现的, 只不过格式不一样而已

##### 使用：
```scss
.center{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.father{
	//继承
  @extend .center;
	//混合
  //@include center;
  width: 300px;
  height: 300px;
  background: red;
}
```

## 条件判断：
	SASS中的条件判断
    和LESS一样SASS中也支持条件判断, 只不过SASS中的条件判断支持得更为彻底
    SASS中支持
    @if(条件语句){}
    @else if(条件语句){}
    ... ...
    @else(条件语句){}
    SASS中当条件不为false或者null时就会执行{}中的代码
    和LESS一样SASS中的条件语句支持通过> >= < <= ==进行判断

##### 使用：
```scss
@mixin triangle($dir, $width, $color){
  width: 0;
  height: 0;
  border-width: $width;
  border-style: solid solid solid solid;
  @if($dir == Up){
    border-color: transparent transparent $color transparent;
  }@else if($dir == Down){
    border-color: $color transparent transparent transparent;
  }@else if($dir == Left){
    border-color: transparent $color transparent transparent;
  }@else if($dir == Right){
    border-color: transparent transparent transparent $color;
  }
}
```

## 循环：
+ SASS中支持两种循环, 分别是for循环和while循环
+  for循环
    @for $i from 起始整数 through 结束整数{}
    @for $i from 起始整数 to 结束整数{}
    两者的区别 through包头包尾, to包头不包尾

+ 3.while循环
    @while(条件语句){}

##### 使用：
```scss
 //@for $i from 5 through 8{
    //@for $i from 5 to 8{
    $i:5;
    @while($i <= 8){
      &:nth-child(#{$i}){
        background: deepskyblue;
      }
      $i:$i+1;
    }
```
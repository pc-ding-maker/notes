# AST
------
+ AST是Abstract Syntax Tree的缩写既"抽象语法树"
+ 它以树状的形式表现编程语言的语法结构
+ [github](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md#toc-lexical-analysis)
+ [在线生成](https://astexplorer.net/)

##### 作用：
>1、在大多数情况下我们是用不到AST的, 但是如果需要做一些大型框架或者第三方工具的时候,
那么AST将是不二选择. 例如家喻户晓的babel、webpack、JD Taro、uni-app等第三方工具和框架都把AST用的淋漓尽致

>2、在利用webpack打包js代码的时候, webpack会在原有代码的基础新增一些代码
在利用babel打包js代码的时候, 可以将高级代码转换为低级代码
那么webpack、babel是如何新增代码, 如何修改的呢, 答案就是通过AST来新增和修改的
所以如果不甘于做一只菜鸟, 想进一步深入学习各种工具、框架的底层实现原理, 那么AST都是必修之课

#### 转换过程:
1. 词法分析
	+ 从左到右一个字符一个字符地读入源程序，从中识别出一个个“单词”"符号"等
2. 语法分析
	+ 根据当前编程语言的语法,将单词序列组合成各类语法短语
3. 生成抽象语法树

#### 示例：
```javascript
/*
	sum:js代码
	tree:转换后的抽象语法树
*/
 	let sum = 10 + 66;
    let tree = {
	//type:类型，program表示是一段程序
        "type": "Program",
	//start:表示起始位置
        "start": 0,
	//end：表示结束位置
        "end": 18,
        "body": [
            {
		//表示是变量申明
                "type": "VariableDeclaration",
		//表示是let类型的声名
                "kind": "let",
                "start": 0,
                "end": 18,
                "declarations": [
                    {
                        "type": "VariableDeclarator",
                        "start": 4,
                        "end": 17,
                        "id": {
                            "type": "Identifier",
                            "start": 4,
                            "end": 7,
			//name:变量名为sum
                            "name": "sum"
                        },
			//init：初始值表达式
                        "init": {
                            "type": "BinaryExpression",
                            "start": 10,
                            "end": 17,
			//左操作数
                            "left": {
                                "type": "Literal",
                                "start": 10,
                                "end": 12,
				//数值为10
                                "value": 10,
                                "raw": "10"
                            },
				//表示是一个‘+’运算符
                            "operator": "+",
                            "right": {
                                "type": "Literal",
                                "start": 15,
                                "end": 17,
				//数值为66
                                "value": 66,
                                "raw": "66"
                            }
                        }
                    }
                ],
            }
        ]
    };
```

## 转换AST
+ 利用@babel/parser解析器进行转换
+ `npm install --save @babel/parser`
+ [地址](https://babeljs.io/docs/en/babel-parser)

```javascript
//1. 引入解析器
import * as parser from "@babel/parser";

//2. 创建一串代码
const code = `let sum = 10 + 66;`;
//3. 通过parse方法进行转换(接收字符串)
const ast = parser.parse(code);
console.log(ast);
```

## 修改AST
+ 要想修改AST中的内容必须先遍历拿到需要修改的节点才能修改
+ 可以通过babel的traverse模块来遍历
+ `npm install --save @babel/traverse`
+ [地址](https://babeljs.io/docs/en/babel-traverse#docsNav)

```javascript
/*
	最后结果:
	原始:let sum = 10 + 66;
	结果:let add = 10 + 66;
*/
//引入修改AST用到的插件
import traverse from "@babel/traverse";
//引入抽象语法树转换为js代码的插件
import generate from '@babel/generator';

// 创建一个抽象语法树
const ast = parser.parse(`let sum = 10 + 66;`);

// 遍历抽象语法树
traverse(ast, {
    enter(path) {
        // console.log(path.node.type);
        if(path.node.type === "Identifier"){
            // 3.修改满足条件的语法树节点
            path.node.name = "add";
            path.stop();
        }
    }
});

// 4.将抽象语法树转换成代码
const res = generate(ast);
```

##	创建AST
+ 可以通过babel的types模块来创建语法树节点然后push到body中
+ `npm install --save-dev @babel/types`
+ [地址](https://babeljs.io/docs/en/babel-types)

```javascript
/*
需求: 要求手动创建 let sum = 10 + 66;的节点, 添加到body
推荐从内向外创建
*/
//引入types模块
import * as t from '@babel/types';

// 1.创建二元运算符左右参与运算的 字面量节点
let left = t.numericLiteral(10);
let right = t.numericLiteral(66);
// 2.创建二元运算符节点
let init = t.binaryExpression("+", left, right);
// 3.创建表达式标识符节点
let id = t.identifier("sum");
// 4.创建内部变量表达式节点
let variable = t.variableDeclarator(id, init);
// 5.创建外部变量表达式节点
let declaration = t.variableDeclaration("let", [variable]);
// 6.将组合好的节点添加到body中
ast.program.body.push(declaration);

//将创建好的抽象语法树转换为js代码
let resultCode = generate(ast);
console.log(resultCode.code);
```

## 删除
+ 利用babel的traverse模块来遍历
+ 利用nodepath的remove方法来删除

##### .NodePath常用属性和方法
	── 属性
	  node   当前节点
	  parent  父节点
	  parentPath 父path
	  scope   作用域
	  context  上下文
	  ...
	── 方法
	   get   当前节点
	   findParent  向父节点搜寻节点
	   getSibling 获取兄弟节点
	   replaceWith  用AST节点替换该节点
	   replaceWithMultiple 用多个AST节点替换该节点
	   insertBefore  在节点前插入节点
	   insertAfter 在节点后插入节点
	   remove   删除节点
	   ...

```javascript
/*
	需求：删除let sum = 10 + 66；
*/
import * as parser from "@babel/parser";
import traverse from "@babel/traverse";

let code = `
    console.log("lnj");
    let sum = 10 + 66;
    let minus = 66 - 33;
    console.log("it666");
`;
//创建抽象语法树
let ast = parser.parse(code);

traverse(ast, {
    /*
    enter方法什么时候调用: 只要遍历到一个节点就会调用, 并且还会传递一个NodePath对象给我们
                           传递的这个对象中就保存了当前遍历到的节点
    * */
    /*
    enter(path){
        console.log(path.node.name);
    }
     */
    /*
    traverse方法中除了有enter方法以外, 还有其它的方法
    只要是抽象语法树中拥有的节点类型都有对应的方法
    那么如果写的不是enter, 而是抽象语法树节点对应类型的方法
    那么只有遍历到对应的类型才会调用
    * */
    Identifier(path){
        // console.log(path.node.name);
        if(path.node.name === "sum"){
            // console.log(path.node);
            path.parentPath.remove();
        }
    }
});

```
# babel
------
>Babel 是一个工具链，主要用于将 ECMAScript 2015+ 版本的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。

+ [文档地址](https://babeljs.io/)

##### 使用：
1. 安装
	`npm install --save-dev babel-loader @babel/core  @babel/preset-env`

2. 配置
	```javascript
	{
    test: /\.js$/,
    exclude: /node_modules/,  // 不做处理的目录
    loader: "babel-loader",
    options: {
        presets: ["@babel/preset-env"],
    },
	}
	```

##### 优化：
	在实际企业开发中默认情况下babel会将所有高于ES5版本的代码都转换为ES5代码
	但是有时候可能我们需要兼容的浏览器已经实现了更高版本的代码, 那么这个时候我们就不需要转换
	因为如果浏览器本身已经实现了, 我们再去转换就会增加代码的体积,就会影响到网页的性能
	所以我们通过配置presets的方式来告诉webpack我们需要兼容哪些浏览器
	然后babel就会根据我们的配置自动调整转换方案, 如果需要兼容的浏览器已经实现了, 就不转换了

```javascript
  {
                test: /\.js$/,
                exclude: /node_modules/, // 告诉webpack不处理哪一个文件夹
                loader: "babel-loader",
                options: {
                    "presets": [["@babel/preset-env", {
                        targets: { // 告诉babel你需要兼容哪些浏览器
                            "chrome": "58",
                        },
                    }]]
                }
            },
```

### 解决无对应关系的语法
>什么叫有对应关系, 什么叫做没有对应关系?
有对应关系就是指ES5中有对应的概念,  例如: 箭头函数对应普通函数, let对应var, 这个就叫做有对应关系
没有对应关系就是指E5中根本就没有对应的语法,  例如Promise, includes等方法是ES678新增的

1. 安装
	`npm install --save @babel/polyfill`
	`npm install --save-dev @babel/plugin-transform-runtime`
	`npm install --save @babel/runtime`
2. 配置
	```javascript
	   {
                test: /\.js$/,
                exclude: /node_modules/, // 告诉webpack不处理哪一个文件夹
                loader: "babel-loader",
                options: {
                    "presets": [["@babel/preset-env", {
                        targets: {
                           "chrome": "58",
                        },
               
                    }]],
                    "plugins": [
                        [
                            "@babel/plugin-transform-runtime",
                            {
                                "absoluteRuntime": false,
                                "corejs": 2,
                                "helpers": true,
                                "regenerator": true,
                                "useESModules": false
                            }
                        ]
                    ]
                }
            },
	```
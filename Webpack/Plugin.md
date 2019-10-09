# plugin
------ 
>plugin 用于扩展webpack的功能
当然loader也是变相的扩展了webpack ，但是它只专注于转化文件这一个领域。
而plugin的功能更加的丰富，而不仅局限于资源的加载。

## HtmlWebpackPlugin
+ HtmlWebpackPlugin会在打包结束之后自动创建一个index.html, 并将打包好的JS自动引入到这个文件中
+ [文档地址](https://www.webpackjs.com/plugins/html-webpack-plugin/)
### 使用步骤：
1. 安装HtmlWebpackPlugin
	+ `npm install --save-dev html-webpack-plugin`
2. 配置HtmlWebpackPlugin
	+ `const HtmlWebpackPlugin = require('html-webpack-plugin');`
`plugins: [new HtmlWebpackPlugin()]`

### 高级使用:
	默认情况下HtmlWebpackPlugin生成的html文件是一个空的文件,
	如果想指定生成文件中的内容可以通过配置模板的方式来实现
	 plugins: [new HtmlWebpackPlugin({
	        template: "index.html"
	    })]
	
	默认情况下生成html文件并没有压缩,
	如果想让html文件压缩可以设置
	new HtmlWebpackPlugin({
	    template: "index.html",
	    minify: {
	        collapseWhitespace: true
	    }
	})]

## clean-webpack-plugin
+ webpack-clean-plugin会在打包之前将我们指定的文件夹清空
应用场景每次打包前将dist目录清空, 然后再存放新打包的内容, 避免新老混淆问题

### 使用步骤：
1. 安装clean-webpack-plugin
	+ `npm install --save-dev clean-webpack-plugin`
2. 配置clean-webpack-plugin
	+ `const { CleanWebpackPlugin } = require('clean-webpack-plugin');`
`plugins: [new CleanWebpackPlugin()]`

## copy-webpack-plugin
+ 在打包项目的时候除了JS/CSS/图片/字体图标等需要打包以外, 可能还有一些相关的文档也需要打包
文档内容是固定不变的, 我们只需要将对应的文件拷贝到打包目录中即可
那么这个时候我们就可以使用copy-plugin来实现文件的拷贝

### 使用步骤：
1. 安装copy-webpack-plugin
	+ `npm install --save-dev copy-webpack-plugin`
2. 配置copy-webpack-plugin
	+ `const CopyWebpackPlugin = require('copy-webpack-plugin');`
`plugins: [new CopyWebpackPlugin([{from:"doc", to:"./doc"}])];`

## mini-css-extract-plugin
+ mini-css-extract-plugin是一个专门用于将打包的CSS内容提取到单独文件的插件
> 通过style-loader打包的CSS都是直接插入到head中的

### 使用步骤：
1. 安装mini-css-extract-plugin
	+ `npm install --save-dev mini-css-extract-plugin`
2. 配置mini-css-extract-plugin
	+ `const MiniCssExtractPlugin = require('mini-css-extract-plugin');`
`new MiniCssExtractPlugin({filename: './css/[name].css',})`

3. 替换style-loader
	+ `loader: MiniCssExtractPlugin.loader,`

## 压缩Css代码
### 步骤：
	1、安装JS代码压缩插件
	npm install --save-dev terser-webpack-plugin
	2、安装CSS代码压缩插件
	npm install --save-dev optimize-css-assets-webpack-plugin
	3、导入插件
	const TerserJSPlugin = require('terser-webpack-plugin');
	const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
	4、配置webpack优化项
	optimization: {
	    minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
	}
	注意: 由于配置了webpack的optimization.minimizer项目会覆盖默认的JS压缩选项,
	所以JS代码也需要通过插件自己压缩

### webpack.config.js配置文件


## 插件和压缩css代码的webpack.config.js配置文件
```javascript
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');

module.exports = {
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "production", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
    ]
};
```
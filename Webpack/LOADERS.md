# LOADERS
------ 
> webapck的本质是一个模块打包工具, 所以webpack默认只能处理JS文件,不能处理其他文件,
因为其他文件中没有模块的概念, 但是在企业开发中我们除了需要对JS进行打包以外,
还有可能需要对图片/CSS等进行打包, 所以为了能够让webpack能够对其它的文件类型进行打包,
在打包之前就必须将其它类型文件转换为webpack能够识别处理的模块,
用于将其它类型文件转换为webpack能够识别处理模块的工具我们就称之为loader



##### 如何使用loader？
+ webpack中的loader都是用NodeJS编写的, 但是在企业开发中我们完全没有必要自己编写,
因为已经有众多大神帮我们编写好了企业中常用的loader, 我们只需要安装、配置、使用即可

+ 使用步骤：
	1. 通过npm安装对应的loader
	2. 按照loader作者的要求在webpack进行相关配置
	3. 使用配置好的loader


##### loader注意点：
+ 单一原则： 一个loader只做一件事情
+ 顺序行：多个loader会按照从右至左, 从下至上的顺序执行

## 常用loader：
###### webpack.config.js 配置：
+ module: 告诉webpack如何处理webpack不能够识别的文件
```javascript
module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024 * 100,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                // use: [ 'style-loader', 'css-loader' ]
                use:[
                    {
                        loader: "style-loader"
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
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    }
```
## 常用部分loader注意点：
##### file-loader
1. 默认情况下fileloader生成的图片名就是文件内容的 MD5 哈希值
如何想打包后不修改图片的名称, 那么可以新增配置  name: "[name].[ext]"
其它命名规则详见: placeholders

2. 默认情况下fileloader会将生成的图片放到dist根目录下面
如果想打包之后放到指定目录下面, 那么可以新增配置 outputPath: "images/"

3. 如果需要将图片托管到其它服务器, 那么只需在打包之前配置 publicPath: "托管服务器地址"即可

##### url-loader
+ url-loader 功能类似于 file-loader，
但是在文件大小（单位 byte）低于指定的限制时，可以返回一个 DataURL
1. 对于比较小的图片, 我们将图片转换成base64的字符串之后, 可以提升网页的性能(因为减少了请求的次数)
                            对于比较大的图片, 哪怕我们将图片转换成了base64的字符串之后, 也不会提升网页的性能, 还有可能降低网页的性能

##### css-loader
+ 默认情况下通过import "./xxx.css"导入的样式是全局样式
也就是只要被导入, 在其它文件中也可以使用
如果想要导入的CSS文件只在导入的文件中有效, 那么就需要开启CSS模块化

+ 方式：
	1. 然后在导入的地方通过 `import xxx from "./xxx.css"`导入
	2. 然后在使用的地方通过 `xxx.className`方式使用即可


## PostCSS
>PostCSS和sass/less不同, 它不是CSS预处理器
PostCSS是一款使用插件去转换CSS的工具，
PostCSS有许多非常好用的插件

### 使用PostCSS自动补全浏览器前缀:
##### 使用步骤：
1. 安装postcss-loader	
	+ `npm i -D postcss-loader`
2. 安装需要的插件
	+ `npm i -D autoprefixer`
3. 配置postcss-loader
	+ 在css-loader or less-loader or sass-loader之前添加postcss-loader
	```javascript
	use:[
                    {
                        loader: "style-loader",
                    },
                    {
                        loader: "css-loader"
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
	```
4. 创建postcss-loader配置文件
	+ `postcss.config.js`
5. 在配置文件中配置autoprefixer

##### postcss.config配置文件
```javascript
module.exports = {
    plugins: {
        "autoprefixer": {
            "overrideBrowserslist": [
                // "ie >= 8", // 兼容IE7以上浏览器
                // "Firefox >= 3.5", // 兼容火狐版本号大于3.5浏览器
                // "chrome  >= 35", // 兼容谷歌版本号大于35浏览器,
                // "opera >= 11.5" // 兼容欧朋版本号大于11.5浏览器,
            ]
        }
    }
};
```

### 使用PostCSS自动将px转换成rem
##### 使用步骤
1. 安装postcss-loader	
	+ `npm i -D postcss-loader`
2. 安装需要的插件
	+ `npm install postcss-pxtorem -D`
3. 在**postcss.config**配置文件中配置postcss-pxtorem
	```javascript
	"postcss-pxtorem": {
            rootValue: 100, // 根元素字体大小
            // propList: ['*'] // 可以从px更改到rem的属性
            propList: ["height"] //只改变height
        }
	```

## image-loader
+ html-withimg-loader
+ [文档地址](https://www.npmjs.com/package/html-withimg-loader)
		通过file-loader或者url-loader已经可以将JS或者CSS中用到的图片打包到指定目录中了
		但是file-loader或者url-loader并不能将HTML中用到的图片打包到指定目录中
		所以此时我们就需要再借助一个名称叫做"html-withimg-loader"的加载器来实现HTML中图片的打包

###### 使用：
1. 安装
		`npm install html-withimg-loader --save`
2. 配置module
		`{
    test: /\.(htm|html)$/i,
    loader: 'html-withimg-loader'
}`

### 图片压缩
+ 每次在打包图片之前,我们可以通过配置webpack对打包的图片进行压缩, 以较少打包之后的体积

+ [image-webpack-loader](https://www.npmjs.com/package/image-webpack-loader)

##### 使用：
1. 安装：
	`npm install image-webpack-loader --save-dev`
2. 配置：在打包图片的规则下面加上
	```javascript
	 {
                        loader: 'image-webpack-loader',
                        options: {
                            mozjpeg: {
                                progressive: true,
                                quality: 65
                            },
                            // optipng.enabled: false will disable optipng
                            optipng: {
                                enabled: false,
                            },
                            pngquant: {
                                quality: [0.65, 0.90],
                                speed: 4
                            },
                            gifsicle: {
                                interlaced: false,
                            },
                            // the webp option will enable WEBP
                            webp: {
                                quality: 75
                            }
                        }
                    },
	```

### 图片合并
+ 过去为了减少网页请求的次数, 我们需要"UI设计师"给我们提供精灵图,
并且在使用时还需要手动的去设置每张图片的位置
但是有了webpack之后我们只需要让"UI设计师"给我们提供切割好的图片
我们可以自己合成精灵图, 并且还不用手动去设置图片的位置

+ [postcss-sprites](https://www.npmjs.com/package/postcss-sprites)
+ [webpack-spritesmith](https://www.npmjs.com/package/webpack-spritesmith)

### 图片打包后路径问题：
+ webpack打包之后给我们的都是相对路径
但是正是因为是相对路径, 所以会导致在html中使用的图片能够正常运行, 在css中的图片不能正常运行

##### 原因：
	在html中, 会去html文件所在路径下找images,正好能找到所以不报错
    但是在css中,  会去css文件所在路径下找images, 找不到所以报错

##### 解决：
	在开发阶段将publicPath设置为dev-server服务器地址
	在上线阶段将publicPath设置为线上服务器地址	
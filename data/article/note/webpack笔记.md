# WebPack 5 学习笔记

基于NodeJS的项目打包工具

## 基础使用

#### 介绍

静态资源打包工具

将开发时的框架、ES6模块化语法、Less/Sass等css预处理器，打包成浏览器识别的JS和CSS等

打包工具有webpack、Vite等

本身只能打包js

#### 开始

1、初始化`package.json`

```text
npm init -y
```

2、下载依赖

```text
npm i webpack webpack-cli -D
```

3、启用

- 开发模式

```text
npx webpack ./src/main.js --mode=development
```

- 生产模式

```text
npx webpack ./src/main.js --mode=production
```

#### 配置文件

##### 五大核心

入口、输出、加载器、插件、模式

配置文件：根目录/webpack.config.json

```js
const path=require("path");

module.exports={
    //1.入口
    entry:"./src/main.js",//相对路径
    //2.输出
    output:{
        //输出路径，要写绝对路径
        path:path.resolve(__dirname,"dist"),
        filename:"main.js",//输出文件名
    },
    //3.加载器
    module:{
        rules:[
            //加载器配置
        ]
    },
    //4.插件
    plugins:[
        //插件配置
    ],
    //5.模式
    mode:"development"
}

//npx webpack .src/main.js --mode=production 在没有写本配置文件的情况下执行
//npx webpack 在当前路径下，寻找webpack.config.js配置文件执行打包const path=require("path");
```

#### 开发模式

开发模式：开发代码时使用的模式。

这个模式下我们主要做两件事：

1. 编译代码，使浏览器能识别运行
   1. 开发时我们有样式资源、字体图标、图片资源、html 资源等，webpack 默认都不能处理这些资源，所以我们要加载配置来编译这些资源
2. 代码质量检查，树立代码规范
   1. 提前检查代码的一些隐患，让代码运行时能更加健壮。
   2. 提前检查代码规范和格式，统一团队编码风格，让代码更优雅美观。

#### 处理样式资源

##### CSS

安装两个处理器：

```bash
npm i css-loader style-loader -D
```

webpack.config.js

```js
module:{
    rules:[
        //加载器配置
        {
            // 用来匹配 .css 结尾的文件
            test: /\.css$/,
            // use 数组里面 Loader 执行顺序是从右到左
            use: ["style-loader", "css-loader"],
        },
    ]
},
```

main.js

```js
//CSS导入，只引入不需要用变量接收
//除了js以外的资源都需要安装加载器
//需要安装css加载器    npm i css-loader style-loader -D
import "./css/index.css";
```

##### Less

在项目中安装加载器

```bash
npm i less-loader -D
```

配置文件

```js
 {
     test: /\.less$/,
     use: ["style-loader", "css-loader", "less-loader"],
 },
```

引入less

```javascript
import "./less/index.less";
```

##### Sass

```bash
npm i sass-loader sass -D
```

```json
{
    test: /\.s[ac]ss$/,
    use: ["style-loader", "css-loader", "sass-loader"],
},
```

##### Styl

```bash
npm i stylus-loader -D
```

```json
{
    test: /\.styl$/,
    use: ["style-loader", "css-loader", "stylus-loader"],
},
```



#### 处理图片资源

过去在 Webpack4 时，我们处理图片资源通过 `file-loader` 和 `url-loader` 进行处理

现在 Webpack5 已经将两个 Loader 功能内置到 Webpack 里了，我们只需要简单配置即可处理图片资源

```json
{
    test: /\.(png|jpe?g|gif|webp)$/,
    type: "asset",
    parser: {
        dataUrlCondition: {
            maxSize: 10 * 1024 // 小于10kb的图片会被base64处理
        }
    }
},
```

图片转移到输出目录下

#### 修改资源输出路径

```js
const path = require("path");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中
  },
  module: {
    rules: [
      //..................此处删去无关代码
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024, // 小于10kb的图片会被base64处理
          },
        },
        generator: {
          // 将图片文件输出到 static/imgs 目录中
          // 将图片文件命名 [hash:8][ext][query]
          // [hash:8]: hash值取8位
          // [ext]: 使用之前的文件扩展名
          // [query]: 添加之前的query参数
          filename: "static/imgs/[hash:8][ext][query]",
        },
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

#### 自动清理上次输出

```json
 output: {
    path: path.resolve(__dirname, "dist"),
    filename: "static/js/main.js",
    clean: true, // 自动将上次打包目录资源清空
  },
```

#### 处理字体/图标资源

```json
 {
     test: /\.(ttf|woff2?)$/,
     type: "asset/resource",
     generator: {
         filename: "static/media/[hash:8][ext][query]",
     },
 },
```

#### 其他资源

和字体一起打包在media了

```javascript
{
        test: /\.(ttf|woff2?|map4|map3|avi)$/,
        type: "asset/resource",
        generator: {
          filename: "static/media/[hash:8][ext][query]",
        },
      },
```





## 优化配置



## 进阶原理
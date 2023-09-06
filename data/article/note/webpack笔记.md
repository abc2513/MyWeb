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

###### 五大核心

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

###### CSS

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

###### Less

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

###### Sass

```bash
npm i sass-loader sass -D
```

```json
{
    test: /\.s[ac]ss$/,
    use: ["style-loader", "css-loader", "sass-loader"],
},
```

###### Styl

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

#### 处理JS资源

###### Eslint代码检查

可组装的JS、JSX检查

1、安装Eslint包

```bash
npm i eslint-webpack-plugin eslint -D
```

2、Eslint 配置文件

- `.eslintrc.js`文件。注意文件名有一点
  - 解析选项
  - 具体规则
  - 继承规则

```js
module.exports = {
  
  parserOptions: {// 解析选项
      ecmaVersion: 6, // ES 语法版本
      sourceType: "module", // ES 模块化
      ecmaFeatures: { // ES 其他特性
          jsx: true // 如果是 React 项目，就需要开启 jsx 语法
      }
  },
  rules: {// 具体检查规则
      //"off" 或 0 - 关闭规则
      //"warn" 或 1 - 开启规则，使用警告级别的错误：warn (不会导致程序退出)
      //"error" 或 2 - 开启规则，使用错误级别的错误：error (当被触发的时候，程序会退出)
      semi: "error", // 禁止使用分号
      "no-var": 2, // 不能使用 var 定义变量
      'array-callback-return': 'warn', // 强制数组方法的回调函数中有 return 语句，否则警告
      'default-case': [
          'warn', // 要求 switch 语句中有 default 分支，否则警告
          { commentPattern: '^no default$' } // 允许在最后注释 no default, 就不会有警告了
      ],
      eqeqeq: [
          'warn', // 强制使用 === 和 !==，否则警告
          'smart' // https://eslint.bootcss.com/docs/rules/eqeqeq#smart 除了少数情况下不会有警告
      ],
  },
  extends: [],// 继承其他规则
  // 其他规则详见：https://eslint.bootcss.com/docs/user-guide/configuring
};
```

3、webpack配置文件

```js
//引入
const ESLintWebpackPlugin = require("eslint-webpack-plugin");

//启用插件
...
{
    plugins: [
        new ESLintWebpackPlugin({
            // 指定检查文件的根目录
            context: path.resolve(__dirname, "src"),
        }),
    ],
}
```

4、VSCode EsLint插件

- 安装Eslint插件

- 配置`eslintignore`

  ```
  dist
  #忽略dist目录下的文件
  ```

  

###### Babel兼容

将ES6+语法转发为向后兼容的JavaScript语法

1、安装包

2、Babel配置文件

`babel.config.js`

```js
module.exports = {
  // 预设
  presets: [],
};
```

- `@babel/preset-env`: 一个智能预设，允许您使用最新的 JavaScript。
- `@babel/preset-react`：一个用来编译 React jsx 语法的预设
- `@babel/preset-typescript`：一个用来编译 TypeScript 语法的预设

3、webpack配置文件

`webpack.config.js`

添加规则

```json
{
    test: /\.js$/,
    exclude: /node_modules/, // 排除node_modules代码不编译
    loader: "babel-loader",
},
```

#### 处理HTML资源

自动将生成的资源文件引入到HTML中

1、下载包

```bash
npm i html-webpack-plugin -D
```

2、webpack配置文件

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");

...
{
    plugins: [
        new HtmlWebpackPlugin({
            template: path.resolve(__dirname, "public/index.html"),
            // 以 public/index.html 为模板创建文件
            // 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源
        }),
    ]
}
```



3、修改html文件

去掉手动引入的js



#### 生产模式

package.json

```json
  "scripts": {
    "start": "npm run dev",
    "dev":"webpack serve --config ./webpack.dev.js",
    "build": "webpack --config ./webpack.prod.js"
  },
```

- webpack.dev.js
  - 输出文件改成undefined
- webpack.prod.js
  - 去掉服务器



#### CSS处理

###### 提取

原来的css是通过styleloader放进js，通过js创建一个 style 标签来生成样式，对于网站来说，会出现闪屏现象，用户体验不好。我们应该是单独的 Css 文件，通过 link 标签加载性能才好

1、下载包

```bash
npm i mini-css-extract-plugin -D
```

2、webpack配置

引入插件、style-loader改成插件加载器、配置插件

```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

...//加载器
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/,
        // use 数组里面 Loader 执行顺序是从右到左
        use: [MiniCssExtractPlugin.loader, "css-loader"],
      },
      {
        test: /\.less$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "less-loader"],
      },
      {
        test: /\.s[ac]ss$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
      },
      {
        test: /\.styl$/,
        use: [MiniCssExtractPlugin.loader, "css-loader", "stylus-loader"],
      },
    
...//插件
    // 提取css成单独文件
    new MiniCssExtractPlugin({
      // 定义输出文件名和目录
      filename: "static/css/main.css",
    }),
```



###### 兼容

1、下载

```bash
npm i postcss-loader postcss postcss-preset-env -D
```

2、webpack.prod.js

- 在cssloader前一个加载
- 写成对象格式

```json

{
    loader: "postcss-loader",
    options: {
        postcssOptions: {
            plugins: [
                "postcss-preset-env", // 能解决大多数样式兼容性问题
            ],
        },
    },
},
```



3、控制兼容性

我们可以在 `package.json` 文件中添加 `browserslist` 来控制样式的兼容性做到什么程度。

```json
{
  // 其他省略
  "browserslist": ["ie >= 8"]
}
```

想要知道更多的 `browserslist` 配置，查看[browserslist 文档](https://github.com/browserslist/browserslist)

以上为了测试兼容性所以设置兼容浏览器 ie8 以上。

实际开发中我们一般不考虑旧版本浏览器了，所以我们可以这样设置：

```json
{
  // 其他省略
  "browserslist": ["last 2 version", "> 1%", "not dead"]
}
```

###### 压缩

只需要下载、引入插件、加载插件

```text
npm i css-minimizer-webpack-plugin -D
```



```js
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
//加载插件
    // css压缩
    new CssMinimizerPlugin(),
```

#### HTML/JS压缩

生产模式下默认会压缩

#### 总结

本章节我们学会了 Webpack 基本使用，掌握了以下功能：

1. 两种开发模式

   - 开发模式：代码能编译自动化运行

   - 生产模式：代码编译优化输出

2. Webpack 基本功能

   - 开发模式：可以编译 ES Module 语法

   - 生产模式：可以编译 ES Module 语法，压缩 js 代码

3. Webpack 配置文件

   - 5 个核心概念 
     - entry入口
     - output输出
     - loader加载器
     - plugins插件
     - mode模式

   - devServer 配置

4. Webpack 脚本指令用法

   - `webpack` 直接打包输出

   - `webpack serve` 启动开发服务器，内存编译打包没有输出

## 高级优化

#### 提升开发体验

###### 源码映射

SourceMap（源代码映射）是一个用来生成源代码与构建后代码一一映射的文件的方案。它会生成一个 xxx.map 文件，里面包含源代码和构建后代码每一行、每一列的映射关系。当构建后代码出错了，会通过 xxx.map 文件，从构建后代码出错位置找到映射后源代码出错位置，从而让浏览器提示源代码文件出错位置，帮助我们更快的找到错误根源。

在webpack配置文件：

```json
//开发模式：打包编译速度快，只有行映射、没有列映射
devtool: "cheap-module-source-map",
//生产环境：打包编译速度慢，包含行/列映射
devtool: "source-map",
```



#### 提升打包速度

###### dev热模块替换

HotModuleReplacementHMR/热模块替换）：在程序运行中，替换、添加或删除模块，而无需重新加载整个页面。(开发时我们修改了其中一个模块代码，Webpack 默认会将所有模块全部重新打包编译，速度很慢。)

CSS：styleloader已经有这个功能，只需要在webpack服务器配置

```js
hot: true, // 开启HMR功能（只能用于开发环境，生产环境不需要了）
```

JS：默认不支持，需要加代码，很麻烦一般不用

```js
// 判断是否支持HMR功能
if (module.hot) {
    module.hot.accept("./js/count.js",()->{console.log("count.js更新了！")});
	module.hot.accept(...);
}
```

Vue/React中的热替换：使用自带的loader



###### 匹配单加载器

打包时每个文件都会经过所有 loader 处理，虽然因为 `test` 正则原因实际没有处理上，但是都要过一遍，比较慢

使用OneOf只匹配一个

```json
module:{
    rules:[
        {
            //规则
        }
    ]
}
//改成
module:{
    rules:[
        oneOf:[
            {
                //规则
            }
		]
	]
}
```



###### 排除第三方库

开发时我们需要使用第三方的库或插件，所有文件都下载到 node_modules 中了。而这些文件是不需要编译可以直接使用的。所以我们在对 js 文件处理时，要排除 node_modules 下面的文件。

用include或者exclude（用的库一般是js

```json
//js加载器中排除
{
    test: /\.js$/,
    // exclude: /node_modules/, // 排除node_modules代码不编译
    include: path.resolve(__dirname, "../src"), // 也可以用包含
    loader: "babel-loader",
},

```

```js
//ESLint中排除
new ESLintWebpackPlugin({
    // 指定检查文件的根目录
    context: path.resolve(__dirname, "../src"),
    exclude: "node_modules", // 默认值
}),
```



###### 使用缓存

每次打包时 js 文件都要经过 Eslint 检查 和 Babel 编译，速度比较慢。我们可以缓存之前的 Eslint 检查 和 Babel 编译结果，这样第二次打包时速度就会更快了。

修改js加载器规则

```json
{
    test: /\.js$/,
    // exclude: /node_modules/, // 排除node_modules代码不编译
    include: path.resolve(__dirname, "../src"), // 也可以用包含
    loader: "babel-loader",
    options: {
        cacheDirectory: true, // 开启babel编译缓存
        cacheCompression: false, // 关闭缓存文件的压缩
    },
},
```

修改ESLint配置

```json
new ESLintWebpackPlugin({
    // 指定检查文件的根目录
    context: path.resolve(__dirname, "../src"),
    exclude: "node_modules", // 默认值
    cache: true, // 开启缓存
    cacheLocation: path.resolve(// 缓存目录
        __dirname,
        "../node_modules/.cache/.eslintcache"
    ),
}),
```



###### 多进程

当项目越来越庞大时，打包速度越来越慢，甚至于需要一个下午才能打包出来代码。这个速度是比较慢的。我们想要继续提升打包速度，其实就是要提升 js 的打包速度，因为其他文件都比较少。而对 js 文件处理主要就是 eslint 、babel、Terser 三个工具，所以我们要提升它们的运行速度。我们可以开启多进程同时处理 js 文件，这样速度就比之前的单进程打包更快了。

**需要注意：请仅在特别耗时的操作中使用，因为每个进程启动就有大约为 600ms 左右开销。**

1、下载包

```bash
npm i thread-loader -D
```

2、引入包

```js
const os = require("os");
const TerserPlugin = require("terser-webpack-plugin");
```



3、获取cpu核数、开启多线程

```js
// cpu核数
const threads = os.cpus().length
```

js加载器

```json
{
    test: /\.js$/,
    // exclude: /node_modules/, // 排除node_modules代码不编译
    include: path.resolve(__dirname, "../src"), // 也可以用包含
    use: [
        {
            loader: "thread-loader", // 开启多进程
            options: {
                workers: threads, // 数量
            },
        },
        {
            loader: "babel-loader",
            options: {
                cacheDirectory: true, // 开启babel编译缓存
            },
        },
    ],
},
```

代码检查

```js
new ESLintWebpackPlugin({
    // 指定检查文件的根目录
    context: path.resolve(__dirname, "../src"),
    exclude: "node_modules", // 默认值
    cache: true, // 开启缓存
    // 缓存目录
    cacheLocation: path.resolve(
        __dirname,
        "../node_modules/.cache/.eslintcache"
    ),
    threads, // 开启多进程
}),
```



css压缩

==删掉原来plugins的`new CssMinimizerPlugin()`,==

```json
optimization: {//压缩的东西一般放这里
    minimize: true,
    minimizer: [
        // css压缩也可以写到optimization.minimizer里面，效果一样的
        new CssMinimizerPlugin(),
        // 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了
        new TerserPlugin({
            parallel: threads // 开启多进程
        })
    ],
},
```





#### 减少代码体积

###### 摇树

- 开发时我们定义了一些工具函数库，或者引用第三方工具函数库或组件库。如果没有特殊处理的话我们打包时会引入整个库，但是实际上可能我们可能只用上极小部分的功能。这样将整个库都打包进来，体积就太大了
- `Tree Shaking` 是一个术语，通常用于描述移除 JavaScript 中的没有使用上的代码。==依赖 `ES Module`。==

- Webpack 已经==默认开启==了这个功能，无需其他配置



###### Babel独立辅助代码

- Babel 为编译的每个文件都插入了辅助代码，使代码体积过大！Babel 对一些公共方法使用了非常小的辅助代码，比如 `_extend`。默认情况下会被添加到每一个需要它的文件中。你可以将这些辅助代码作为一个独立模块，来避免重复引入。
- `@babel/plugin-transform-runtime`: 禁用了 Babel 自动对每个文件的 runtime 注入，而是引入 `@babel/plugin-transform-runtime` 并且使所有辅助代码从这里引用。

1. 下载包

   ```bash
   npm i @babel/plugin-transform-runtime -D
   ```

2. 配置

   ```json
   {
       test: /\.js$/,
       // exclude: /node_modules/, // 排除node_modules代码不编译
       include: path.resolve(__dirname, "../src"), // 也可以用包含
       use: [
           {
               loader: "babel-loader",
               options: {
                   cacheDirectory: true, // 开启babel编译缓存
                   cacheCompression: false, // 缓存文件不要压缩
                   plugins: ["@babel/plugin-transform-runtime"], // 减少代码体积
               },
           },
       ],
   },
   ```

   



###### 压缩图片

- 开发如果项目中引用了较多图片，那么图片体积会比较大，将来请求速度比较慢。我们可以对图片进行压缩，减少图片体积。**如果项目中图片都是在线链接，那么就不需要了。本地项目静态图片才需要进行压缩。**
- `image-minimizer-webpack-plugin`: 用来压缩图片的插件

1. 下载包

    ```bash
    npm i image-minimizer-webpack-plugin imagemin -D
    #无损压缩
    npm install imagemin-gifsicle imagemin-jpegtran imagemin-optipng imagemi
    #有损压缩
    npm install imagemin-gifsicle imagemin-mozjpeg imagemin-pngquant imagemin-svgo -D
    ```

2. 配置无损压缩

    ```js
    const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");
    //  optimization: { minimizer: [
    
          // 压缩图片
          new ImageMinimizerPlugin({
            minimizer: {
              implementation: ImageMinimizerPlugin.imageminGenerate,
              options: {
                plugins: [
                  ["gifsicle", { interlaced: true }],
                  ["jpegtran", { progressive: true }],
                  ["optipng", { optimizationLevel: 5 }],
                  [
                    "svgo",
                    {
                      plugins: [
                        "preset-default",
                        "prefixIds",
                        {
                          name: "sortAttrs",
                          params: {
                            xmlnsOrder: "alphabetical",
                          },
                        },
                      ],
                    },
                  ],
                ],
              },
            },
          }),
    ```

3. 我们需要安装两个文件到 node_modules 中才能解决, 文件可以从课件中找到：

    - jpegtran.exe

    需要复制到 `node_modules\jpegtran-bin\vendor` 下面

    > [jpegtran 官网地址](http://jpegclub.org/jpegtran/)

    - optipng.exe

    需要复制到 `node_modules\optipng-bin\vendor` 下面

    > [OptiPNG 官网地址](http://optipng.sourceforge.net/)

#### 优化运行性能

##### JS代码分割

- 分割文件、按需加载

###### 多入口多输出 

- 复用代码会被重复打包

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const path=require('path');
module.exports={
    entry:{
        app:'./src/app.js',
        main:'./src/main.js',
    },
    output:{
        path:path.resolve(__dirname,"dist"),
        filename:"[name].js",
        //[name]为文件名
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:path.resolve(__dirname,"public/index.html")
        })
    ]
};
```

###### 提取重复代码

如果多入口文件中都引用了同一份代码，我们不希望这份代码被打包到两个文件中，导致代码重复，体积更大。我们需要提取多入口的重复代码，只打包生成一个 js 文件，其他文件引用它就好。

```json
optimization: {
    // 代码分割配置
    splitChunks: {
        chunks: "all", // 对所有模块都进行分割
        // 以下是默认值
        // minSize: 20000, // 分割代码最小的大小
        // minRemainingSize: 0, // 类似于minSize，最后确保提取的文件大小不能为0
        // minChunks: 1, // 至少被引用的次数，满足条件才会代码分割
        // maxAsyncRequests: 30, // 按需加载时并行加载的文件的最大数量
        // maxInitialRequests: 30, // 入口js文件最大并行请求数量
        // enforceSizeThreshold: 50000, // 超过50kb一定会单独打包（此时会忽略minRemainingSize、maxAsyncRequests、maxInitialRequests）
        // cacheGroups: { // 组，哪些模块要打包到一个组
        //   defaultVendors: { // 组名
        //     test: /[\\/]node_modules[\\/]/, // 需要打包到一起的模块
        //     priority: -10, // 权重（越大越高）
        //     reuseExistingChunk: true, // 如果当前 chunk 包含已从主 bundle 中拆分出的模块，则它将被重用，而不是生成新的模块
        //   },
        //   default: { // 其他没有写的配置会使用上面的默认值
        //     minChunks: 2, // 这里的minChunks权重更大
        //     priority: -20,
        //     reuseExistingChunk: true,
        //   },
        // },
        // 修改配置
        cacheGroups: {
            // 组，哪些模块要打包到一个组
            // defaultVendors: { // 组名
            //   test: /[\\/]node_modules[\\/]/, // 需要打包到一起的模块
            //   priority: -10, // 权重（越大越高）
            //   reuseExistingChunk: true, // 如果当前 chunk 包含已从主 bundle 中拆分出的模块，则它将被重用，而不是生成新的模块
            // },
            default: {
                // 其他没有写的配置会使用上面的默认值
                //minSize: 0, // 我们定义的文件体积太小了，所以要改打包的最小文件体积
                //minChunks: 2,
                //priority: -20,
                //reuseExistingChunk: true,
            },
        },
    },
},
```

###### 按需加载，动态导入

需要在代码中修改(es11+)

```js
console.log('app');
//动态导入
document.body.onclick = function(){
    import('./js/add.js')
    .then((res)=>{res.default(1,2)})
    .then((err)=>{})
}
```

我们可以发现，一旦通过 import 动态导入语法导入模块，模块就被代码分割，同时也能按需加载了

###### 单入口

开发时我们可能是单页面应用（SPA），只有一个入口（单入口）。那么我们需要这样配置

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  // 单入口
  entry: "./src/main.js",
  // 多入口
  // entry: {
  //   main: "./src/main.js",
  //   app: "./src/app.js",
  // },
  output: {
    path: path.resolve(__dirname, "./dist"),
    // [name]是webpack命名规则，使用chunk的name作为输出的文件名。
    // 什么是chunk？打包的资源就是chunk，输出出去叫bundle。
    // chunk的name是啥呢？ 比如： entry中xxx: "./src/xxx.js", name就是xxx。注意是前面的xxx，和文件名无关。
    // 为什么需要这样命名呢？如果还是之前写法main.js，那么打包生成两个js文件都会叫做main.js会发生覆盖。(实际上会直接报错的)
    filename: "js/[name].js",
    clean: true,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
  ],
  mode: "production",
  optimization: {
    // 代码分割配置
    splitChunks: {
      chunks: "all", // 对所有模块都进行分割
      // 以下是默认值
      // minSize: 20000, // 分割代码最小的大小
      // minRemainingSize: 0, // 类似于minSize，最后确保提取的文件大小不能为0
      // minChunks: 1, // 至少被引用的次数，满足条件才会代码分割
      // maxAsyncRequests: 30, // 按需加载时并行加载的文件的最大数量
      // maxInitialRequests: 30, // 入口js文件最大并行请求数量
      // enforceSizeThreshold: 50000, // 超过50kb一定会单独打包（此时会忽略minRemainingSize、maxAsyncRequests、maxInitialRequests）
      // cacheGroups: { // 组，哪些模块要打包到一个组
      //   defaultVendors: { // 组名
      //     test: /[\\/]node_modules[\\/]/, // 需要打包到一起的模块
      //     priority: -10, // 权重（越大越高）
      //     reuseExistingChunk: true, // 如果当前 chunk 包含已从主 bundle 中拆分出的模块，则它将被重用，而不是生成新的模块
      //   },
      //   default: { // 其他没有写的配置会使用上面的默认值
      //     minChunks: 2, // 这里的minChunks权重更大
      //     priority: -20,
      //     reuseExistingChunk: true,
      //   },
      // },
  },
};
```

###### 最终配置

单入口+代码分割+动态导入方式

```js
const os = require("os");
const path = require("path");
const ESLintWebpackPlugin = require("eslint-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");
const TerserPlugin = require("terser-webpack-plugin");
const ImageMinimizerPlugin = require("image-minimizer-webpack-plugin");

// cpu核数
const threads = os.cpus().length;

// 获取处理样式的Loaders
const getStyleLoaders = (preProcessor) => {
  return [
    MiniCssExtractPlugin.loader,
    "css-loader",
    {
      loader: "postcss-loader",
      options: {
        postcssOptions: {
          plugins: [
            "postcss-preset-env", // 能解决大多数样式兼容性问题
          ],
        },
      },
    },
    preProcessor,
  ].filter(Boolean);
};

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "../dist"), // 生产模式需要输出
    filename: "static/js/main.js", // 将 js 文件输出到 static/js 目录中
    clean: true,
  },
  module: {
    rules: [
      {
        oneOf: [
          {
            // 用来匹配 .css 结尾的文件
            test: /\.css$/,
            // use 数组里面 Loader 执行顺序是从右到左
            use: getStyleLoaders(),
          },
          {
            test: /\.less$/,
            use: getStyleLoaders("less-loader"),
          },
          {
            test: /\.s[ac]ss$/,
            use: getStyleLoaders("sass-loader"),
          },
          {
            test: /\.styl$/,
            use: getStyleLoaders("stylus-loader"),
          },
          {
            test: /\.(png|jpe?g|gif|svg)$/,
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
          {
            test: /\.(ttf|woff2?)$/,
            type: "asset/resource",
            generator: {
              filename: "static/media/[hash:8][ext][query]",
            },
          },
          {
            test: /\.js$/,
            // exclude: /node_modules/, // 排除node_modules代码不编译
            include: path.resolve(__dirname, "../src"), // 也可以用包含
            use: [
              {
                loader: "thread-loader", // 开启多进程
                options: {
                  workers: threads, // 数量
                },
              },
              {
                loader: "babel-loader",
                options: {
                  cacheDirectory: true, // 开启babel编译缓存
                  cacheCompression: false, // 缓存文件不要压缩
                  plugins: ["@babel/plugin-transform-runtime"], // 减少代码体积
                },
              },
            ],
          },
        ],
      },
    ],
  },
  plugins: [
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "../src"),
      exclude: "node_modules", // 默认值
      cache: true, // 开启缓存
      // 缓存目录
      cacheLocation: path.resolve(
        __dirname,
        "../node_modules/.cache/.eslintcache"
      ),
      threads, // 开启多进程
    }),
    new HtmlWebpackPlugin({
      // 以 public/index.html 为模板创建文件
      // 新的html文件有两个特点：1. 内容和源文件一致 2. 自动引入打包生成的js等资源
      template: path.resolve(__dirname, "../public/index.html"),
    }),
    // 提取css成单独文件
    new MiniCssExtractPlugin({
      // 定义输出文件名和目录
      filename: "static/css/main.css",
    }),
    // css压缩
    // new CssMinimizerPlugin(),
  ],
  optimization: {
    minimizer: [
      // css压缩也可以写到optimization.minimizer里面，效果一样的
      new CssMinimizerPlugin(),
      // 当生产模式会默认开启TerserPlugin，但是我们需要进行其他配置，就要重新写了
      new TerserPlugin({
        parallel: threads, // 开启多进程
      }),
      // 压缩图片
      new ImageMinimizerPlugin({
        minimizer: {
          implementation: ImageMinimizerPlugin.imageminGenerate,
          options: {
            plugins: [
              ["gifsicle", { interlaced: true }],
              ["jpegtran", { progressive: true }],
              ["optipng", { optimizationLevel: 5 }],
              [
                "svgo",
                {
                  plugins: [
                    "preset-default",
                    "prefixIds",
                    {
                      name: "sortAttrs",
                      params: {
                        xmlnsOrder: "alphabetical",
                      },
                    },
                  ],
                },
              ],
            ],
          },
        },
      }),
    ],
    // 代码分割配置
    splitChunks: {
      chunks: "all", // 对所有模块都进行分割
      // 其他内容用默认配置即可
    },
  },

  mode: "production",
  devtool: "source-map",
};
```

###### 给模块命名！

https://www.bilibili.com/video/BV14T4y1z7sw/?p=46&spm_id_from=pageDriver&vd_source=b3a61f4abb2aa7da517f6cf364e96fdd

###### ESLint配置！

##### 空闲时加载

##### 网络缓存

##### 兼容性补丁

- 过去我们使用 babel 对 js 代码进行了兼容性处理，其中使用@babel/preset-env 智能预设来处理兼容性问题。它能将 ES6 的一些语法进行编译转换，比如箭头函数、点点点运算符等。但是如果是 async 函数、promise 对象、数组的一些方法（includes）等，它没办法处理。所以此时我们 js 代码仍然存在兼容性问题，一旦遇到低版本浏览器会直接报错。所以我们想要将 js 兼容性问题彻底解决。
- `core-js` 是专门用来做 ES6 以及以上 API 的 `polyfill`。
- `polyfill`翻译过来叫做垫片/补丁。就是用社区上提供的一段代码，让我们在不兼容某些新特性的浏览器上，使用该新特性

下载

```js
npm i @babel/eslint-parser -D
```

配置

完整引入







##### 离线服务

#### 总结



## 进阶原理
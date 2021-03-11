---
title: webpack简单配置
tags:
  - 工具
  - webpack
---

#### 1、简介

Webpack 是一个模块打包工具(module bundler)，其最主要的功能就是模块打包，模块打包，通俗地说就是：找出模块之间的依赖关系，按照一定的规则把这些模块组织合并为一个 JavaScript 文件。Webpack 认为一切都是模块，JS 文件、CSS 文件、jpg/png 图片等等都是模块。

<!-- more -->

#### 2、 安装

- 新建项目目录

- 在目录运行`npm init -y`,初始化 npm
- 安装 webpack`npm install webpack webpack-cli --save-dev`

#### 3、mode(模式)

development（开发模式）和 production（生产模式）

- 在开发环境中，SourceMap 更全，可以快速定位代码的问题

* 在开发环境中，代码一般不需要压缩；生产环境，代码会被压缩

webpack-dev-server：能够用于快速开发应用程序

```
...
const base = require("./webpack.config.base.js")
module.exports = {
  ...base,
  mode: "development",
  devtool: "inline-source-map",
  devServer: {
    contentBase: "./dist",
  },
 ...
}
...
```

```
//使用不同的webpack配置文件
  "scripts": {
    "start": "webpack-dev-server",//使用默认的
    "build": "rm -rf dist && webpack --config webpack.config.prod.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

**webpack-merge**

#### 4、 入口和出口

入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始

```
module.exports = {
  entry: './src/index.js'
};
```

output 属性告诉 webpack 在哪里输出它所创建的文件，以及如何命名这些文件，默认值为 `./dist`

```
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),   //默认值
    filename: 'index.js'
  }
};
```

#### 5、 浏览器缓存和 hash

- 浏览器缓存：
  - 当浏览器访问一个 html 页面时，html 页面会加载 JS、CSS 和图片等外部资源，这需要花费一定的下载时间。由于很多资源是长时间不变的，因此可以把这些资源存储在本地，这样就不需要花时间下载。
  - 可以通过响应头，`cache-control`设置缓存时间
  - 如果文件变化了，可以设置一个新的文件名，这样在请求的时候浏览器就知道文件改变了，而请求新的文件
  - 只要内容不变，就文件名可以一直使用

* Webpack 与 hash 算法

  - 在使用 Webpack 对构建的时候，Webpack 会根据所有的文件内容计算出一个特殊的字符串。只要有文件的内容变化了，Webpack 就会计算出一个新的特殊字符串。
  - hash/chunkhash/contenthash:
    hash 所有文件哈希值相同； chunkhash 根据不同的入口文件(Entry)进行依赖文件解析、构建对应的 chunk，生成对应的哈希值； contenthash 计算与文件内容本身相关.

    ```
    output: {
       filename: "[name].[contenthash].js",
    },
    ```

    [可替换的内容](https://webpack.docschina.org/configuration/output/#template-strings)

#### 6、loader 和 plugin

##### loader

- babel-loader
  主要作用是在 Webpack 打包的时候，用 Babel 将 ES6 的代码转换成 ES5 版本的。

  ```
    rules: [
       {
         test: /\.js$/,
         exclude: /node_modules/,
         use: {
           loader: 'babel-loader',
           options: {
             presets: ['@babel/preset-env']
           }
         }
       }
  ```

* 加载 CSS 文件
  使用 style-loader 和 css-loader 这两个 loader 来处理 CSS

  ```
    rules: [{
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }]
  ```

  SCSS

  ```
     {
        test: /\.scss$/,
        use: [
          "style-loader",
          "css-loader",
          {
            loader: "sass-loader",
            options: {
              implementation: require("dart-sass"),   //使用dart-sass 代替 node-sass
            },
          },
        ],
      },
  ```

  其他

  ```
      {
        test: /\.styl$/,
        use: ["style-loader", "css-loader", "stylus-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
  ```

  - css-loader 的作用是解析 CSS 文件，包括解析@import 等 CSS 自身的语法。把 CSS 文件解析后，以字符串的形式打包到 JS 文件中。
  - style-loader 就来发挥作用了，它可以把 JS 里的样式代码插入到 html 文件里。它的原理很简单，就是通过 JS 动态生成 style 标签插入到 html 文件的 head 标签里。

##### plugin

- HtmlWebpackPlugin
  生成 HTML5 文件，使用 script 标签将您的所有捆绑包包括在内。
  [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin#configuration)
  ```
  const HtmlWebpackPlugin = require("html-webpack-plugin")
  ...
  plugins: [
    new HtmlWebpackPlugin({
      //设置标题
      title: "标题",
      //模板文件路径
      template: "src/assets/index.html",
    }),
  ],
  ...
  ```

* MiniCssExtractPlugin

  将 CSS 提取到单独的文件中。它为每个包含 CSS 的 JS 文件创建一个 CSS 文件。

  ```
  const MiniCssExtractPlugin = require('mini-css-extract-plugin');

  module.exports = {
  plugins: [new MiniCssExtractPlugin()],
  module: {
     rules: [
        {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, 'css-loader'],
        },
     ],
  },
  };
  ```

  #### 7、loader 和 plugin 的区别

  loader，它是一个转换器，将 A 文件进行编译成 B 文件，比如：将 A.less 转换为 A.css，单纯的文件转换过程。例:babel-loader 将 JS 文件转换为浏览器可以识别的更低版本的 JS

  plugin 是一个扩展器，它丰富了 webpack 本身，针对是 loader 结束后，webpack 打包的整个过程，它并不直接操作文件，而是基于事件机制工作，会监听 webpack 打包过程中的某些节点，执行广泛的任务。例：MiniCssExtractPlugin 可以将 CSS 抽离成一个文件

  #### 8、webpack 懒加载

  在用户执行某些操作的时候，再加载某些模块

  ```
   //lazy.js
   export default function fn() {
   console.log("懒加载")}
  ```

  ```
   button.onclick = e => import(./lazy.js').then(module => {
     let fn = module.default;
     fn();
  });
  ```

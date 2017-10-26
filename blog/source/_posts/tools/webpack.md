---
title: webpack 学习笔记
date: 2017-10-24 09:32:03
tags:
  webpack
categories:
  笔记
---

[git上的一个webpack配置案例](https://github.com/hugzh/webpack-simple-demo)

webpack 是什么？

Webpack就是个模块打包工具，将模块及其依赖打包生成静态资源。在Webpack的机制里，所有的资源都是模块(js,css,图片等)，而且可以通过代码分隔(Code Splitting)的方法异步加载，实现性能上的优化。
<!-- more -->
1. 模块化
2. 自定义文件或npm install
3. 静态文件模块化
4. 借助于插件和加载器

webpack的优势

1. 代码分离
2. 装载器（css, sass, jsx 等等）
3. 智能解析（require("./template/"+names+'.ejs')）

webpack安装流程

1. npm install -g webpack
2. npm install webpack-dev-server

```
var cats = ['aa', 'bb', 'cc'];
module.exports = cats;

var cats = require('./cats.js');
console.log(cats);
```

编译命令：

- webpack app.js bundle.js
  注：app.js 需要编译的文件
    bundle.js 编译后输出的文件
- webpack app.js bundle.js --watch

webpack 第三方库

1. 使用第三方（jquery）
2. 模块化静态文件（css）
3. 使用配置文件webpack.config.js
4. 使用webpack-dev-server

**安装第三方库 jquery**
npm install jquery --save

**css模块化**
安装插件 npm install css-loader style-loader --save-dev

使用：require('!style-loader!css-loader!./style.css');

**webpack-dev-server**

npm install -g webpack-dev-server

在package.js 的scripts 中配置 "webpack-dev-server --entry ./src/js/app.js --output-filename ./dist/bundle.js"
```
{
  "name": "test-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server --port 9001 --entry ./src/js/app.js --output-filename ./dist/bundle.js",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "jquery": "^3.2.1"
  },
  "devDependencies": {
    "css-loader": "^0.28.7",
    "style-loader": "^0.19.0"
  }
}
```
1. 执行命令: npm start
2. 打开 localhost:8080

注意：Webpack与webpack-dev-server版本不兼容导致npm start 报错，解决方法：
  - 将webpack-dev-server卸载掉：npm uninstall webpack-dev-server -g
  - 然后安装1.15.0版本的webpack-dev-server：npm install webpack-dev-server@1.15.0 -g

**html-webpack-plugin**

安装：npm install html-webpack-plugin --save

```
var htmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: {
    main: './src/js/main.js',
    a: './src/js/a.js'
  },
  output: {           // 出口文件
    path: __dirname + '/dist',
    filename: 'js/[name]-[chunkhash].js',
    publicPath: 'http://cdn.com/' // 上线地址
  },
  // 需要依赖的插件或者是装载器
  module: {
    loaders: [
      {test: /\.css$/, loader:'style-loader!css-loader'},
      {
        test: /\.js$/, loader:'babel-loader',
        exclude: '/node_modules/',   // 优化打包速度
        include: './src',            // 优化打包速度
        query: {
          presets: ['es2015']
        }
      }
    ]
  },
  plugins: [
    new htmlWebpackPlugin({
      template: 'index.html',
      filename：'index.html',
      inject: false,                          // 是否默认注入javascript
      title: 'webpack demo',                  // 使用方法，在html中引用 <%= htmlWebpackPlugin.options.title %>
      minify: {                               // 压缩
        removeComments: true,                 // 注释
        collapseWhitespace: true,             // 删除空格
      },
      chunks: ['main', 'a']                   // 只引入需要的chuncks, 用于多页面配置, 注意设置inject: true
    }),
    new htmlWebpackPlugin({
      ...
    })
  ]
}
```
> 通过配置多个htmlWebpackPlugin 可以配置多个页面

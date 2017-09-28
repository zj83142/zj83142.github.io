---
title: vue 学习笔记 —— 安装
date: 2017-09-21 10:03:05
tags:
  笔记
categories:
  vue
---

> 最近由于Facebook的专利问题，百度已经宣布停用一切React软件产品，React开发人员甚是惊慌。Facebook到目前为止拒绝修改React的开源许可条款。虽然还不确定最终的结果，但是学习一下新的知识也是有备无患的，就是不知道vue过阵子是不是也会爆出来侵权or专利问题。悲催的前端工程师……

### 安装

**兼容性 **

  Vue.js 不支持 IE8 及其以下版本，因为 Vue.js 使用了 IE8 不能模拟的 ECMAScript 5 特性。

  Vue.js 支持所有[兼容ECMAScript 5的浏览器](http://caniuse.com/#feat=es5)。

  ![](/images/compatible-es5-browser.png)


**安装**

  1. 直接script标签引入

    下载地址：
      - [开发版本](https://vuejs.org/js/vue.js), 包含完整的警告和调试模式
      - [生产版本](https://vuejs.org/js/vue.min.js)，删除了警告，28.96kb min+gzip

  2. NPM 

    $ npm install vue

  3. 命令行工具

    Vue 提供一个官方[命令行工具](https://github.com/vuejs/vue-cli),可用于快速搭建大型单页面应用。

    $ npm install --global vue-cli
    $ vue init webpack my-project
    $ cd my-project
    $ npm install
    $ npm run dev

    > 注意：
      - nodejs 版本升级，网上命令行没有效果，[node官网](https://nodejs.org/zh-cn/)下载安装包，直接重新安装的。
      - 建议将npm的注册表源设置为[国内的镜像](http://riny.net/2014/cnpm/),可以大幅提升安装速度。

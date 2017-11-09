---
title: Promise 对象
date: 2017-11-09 18:44:21
tags:
  - Promise
categories: 
  - Javascript
---

[Promise对象 - 阮一峰](http://javascript.ruanyifeng.com/advanced/promise.html#toc9)

promise 对象是CommonJs工作组提出的一种规范，目的是为异步操作提供统一接口。

### javascript 的异步执行

javascript语言的执行环境是单线程(single thread)。每次只能处理一件任务，如果有多个任务，就必须排队。一个执行完成，再执行后面一个任务。这种模式的好处是实现起来比较简单，执行环境相对简单。坏处是只要有一个任务耗时很长，后面的任务都必须排队等待，会拖延整个程序的执行。常见的浏览器无应用（假死），往往就是这样形成的。

javascript语言本身并不慢，慢的是读写外部数据，比如等待Ajax请求返回结果。这个时候，如果服务器迟迟没有相应。或者网络不通畅，就会导致脚本长时间停滞。

为了解决这个问题，javascript语言将任务的执行模式分为两种，同步(synchronous) 和异步(Asynchronous)。

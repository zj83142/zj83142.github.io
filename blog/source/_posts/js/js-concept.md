---
title: javascript中的一些概念
date: 2017-10-30 15:09:21
tags:
  - javascript
  - 概念
categories: 
  - 笔记

### setTimeout 和 seInterval

javascript是单线程，所以setTimeout和setInterval两个函数是利用代码插入的方式实现伪异步，和Ajax的原理实际上是一样的。
```
function fn() { 
  setTimeout(function(){alert('can you see me?');},1000); 
  while(true) {

  } 
}
```
上面代码很清楚的说明了这个问题，alert永远不会执行，因为while这段代码没有执行完成。

### 变量声明
```js
var name = 'World';
(function() {
  if(typeof name === 'undefined') {
    var name = 'Jack';
    console.log('Goodbey ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();

```
控台打印的正确结果是：Goodbye jack, 上面代码其实相当于：
```js
var name = 'World';
(function() {
  var name;
  if(typeof name === 'undefined') {
    name = 'Jack';
    console.log('Goodbye ' + name);
  } else {
    console.log('Hello ' + name);
  }
})();
```


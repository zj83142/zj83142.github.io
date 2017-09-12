---
title: js类型检测
date: 2017-09-12 14:59:00
tags:
---


### 判断数据类型的方法

1. 根据数据类型判断
  - typeof
  - instaneof

2. 根据构造函数判断
  - Object.prototype.toString
  - construcor

[慕课网一个老师的视频-讲Zepto](http://www.imooc.com/course/comment/id/745?page=4)

### 原型与原型链

1. 每个函数都有一个prototype属性
2. 所有通过函数new出来的对象，这个对象都有一个__proto__指向这个函数的prototype
3. 当你想要使用一个对象（或者一个数组）的某个功能时：如果该对象本身具有这个功能，则直接使用，如果该对象本身没有这个功能，则去__proto__中找

prototype 显示原型
> 每一个函数在创建之后都会拥有一个名为prototype的属性，这个属性指向函数的原型对象，通过Function.prototype.bind方法构造出来的函数都是个例外，它没有prototype属性

```
var fu = function() {};
console.log(fn.prototype);
```
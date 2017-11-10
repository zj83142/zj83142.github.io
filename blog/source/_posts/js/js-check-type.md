---
title: js类型检测
date: 2017-09-12 14:59:00
tags:
  - 类型检测
---
  - js


### 判断数据类型的方法

1. 根据数据类型判断
  - typeof
  - instaneof

2. 根据构造函数判断
  - Object.prototype.toString
  - construcor
<!-- more -->
[慕课网一个老师的视频-讲Zepto](http://www.imooc.com/course/comment/id/745?page=4)
<!-- more -->
### 判断方法

- isFunction 判断是否是函数
  ```
  function isFunction(value) { return type(value) == "function" }
  ```
- isWindow
  ```
  // 判断是否是 window对象（注意，w为小写）指当前的浏览器窗口，window对象的window属性指向自身。
  // 即 window.window === window
  function isWindow(obj)     { return obj != null && obj == obj.window }
  ```
- isDocument 判断是否是 document 对象
  ```
  // window.document.nodeType == 9 数字表示为9，常量表示为 DOCUMENT_NODE
  function isDocument(obj)   { return obj != null && obj.nodeType == obj.DOCUMENT_NODE }
  ```
- isObject 判断是否是 object
  ```
  function isObject(obj)     { return type(obj) == "object" }
  ```
- isPlainObject
  ````
  function isPlainObject(obj) {
    return isObject(obj) && !isWindow(obj) && Object.getPrototypeOf(obj) == Object.prototype
  }
  ```
- isArray 判断是否是arr
  ```
  isArray = Array.isArray || function(object){ return object instanceof Array };
  ```
- likeArray
  ```
  // 判断是否是数组或者对象数组
  // !!的作用是把一个其他类型的变量转成的bool类型。
  // !!obj 直接过滤掉了false，null，undefined，''等值
  function likeArray(obj) {
  var length = !!obj && 'length' in obj && obj.length,

  // 获取obj的数据类型
  type = $.type(obj);

  // 不能是function类型，不能是window
  // 如果是array则直接返回true
  // 或者当length的数据类型是number，并且其取值范围是0到(length - 1)这里是通过判断length - 1 是否为obj的属性

  return 'function' != type && !isWindow(obj) && (
      'array' == type || length === 0 ||
      (typeof length == 'number' && length > 0 && (length - 1) in obj)
    )
  }
  ```
- isEmptyObject 空对象
  ```
  $.isEmptyObject = function(obj) {
    var name
    for (name in obj) return false
    return true
  }
  ```
- isNumeric 数字
  ```
  $.isNumeric = function(val) {
    var num = Number(val), type = typeof val;
    return val != null && type != 'boolean' &&
      (type != 'string' || val.length) &&
      !isNaN(num) && isFinite(num) || false
  }
  ```
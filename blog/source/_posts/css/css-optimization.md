---
title: css 性能优化
date: 2017-09-28 14:51:46
tags:
  - css性能优化
categories: 
  - CSS
---

css优化空间很大，对性能的影响也很大。事实上大部分复杂的页面完全不修改javascript代码，可以通过css的优化大幅提升render performance。这就需要我们对css的渲染有更深入的了解。css优化可以从加载性能、选择器性能、渲染性能、可维护性，健壮性方面入手...
<!-- more -->

### 1. css 渲染规则

css选择符是有权重的，**css样式规则渲染是先就近渲染，然后才一句选择符权重进行选渲染**

从右到左渲染：
```
.nav h3 a{font-size: 14px;}
```

渲染过程大概是：首先找到所有的a，沿着a的父元素查找h3，然后再沿着h3，查找.nav。中途找到了符合匹配规则的节点就加入结果集。如果找到根元素html都没有匹配，则不再遍历这条路径，从下一个a开始重复这个查找匹配（只要页面上有多个最右节点为a）。

### 2. 嵌套层级不要超过3级

一般情况下，元素的嵌套层级不能超过3级，过度的嵌套会导致代码变得臃肿，沉余，复杂。导致css文件体积变大，造成性能浪费，影响渲染的速度！而且过于依赖HTML文档结构。这样的css样式，维护起来，极度麻烦，如果以后要修改样式，可能要使用!important覆盖。

### 3. 样式重置
```css
  body,dl,dd,h1,h2,h3,h4,h5,h6,p,form,ol,ul {
    margin: 0;
    padding: 0;
  }
  h1, h2, h3, h4, h5, h6 {
    font-weight: normal;
  }
  ol, ul {
    list-style: none;
  }
  h1{
    font-size: 24px;
  }
  h2{
    font-size: 20px;
  }
  h3{
    font-size: 18px;
  }
  h4 {
    font-size: 16px;
  }
  h5{
    font-size: 14px;
  }
  h6{
    font-size: 12px;
  }
```

### 4. 样式级别

**css选择符权重渲染规则：important > 内联 > ID > 类 > 标签 | 伪类 | 属性选择 > 伪对象 >通配符 > 继承**

### 5. 图片要设置width和height

如果页面有使用img标签，那么img很建议设置width和height。目的是为了在网速差或者其它原因加载不出图片的时候，保证布局不会乱。

设置width和height，注意几点：
  - 1.PC站，建议在img标签的属性设置width和height。这样避免加载不出css而错位
  - 手机站，建议用css设置img的width和height，因为手机站要做适配，在属性设置width和height不灵活，比如使用rem布局，在属性那里设置不了width和height。
  - 如果图片不固定，但是有一个max-width和max-height，那么建议在img的父元素设置width和height。img根据父元素的width和height设置max-width和max-height。

### 6. 图片预加载

- 懒加载：页面加载的时候，先加载一部分内容（一般是先加载首屏内容），其它内容等到需要加载的时候再进行加载！

- 预加载：页面加载的时候，先加载一部分内容（一般是先加载首屏内容），其它内容等到先加载的一部分内容（一般是首屏内容）加载完了，再进行加载。

两种方式，都是为了减少用户进入网站的时候，更快的看到首屏的内容！

下面栗子，将这#preloader这个元素加入到到html中，就可以实现通过CSS的background属性将图片预加载到屏幕外的背景上。只要这些图片的路径保持不变，当它们在web页面的其他地方被调用时，浏览器就会在渲染过程中使用预加载（缓存）的图片。简单、高效，不需要任何JavaScript。
```
#preloader {
    /*需要预加载的图片*/
    background: url(image1.jpg) no-repeat,url(image2.jpg) no-repeat,url(image3.jpg) no-repeat;
    width: 0px;
    height: 0px;
    display: inline;
}
```
但是这样会有一个问题，因为#preloader预加载的图片，会和页面上的其他内容一起加载，增加了页面的整体加载时间。所以需要用js控制
```
function preloader(urlArr,obj) {
    var bgText='';
    for(var i=0,len=urlArr.length;i<len;i++){
        bgText+='url('+urlArr[i]+') no-repeat,';
    }
    obj.style.background=bgText.substr(0,bgText.length-1);
}
window.onload = function() {
   preloader(['image1.jpg','image2.jpg','image3.jpg'],document.getElementById('preloader'));
}
```
原理也很简单，就是先让首屏的图片加载完，然后再加载其它的图片。通过给#preloader设置背景图片，加载所需要的图片，然后页面上需要加载这些图片的时候，就直接从缓存里面拿图片，不需要通过http请求获取图片，这样加载就很快。

### 7. 慎用*通配符

在做网页的时候经常会使用下面两种方式重置样式，以此来消除标签的默认布局和不同浏览器对于同一个标签的渲染。

*{margin：0；padding：0;}
上面这种方式，代码少，但是性能差，因为渲染的时候，要匹配页面上所有的元素！很多基础样式没有margin和padding的元素，比如div，li等。都被匹配，完全没必要！
下面看另一种方式。

body,dl,dd,h1,h2,h3,h4,h5,h6,p,form,ol,ul{margin：0；padding：0;}
这种方式，代码稍微多，但是性能比上面的方式好，在渲染的时候，只匹配body,dl,dd,h1,h2,h3,h4,h5,h6,p,form,ol,ul这里面的元素，这些元素带有margin和padding，需要重置！
再看例子：

.test * {color: red;}
匹配文档中所有的元素，然后分别向上逐级匹配class为test的元素，直到文档的根节点

.test a {color: red;}
匹配文档中所有a的元素，然后分别向上逐级匹配class为test的元素，直到文档的根节点

两种方式，哪种更好不言而喻，所以在开发的时候，建议避免使用通配选择器。


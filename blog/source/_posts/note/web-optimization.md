---
title: web 前端优化策略
date: 2017-11-06 16:36:48
tags:
  笔记
categories:
  前端优化
---

前端优化的目的：
- 从用户角度讲，优化能够让页面加载的更快，用户体验更好。
- 从服务商角度讲，优化能够减少页面请求数、或者减少请求所占带宽，能够节省资源。

PC浏览器优化策略有很多前端优化主要包括： 网络加载类、页面渲染类、CSS优化类、Javascript执行类、缓存类、图片类、构架协议类……

<!-- more -->

### 网络加载类

**1. 减少HTTP资源请求次数**
在前端页面中，通常建议尽量合并静态资源图片，Javascript或CSS代码，减少页面请求书和资源请求消耗，这样可以缩短页面首次访问的用户等待时间，通过构建工具合并雪碧图、CSS、Javascript文件等都是为了减少HTTP资源请求次数。另外也要避免重复的资源，防止增加多余请求。

**2. 减少HTTP请求大小**
除了减少HTTP资源请求次数，也要尽量减少每个HTTP请求的大小。如减少没必要的图片，Javascript、CSS以及HTML代码，对文件进行压缩优化，或者使用gzip压缩传输内容等，都可以用来减小文件，缩短网络传输等待延时，我们使用构建工具来压缩静态图片资源以及移除代码中的注释并压缩，目的都是为了减小HTTP请求的大小。

**3. 将CSS或Javascript放到外部文件中，避免使用style或script标签直接引入**
在ＨＴＭＬ文件中引用外部资源可以有效利用浏览器的静态资源缓存，但有时候在移动端页面CSS或Javascript比较简单的情况下为了减少请求，也会将CSS或Javascipt直接写到HTML里面，具体要根据CSS或Javascript文件的大小和无业务的场景来分析，如果CSS或Javascript文件内容比较多，业务逻辑比较复杂，建议放到外部文件引入。
```
<link rel="stylesheet" href="//cdn.domain.com/path/main.css" >
<script src="//cdn.domain.com/path/main.js"></script>
```

**4. 避免页面中空的href和src**
当link标签的href属性为空，或script、img、iframe标签的src属性为空时，浏览器在渲染的过程中仍会将href属性或src属性中的空白内容进行加载，直至加载失败，这样就阻塞了页面中其他资源的下载进度。而且最终加载到的内容是无效的，因此要尽量避免
```
<!--不推荐-->
<img src="" alt="photo" >
<a href="">点击链接</a>
```

**5. 为HTML指定Cache-Control或Expiress**
为HTML内容设置Cache-Control或Expires可以将HTML内容缓存起来，避免频繁向服务器发送请求，在页面 Cache-Control 或 Expires 头部有效时，浏览器将直接从缓存中读取内容，不向服务器端发送请求。
```
<meta http-equiv="Cache-Control" content="max-age=7200">
<meta http-equiv="Expires" content="Mon,20Jul201623:00:00GMT">
```

**6. 合理设置Etag 和 Last-Modified**
合理设置 Etag 和 Last-Modified 使用浏览器缓存，对于未修改的文件，静态资源服务器会向浏览器端返回304，让浏览器从缓存中读取文件，减少 Web 资源下载的带宽消耗并降低服务器负载。
```
<meta http-equiv="last-modified" content="Sun,05 Nov 2017 13:45:57 GMT">
```

**7．减少页面重定向**
页面每次重定向都会延长页面内容返回的等待延时，一次重定向大约需要200毫秒不等的时间开销（无缓存），为了保证用户尽快看到页面内容，要尽量避免页面重定向。

**8．使用静态资源分域存放来增加下载并行数**
浏览器在同一时刻向同一个域名请求文件的并行下载数是有限的，因此可以利用多个域名的主机来存放不同的静态资源，增大页面加载时资源的并行下载数，缩短页面资源加载的时间。通常根据多个域名来分别存储 JavaScript、CSS 和图片文件。
```
<link rel="stylesheet" href="//cdn1.domain.com/path/main.css" >
<script src="//cdn2.domain.com/path/main.js"></script>
```

**9．使用静态资源 CDN 来存储文件**
如果条件允许，可以利用 CDN 网络加快同一个地理区域内重复静态资源文件的响应下载速度，缩短资源请求时间。

**10．使用 CDN Combo 下载传输内容**
CDN Combo 是在 CDN 服务器端将多个文件请求打包成一个文件的形式来返回的技术，这样可以实现 HTTP 连接传输的一次性复用，减少浏览器的 HTTP 请求数，加快资源下载速度。例如同一个域名 CDN 服务器上的 a.js，b.js，c.js 就可以按如下方式在一个请求中下载。
```
<script src="//cdn.domain.com/path/a.js,b.js,c.js"></script>
```

**11．使用可缓存的 AJAX**
对于返回内容相同的请求，没必要每次都直接从服务端拉取，合理使用 AJAX 缓存能加快 AJAX 响应速度并减轻服务器压力。
```
$.ajax({
    url : url,
    type : 'get',
    cache : true, //推荐使用缓存
    data : {},
    success (){//...},
    error (){//...}
});
```

**12．使用 GET 来完成 AJAX 请求**
使用 XMLHttpRequest 时，浏览器中的 POST 方法会发起两次 TCP 数据包传输，首先发送文件头，然后发送 HTTP 正文数据。而使用 GET 时只发送头部，所以在拉取服务端数据时使用 GET 请求效率更高。
```
$.ajax({
    url : url,
    type : 'get', //推荐使用get完成请求
    data : {},
    success (){//...},
    error(){//...}
});
```

**13．减少 Cookie 的大小并进行 Cookie 隔离**
HTTP 请求通常默认带上浏览器端的 Cookie 一起发送给服务器，所以在非必要的情况下，要尽量减少 Cookie 来减小 HTTP 请求的大小。对于静态资源，尽量使用不同的域名来存放，因为 Cookie 默认是不能跨域的，这样就做到了不同域名下静态资源请求的 Cookie 隔离。

**14．缩小 favicon.ico 并缓存**
有利于 favicon.ico 的重复加载，因为一般一个 Web 应用的 favicon.ico 是很少改变的。

**15．推荐使用异步 JavaScript 资源**
异步的 JavaScript 资源不会阻塞文档解析，所以允许在浏览器中优先渲染页面，延后加载脚本执行。例如 JavaScript 的引用可以如下设置，也可以使用模块化加载机制来实现。
```
<script src="main.js" defer></script>
<script src="main.js" async></script>
```
使用 async 时，加载和渲染后续文档元素的过程和 main.js 的加载与执行是并行的。使用 defer 时，加载后续文档元素的过程和 main.js 的加载是并行的，但是 main.js 的执行要在页面所有元素解析完成之后才开始执行。

**16．消除阻塞渲染的 CSS 及 JavaScript**

对于页面中加载时间过长的 CSS 或 JavaScript 文件，需要进行合理拆分或延后加载，保证关键路径的资源能快速加载完成。

**17．避免使用 CSS import 引用加载 CSS**

CSS 中的 ＠import 可以从另一个样式文件中引入样式，但应该避免这种用法，因为这样会增加 CSS 资源加载的关键路径长度，带有 ＠import 的 CSS 样式需要在 CSS 文件串行解析到 @import 时才会加载另外的 CSS 文件，大大延后 CSS 渲染完成的时间。
```
<!--不推荐-->
<style>
    @import "path/main.css";
</style>

<!--推荐-->
<link rel="stylesheet" href="//cdn1.domain.com/path/main.css" >
```

### 页面渲染类

**1．把 CSS 资源引用放到 HTML 文件顶部**
一般推荐将所有 CSS 资源尽早指定在 HTML 文档 <head> 中，这样浏览器可以优先下载 CSS 并尽早完成页面渲染。

**2．JavaScript 资源引用放到 HTML 文件底部**
JavaScript 资源放到 HTML 文档底部可以防止 JavaScript 的加载和解析执行对页面渲染造成阻塞。由于 JavaScript 资源默认是解析阻塞的，除非被标记为异步或者通过其他的异步方式加载，否则会阻塞 HTML DOM 解析和 CSS 渲染的过程。

**3．尽量预先设定图片等大小**
在加载大量的图片元素时，尽量预先限定图片的尺寸大小，否则在图片加载过程中会更新图片的排版信息，产生大量的重排

**4．不要在 HTML 中直接缩放图片**
在 HTML 中直接缩放图片会导致页面内容的重排重绘，此时可能会使页面中的其他操作产生卡顿，因此要尽量减少在页面中直接进行图片缩放。

**5．减少 DOM 元素数量和深度**
HTML 中标签元素越多，标签的层级越深，浏览器解析 DOM 并绘制到浏览器中所花的时间就越长，所以应尽可能保持 DOM 元素简洁和层级较少。
```
<!--不推荐-->
<div>
    <span>
        <a href="javascript:void(0);">
            <img src="./path/photo.jpg" alt="图片">
        </a>
    </span>
</div>

<!--推荐-->
<img src="./path/photo.jpg" alt="图片" >
```

**6．尽量避免在选择器末尾添加通配符**
CSS 解析匹配到 渲染树的过程是从右到左的逆向匹配，在选择器末尾添加通配符至少会增加一倍多计算量。

**7．减少使用关系型样式表的写法**
直接使用唯一的类名即可最大限度的提升渲染引擎绘制渲染树等效率

**8．尽量减少使用JS动画**
JS 直接操作 DOM 极容易引起页面的重排

**9．CSS 动画使用 translate、scale 代替 top、height**
尽量使用 CSS3 的 translate、scale 属性代替 top、left 和 height、width，避免大量的重排计算

**10．尽量避免使用<table>、<iframe>**
<table> 内容的渲染是将 table 的 DOM 渲染树全部生成完并一次性绘制到页面上的，所以在长表格渲染时很耗性能，应该尽量避免使用它，可以考虑使用列表元素 <ul> 代替。尽量使用异步的方式动态添加 iframe，因为 iframe 内资源的下载进程会阻塞父页面静态资源的下载与 CSS 及 HTML DOM 的解析。

**11．避免运行耗时的 JavaScript**
长时间运行的 JavaScript 会阻塞浏览器构建 DOM 树、DOM 渲染树、渲染页面。所以，任何与页面初次渲染无关的逻辑功能都应该延迟加载执行，这和 JavaScript 资源的异步加载思路是一致的。

**12．避免使用 CSS 表达式或 CSS 滤镜**
CSS 表达式或 CSS 滤镜的解析渲染速度是比较慢的，在有其他解决方案的情况下应该尽量避免使用。
```
//不推荐
.opacity{
    filter : progid : DXImageTransform.Microsoft.Alpha( opacity = 50 );
}
```


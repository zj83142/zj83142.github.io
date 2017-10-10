---
title: css-loadding 效果积累
date: 2017-10-10 15:29:08
tags:
  css加载效果
categories:
  CSS
---

> 在日常中遇到的感觉不错的loading效果，先积累起来，有时间好好看看具体是如何实现的。

### 第一种效果 [查看](https://codepen.io/augbog/pen/ZXQVVZ/)
```
html, body {
  height: 100%;
  background: grey;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.loader {
  position: relative;
  margin: 0 auto;
  width: 300px;
  height: 300px;
}

.bounce {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  opacity: 0.8;
  position: absolute;
  top: 0;
  left: 0;
  -webkit-animation: bounce 5s infinite ease-in;
  animation: bounce 5s infinite ease-in;
}

.color1 {
  background-color: #06ceb4;
  animation-delay: -1s;
}

.color2 {
  background-color: #ffeead;
  animation-delay: -2s;
}

.color3 {
  background-color: #ff6f69;
  animation-delay: -3s;
}

.color4 {
  background-color: #ffcc5c;
  animation-delay: -4s;
}

.color5 {
  background-color: #88d8b0;
  animation-delay: -5s;
}

@keyframes bounce {
  0%, 100% {
    transform: scale(0);
  }
  50% {
    transform: scale(1);
  }
}
<div class="loader">
  <div class="bounce color1"></div>
  <div class="bounce color2"></div>
  <div class="bounce color3"></div>
  <div class="bounce color4"></div>
  <div class="bounce color5"></div>
</div>

```

### 第二种效果 [查看](https://codepen.io/drenther/pen/Yrwvee)

```
html, body {
  height: 100%;
  width: 100%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: center;
      -ms-flex-pack: center;
          justify-content: center;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
  background: #f5f5f5;
}
html .loader,
body .loader {
  height: 50px;
  width: 50px;
  position: relative;
}
html .loader::after, html .loader::before,
body .loader::after,
body .loader::before {
  content: "";
  width: 50px;
  height: 50px;
  position: absolute;
  border: solid 8px transparent;
  border-radius: 50%;
  -webkit-animation: wiggle 1.4s ease infinite;
          animation: wiggle 1.4s ease infinite;
}
html .loader::before,
body .loader::before {
  border-top-color: #4285f4;
  border-bottom-color: #34a853;
}
html .loader::after,
body .loader::after {
  border-left-color: #fbbc05;
  border-right-color: #ea4335;
  -webkit-animation-delay: 0.7s;
          animation-delay: 0.7s;
}

@-webkit-keyframes wiggle {
  0% {
    -webkit-transform: rotate(0deg) scale(1);
            transform: rotate(0deg) scale(1);
  }
  50% {
    -webkit-transform: rotate(180deg) scale(0.5);
            transform: rotate(180deg) scale(0.5);
  }
  100% {
    -webkit-transform: rotate(360deg) scale(1);
            transform: rotate(360deg) scale(1);
  }
}

@keyframes wiggle {
  0% {
    -webkit-transform: rotate(0deg) scale(1);
            transform: rotate(0deg) scale(1);
  }
  50% {
    -webkit-transform: rotate(180deg) scale(0.5);
            transform: rotate(180deg) scale(0.5);
  }
  100% {
    -webkit-transform: rotate(360deg) scale(1);
            transform: rotate(360deg) scale(1);
  }
}

<div class="loader"></div>

```
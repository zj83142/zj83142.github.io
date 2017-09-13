---
title: js 随机颜色的三种方式和颜色格式的转化
date: 2017-09-13 14:04:28
tags:
  - javascript
  - color
  - translate
categories: 
  - 笔记
---

### 生成随机颜色

#### 十六进制格式（#000000-#FFFFFF）

```
function randomHexColor() {
  let hex = Math.floor(Math.random() * 16777216).toString(16); // 生成ffffff以内16进制数
  while (hex.length < 6) { //while循环判断hex位数，少于6位前面加0凑够6位
    hex = '0' + hex;
  }
  return '#' + hex; //返回‘#’开头16进制颜色
}
```

```
function randomHexColor() { 
  return '#' + ('00000' + (Math.random() * 0x1000000 << 0).toString(16)).substr(-6);
}
```
执行数序：
1. 先执行Math.random() * 0x1000000，其中0x1000000=0xffffff+1，因为Math.random()取到1，所以+1，这样就会生成一个1-16777216(不包含)以内的浮点数。
2. 然后执行<<0，这是取整运算，去掉后面的小数点。这时为一个16777216(不包含)以内的十进制数。
3. 之后执行.toString(16)，把十进制数转化为六位以下16进制数。
4. 再后执行'00000'+，这时因为之前生成的16进制数最少可能仅一位，在前面加上5个0。
5. 最后执行.substr(-6)，是去从-6开始的后面所有字符串，也就是最后6位数。
6. 前面加上#并retuen

#### RGB格式

```
function randomRgbColor() { //随机生成RGB颜色
  const r = Math.floor(Math.random() * 256); //随机生成256以内r值
  const g = Math.floor(Math.random() * 256); //随机生成256以内g值
  const b = Math.floor(Math.random() * 256); //随机生成256以内b值
  return `rgb(${r},${g},${b})`; //返回rgb(r,g,b)格式颜色
}
```

#### RGBA格式

```
function randomRgbaColor() { //随机生成RGBA颜色
  const r = Math.floor(Math.random() * 256); //随机生成256以内r值
  const g = Math.floor(Math.random() * 256); //随机生成256以内g值
  const b = Math.floor(Math.random() * 256); //随机生成256以内b值
  const alpha = Math.random(); //随机生成1以内a值
  return `rgb(${r},${g},${b},${alpha})`; //返回rgba(r,g,b,a)格式颜色
}
```

### 颜色格式转化

#### 十六进制转为RGB

```
function hex2Rgb(hex) { //十六进制转为RGB
  let rgb = []; // 定义rgb数组
  if (/^\#[0-9A-F]{3}$/i.test(hex)) { //判断传入是否为#三位十六进制数
    let sixHex = '#';
    hex.replace(/[0-9A-F]/ig, function(kw) {
        sixHex += kw + kw; //把三位16进制数转化为六位
    });
    hex = sixHex; //保存回hex
  }
  if (/^#[0-9A-F]{6}$/i.test(hex)) { //判断传入是否为#六位十六进制数
    hex.replace(/[0-9A-F]{2}/ig, function(kw) {
        rgb.push(parseInt(kw,16)); //十六进制转化为十进制并存入数组
    });
    return `rgb(${rgb.join(',')})`; //输出RGB格式颜色
  } else {
    console.log(`Input ${hex} is wrong!`);
    return 'rgb(0,0,0)';
  }
}
```

#### RGB转为十六进制

```
function rgb2Hex(rgb) {
  if (/^rgb\((\d{1,3}\,){2}\d{1,3}\)$/i.test(rgb)) { //test RGB
    let hex = '#'; //定义十六进制颜色变量
    rgb.replace(/\d{1,3}/g, function(kw) { //提取rgb数字
      kw = parseInt(kw).toString(16); //转为十六进制
      kw = kw.length < 2 ? 0 + kw : kw; //判断位数，保证两位
      hex += kw; //拼接
    });
    return hex; //返回十六进制
  } else {
    console.log(`Input ${rgb} is wrong!`);
    return '#000'; //输入格式错误,返回#000
  }
}
```
---
title: js 问题积累
date: 2017-09-20 15:35:48
tags:
  javascript
categories:
  - js
  - dropzone
  - datetimepicker
  - js下载文本
---

工作中遇到一些问题，虽然最后解决了，但是过一段时间可能又忘了，再遇到又要去查资料。觉得很有必要记录下来。
<!-- more -->
### 上传文件插件dropzone

1. 设置 Dropzone.autoDiscover = false

  (禁止对所有元素的自动查找) 否则前端会报错: Dropzone already attached.

2. 基本用法
  ```
  <div id="myDropzone" class="dropzone"></div>

  var myDropzone = new Dropzone("#upload_logo", { 
    url: "xxxx",  // 上传文件地址
    maxFiles: '1', // 最多上传一个文件
    dictDefaultMessage: '上传 Logo 图片', // 修改 提示文字
    addRemoveLinks: 'dictRemoveFile', // 显示删除文件链接
    acceptedFiles: 'image/*'  // 接收的文件类型
  });
  // 文件上传成功
  myDropzone.on('success', function(file, response) {
    var res = JSON.parse(response);
    ...
  });

  ```

3. 显示服务器上已存在的图片或文件

  ```
  var mockFile = { name: filename, size: 123, accepted: true };
  myDropzone.emit( "addedfile", mockFile);
  myDropzone.emit( "thumbnail", mockFile, fileUrl); // 生成缩略图
  myDropzone.files.push( mockFile ); 
  // $('div.dz-preview').addClass(' dz-processing dz-image-preview dz-success dz-complete'); // 这个会有上传成功的动画，不太需要
  $('div.dz-preview').addClass(' dz-complete'); // 设置样式，如果不设置，缩略图上会显示上传进度条
  ```

4. 删除文件

  ```
  myDropzone.removeAllFiles();
  ```

5. 还有很多方法，具体可以查看源码，或者说明文档，参考地址：[DropzoneJS](http://wxb.github.io/dropzonejs.com.zh-CN/dropzonezh-CN/#)

### bootstrap datetimepicker 

  1. 月份选择  
    ```
    $('#xxx').datetimepicker({
      minView : "year", //  选择时间时，最小可以选择到那层；默认是‘hour’也可用0表示
      startView: 'year',
      language : 'zh-CN', // 语言
      autoclose : true, //  true:选择时间后窗口自动关闭
      format : 'yyyy-mm', // 文本框时间格式，设置为0,最后时间格式为2017-03-23 17:00:00
    });
    ```

### js点击下载文本到本地文件中

```
function downloadText(content) {
  let fileName = 'xxxx.txt';  // 文件名
  let aLink = document.createElement('a'); 
  let evt = document.createEvent("MouseEvents");
  evt.initEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null); // 点击
  aLink.download = fileName;
  aLink.dispatchEvent(evt);
}
```
**知识点：事件触发器**
> 事件触发器就是用来触发某个元素下的某个事件，IE下有fireEvent()，高级浏览器（chrome，firefox等）有dispatchEvent()

#### IE浏览器
```
var fireEvent = function(element, event) {
  // IE浏览器支持fireEvent方法
  if (document.createEventObject) {
    var evt = document.createEventObject();
    return element.fireEvent('on' + event, evt)
  } else {
    // 其他标准浏览器使用dispatchEvent方法
    var evt = document.createEvent('HTMLEvents');
    // initEvent接受3个参数：
    // 事件类型，是否冒泡，是否阻止浏览器的默认行为
    evt.initEvent(event, true, true);
    return !element.dispatchEvent(evt);
  }
};
```
或 document上绑定自定义事件ondataavailable
```
document.attachEvent('ondataavailable', function (event) {
  alert(event.eventType);
});
var obj=document.getElementById("obj");
//obj元素上绑定click事件
obj.attachEvent('onclick', function (event) {
  alert(event.eventType);
});

//调用document对象的createEventObject方法得到一个event的对象实例。
var event = document.createEventObject();
event.eventType = 'message';

//触发document上绑定的自定义事件ondataavailable
document.fireEvent('ondataavailable', event);

//触发obj元素上绑定click事件
document.getElementById("test").onclick = function () {
  obj.fireEvent('onclick', event);
};
```
chrome、firefox等浏览器
```
//document上绑定自定义事件ondataavailable
document.addEventListener('ondataavailable', function (event) {
  alert(event.eventType);
}, false);
var obj = document.getElementById("obj");

//obj元素上绑定click事件
obj.addEventListener('click', function (event) {
  alert(event.eventType);
}, false);

//调用document对象的 createEvent 方法得到一个event的对象实例。
var event = document.createEvent('HTMLEvents');

// initEvent接受3个参数：
// 事件类型，是否冒泡，是否阻止浏览器的默认行为
event.initEvent("ondataavailable", true, true);
event.eventType = 'message';

//触发document上绑定的自定义事件ondataavailable
document.dispatchEvent(event);
var event1 = document.createEvent('HTMLEvents');
event1.initEvent("click", true, true);
event1.eventType = 'message';

//触发obj元素上绑定click事件
document.getElementById("test").onclick = function () {
  obj.dispatchEvent(event1);
};
```

### [0]
```js
var a = [0];
if ([0]) {
  console.log(a == true); // 打印 true，表示[0]为true， 但是控台上直接输入[0] == true 却打印出false
} else {
  console.log("wut");
}
```




### 其他注意事项

  - form表单调用reset： $('#form_test')[0].reset();
  - redio设置选中：$('input[name="xxx"][value="' + val + '"]').attr("checked", true);




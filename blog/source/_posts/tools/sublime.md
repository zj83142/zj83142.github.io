---
title: sublime 使用
date: 2017-10-17 10:35:40
tags:
  sublime
categories:
  笔记
---

### sublime 安装插件

> 查看sublime的插件列表 Ctrl+Shift+P 输入：list packages

大部分插件安装步骤：

1. 在sublime中打开PackageControl, 快捷键：Ctrl+Shift+P

2. 打开Install Package 窗口，直接输入 install package 按回车

3. 弹出安装插件框，输入 vue 找到 xxx 按回车

4. 重新打开文件或重启sublime

常用插件

- 添加支持vue代码高亮控件 —— Vue Syntax Hightlight

-  超高速前端开发工具 —— Emmet Style Reflector

### sunlime 配置

选择Preference ——> settings ——> User, 贴入一下代码
```
{
  "font_size": 16, // 默认字体大小为16
  "highlight_line": true,
  "ignored_packages":
  [
    "Vintage"
  ],
  "tab_size": 2,  // 默认tab 为2个空格
  "translate_tabs_to_spaces": true
}
```

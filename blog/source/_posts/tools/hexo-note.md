---
title: hexo 笔记 —— 遇到的问题
date: 2017-09-05 21:36:36
tags:
	hexo 问题积累
categories: 
  - 笔记
---

> hexo 博客在家里电脑clone了一份，npm install 了所有的安装包后运行不起来，遇到了一些列的问题，记录一下，方便以后回顾。

1. hexo s 后报错 没有找到 layout index.html。原因是从git上clone下来的项目themes文件夹是空的，重新安装next主题：**git clone https://github.com/iissnan/hexo-theme-next themes/next**

2. hexo g 生成文件时报错 提示zh-tw.yml 错误，删掉languages文件夹下对应的字体文件。

3. 左侧导航点击报错：hexo Cannot GET /tags/，找不到tags页面，解决方法：1）$ hexo new page 'tags'。2）设置页面 type: tags。 3）在主题配置文件，菜单中添加连接：tags: /tags


---
title: 日常踩坑集合(一)
date: 2017-10-10 15:51:04
tags:
  笔记
categories:
  踩坑
---

### 解决目录层次太深删除报错的问题

问题：在使用npm当中，自动生成的node_modules文件夹，因为文件目录层级太深，无法系统删除

解决方案： 1、 安装：npm install -g rimraf（全局安装） 2、 使用：先定位目标文件夹的父级目录，然后命令行输入rimraf ***（***为需要删除的文件夹名称）
<!-- more -->

### 命令积累

- 查看域名对应的服务器ip地址，如：nslookup www.baidu.com


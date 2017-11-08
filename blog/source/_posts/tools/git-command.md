---
title: git 常用命令积累
date: 2017-10-26 14:36:28
tags:
  git
categories: 
  - 笔记
---

日常工作中使用git同步代码，虽然对git了解不是很多，但是掌握初始化、克隆、拉代码、推代码、切换分支等等，已经基本上满足一个git小白的需求了。为了方便将平时工作中用到的git命令积累起来，这样方便下次使用的查询，还可以在没事的时候翻翻博客，加强一下记忆 ...
<!-- more -->

- 修改远程仓库地址
```
  git remote set-url origin [url]
   或
  git remote rm origin
  git remote add origin [url]
```
  使用 git remote -v 查看当前远程仓库地址

### 分支操作
- 查看远程分支
```
  git branch -a
```

- 查看本地分支
```
  git branch
```

- 创建分支
```
  git branch test
```

- 把本地分支推到远程分支
```
  git push origin test
``

- 切换到分支
```
  git branch
  git checkout test
```

- 删除本地分支
```
  git branch -d test
```

- 删除远程分支
```
  git branch -r -d origin/branch -name
  git push origin :branch-name
```





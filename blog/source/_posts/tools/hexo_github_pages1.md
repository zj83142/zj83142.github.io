---
title: Hexo + github pages搭建个人博客 —— 基础篇
date: 2017-08-24 15:27:22
tags:
  - hexo
  - github pages
categories: 
  - 笔记
---

Hexo 确实是一个神奇的好东西，通过Hexo和github搭配就能生成完美的个人博客，还有丰富的主题样式可以选择。不用自己管理服务器，是不是很酷？以下记录了我搭建博客的步骤和搭建过程中遇到的一些问题。

> 学习不能含糊，不能似是而非，要有打破砂锅问到底的精神！
<!-- more -->

### 准备工作

1. 需要申请一个github帐号，用来搭载你的博客
2. 需要安装node，用来安装依赖文件
3. 安装git，因为Hexo有很多操作都涉及到命令行，可以使用Git Bash来操作
4. 需要有一个美好的心情 —— 你马上就可以有自己的博客了，激动不？

### 安装hexo

1. npm install hexo-cli -g
2. hexo init blog
3. cd blog
4. npm install 
5. hexo generate 或 hexo g
6. hexo server 或 hexo s

> 注意: 执行hexo server提示找不到该指令, 解决办法：在Hexo 3.0 后server被单独出来了，需要安装server，安装的命令如下：$ npm install hexo -server --save

到这里访问 http://localhost:4000/ 地址 就可以看到你的博客了

### 部署到github

1.申请github帐号，网站地址：https://github.com
2.新建项目，项目名为 "你的github帐号" + ".github.io", 然后在master上提交你的项目代码，这样就可以通过网址：https://"你的github帐号" + ".github.io" 来访问你的博客
3.安装 hexo-deployer-git, 用来将博客一键部署到github上， 命令： $ npm install hexo-deployer-git --save
4.修改文件夹下的  _config.yml 文件

  ```
  deploy:
    type: git
    repo: <repository url>
    branch: [branch]
  ```
  > branch为分支，默认为master,可以不配置 
    repo为仓库地址，在github上新建仓库后，可复制此地址

5.部署 $ hexo d     
  d: deploy 的缩写, 部署博客到github上

到这里访问 https:// + "你的github帐号".github.io 地址 就可以看到你线上的博客了，像我这么喜欢炫耀的人，最喜欢发给周围的人去炫耀了……

> 注意：部署到github的时候hexo文件和生成的静态文件可以放在一个git仓库下的不同分支，也可以放在不同的git仓库。如果放在同一个git仓库，需要注意的是创建分支： git checkout -b dev 创建并切换分支， git checkou master 切换到master分支


### 使用方法

1.发一篇文章
  使用命令行
  ```
  $ hexo new test  # 新建文章
  $ hexo clean  # 清除缓存，强烈建议每次执行命令前先清理缓存，每次部署前先删除 .deploy 文件夹,
  $ hexo g   # 生成网页，可以在 public 目录查看整个网站的文件
  $ hexo s   # 本地预览，'Ctrl+C'关闭
  ```
  > 注意： 有时候clean会报错，需要安装 npm install hexo-util --save

  直接方式创建文件
  在 **source/_posts/**下新建一个.md文件也可

2.添加文件分类
  直接在source/_posts 文件夹下新建文件夹即可，很方便

### 全局配置 _config.yml

```
# Hexo Configuration
## Docs: http://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/
# Site #站点信息
title:  #标题
subtitle:  #副标题
description:  #站点描述，给搜索引擎看的
author:  #作者
email:  #电子邮箱
language: zh-CN #语言
# URL #链接格式
url:  #网址
root: / #根目录
permalink: :year/:month/:day/:title/ #文章的链接格式
tag_dir: tags #标签目录
archive_dir: archives #存档目录
category_dir: categories #分类目录
code_dir: downloads/code
permalink_defaults:
# Directory #目录
source_dir: source #源文件目录
public_dir: public #生成的网页文件目录
# Writing #写作
new_post_name: :title.md #新文章标题
default_layout: post #默认的模板，包括 post、page、photo、draft（文章、页面、照片、草稿）
titlecase: false #标题转换成大写
external_link: true #在新选项卡中打开连接
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
highlight: #语法高亮
  enable: true #是否启用
  line_number: true #显示行号
  tab_replace:
  # Category & Tag #分类和标签
default_category: uncategorized #默认分类
category_map:
tag_map:
# Archives
2: 开启分页
1: 禁用分页
0: 全部禁用
archive: 2
category: 2
tag: 2
# Server #本地服务器
port: 4000 #端口号
server_ip: localhost #IP 地址
logger: false
logger_format: dev
# Date / Time format #日期时间格式
date_format: YYYY-MM-DD #参考http://momentjs.com/docs/#/displaying/format/
time_format: H:mm:ss
# Pagination #分页
per_page: 10 #每页文章数，设置成 0 禁用分页
pagination_dir: page
# Disqus #Disqus评论，替换为多说
disqus_shortname:
# Extensions #拓展插件
theme: landscape-plus #主题
exclude_generator:
plugins: #插件，例如生成 RSS 和站点地图的
- hexo-generator-feed
- hexo-generator-sitemap
# Deployment #部署，将 lmintlcx 改成用户名
deploy:
  type: git
  repo: github创库地址.git
  branch: master
```






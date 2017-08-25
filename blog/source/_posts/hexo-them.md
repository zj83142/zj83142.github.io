---
title: hexo 主题设置
date: 2017-08-25 10:26:24
tags:
---

### next主题

1. 安装 hexo-theme-next 到 themes/next文件夹下
  ```
    $ git clone https://github.com/iissnan/hexo-theme-next themes/next
  ```
2. 启用主题
  _config.yml 文件下修改 theme: next (默认 theme: landscape)

3. 主题设定 themes/next/_config.yml
  
  Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观。目前 NexT 支持三种 Scheme：

    - Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
    - Mist - Muse 的紧凑版本，整洁有序的单栏外观
    - Pisces - 双栏 Scheme，小家碧玉似的清新
    - Gemini

4. 设置头像
  在theme/next/source/images 文件加下添加avater.png 图片，然后配置theme/next/_config.yml 文件中的 avatar: /images/avatar.png

5. 修改布局宽度
  Pisces Scheme 编辑 themes/next/source/css/_schemes/Picses/_layout.styl 文件，更改以下 css 选项定义值：

  ```
    .header{ width: 1150px; }
    .container .main-inner { width: 1150px; }
    .content-wrap { width: calc(100% - 260px); }
  ```

  其他主题布局宽度调整方法：
    **第一种方法**
    现在一般都用宽屏显示器，博客页面两侧留白太多，调整一下宽度。
    打开 \themes\next\source\css\_common\components\post\post-expand.styl 文件，找到

      @media (max-width: 767px) 改为 @media (max-width: 1080px)
    
    打开 \themes\next\source\css\ _variables\base.styl 文件，找到
      ```
        $main-desktop                   = 960px
        $main-desktop-large             = 1200px
        $content-desktop                = 700px
        修改 $main-desktop 和 $content-desktop 的数值：

        1
        2
        3
        $main-desktop                   = 1080px
        $main-desktop-large             = 1200px
        $content-desktop                = 810px
      ```
    Next.Mist 主题的文章宽度至此改完了。如果你用的是 Next.Pisces，还需要继续修改。
    打开 \themes\next\source\css\_schemes\Pisces\_layout.styl 文件，将第 4 行的 width改为1080px ，修改后如下：
      ```
      .header {
        position: relative;
        margin: 0 auto;
        width: 1080px;
      }
      ```
    **第二种方法（Next文档中给出的方法）**
    
    编辑 themes/next/source/css/_variables/custom.styl 文件，新增变量：
      ```
      // 修改成你期望的宽度
      $content-desktop = 700px
      // 当视窗超过 1600px 后的宽度
      $content-desktop-large = 900px
      ```


 
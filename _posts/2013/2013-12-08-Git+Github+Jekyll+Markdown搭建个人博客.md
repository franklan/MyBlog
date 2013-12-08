---
layout: default
title: Git+Github+Jekyll+Markdown搭建个人博客
---

## Git+Github+Jekyll+Markdown搭建个人博客 ##


### 简介 ###

Git是一个开源的分布式版本控制系统，用以有效、高速的处理从很小到非常大的项目版本管理。

GitHub可以托管各种git库的站点。

GitHub Pages免费的静态站点，三个特点：免费托管、自带主题、支持自制页面和Jekyll。

Jekyll 其实是一个转换引擎，可以把markdown文本根据模板转成html文件，也就是静态网站，这样这些内容可以很方便的放置在你的服务器上。

Markdown 是一种 Markup Language(标记语言)。它提供给你一种方便阅读、方便书写的纯文本格式，让博客作者更加专注于博客内容。

### 搭建Github blog过程 ###

创建Githun账号，[Github](https://github.com/)。创建一个名为username.github.io的版本库（username是你的账户名），过10分钟，就会发现在http://username.github.io，博客已经生成了。

下载git for windows。[地址](http://windows.github.com/)

在windows上创建工程目录，名字为MyBlog。打开git shell，进入MyBlog位置。

    cd MyBlog
    git init
    git checkout --orphan gh-pages

分支必须是gh-pages。因为github规定，只有该分支中的页面，才会生成网页文件。

然后就可以开始创建博客，详细步骤可以参考[搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

详细的git操作可以参考[git - 简易指南](http://rogerdudler.github.io/git-guide/index.zh.html)


### 使用MarkdownPad2写博文 ###

我是在Win8下搭建的，
MarkdownPad2[下载地址](http://markdownpad.com/download.html)。Mac环境请参考[地址](http://www.lizherui.com/pages/2013/08/17/build_blog.html)。

在MarkdownPad2中写博文，如图所示：

<table style="width:auto;"><tr><td><a href="https://picasaweb.google.com/lh/photo/7kA0WPQ3VVWL8k23-_Zdw9MTjNZETYmyPJy0liipFm0?feat=embedwebsite"><img src="https://lh4.googleusercontent.com/-n8AelOespec/UqRXYQrOk8I/AAAAAAAAALI/kxVFw1uDaxE/s800/markdownpad2.jpg" height="500" width="800" /></a></td></tr><tr><td style="font-family:arial,sans-serif; font-size:11px; text-align:right">From <a href="https://picasaweb.google.com/107523979648406931368/BlogImage?authuser=0&feat=embedwebsite">BlogImage</a></td></tr></table>

MarkdownPad2右侧栏可以即时看到博客的样子。担心博客生成有问题可以安装本地Jekyll环境查看，[详细步骤](http://www.cnblogs.com/purediy/archive/2013/03/07/2948892.html)

使用[Picasa](https://picasaweb.google.com)管理博客的图片，上传图片，然后把图片的link拷贝回MarkdownPad2。如图所示：

<table style="width:auto;"><tr><td><a href="https://picasaweb.google.com/lh/photo/zsIQWdz__4AFMem-9EfM1NMTjNZETYmyPJy0liipFm0?feat=embedwebsite"><img src="https://lh6.googleusercontent.com/-w0uLEJcw-j8/UqRYTv9TUXI/AAAAAAAAALU/ipwropfEPOU/s800/picasa.jpg" height="500" width="800" /></a></td></tr><tr><td style="font-family:arial,sans-serif; font-size:11px; text-align:right">From <a href="https://picasaweb.google.com/107523979648406931368/BlogImage?authuser=0&feat=embedwebsite">BlogImage</a></td></tr></table>

### 完善博客 ###

可以使用Jekyll-Bootstrap改变博客的样式，[详细资料](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)。

或者直接拷贝或Fork其他博主Git库。推荐[Luke的Git库](https://github.com/kejinlu/kejinlu.github.com)。注意修改原博主的资料信息。

安装第三方评论系统[Disqus](http://disqus.com)。需要填写Site Name，Site Url，Site Shortname。记住shortname和Disqus给你的js代码，把它添加到你的_layouts目录下post.html文件中即可。[详细步骤](http://pizn.github.io/2011/11/15/use-disqus-for-your-post.html)。


### 申请独立域名 ###

在Godaddy上用支付宝花购买为期一年的顶级域名。
在你的 github 项目主目录下创建 CNAME 文件，里面输入你的域名地址，如：
www.lantianyue.com

在godaddy中修改记录，把@的对应IP改成github地址，这个地址可以通过ping得到。如图所示：

<table style="width:auto;"><tr><td><a href="https://picasaweb.google.com/lh/photo/OdhS_50p-6V5EdhOCfujH9MTjNZETYmyPJy0liipFm0?feat=embedwebsite"><img src="https://lh5.googleusercontent.com/-IiE8ZLIpymI/UqRdAk1eQcI/AAAAAAAAALg/8LAn-1r_uMM/s800/godaddy.jpg" height="500" width="800" /></a></td></tr><tr><td style="font-family:arial,sans-serif; font-size:11px; text-align:right">From <a href="https://picasaweb.google.com/107523979648406931368/BlogImage?authuser=0&feat=embedwebsite">BlogImage</a></td></tr></table>

至此，博客搭建基本完成。在实际操作过程中，我遇到了不少问题，都是自己google解决。在此想分享资料和经验的程序猿们表示感谢。

我的博客 [www.lantianyue.com](http://lantianyue.com/)。
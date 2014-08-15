title: hexo博客的优化技巧
date: 2014-08-15 20:37:43
categories: Hexo
tags: [hexo,优化]
---
##添加“多说”评论  
1、在[多说](http://duoshuo.com/)进行注册，获得通用代码。  
2、将通用代码粘贴到themes*\light\layout\_partial\comment.ejs*里面，如下：  

    <% if ( page.comments){ %>
    <section id="comment">
    通用代码
    </section>
    <% } %>
<!--more-->  
##添加『页面导航』
在刚才添加「多说」评论的文件中，加入一段代码，如下：

    <% if ( page.comments){ %>
    <nav id="pagination" >
    <% if (page.prev) { %>
    <a href="<%- config.root %><%- page.prev.path %>" class="alignleft prev" ><%= __('prev') %></a>
    <% } %>
    <% if (page.next) { %>
    <a href="<%- config.root %><%- page.next.path %>" class="alignright next" ><%= __('next') %></a>
    <% } %>
    <div class="clearfix"></div>
    </nav>
    <section id="comment">


##添加“百度分享”
到百度分享获得代码，在themes/light/layout/_partial/article.ejs中，将<%-partial('post/share')%>删掉，替换为百度分享的代码。

##添加小图标

在themes/light/layout/_partial/head.ejs里将<link href="<%- config.root %>favicon.png" rel="icon">替换为<link href="<%- config.root %>favicon.ico" rel="icon" type="image/x-ico">。将favicon.ico图标文件放在source目录下。制作图标的网站，http://www.faviconer.com。

##添加分类、标签云widget
很简单，在themes/light/_config.yml中，添加如下：

     widgets:
     - category
     - tagcloud

##添加友情链接widget
在themes/light/layout/_widget中新建名为blogroll.ejs的文件，编辑内容如下：

    <div class="widget tag">
    <h3 class="title">友情链接</h3>
    <ul class="entry">
    <li><a href="http://wlcyhao.github.io/" title="Horace's  IT Blog！">Horace</a></li>
    </ul>
    </div>

在themes/light/_config.yml中，添加如下：

    widgets:
    - blogroll

##生成post时默认生成categories配置项
在scaffolds/post.md中，添加一行categories:。同理可应用在page.md和photo.md。

##导航栏添加”关于”

 1、hexo new page "about"  
 2、到source/about/index.md编辑内容。  
 3、在themes/light/_config.yml中，添加如下：

    menu:
     关于: /about

##主页文章显示摘要

编辑md文件的时候，在要作为摘要的文字后面添加\<\!--more-->即可。


##添加RSS
hexo提供了RSS的生成插件，需要手动安装和设置。步骤如下：
安装RSS插件到本地：npm install hexo-generator-feed
开启RSS功能：编辑hexo/_config.yml，添加如下代码：

    plugins:
    - hexo-generator-feed

在站点添加链接：
在 **themes/light/_config.yml** 中，编辑**rss: /atom.xml**
在**themes/light/layout/_partial/header**，`<ul></ul>之间，添加一样代码  
```
<li> <a href="/atom.xml">RSS</a> </li>
```

##文章中插入图片
使用markdown写文章，插入图片的格式为:\!\[图片名称](链接地址)，这里要说的是链接地址怎么写。对于hexo，有两种方式：  
1、使用本地路径：在hexo/source目录下新建一个img文件夹，将图片放入该文件夹下，插入图片时链接即为/img/图片名称。  
2、使用微博图床，地址http://weibotuchuang.sinaapp.com/，将图片拖入区域中，会生成图片的URL，这就是链接地址。

##加入「fork me on github」
[这里](https://github.com/blog/273-github-ribbons)有 github 给出的教程，把代码插入到任意一个全局的模板文件中就行，比如layout.ejs的末尾。


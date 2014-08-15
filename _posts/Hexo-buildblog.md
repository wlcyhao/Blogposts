title: 利用Hexo搭建Blog
date: 2014-08-08 16:01:14
categories: Hexo
tags: [Hexo,Blog]
---
###注意：本文只针对Windows用户。

##安装Git

下载[msysgit](http://msysgit.github.io/) 并执行即可完成安装。

##安装Node.js

在 Windows 环境下安装 Node.js 非常简单，仅须[下载](http://nodejs.org/)安装文件并执行即可完成安装。

<!--more-->
##安装hexo

利用 npm 命令即可安装。（在任意位置点击鼠标右键，选择Git bash）


	npm install -g hexo


##创建hexo文件夹

安装完成后，在你喜爱的文件夹下（如D:\hexo），执行以下指令(在D:\hexo内点击鼠标右键，选择Git bash)，Hexo 即会自动在目标文件夹建立网站所需要的所有文件。


	hexo init

##安装依赖包

	npm install

##本地查看

现在我们已经搭建起本地的hexo博客了，执行以下命令(在D:\hexo)，然后到浏览器输入:[地址](localhost:4000)看看。

	hexo generate								
	hexo server

##安装light主题

###Install

	git clone https://github.com/hexojs/hexo-theme-light.git themes/light

###Update	
	cd themes/light

	git pull


好了，至此，本地博客已经搭建起来了，只是本地哦，别人看不到的。下面，我们要部署到Github。

##注册Github账号

已有账号可以跳过，没有的，请[在此](https://github.com/)进行注册，很简单，这里就不介绍了。

##创建repository

在自己[Github](https://github.com/)主页右下角，创建一个新的repository。比如我的[Github](https://github.com/)账号是abc，那么我应该创建的repository名字应该是abc.github.io。

##部署
编辑_config.yml(在D:\hexo下)。你在部署时，要把下面的abc都换成你的账号名。

	deploy:           
    type: github
    repository: https://github.com/abc/abc.github.io.git
    branch: master


执行下列指令即可完成部署。

	hexo generate
	hexo deploy

**注意**：有些新用户需要设置 ssh，否则上述命令会失败。ssh 的介绍和设置方法请看[官方教程](https://help.github.com/articles/generating-ssh-keys)，不用担心，很简单。

**记住**：每次修改本地文件后，需要hexo generate才能保存。每次使用命令时，都要在D:\hexo目录下。
Okay,我们的博客已经完全搭建起来了，在浏览器访问abc.github.io就能看到你的成就了！

##tips
hexo现在支持更加简单的命令格式了，比如：

hexo g == hexo generate

hexo d == hexo deploy

hexo s == hexo server

hexo n == hexo new
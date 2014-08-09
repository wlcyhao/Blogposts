title: 关于上传文件到github的方法
date: 2014-08-09 20:47:37
categories: github
tags: [github,上传文件]
---
##第一步、建立先仓库  

第一步的话看一般的提示就知道了，在github新建一个repository（谷歌可以解决），都是可视化的界面操作，所以难度不大。或者看这里：[官方help](https://help.github.com/articles/create-a-repo) ，虽然是英文的，但是基本都是图和代码，所以很容易读懂。

在github首页的右上角，点击红框中的Create New Repo。
![Create New Repo](/img/2012082415173733.png)
    
<!--more-->
进入新建仓库的界面  

![Create repository](/img/2012082416481736.png)

填一下仓库名称，Initialize this repository with a README是可选的，不过本人建议最好选上，可以在后面省一个步骤。填好之后，点Create repository就行了。  

##第二步、克隆仓库  

第二步开始就基本进入命令行模式了，不过要先从github上下载命令行工具。[下载地址](http://windows.github.com/)

然后进行简单的安装之后，会在桌面上创建两个图标，GitHub和Git Shell，GitHub是图形界面，Git Shell是命令行模式，而且默认的Git仓库是建在C盘的，建议要把路径重设下。

点开Git Shell，进入命令行。首先我们先要把GitHub上的我们新建的仓库clone下来，为了演示，我在GitHub上新建了一个名称为myRepoForBlog的git。

在初始化版本库之前，先要确认认证的公钥是否正确，如下：

<pre><code>
ssh -T git@github.com
</code></pre>

正确地结果如下：
<pre><code>
Warning: Permanently added 'github.com,207.97.227.239' (RSA) to the list of known hosts.  
Hi findingsea! You've successfully authenticated, but GitHub does not provide shell access.
</code></pre>

会有一个Warning，不用理会。

接下来对库进行clone，如下：
<pre><code>
git clone https://github.com/findingsea/myRepoForBlog.git
</code></pre>

上面的地址可以在如下界面找到：

![Create repository](/img/2012082417211682.png)

clone成功如下：

<pre><code>
Cloning into 'myRepoForBlog'...
Warning: Permanently added 'github.com,207.97.227.239' (RSA) to the list of known hosts.
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
</code></pre>

##第三步、上传README.md文件 
 
这个时候，我们的GitHub文件夹下就多了一个myRepoForBlog文件夹，进入文件夹目录，对仓库进行初始化，如果我们之前没有勾选创建README，则要先创建README.md文件，不然上传文件会报错。如果在第一步就勾选过了**Initialize this repository with a README**,
则可以直接进入第四步。

<pre><code>
git init  
touch README.md  
git add README.md  
git commit -m 'first_commit'  
git remote add origin https://github.com/findingsea/  myRepoForBlog.git
git push origin master
</code></pre>

##第四步、push文件

创建完README.md后，就可以push了，代码类似。
<pre><code>
git add .  
git commit -m 'first_commit'  
git remote add origin https://github.com/findingsea/myRepoForBlog.git  
git push origin master  
</code></pre>

如果执行
<pre><code>
git remote add origin https://github.com/findingsea/myRepoForBlog.git，
</code></pre>

出现错误：
<pre><code>
fatal: remote origin already exists
</code></pre>

则执行以下语句：
<pre><code>
git remote rm origin
</code></pre>

再往后执行
<pre><code>
git remote add origin https://github.com/findingsea/myRepoForBlog.git
</code></pre>


再执行
<pre><code>
git push origin master
</code></pre>

如果出现报错：
<pre><code>
error:failed to push som refs to.......
</code></pre>

则执行以下语句：
<pre><code>
git pull origin master
</code></pre>

先把远程服务器github上面的文件拉先来，再push 上去。

【结束】

再次要强调这篇文章主要是对初学者的，也就我这种github菜鸟的。

最后感谢那些无私分享自己经验和知识的博主们。
title: 使用rlwrap调用sqlplus中历史命令 
date: 2014-08-07 16:05:12
categories: Oracle
tags: [Oracle,sqlplus]
---

当在Linux Shell中运行SQL*Plus的时候，并不提供浏览历史命令行的功能。为了在Linux中达到“使用向上，向下键来跳回之前已经执行过的SQL语句”的目的，可以安装 rlwrap。

rlwrap最新的版本rlwrap-0.37.tar.gz，
<!--more-->
[官方主页]：http://utopia.knoware.nl/~hlub/uck/rlwrap/

安装完成后，可使用如下命令：

```
/usr/local/rlwrap/bin/rlwrap sqlplus
```

然后就可以使用 上、下、左、右 键来编辑已执行过的命令；

如果嫌每次输入这么长的命令很麻烦的话，可以在 oracle用户下的  **.bash_profil**中加入一条alias，如下：

```
alias sqlplus='/usr/local/rlwrap/bin/rlwrap sqlplus'
```

这样每次只要直接输入 sqlplus命令就可以使用回调命令的功能了。
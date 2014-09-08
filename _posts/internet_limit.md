title: Windows系统无Internet访问权限的解决办法
date: 2014-08-28 10:13:55
categories: Windows
tags: [网路，访问权限]
---
方法一：
<!--more-->

	运行，输入：netsh winsock reset cata.log回车
	然后再输入：netsh int ip reset reset.log回车
	最后重启系统即可

方法二：

	运行，输入:netsh int reset ipconfig/flushdns回车
	重启系统并分配静态网关
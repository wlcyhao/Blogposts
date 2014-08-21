title: windows7 连接共享时提示出现了一个错误(null)的解决办法
date: 2014-08-21 21:23:25
categories: Windows
tags: [win7,共享]
---
在windows7下跑vmware测东西，虚拟机器上不了网，启动连接共享的时候提示：internet连接共享访问被启用时，出现了一个错误(null)
<!--more-->
这是因为关掉了系统防火墙的原因。

解决办法很简单：

点击 Windows + R 在运行窗口中输入 services.msc 。

找到 windows Firewall 服务，点击启动即可。
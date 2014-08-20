title: 取消ubuntu让人崩溃的sudo
date: 2014-08-20 15:23:35
categories: 服务器运维
tags: [ubuntu,vagrant]
---
使用Vagrant，因为大部分box都是基于ubuntu的，初始化之后，它会自动生成一个账号 *vagrant*，这货不是root，于是执行各种命令前都要加一个`sudo`

本地虚拟机，就自己一个人用，还要sudo，这种脑残设定太恶心了

在ubuntu中取消默认的sudo，方法如下：

    cd ~
    echo "sudo bash">>.bash_login

重新登录之后，你就发现 root 在等你
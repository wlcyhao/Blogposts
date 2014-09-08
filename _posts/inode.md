title: 理解inode
date: 2014-09-08 08:28:23
categories: Linux
tags: inode
---
inode是linux文件系统的一个重要部分，是磁盘上用于描述文件的一种数据结构。它保存了文件的大部分重要信息，包括文件数据块在磁盘上的地址。每个inode都有自己的标识号，称为i-number。
<!--more-->
inode保存文件的下列信息：

* 文件所有权：拥有文件的用户和组。
* 文件访问模式：不同的用户和组是否可以读、写或执行文件。
* 文件时间标记：文件最后一次被 修改的时间、最后被访问的时间和inode最后被修改的时间。
* 文件类型：是否为常规文件、特殊文件或其他类型的抽象伪装文件。

文件系统被创建时，会为每个文件系统创建若干数量的inode。该数目是文件系统能容纳的最大文件数。只要不重新初始化文件系统，就不能改变这个数目，否则会损坏该文件系统上所有的数据。很有可能文件系统会将inode用光－－当文件系统中有很多很多小文件时。

使用ls -i命令可以显示文件的索引号

Gentoo bin # ls -i mysystem.sh
 3702796 mysystem.sh

文件mysystem.sh的i-number为3702796

使用df -i命令可以显示文件系统的inode使用情况
Gentoo bin # df -i
 Filesystem Inodes IUsed IFree IUse% Mounted on
 /dev/hda3 4751360 388148 4363212 9% /
 udev 64222 407 63815 1% /dev
 /dev/hda1 26104 34 26070 1% /boot
 none 64222 1 64221 1% /dev/shm

使用stat命令可以列出inode中的几乎所有信息

Gentoo bin # stat mysystem.sh
 File: `mysystem.sh’
Size: 416 Blocks: 8 IO Block: 4096 regular file
 Device: 303h/771d Inode: 3702796 Links: 1
 Access: (0755/-rwxr-xr-x) Uid: ( 0/ root) Gid: ( 0/ root)
 Access: 2007-09-13 11:29:17.000000000 +0800
 Modify: 2007-09-13 11:29:17.000000000 +0800
 Change: 2007-09-13 11:29:17.000000000 +0800

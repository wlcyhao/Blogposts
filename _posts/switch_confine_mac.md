title: 华为交换机限制某个mac使用网络的方法
date: 2014-08-27 19:00:29
categories: 交换路由
tags: [arp,mac,路由器]
---
网络里面有人接了个无线路由器，开了dhcp server，与单位的dhcp服务器冲突了，通过抓包发现了那个路由器的mac地址，想把它屏蔽了，找了一下，
<!--more-->
可以通过这个命令：

mac-address blackhole 001d-0f99-b53e

这下子，网络里面那个家伙的广播包不见了，呵呵，不错不错，不用一直接电话了。
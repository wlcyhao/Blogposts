title: 浅谈ip default-network与ip route的异同
date: 2014-08-27 19:09:19
categories: 交换路由
tags: route
---
1、`ip default-gateway`
当路由器上的ip routing无效时，使用它指定默认路由，用于RXBoot模式（no ip routing）下安装IOS等。
<!--more-->
2、`ip default-network`和`ip route 0.0.0.0 0.0.0.0`
两者都用于ip routing有效的路由器上，区别主要在于路由协议是否传播这条路由信息。比如：IGRP无法识别0.0.0.0，因此传播默认路由时必须用`ip default-network`。

当用`ip default-network`指令设定多条默认路由时，administrative distance最短的成为最终的默认路由；如果有复数条路由distance值相等，那么在路由表（show ip route）中靠上的成为默认路由。

同时使用`ip default-network`和`ip route 0.0.0.0 0.0.0.0`双方设定默认路由时，如果`ip default-network`设定的网络是直连（静态、且已知）的，那么它就成为默认路由；如果`ip default-network`指定的网络是由交换路由信息得来的，则`ip route 0.0.0.0 0.0.0.0`指定的表项成为默认路由。

最后，如果使用多条`ip route 0.0.0.0 0.0.0.0`指令，则流量会自动在多条链路上负载均衡。

## 补充一下
1,在用`ip default-gateway`(因为关掉路由,no ip routing)时,在大我数路由协议中	
	
要把通过上命令得到的路由通告出去的话,必须用启用

	#default-information originate 和redistribute static

或者可以用命

	#network 0.0.0.0

这样可以完成此静态路由的再发布.对 ip route 同样如此

2,ip routing中,如果是指向下一跳,那么administrative distance 是1,如果是指向
exit-interface外出,那么管理距离为0.

3,在负载均衡中,一般协议默认是可以4个,最大可设为6条,根据variance命令完成
但BGP是默认一条,最大可设6条,用命令`maximum-path` 完成.注意和variance实现的方式不一样.

4,为获取一跳,应用ip default-network时(因为IGRP不支持四0路由),
后面的网络号并不一定要是真实的网络,目的只是要让其他路由器知道本路由器是其他路由器发住未知网络的下一跳即可.
比如

	ip route 100.0.0.0 255.255.255.0 s0
	ip default-network 100.0.0.0
	igrp 2
	redistribute static

中,这个100….的网络可以不存在,但S0口外出是连外部网络就行,这样100….其就是0.0.0.0的替代.

同时,如果你在RIP之类的网络中启用ip default-networ命令时,RIP会把它作为一打0.0.0.0的标记通告出去,而不是具体命令所指的网络
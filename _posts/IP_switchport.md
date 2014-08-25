title: 根据IP地址查交换机端口
date: 2014-08-25 16:53:26
categories: 交换路由
tags: [交换机，端口]
---
   在一个Cisco 交换网络中间，已知某台机器的IP地址，如何找出它连接到了哪台交换机的哪个端口上呢?最方便快捷的方法使使用CiscoWorks 2000 LMS网管软件的User tracking 功能，图形化界面，一目了然。
<!--more-->
　　如果没有这个软件，也可以使用以下手工分析方法来找出答案:

　　示例网络:核心交换机为6509(交换引擎SE用CatOS, MSFC运行IOS软件)
　　1. 找出该IP所对应的MAC地址:
　　通过查看系统的ARP缓存表可以找出某IP所对应的MAC地址。由于ARP不能跨VLAN进行，所以连接各个VLAN的路由模块MSFC就是最佳的选择–一般它在每一个VLAN都有一个端口(interface vlan n)，能正确地进行ARP解释。
　　6509MSFC#ping 10.10.1.65
　　Type escape sequence to abort.
　　Sending 5, 100-byte ICMP Echos to 10.10.1.65, timeout is 2 seconds:
　　!!!!!
　　Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/4 ms
　　6509MSFC#show arp | in 10.10.1.65
　　Internet 10.10.1.65 2 0006.2973.121d ARPA Vlan2
　　通过以上命令，我们知道10.10.1.65的MAC地址是0006.2973.121d, 这是IOS设备的MAC地址表达方式，在CatOS中，应写为00-06-29-73-12-1d.

　　2.在交换机上找出MAC地址所对应的端口
　　6509SE> (enable) show cam 00-06-29-73-12-1d
　　* = Static Entry. + = Permanent Entry. # = System Entry. R = Router Entry.
　　X = Port Security Entry $ = Dot1x Security Entry
　　VLAN Dest MAC/Route Des [CoS] Destination Ports or VCs / [Protocol Type]
2 00-06-29-73-12-1d 9/41 [ALL]
　　Total Matching CAM Entries Displayed =1

　　这是不是说IP为 10.10.1.65的机器就接在端口9/41上呢?
　　不一定。如果以下命令中显示该端口上只有一个活动的MAC地址，那么答案就是肯定的:
　　6509SE> (enable) show cam dynamic 9/41
　　* = Static Entry. + = Permanent Entry. # = System Entry. R = Router Entry.
　　X = Port Security Entry $ = Dot1x Security Entry
　　VLAN Dest MAC/Route Des [CoS] Destination Ports or VCs / [Protocol Type]
　　-------------------------------------------------------

　　2 00-06-29-73-12-1d 9/41 [ALL]
　　Total Matching CAM Entries Displayed =1
　　如果该命令显示该端口上有多个活动的MAC地址，那么这个端口应该连接到别的交换机或HUB设备上，见下面的例子(查找IP为10.10.1.250所对应的交换机端口):

　　6509MSFC#ping 10.10.1.250
　　Type escape sequence to abort.
　　Sending 5, 100-byte ICMP Echos to 10.10.1.250, timeout is 2 seconds:
　　!!!!!
　　Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
　　6509MSFC#show arp | in 10.10.1.250
　　Internet 10.10.1.250 4 0009.6b8c.64ec ARPA Vlan2
　　6509SE> (enable) show cam 00-09-6b-8c-64-ec
　　* = Static Entry. + = Permanent Entry. # = System Entry. R = Router Entry.
　　X = Port Security Entry $ = Dot1x Security Entry
　　VLAN Dest MAC/Route Des [CoS] Destination Ports or VCs / [Protocol Type]
　　-------------------------------------------------------

　　2 00-09-6b-8c-64-ec 3/11 [ALL]
　　Total Matching CAM Entries Displayed =1
　　6509SE> (enable) show cam dy 3/11
　　* = Static Entry. + = Permanent Entry. # = System Entry. R = Router Entry.
　　X = Port Security Entry $ = Dot1x Security Entry
　　VLAN Dest MAC/Route Des [CoS] Destination Ports or VCs / [Protocol Type]
1 00-03-e3-4b-06-80 3/11 [ALL]
　　1 00-08-02-e6-b0-cd 3/11 [ALL]
　　1 00-02-a5-ee-f2-4f 3/11 [ALL]
　　1 00-09-6b-8c-66-d6 3/11 [ALL]
　　1 00-09-6b-63-17-d9 3/11 [ALL]
　　1 00-0b-cd-03-ec-f5 3/11 [ALL]
　　1 00-09-6b-63-17-d8 3/11 [ALL]
　　1 00-08-02-e6-b0-c1 3/11 [ALL]
　　1 00-08-02-e6-b0-85 3/11 [ALL]
　　1 00-08-02-e6-b0-81 3/11 [ALL]
　　1 00-02-a5-ef-16-af 3/11 [ALL]
　　1 00-02-a5-ee-f2-93 3/11 [ALL]
　　1 00-02-55-c6-05-61 3/11 [ALL]
　　2 00-09-6b-8c-64-ec 3/11 [ALL]
　　1 00-08-02-e6-b0-ed 3/11 [ALL]
　　1 00-08-02-e6-b0-a9 3/11 [ALL]
　　1 00-02-55-54-7a-e0 3/11 [ALL]
　　1 00-02-a5-ef-15-a6 3/11 [ALL]
　　1 00-08-02-e6-af-8f 3/11 [ALL]
　　1 00-08-02-e6-b0-bd 3/11 [ALL]
　　1 00-0b-cd-03-db-8b 3/11 [ALL]
　　1 00-09-6b-8c-25-50 3/11 [ALL]
　　Do you wish to continue y/n [n]? n

　　由于该端口连接到另一台交换机或HUB,必须继续追查，方法如下:
　　6509SE> (enable) show cdp nei 3/11
　　* – indicates vlan mismatch.
　　# – indicates duplex mismatch.
　　Port Device-ID Port-ID Platform
　　------------------------------------------------------

　　3/11 Cisco2924 GigabitEthernet1/1 cisco WS-C2924M-XL
　　该命令显示对端设备是一台Cisco2924，如果没有显示，那么说明连接的是别的厂家的设备，可能要到该交换机上用类似的办法继续追查。本例子中是Cisco 设备，所有我们可以继续:

　　6509SE> (enable) show cdp nei 3/11 de
　　Port (Our Port): 3/11
　　Device-ID: Cisco2924
　　Device Addresses:
　　IP Address: 10.10.0.60
　　Holdtime: 153 sec
　　Capabilities: TRANSPARENT_BRIDGE SWITCH
　　Version:
　　Cisco Internetwork Operating System Software
　　IOS ™ C2900XL Software (C2900XL-C3H2S-M), Version 12.0(5.2)XU, MAINTENANCE INTERIM SOFTWARE
　　Copyright (c) 1986-2000 by cisco Systems, Inc.
Compiled Mon 17-Jul-00 17:35 by ayounes
　　Platform: cisco WS-C2924M-XL
　　Port-ID (Port on Neighbors’s Device): GigabitEthernet1/1
　　VTP Management Domain: lan
　　Native VLAN: 1
　　Duplex: full
　　System Name: unknown
　　System Object ID: unknown
　　Management Addresses: unknown
　　Physical Location: unknown
　　Cisco2924#show mac-address-table dynamic address 0009.6b8c.64ec
　　Non-static Address Table:
　　Destination Address Address Type VLAN Destination Port
　　-------------------------------------------------------
　　0009.6b8c.64ec Dynamic 2 FastEthernet0/2
　　Cisco2924#show mac-address-table dynamic interface f0/2
　　Non-static Address Table:
　　Destination Address Address Type VLAN Destination Port
　　-------------------------------------------------------
　　0009.6b8c.64ec Dynamic 2 FastEthernet0/2
　　通过以上命令可知，MAC地址0009.6b8c.64ec 与Cisco 2924交换机相连，且是该端口上唯一活动的MAC地址，所以IP为10.10.1.250的机器应该就连接在这个端口上。
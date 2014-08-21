title: 交换机&路由器命令小全（带中文注释） 
date: 2014-08-09 14:45:22
categories: 交换路由
tags: [交换机,路由器]
---
##1. 交换机支持的命令：

交换机基本状态： 
<pre><code>
switch: ；ROM状态， 路由器是rommon>

hostname> ；用户模式

hostname# ；特权模式

hostname(config)# ；全局配置模式

hostname(config-if)# ；接口状态
</code></pre>
<!--more-->
交换机口令设置： 

<pre><code>
switch>enable ；进入特权模式

switch#config terminal ；进入全局配置模式

switch(config)#hostname  ；设置交换机的主机名

switch(config)#enable secret xxx ；设置特权加密口令

switch(config)#enable password xxa ；设置特权非密口令

switch(config)#line console 0 ；进入控制台口

switch(config-line)#line vty 0 4 ；进入虚拟终端

switch(config-line)#login ；允许登录

switch(config-line)#password xx ；设置登录口令xx

switch#exit ；返回命令

</code></pre>

交换机VLAN设置：

<pre><code>
switch#vlan database ；进入VLAN设置

switch(vlan)#vlan 2 ；建VLAN 2

switch(vlan)#no vlan 2 ；删vlan 2

switch(config)#int f0/1 ；进入端口1

switch(config-if)#switchport access vlan 2 ；当前端口加入vlan 2

switch(config-if)#switchport mode trunk ；设置为干线

switch(config-if)#switchport trunk allowed vlan 1，2 ；设置允许的vlan

switch(config-if)#switchport trunk encap dot1q ；设置vlan 中继

switch(config)#vtp domain  ；设置发vtp域名

switch(config)#vtp password  ；设置发vtp密码

switch(config)#vtp mode server ；设置发vtp模式

switch(config)#vtp mode client ；设置发vtp模式
</code></pre>

交换机设置IP地址：

<pre><code>
switch(config)#interface vlan 1 ；进入vlan 1

switch(config-if)#ip address   ；设置IP地址

switch(config)#ip default-gateway  ；设置默认网关

switch#dir flash: ；查看闪存
</code></pre>

交换机显示命令：

<pre><code>
switch#write ；保存配置信息

switch#show vtp ；查看vtp配置信息

switch#show run ；查看当前配置信息

switch#show vlan ；查看vlan配置信息

switch#show interface ；查看端口信息

switch#show int f0/0 ；查看指定端口信息    
</code></pre>

##2. 路由器支持的命令：

路由器显示命令：

<pre><code>
router#show run ；显示配置信息

router#show interface ；显示接口信息

router#show ip route ；显示路由信息

router#show cdp nei ；显示邻居信息

router#reload 　 　 ；重新起动
</code></pre>

路由器口令设置：
<pre><code>
router>enable ；进入特权模式

router#config terminal ；进入全局配置模式

router(config)#hostname  ；设置交换机的主机名

router(config)#enable secret xxx ；设置特权加密口令

router(config)#enable password xxb ；设置特权非密口令

router(config)#line console 0 ；进入控制台口

router(config-line)#line vty 0 4 ；进入虚拟终端

router(config-line)#login ；要求口令验证

router(config-line)#password xx ；设置登录口令xx

router(config)#(Ctrl+z) ； 返回特权模式

router#exit ；返回命令
</code></pre>

路由器配置：
<pre><code>
router(config)#int s0/0 ；进入Serail接口

router(config-if)#no shutdown ；激活当前接口

router(config-if)#clock rate 64000 ；设置同步时钟

router(config-if)#ip address   ；设置IP地址

router(config-if)#ip address  second ；设置第二个IP

router(config-if)#int f0/0.1 ；进入子接口

router(config-subif.1)#ip address  ；设置子接口IP

router(config-subif.1)#encapsulation dot1q  ；绑定vlan中继协议

router(config)#config-register 0x2142 ；跳过配置文件

router(config)#config-register 0x2102 ；正常使用配置文件

router#reload ；重新引导
</code></pre>

路由器文件操作：
<pre><code>
router#copy running-config startup-config ；保存配置

router#copy running-config tftp ；保存配置到tftp

router#copy startup-config tftp ；开机配置存到tftp

router#copy tftp flash: ；下传文件到flash

router#copy tftp startup-config；下载配置文件
ROM状态：

Ctrl+Break ；进入ROM监控状态

rommon>confreg 0x2142 ；跳过配置文件

rommon>confreg 0x2102 ；恢复配置文件

rommon>reset　 ；重新引导

rommon>copy xmodem: flash: ；从console传输文件
rommon>IP_ADDRESS=10.65.1.2 ；设置路由器IP

rommon>IP_SUBNET_MASK=255.255.0.0 ；设置路由器掩码

rommon>TFTP_SERVER=10.65.1.1 ；指定TFTP服务器IP

rommon>TFTP_FILE=c2600.bin ；指定下载的文件

rommon>tftpdnld ；从tftp下载

rommon>dir flash: ；查看闪存内容

rommon>boot ；引导IOS
</code></pre>

静态路由：
<pre><code>
ip route    ；命令格式

router(config)#ip route 2.0.0.0 255.0.0.0 1.1.1.2 ；静态路由举例

router(config)#ip route 0.0.0.0 0.0.0.0 1.1.1.2 ；默认路由举例
</code></pre>

动态路由：
<pre><code>
router(config)#ip routing ；启动路由转发

router(config)#router rip ；启动RIP路由协议。

router(config-router)#network  ；设置发布路由

router(config-router)#negihbor  ；点对点帧中继用。
</code></pre>

帧中继命令：
<pre><code>
router(config)#frame-relay switching ；使能帧中继交换

router(config-s0)#encapsulation frame-relay ；使能帧中继

router(config-s0)#fram-relay lmi-type cisco ；设置管理类型

router(config-s0)#frame-relay intf-type DCE ；设置为DCE

router(config-s0)#frame-relay dlci 16 ；

router(config-s0)#frame-relay local-dlci 20 ；设置虚电路号

router(config-s0)#frame-relay interface-dlci 16 ；

router(config)#log-adjacency-changes ；记录邻接变化

router(config)#int s0/0.1 point-to-point ；设置子接口点对点

router#show frame pvc ；显示永久虚电路

router#show frame map ；显示映射 
</code></pre>

基本访问控制列表：

<pre><code>
router(config)#access-list  permit|deny  

router(config)#interface  ；default:deny any

router(config-if)#ip access-group  in|out ；default:out
例1：

router(config)#access-list 4 permit 10.8.1.1

router(config)#access-list 4 deny 10.8.1.0 0.0.0.255

router(config)#access-list 4 permit 10.8.0.0 0.0.255.255

router(config)#access-list 4 deny 10.0.0.0 0.255.255.255

router(config)#access-list 4 permit any

router(config)#int f0/0

router(config-if)#ip access-group 4 in
</code></pre>

扩展访问控制列表：
 
<pre><code>
access-list  permit|deny icmp  <DESTINATIONIP

wild>[type]

access-list  permit|deny tcp  <DESTINATIONIP

wild>[port]

例2：

router(config)#access-list 101 deny icmp any 10.64.0.2 0.0.0.0 echo

router(config)#access-list 101 permit ip any any

router(config)#int s0/0

router(config-if)#ip access-group 101 in

例3：

router(config)#access-list 102 deny tcp any 10.65.0.2 0.0.0.0 eq 80

router(config)#access-list 102 permit ip any any

router(config)#interface s0/1

router(config-if)#ip access-group 102 out
</code></pre>

删除访问控制例表:

<pre><code>
router(config)#no access-list 102

router(config-if)#no ip access-group 101 in
</code></pre>

路由器的nat配置

<pre><code>
Router(config-if)#ip nat inside ；当前接口指定为内部接口

Router(config-if)#ip nat outside ；当前接口指定为外部接口

Router(config)#ip nat inside source static [p] <私有IP><公网IP> [port]

Router(config)#ip nat inside source static 10.65.1.2 60.1.1.1

Router(config)#ip nat inside source static tcp 10.65.1.3 80 60.1.1.1 80

Router(config)#ip nat pool p1 60.1.1.1 60.1.1.20 255.255.255.0

Router(config)#ip nat inside source list 1 pool p1

Router(config)#ip nat inside destination list 2 pool p2

Router(config)#ip nat inside source list 2 interface s0/0 overload

Router(config)#ip nat pool p2 10.65.1.2 10.65.1.4 255.255.255.0 type rotary

Router#show ip nat translation

rotary 参数是轮流的意思，地址池中的IP轮流与NAT分配的地址匹配。

overload参数用于PAT 将内部IP映射到一个公网IP不同的端口上。
</code></pre>

外部网关协议配置：

<pre><code>
routerA(config)#router bgp 100

routerA(config-router)#network 19.0.0.0

routerA(config-router)#neighbor 8.1.1.2 remote-as 200
</code></pre>

配置PPP验证：

<pre><code>
RouterA(config)#username  password 

RouterA(config)#int s0

RouterA(config-if)#ppp authentication {chap|pap}
</code></pre>

3．PIX防火墙命令

<pre><code>
Pix525(config)#nameif ethernet0 outside security0 ；命名接口和级别

Pix525(config)#interface ethernet0 auto ；设置接口方式

Pix525(config)#interface ethernet1 100full ；设置接口方式

Pix525(config)#interface ethernet1 100full shutdown

Pix525(config)#ip address inside 192.168.0.1 255.255.255.0

Pix525(config)#ip address outside 133.0.0.1 255.255.255.252
Pix525(config)#global (if_name) natid ip-ip ；定义公网IP区间

Pix525(config)#global (outside) 1 7.0.0.1-7.0.0.15 ；例句

Pix525(config)#global (outside) 1 133.0.0.1 ；例句

Pix525(config)#no global (outside) 1 133.0.0.1 ；去掉设置
Pix525(config)#nat (if_name) nat_id local_ip [netmark]

Pix525(config)#nat (inside) 1 0 0

内网所有主机(0代表0.0.0.0)可以访问global 1指定的外网。

Pix525(config)#nat (inside) 1 172.16.5.0 255.255.0.0

内网172.16.5.0/16网段的主机可以访问global 1指定的外网。
Pix525(config)#route if_name 0 0 gateway_ip [metric] ；命令格式

Pix525(config)#route outside 0 0 133.0.0.1 1 ；例句

Pix525(config)#route inside 10.1.0.0 255.255.0.0 10.8.0.1 1 ；例句
Pix525(config)#static (inside， outside) 133.0.0.1 192.168.0.8

表示内部ip地址192.168.0.8，访问外部时被翻译成133.0.0.1全局地址。
Pix525(config)#static (dmz， outside) 133.0.0.1 172.16.0.8

中间区域ip地址172.16.0.8，访问外部时被翻译成133.0.0.1全局地址。
</code></pre>
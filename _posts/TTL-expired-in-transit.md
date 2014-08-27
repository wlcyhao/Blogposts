title: ping 时显示TTL expired in transit
date: 2014-08-27 19:15:20
categories: 交换路由
tags: [ping，TTL]
---
当你在PING一台机器IP的时候出现如果显示,你可的注意了
<!--more-->

	C:\Documents and Settings\Administrator>ping *.*.4.11

	Pinging *.*.4.11 with 32 bytes of data:

	Reply from *.*.4.1: TTL expired in transit.
	Reply from *.*.4.1: TTL expired in transit.
	Reply from *.*.4.1: TTL expired in transit.
	Reply from *.*.4.1: TTL expired in transit.

在TCP/IP网络中，网络层并不对数据包进行可靠性传输保证，只通过ICMP报文提供反馈机制(例如：差错控制)。PING命令就是ICMP的请求/响应报文，也是网络最常用的测试手段。通常使用PING命令测试互通性时有以下几种消息反馈：

1、Request Time Out
2、Destination Unreachable
3、TTL Expired in transit

情况1：当信源机PING某信宿机时，信源机在一段时间内（信源机发送ICMP请求报文后，会启动定时器0）无法收到ICMP响应报文，就会产生该种情况。出现上述问题的原因在于，信源到信宿的路由正常，而信宿到信源无可用通路。

情况2：当信源机到信宿机无可用通路时，就会产生该种原因。


情况3：当信源机发送IP数据包时（ICMP是被直接封装在IP包中），会加上包的TTL（Time to Live）时间，数据包在每经过一个路由器时，路由器会将包的TTL时间减1，如果在ICMP请求报文未到信宿机之前，该数据包的TTL为0，则相应的网关丢弃该报文，同时向信源机发送ICMP的超时报文，在信源机上应将显示TTL Expired in transit消息。该问题主要是在网络内部出现了路由循环造成数据包无法到达信宿机，可使用Tracert跟踪，判断故障出处（使用该命令时最好在主机上完成）。

注：某些路由器对包的TTL时间并不是减1，但一般情况是这样。
方法：如果正常PING通某主机的情况下，可简单从回应信息中分析数据包所经过的路由跳数
i.e. replay xxx.xxx.xxx.xxx: byte=xxx time=xxxms ttl=xxx
用256减去该条信息中TTL的值，即可得所经的路由跳数，如TTL时间过小，则可能网络中出现短暂的路由环路。

在配置静态路由时易出现该种情况，动态路由协议中RIPV1易出现，而RIPV2和OSPF不易出现。
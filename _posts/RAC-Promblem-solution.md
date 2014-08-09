title: Oracle RAC安装过程中碰到问题及解决方法
date: 2014-08-09 11:19:25
categories: Oracle
tags: [Oracle,RAC]
---
##这一篇主要讨论整个RAC安装过程中碰到的问题以及是如何解决的。    
###错误一：
配置共享磁盘的问题。如果共享磁盘本身有内容，可能会导致在安装完CLUSTERWARE后，执行root.sh时出错，错误信息为：

```
Failed to upgrade Oracle Cluster Registry configuration。
```  

这时可以利用dd命令来清除ocr和voting disk的共享磁盘。类似命令为：

```
dd if=/dev/zero of=/dev/rdsk/c2t0d2s3 bs=1073741824 count=1
```

其中of指定需要清除的共享磁盘设备，而bs指定该设备的空间大小。
清除之后，再次运行root.sh，则问题消失。
<!--more-->  

###错误二：  
Oracle默认不会使用s0分区，如果指定了s0分区作为ocr或voting disk,
那么在执行root.sh时也会收到同样的错误信息：

```
Failed to upgrade Oracle Cluster Registry configuration。
```

这个时候可以指定其他的分区来替换s0分区。

###错误三：
vip找不到public interface的问题。这个问题实际上是Oracle的bug。
Oracle认为192.168.*.*、10.*.*.*、172.16.*.*——172.31.*.*等ip属于private IP。因此无法自动绑定到interface上。
在使用cluvfy工具验证时会出现下面的错误：

```
ERROR: Could not find a suitable set of interfaces for VIPs.
```
而在安装完ClusterWare后，第二个节点执行root.sh脚本之后，会出现下面的错误：
```
The given interface(s), "ce0" is not public. Public interfaces should be used to configure virtual IPs.
```

这个的解决方法就是用root身份手工启动VIPCA，进行手工配置。

Oracle对这个问题的详细描述是：

```
Doc ID:Note:316583.1：Subject:VIPCA FAILS COMPLAINING THAT INTERFACE IS NOT PUBLIC。
```

相关的bug信息是：

```
Bug 4437727 - VIPCA FAILS COMPLAINING THAT INTERFACE IS NOT PUBLIC。
``` 

###错误四：
那就是如果没有设置默认的网关信息，那么手工配置VIPCA的时候会出错。
如果/etc/defaultrouter没有正确的配置，那么启动vipca后，进行正确的配置。
Oracle会执行6个步骤，Create VIP application resource、Create GSD application resource、Create ONS application resource、Start VIP application resource、Start GSD application resource、Start ONS application resource。

当执行到第四个步骤Starting VIP application resource时后出现错误。错误信息为：

```
CRS-1006: No more members to consider   
CRS-0215: Could not start resource 'racnode1-vip'.   
CRS-1006: No more members to consider   
CRS-0215: Could not start resource 'racnode2-vip'.
```
  
配置了默认路由，就可以解决这个问题了。

###错误五：
也是在安装ClusterWare时碰到的。对于绑定PRIVATE ID的概念理解的不是很清晰，在加上cluvfy工具验证时出现的错误：

```
ERROR: Could not find a suitable set of interfaces for VIPs.
```

因此尝试手工通过下面的命令绑定VIP。

```
ifconfig eth0:1 plumb  
ifconfig eth0:1 172.25.198.224 netmask 255.255.0.0              broadcast 172.25.255.255 up  
```

但是Oracle需要自动绑定这个虚拟IP，这种通过手工绑定的方式会导致ClusterWare安装配置IP时出现下面的错误：

```
SEVERE: The virtual hostname(s), vip-node1,vip-node2, you have specified appears to be already assigned to another system on the network
```

解决方法就是去掉手工绑定的VIP，通过Oracle的配置工具使得Oracle自动进行绑定。

###错误六：
在数据库安装阶段碰到了。在Oracle编译racg_install时出现编译错误。检查log文件发现类似下面的问题：

```
makefile '/data/oracle/product/10.2/racg/lib/ins_has.mk' 的目标 'racg_install' 时出错。
```

在make.log中出现类型下面的错误：

```
racg_install// data /oracle/product/10.2/racg/lib/ins_has.mk:7: /data/oracle/product/10.2/crs/lib/env_has.mk: 
没有那个文件或目录 make:\*** 没有规则可以创建目标“/data/oracle/product/10.2/crs/lib/env_has.mk”。 停止。
```
根据错误信息发现是找不到env_has.mk文件造成的问题。而这个错误居然在METALINK和GOOGLE上都找不到产生问题的原因。

到$ORACLE_HOME/crs/lib目录下，确实找不到相应的env_has.mk。但是在$ORACLE_HOME/crs/crs/lib目录下可以找到这个文件。

造成这个错误出现的主要原因是CRS的主目录和ORACLE_HOME主目录出现冲突。

当时设置的ORACLE_HOME是/data/oracle/product/10.2，而安装Oracle的ClusterWare时指定的主目录是/data/oracle/product/10.2/crs。由于两个目录存在着嵌套关系，导致了这个问题的产生。

而后将ORACLE_HOME设置为/data/oracle/product/10.2/database，而Oracle的ClusterWare目录不变，仍为/data/oracle/product/10.2/crs，错误不在出现。

###错误七：
在第二次重建系统时出现的。由于存储设备没有进行格式化。因此存储设备本身保留了ASM的配置信息。

这种情况下第一次已经分配的ASM磁盘对于新的ASM实例是不可用的。

解决方法是通过第一个问题中介绍的清除ocr和voting disk的方式来清除裸设备的信息，比如：

```
dd if=/dev/zero of=/dev/rdsk/c2t0d2s3 bs=1073741824 count=1
```

更为重要的是，选择ASM磁盘组的时候，不能选择和上次配置同名的磁盘组，否则会出现错误信息：

```
ORA-15032和ORA-15063。
```

指定另外的ASM磁盘组名称后，问题得以解决。

###错误八：
在安装5117016补丁集后出现的节点2上的数据库无法启动的错误。

错误信息为：

```
ORA-01078: failure in processing system parameters 
ORA-01565: error in identifying file '+DISK/testrac/spfiletestrac.ora' 
ORA-17503: ksfdopn:2 Failed to open file +DISK/testrac/spfiletestrac.ora 
ORA-03113: end-of-file on communication channel
```

同时从后台的alert文件中可以看到如下的错误：

```
Errors in file /data/oracle/admin/testrac/udump/testrac2_ora_4598.trc: 
ORA-07445: 出现异常错误: 核心转储 [kkxcms()+1160] [SIGSEGV] [Address not mapped to object] [0x000000168] [] []
```

Oracle的Note:390591.1上有详细的描述和解决方法。

整个过程碰到的比较麻烦和难于处理的问题都已经列出来了。安装过程中碰到的小问题更多。可能这些问题对于一个有了一定安装经验的人来说，不算什么。 但是对于缺少安装经验的人或者第一次尝试安装RAC环境的人，每个问题都是一次考验。考验你是否进行了充分的知识准备；考验你的问题分析、解决能力；考验 你搜索、寻找问题解决方法的能力；最重要的是考验你的信心、耐心和毅力。
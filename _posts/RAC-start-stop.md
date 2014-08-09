title: RAC启动与停止的先后顺序小结 
date: 2014-08-07 14:59:56
categories: Oracle
tags: [Oracle,RAC]
---
RAC启动与停止的先后顺序小结
 
 
###停止RAC：
                emctl stop dbconsole
                srvctl stop instance -d rac -i rac1  
                srvctl stop instance -d rac -i rac2
                srvctl stop asm -n rac1
                srvctl stop asm -n rac2
                srvctl stop nodeapps -n rac1
                srvctl stop nodeapps -n rac2
<!--more-->
###启动RAC：     
                和上面的步骤正好相反即
                srvctl start nodeapps -n rac1
                srvctl start nodeapps -n rac2
                srvctl start asm -n rac1
                srvctl start asm -n rac2
                srvctl start instance -d rac -i rac2
                srvctl start instance -d rac -i rac1
                emctl start dbconsole 

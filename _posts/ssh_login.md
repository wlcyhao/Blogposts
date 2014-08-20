title: 提高ssh登录服务器的响应速度
date: 2014-08-20 15:09:04
categories: 服务器运维
tags: [ssh,login]
---
server A--> server B

从A登录B，修改服务器B上的ssh配置文件: `/etc/ssh/sshd_config` 在文件尾部添加一行

`UseDNS no`

重启sshd，明显可以感觉到ssh登录速度的提升
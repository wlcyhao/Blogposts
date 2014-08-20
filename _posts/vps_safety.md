title: 3步做好vps安全防范
date: 2014-08-20 15:19:21
categories: 服务器运维
tags: [linux,安全,ssh]
---
如果有垃圾扫描vps的sshd端口，烦不胜烦，赶紧加强一下防范，图个清静

1. `passwd` 把登陆密码改成30位以上，越多越好，字母数字符号大小写都要包括，然后日常管理用ssh密钥对的方式登陆
2. 改掉sshd标准的22端口，修改`/etc/ssh/sshd_config`这个文件即可，记得要重启sshd
>
    /etc/init.d/ssh restart

3. 把 `iptables -I INPUT -p tcp -s 要封的IP --dport 骚扰的端口 -j DROP ` 这条命令放在手边，随时可以执行

攻击日志在/var/log/secure里面，经常查看，如果这个文件不存在,那么可以在 `/etc/syslog.conf` 里面配置
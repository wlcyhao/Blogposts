title: 远程ssh登陆后关掉机房机器的脚本
date: 2014-08-25 16:43:33
categories: 服务器运维
tags: [ssh,远程]
---
需求：
机房到放学时候学生都不走，希望在管理机上ssh登陆关机，对整个机房的所有学生机执行关机命令。
解决办法一：
passwd.txt 存放ip和密码，格式为：ip空格密码，比如
192.168.1.1 123456
192.168.1.2 654321
<!--more-->
expect.sh
```
#!/usr/local/bin/expect
spawn ssh root@[lindex $argv 0]
sleep .2
expect “(yes/no)?” {
send “yes\r”
expect “*assword:”
send “[lindex $argv 1]\r”
} “*assword:”
send “[lindex $argv 1]\r”
expect -re {[$#] }
send “halt -p\r”
expect EOF
```
```
#!/bin/sh
while read ip passwd
do
echo “$host…..”
/tmp/expect.sh $ip $passwd
done < passwd.txt
```
为了省事，halt.sh里面调用expect就用了绝对路径，需要根据实际需要修改，呵呵。

解决方法二：
passwd.txt同上面存放ip和密码，格式为：ip空格密码，比如
192.168.1.1 123456
192.168.1.2 654321

halt.sh
```
#!/usr/local/bin/expect
set f [open passwd.txt r]
set timeout 150
while {[gets $f line] >= 0} {
spawn ssh root@[lindex $line 0]
expect “(yes/no)?” {
send “yes\r”
expect “*assword:”
send “[lindex $line 1]\r”
} “*assword:”
send “[lindex $line 1]\r”
expect -re {[$#] }
send “halt -p\r”
expect eof
}
close $f
```
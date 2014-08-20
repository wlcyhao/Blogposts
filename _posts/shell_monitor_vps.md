title: 用shell脚本监控vps实时流量
date: 2014-08-20 14:52:13
categories: 服务器运维
tags: [linux,iftop,流量监控]
---
一般来说，实时流量监控有两种方法

* 安装iftop，它会通过ascii图形化显示实时流量数据，比较直观
* 用shell脚本采集`/proc/net/dev`中的实时数据，很简单，不依赖任何安装包，对于内网linux服务器特别有帮助

<!--more-->

脚本如下:

```bash
#!/bin/bash

# test network width

function usage

{

        echo "Usage: $0  "

        echo "    e.g. $0 eth0 2"

        exit 65

}


if [ $# -lt 2 ];then
usage
fi
typeset in in_old dif_in
typeset out out_old dif_out
typeset timer
typeset eth

eth=$1
timer=$2

in_old=$(cat /proc/net/dev | grep $eth | sed -e "s/\(.*\)\:\(.*\)/\2/g" | awk ' { print $1 }' )
out_old=$(cat /proc/net/dev | grep $eth | sed -e "s/\(.*\)\:\(.*\)/\2/g" | awk ' { print $9 }' )

while true
do
sleep ${timer}
in=$(cat /proc/net/dev | grep $eth | sed -e "s/\(.*\)\:\(.*\)/\2/g" | awk ' { print $1 }' )
out=$(cat /proc/net/dev | grep $eth | sed -e "s/\(.*\)\:\(.*\)/\2/g" | awk ' { print $9 }' )
dif_in=$(((in-in_old)/timer))
dif_out=$(((out-out_old)/timer))
echo "IN: ${dif_in} Byte/s OUT: ${dif_out} Byte/s "
in_old=${in}
out_old=${out}
done
exit 0
```

使用很简单，只有两个参数

+ 参数1， 网卡设备号，一般就是 eth0
+ 参数2，统计间隔的秒数，2 表示2秒计算一次
+ 流量统计单位是 Byte/s
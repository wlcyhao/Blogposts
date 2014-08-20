title: 消除 vi 突然显示空格的颜色
date: 2014-08-20 15:05:47
categories: 服务器运维
tags: [vim]
---

不知道为什么，用vi打开代码文件，空格的地方全部是黄色亮块，清楚办法
```
  cd ~

  open .vimrc

  加入一行 set nohls
```

set nohls在vim里面可以直接用，只限于当前session，写入到vimrc中，可以持久化保存，vimrc里面还可以放入更多的配置指令

另外

`cd -` 是个好命令，可以回到上一次路径

title: 修复secureCRT 登录 vps 中文乱码问题
date: 2014-08-20 15:00:45
categories: 服务器运维
tags: [linux,vps]
---
让vps正常显示中文，只需要三步

1. `vim ~/.profile` 加入下面一行 export LANG="zh_CN.UTF-8"

2. 修改SecureCRT 编码 
选项->会话选项->外观->字符编码->uft-8，退出重新登录就行了

3. 让vim支持中文显示，修改文件（如果没有就新建一个）:~/.vimrc，添加下面三行
```
    1. set encoding=utf-8
    2. set fileencodings=utf-8,gb2312,gb18030,gbk
    3. set termencoding=utf-8

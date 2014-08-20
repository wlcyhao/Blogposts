title: ssh指定私钥的配置管理
date: 2014-08-20 11:40:50
categories: 服务器运维
tags: ssh
---
# ssh指定私钥的配置管理

用ssh 公钥认证方式登录非常常见，快速方便，通常就是用`ssh-keygen -t rsa` 在 ~/.ssh 目录下生成默认名称的id_rsa和id_rsa.pub 文件

特别是git流行之后，用ssh的方式访问git服务器，部署起来最容易

实际上，ssh 私钥的名称和生成地点都可以单独指定的, **和硬件操作系统无关**
<!--more-->
你可以在任意一台服务器上生成 公钥,私钥对，保存起来，到处分发使用

指定的配置文件叫做 ssh_config , man ssh_config ，有详细说明

## 个性化指定 私钥的步骤

vi ~/.ssh/config文件，加入如下 配置行
```
1.   Host xxxx
2.   IdentityFile 私钥文件名（不是id_rsa.pub)
3.   Port 端口号
4.   User 你登陆xxxx服务器用的账号
     ...
```

**注意：**

+ 这个文件没有格式，一行一条记录，不需要tab缩进
+ 你可以在这个文件里面指定多个Host
+ 一个 Host开头的行 到下一个 Host开头的行 是 这个host的细节设置
+ 全局的ssh config 文件在` /etc/ssh/ssh_config`
title: 怎么去掉WIN7开机时选择用户登陆？
date: 2014-08-29 19:15:19
categories: Windows
tags: [win7,登陆]
---
WIN7在开机时不会自动进入系统，而是停留在一个选择用户的界面，当然有的GHOST版是去掉了这个，但是可能使用纯净版或正版会遇到这个问题。
<!--more-->
## 方法/步骤：

+ “Win + R”快捷键调出运行命令窗口在窗口

+ 输入 `control userpasswords2`或输入
`Rundll32 netplwiz.dll,UsersRunDll`后回车

+ 在弹出的对画框中选中你想登录的用户名

+ 去掉“要使用本机，必须输入用户名和密码”前的勾选！ 确定后，退出。

+ 最后会弹出登录的界面,确定


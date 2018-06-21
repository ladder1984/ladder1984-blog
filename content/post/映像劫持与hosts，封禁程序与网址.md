---
title: 映像劫持与hosts，封禁程序与网址
id: 1027
categories:
  - About计算机
date: 2014-01-09 16:42:13
tags:
---

当Deadline临近，项目还没完成，必须管住自己乱点的手，比如禁掉自己打开某个网页，某个游戏。可以通过映像劫持和hosts解决。

## 封禁制定程序-映像劫持：

1.  找到要封禁的程序的名字，比如war.exe
2.  开始-运行-输入regedit (或者win+R，输入regedit)，打开注册表
3.  找到HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image
File Execution Options\
4.  新建项，名字为上述的进程名，比如war3.exe
5.  在该项中新建字符串值，名称为**debugger** ，值为1

可以和war3说再见了。。。

## 封禁指定网页-hosts：

1.  找到hosts文件，一般在C:\Windows\System32\drivers\etc
2.  记事本打开hosts
3.  输入如下格式，`0.0.0.0 user.qzone.qq.com ` 注意**不**要加http://
4.  可以顺手在cmd里输入ipconfig /flushdns，刷新下本地DNS缓存

忽然想起以前写的[在SNS社交网络注销账号](http://www.itoldme.net/archives/689 "在SNS社交网络注销账号")

* * *

好久没写文章了，也忽然不知道写什么了，随便写点东西。

按照上文，封下自己的电脑，然后默默继续课程设计。。。

哎。。。。
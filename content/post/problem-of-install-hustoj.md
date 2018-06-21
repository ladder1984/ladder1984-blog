---
title: Ubuntu下HUSTOJ安装时候遇到的问题——命令行方面
tags:
  - oj
  - ubuntu
id: 42
categories:
  - About互联网
date: 2012-12-06 22:24:14
---

### 0.LIVECD安装时未能进入图形界面

> alt+F1,输入startx

### 2.修改相关密码

> 修改root密码sudo passwd root
> 
> 修改mysql密码： mysqladmin -uroot -proot password 123456
> 
> 网站管理员 admin/admin

### 3.安装openssh

> sudo apt-get install openssh-server
> 
> > 如果遇到：
> > 
> > 在终端输入，sudo apt-get install opensshserver
> >     正在读取软件包列表...完成
> >     正在分析软件包的依赖关系树
> >     正在读取状态信息...完成
> >     现在没有可用的软件包 openssh-server，但是他被其他的软件包引用了
> >     这可能意味着这个缺失的软件包可能已被废弃，
> >     或者只能在其他发布源中找到
> >     E:软件包 openssh-server 还没有可供安装的候选者
> > 
> > **则**
> > 
> > 先用
> >     sudo apt-get update
> >     然后再
> >     sudo apt-get install openssh-server

<font size="4" face="宋体">4.安装phpmyadmin</font>

> <font size="4" face="宋体">在ubuntu下，运行： </font>
>   <font size="4" face="宋体">sudo apt-getinstall phpmyadmin </font>
>   <font size="4" face="宋体">过一会后会有一些设置，如选择服务器、密码设定等等内容。安装完成后，访问http://localhost/phpmyadmin会出现404错误，这是因为没有将phpmyadmin目录映射到apache目录下面，运行下面命令即可： </font>
>   <font size="4" face="宋体">sudo ln -s /usr/share/phpmyadmin /var/www</font>

<font size="4" face="宋体"></font>

</blockquote>
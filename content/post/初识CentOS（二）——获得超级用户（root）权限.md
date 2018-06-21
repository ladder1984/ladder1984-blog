---
title: 初识CentOS（二）——获得超级用户（root）权限
tags:
  - centos
  - Linux
  - 笔记
id: 736
categories:
  - About计算机
date: 2013-05-19 01:10:35
---

　　为了安全起见，linux对普通用户权限限制较大，但我们平时还是需要超级用户权限的，这类似于windows里的管理员权限。本文简单介绍如何在centos中获取超级用户权限，各linux发行版类似

# 一.直接以root用户登录

　　在登陆界面选择“other”后输入用户名root及密码直接登陆即可。如图：![centos登陆界面](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130519003908.png)

# 二.普通用户进入超级用户状态

　　在终端输入su -，然后输入密码即可，如图：![](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130519004434.png)

# 三.以普通用户使用超级用户权限

　　一些命令的执行只有root有权限，这时候只要在命令前加上sudo即可。但需要注意的，默认情况下，centos并不给普通用户sudo的权限。需要编辑/etc/sudoers 文件进行添加。方法如下：

<pre>  
su - //进入root用户状态
chmod a+w /etc/sudoers //把该文件修改为可写状态
gedit /etc/sudoers //用gedit编辑器打开文件
root ALL(ALL) ALL一行之后加上用户名root ALL=(ALL) ALL //注意中间的空格
</pre>

如图：![](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130519010140.png)
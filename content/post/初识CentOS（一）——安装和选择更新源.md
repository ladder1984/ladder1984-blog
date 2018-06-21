---
title: 初识CentOS（一）——安装和选择更新源
tags:
  - centos
  - Linux
  - 笔记
id: 706
categories:
  - About计算机
date: 2013-05-08 22:04:25
---

## 什么是CentOS

CentOS（Community Enterprise Operating System）是Linux发布版之一，它是来自于Red Hat Enterprise Linux依照开放源代码规定发布的源代码所编译而成。由于出自同样的源代码，因此有些要求高度稳定性的服务器以CentOS替代商业版的Red Hat Enterprise Linux使用。两者的不同，在于CentOS并不包含封闭源代码软件。CentOS 对上游代码的主要修改是为了移除不能自由使用的商标。

简单来说，CentOS就是一个优秀RedHat的替代品，适合不想用盗版又没钱买商业版Red Hat（RHEL，Red Hat Enterprise Linux）的人。 更多请参考维基百科词条[CentOS](https://zh.wikipedia.org/wiki/CentOS)

* * *

## 下载Centos

打开CentOS官方网站，点击Downloads，选择一个镜像（Mirror）下载，下载CD1即可，CD2是软件包。

* * *

## 安装CentOS

按理说，安装应该是个简单的事情，推荐像我这样的新手在虚拟机里尝试，主流的虚拟机有VMware和VirtualBox（免费），VMware提供CentOS的快速安装，相当简便。

* * *

## 更新yum

#### 什么是Yum

**Yum** (Yellow dog Updater, Modified) 由Duke University团队，修改Yellow Dog Linux的Yellow Dog Updater开发而成，是一个基于 RPM 包管理的字符前端软件包管理器。能够从指定的服务器自动下载 RPM 包并且安装，可以处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。被Yellow Dog Linux本身，以及Fedora、Red Hat Enterprise Linux采用。

#### 选择Yum更新源

默认的yum更新源速度很慢，建议使用国内的镜像，比如网易的，方法如下：

使用网易更新源：

> wget http://mirrors.163.com/.help/CentOS-Base-163.repo

刷新设置：

> yum makecache

更新yum到最新

换过yum更新源后就享受飞一样的速度吧，博主1+M/s的下载速度。

> yum -y update

* * *

**未完待续**

<menu id="userscript-search-by-image" type="context"></menu><menu id="userscript-search-by-image" type="context"></menu>
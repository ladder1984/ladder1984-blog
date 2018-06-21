---
title: 虚拟机安装Linux操作系统
tags:
  - centos
  - Linux
  - 虚拟机
id: 708
categories:
  - About计算机
date: 2013-05-09 21:59:07
---

# 什么是linux

**Linux**（/ˈlɪnəks/ LIN-əks）是一种自由和开放源代码的类UNIX操作系统。定义Linux的组件是Linux内核，该操作系统内核由林纳斯·托瓦兹在1991年10月5日首次发布。

严格来讲，术语Linux只表示操作系统内核本身，但通常采用Linux内核来表达该意思。Linux则常用来指基于Linux内核的完整操作系统，包括GUI组件和许多其他实用工具。由于这些支持用户空间的系统工具和库主要由理查德·斯托曼于1983年发起的GNU计划提供，自由软件基金会提议将该组合系统命名为GNU/Linux。

Linux也是自由软件和开放源代码软件发展中最著名的例子。只要遵循GNU通用公共许可证,任何个人和机构都可以自由地使用Linux的所有底层源代码，也可以自由地修改和再发布。通常情况下，Linux被打包成供个人计算机和服务器使用的Linux发行版，一些流行的主流Linux发布版，包括Debian（及其派生版本Ubuntu，Linux Mint），Fedora（及其相关版本Red Hat Enterprise Linux，CentOS）和openSUSE等。Linux发行版包含Linux内核和支撑内核的实用程序和库 ，通常还带有大量可以满足各类需求的应用程序。个人计算机使用的Linux发行版通常包X Window和一个相应的桌面环境，如GNOME或KDE。桌面Linux操作系统常用的应用程序,包括Firefox网页浏览器，LibreOffice办公软件， GIMP图像处理工具等。由于Linux是自由软件，任何人都可以创建一个符合自己需求的Linux发行版。

参见维基百科Linux词条：[http://zh.wikipedia.org/zh-cn/Linux](http://zh.wikipedia.org/zh-cn/Linux)

# 选择Linux发行版

Linux并不像Microsoft Windows有统一的版本，而是由大量的发行版，如果你有兴趣，也可以自己做一个发行版。主流的Linux发行版有RedHat、Debian、Ubuntu、Fedora、Centos等等。推荐使用CentOS，本文以下内容也以CentOS为例。原因很简单，RedHat是目前服务器领域最常见的Linux发行版之一，大学机房中安装的一般也是RedHat，而CentOS是RedHat的免费版本。

### 关于CentOS

**CentOS**（Community Enterprise Operating System）是Linux发布版之一，它是来自于Red Hat Enterprise Linux依照开放源代码规定发布的源代码所编译而成。由于出自同样的源代码，因此有些要求高度稳定性的服务器以CentOS替代商业版的Red Hat Enterprise Linux使用。两者的不同，在于CentOS并不包含封闭源代码软件。CentOS 对上游代码的主要修改是为了移除不能自由使用的商标

更多Linux请参见：Linux发行版列表: [http://zh.wikipedia.org/wiki/Linux%E5%8F%91%E8%A1%8C%E7%89%88%E5%88%97%E8%A1%A8](http://zh.wikipedia.org/wiki/Linux%E5%8F%91%E8%A1%8C%E7%89%88%E5%88%97%E8%A1%A8)

# 什么是虚拟机

虚拟机（Virtual Machine），在计算机科学中的体系结构里，是指一种特殊的软件，他可以在计算机平台和终端用户之间创建一种环境，而终端用户则是基于这个软件所创建的环境来操作软件。在计算机科学中，虚拟机是指可以像真实机器一样运行程序的计算机的软件实现。

简单来说，虚拟机就是在你的电脑系统上装一个虚拟的电脑系统。参见：[http://zh.wikipedia.org/zh-cn/%E8%99%9B%E6%93%AC%E6%A9%9F%E5%99%A8](http://zh.wikipedia.org/zh-cn/%E8%99%9B%E6%93%AC%E6%A9%9F%E5%99%A8)

目前主流的虚拟机软件有VMware和VirtualBox。论功能而言，前者更强大，后者的有点是免费和轻量级。本文以VMware Workstation为例。

# 虚拟机安装Centos

### 下载VMware

在官网下载，地址为：[http://www.vmware.com/cn/products/desktop_virtualization/workstation/overview.html](http://www.vmware.com/cn/products/desktop_virtualization/workstation/overview.html)，或者网盘下载（速度较快）：[http://pan.baidu.com/share/link?shareid=371659&amp;uk=1779553941](http://pan.baidu.com/share/link?shareid=371659&amp;uk=1779553941),

汉化补丁：[http://pan.baidu.com/share/link?shareid=427719&amp;uk=1779553941](http://pan.baidu.com/share/link?shareid=427719&amp;uk=1779553941)，

使用方法参见：[http://bbs.kafan.cn/thread-1495026-1-1.html](http://bbs.kafan.cn/thread-1495026-1-1.html)。

**注意**：VMware Workstation是收费软件，在试用期过后请购买正版或自行google解决。这是一枚测试用的key：<span style="font-family: 微软雅黑;"><span style="font-size: medium;"><span style="color: green;">1F21T-2P313-TZQ69-EV3Q6-2C11U</span></span></span>

下载完后，除了赶紧去安装，我想不出什么更好的描述。

### 下载CentOS

官方下载地址是[http://www.centos.org/modules/tinycontent/index.php?id=15](http://www.centos.org/modules/tinycontent/index.php?id=15)，就速度而言，当然是国内镜像速度更快些。网易镜像地址为：http://mirrors.163.com/centos/，最新版本CentOS 6.4下载链接：[http://mirrors.163.com/centos/6.4/isos/i386/CentOS-6.4-i386-bin-DVD1.iso](http://mirrors.163.com/centos/6.4/isos/i386/CentOS-6.4-i386-bin-DVD1.iso)

如果你不知道下载到的这个扩展名为iso的文件是什么的话，首先可以肯定的是这不是能吃的东西，并且请继续阅读文章。如果你知道，就不必继续阅读了。

### 开始安装

点击左上角，“文件-新建虚拟机”，打开新建虚拟机向导 ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130509213336.png) ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130509213223.png) ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130509213359.png) ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130509213412.png) ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130509213442.png) ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130509213503.png) 然后等待安装完毕： ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130509213526.png) ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130509213607.png)

就这么简单的安装完毕了，虚拟机会自动重启系统： ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130509215031.png)

现在就可以使用CentOS了： ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/20130509215533.png)

**注意：**ctrl+alt把鼠标退出虚拟机。

更多关于CentOS的文章，请关注本站[《初识CentOS》](http://www.itoldme.net/archives/706)系列教程。

<menu id="userscript-search-by-image" type="context"></menu>
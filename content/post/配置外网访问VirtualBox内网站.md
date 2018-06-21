---
title: 配置外网访问VirtualBox内网站
tags:
  - VirtualBox
  - 虚拟机
id: 18
categories:
  - About互联网
date: 2012-12-02 17:32:57
---

VirtualBox网址http://www.virtualbox.org/ 网络连接方式选择NAT ![](http://www.itoldme.net/wordpress/wp-content/uploads/2014/09/18722.png)

选择端口转发,主机端口任意填写,如8888,子系统端口要选择默认端口80.IP可以忽略![](http://www.itoldme.net/wordpress/wp-content/uploads/2014/09/18723.png)

&nbsp;

保存设置即可.

&nbsp;

访问主机IP的8888端口即可,如[http://11.11.11.11:8888](http://11.11.11.11:8888)

让别人telnnet 本机IP 80即可知道自己的80端口是否被封

由于ISP一般把80端口封了,所以主机端口选80没戏了
<div class="pv-gallery-preloaded-img-container" style="display: none;"></div>
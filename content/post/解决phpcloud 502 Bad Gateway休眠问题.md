---
title: 解决phpcloud 502 Bad Gateway休眠问题
tags:
  - phpcloud
id: 511
categories:
  - 网站建设
date: 2013-04-09 08:20:20
---

　　phpcloud是个不错的免费空间,探针可见[http://blog.itoldme.net/tz.php](http://blog.itoldme.net/tz.php)，但是也存在一个问题，如果你cname方式绑定的域名一段时间没有访问，那么通过这个域名就会进入休眠机制，即无法访问。这个时间段有多长，据说是6小时，但可以肯定的是24小时之内。还好这个问题可以轻松的解决。

休眠后访问绑定的域名会看到如下提示：

> 502 – Bad Gateway The website encountered an error while serving your request. Please wait a few seconds and try to refresh the page. If it does not help then please provide feedback so our engineers can try to resolve the problem. Please mention the URL or container name you failed to access and the error code below. HTTP Error 502 (Bad Gateway): reported by nginx.

## 解决方法：

1.手动访问&#42;**.my.phpcloud.com形式的原域名激活

2.既然要访问，那找个东西去访问它就好了，比如在线网站监控。比如[http://www.siteuptime.com](http://www.siteuptime.com),免费帐号可以添加一个监控，最短监控间隔是半小时。注册后在My Monitors 中Add (1 available)即可 ![enter image description here](http://www.itoldme.net/wordpress/wp-content/uploads/2013/12/adasfwd.png)
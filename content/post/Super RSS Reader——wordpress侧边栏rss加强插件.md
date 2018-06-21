---
title: Super RSS Reader——wordpress侧边栏rss加强插件
tags:
  - wordpress
id: 902
categories:
  - 网站建设
date: 2013-09-22 20:43:10
---

　　作为一名RSS低端老用户，[悼念完Google Reader](http://www.itoldme.net/archives/884)依然还是用着RSS，阅读的频率反而比以前更高了。本想写篇文章比较下当前的RSS Reader，但Google Reader之后的时代，各家也是更新频繁，没有比较的意义。现在看来，[InoReader](https://inoreader.com/)和[The Old Reader](http://theoldreader.com)还是比较不错的。

　　今天兴起，在[墙外的梯子](http://www.itoldme.net)的右侧边栏加上了RSS分享，由于刚迁移到The Old Reader，所以分享的条目非常少，会慢慢增加的。本博客的更新频率下降了，但文章的分享会逐渐的增多的。

　　wordpress自带的RSS小工具，使用体验感觉很糟糕，所以就搜索插件，找到了Super RSS Reader。该插件可在**后台搜索**或**wordpress插件中心**[下载](http://wordpress.org/plugins/super-rss-reader/)。
![](http://www.itoldme.net/wordpress/wp-content/uploads/2013/09/super-rss-reader.png)
如图所示，该插件主要有以下功能：

*   显示描述 （自带小工具是鼠标放在链接上显示，感觉很糟糕）
*   如果缩略图存在则显示
*   显示动画 （好吧，我关掉了）
*   多种样式 （几个挺简洁的样式）
*   新窗口打开链接 （这是自带小工具所没有的，也是我最需要的）

另外，略微修改，就可以给链接加上nofollow，方法如下：

在插件编辑中打开Super RSS Reader中的Super RSS Reader.php文件。搜索_blank并找到

> $newtab = ($open_newtab) ? ' target="_blank"' : '';

添加rel="nofollow"，修改为：

> $newtab = ($open_newtab) ? 'rel="nofollow" target="_blank"' : '';

　　即便Google Reader陨落，RSS Reader却越多越好，体会到RSS好处的人也不会放弃RSS，至少在相当长的可见未来内，RSS不会消失。而即便RSS消失了，也不会是因为**发现-订阅-阅读**信息的需求消失，而是出现了更好的方式。
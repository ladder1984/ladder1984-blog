---
title: 把wordpress迁移到子目录
tags:
  - wordpress
id: 1318
categories:
  - 网站建设
date: 2014-12-12 23:56:13
---

之前把wordpress安装在根目录，虽然这样建立wordpress很方便，但也会把根目录弄得很混乱，尤其是建立其他站点时很不方便，好在把wordpress迁移到子目录，并且**不改变站点地址**并不麻烦。

方法可以参考官方文档[将 WordPress 文件置于独立子目录](http://codex.wordpress.org/zh-cn:%E5%B0%86_WordPress_%E6%96%87%E4%BB%B6%E7%BD%AE%E4%BA%8E%E7%8B%AC%E7%AB%8B%E5%AD%90%E7%9B%AE%E5%BD%95)。

## 迁移方法

1.  建立子目录，如wordpress
2.  进入wordpress后台的**设置** - **常规**，设置index.php 和 .htaccess为新的地址，比如http://www.itoldme.net/wordpress，站点地址（URL）无需修改。
3.  把wordpress所有文件**剪切**到wordpress目录下
4.  把index.php 和 .htaccess**复制**到根目录
5.  编辑index.php，require('./wp-blog-header.php'); 修改为 require('./wordpress/wp-blog-header.php'); 其中的wordpress是你的子目录
6.  此刻站点迁移完成，新的后台地址将会添加子目录，比如成为http://example.com/wordpress/wp-admin/

## 注意事项：

1.  如果图片放置在wordpress中，由于wordpress采用绝对路径，所以新的图片路径也变了，会在wp-content前添加子目录名称，如www.itoldme.net/wordpress/wp-content/uploads/。则文章中引用的图片路径需要修改，方法是修改数据库。如果不会sql语句的话，可以使用插件**Search &amp; Replace**，把www.itoldme.net/wp-content/uploads/全部替换成www.itoldme.net/wordpress/wp-content/uploads/，如图![Search &amp; Replace示意图](http://www.itoldme.net/wordpress/wp-content/uploads/2014/12/20141212235031.png)

2.  如果启用了WP Super Cache的CDN功能，需要在站点地址Off-site URL中添加子目录，如图![cdn设置示意图](http://www.itoldme.net/wordpress/wp-content/uploads/2014/12/20141213000651.png)